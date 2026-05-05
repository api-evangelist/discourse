---
title: "How Jennifer Aniston and Friends Cost Us 377GB and Broke ext4 Hardlinks"
url: "https://blog.discourse.org/2026/04/how-jennifer-aniston-and-friends-cost-us-377gb-and-broke-ext4-hardlinks/"
date: "Thu, 09 Apr 2026 22:30:53 GMT"
author: "Jake Goldsborough"
feed_url: "https://blog.discourse.org/rss/"
---
<img alt="How Jennifer Aniston and Friends Cost Us 377GB and Broke ext4 Hardlinks" src="https://storage.ghost.io/c/7d/70/7d70d59c-7408-4583-b44d-98a43cdfa8fd/content/images/2026/04/Discourse-Header-Images--1-.png" /><p>It started with backup issues. Sites with hundreds of gigabytes of uploads were running out of disk space during backup generation. One site had 600+ GB of uploads and the backup process kept dying.</p><p>While looking into reliable large backups, we discovered something wild in one of those sites: the actual unique content was a fraction of the reported size. They were storing the same files over and over again, each with a different filename. The duplication was absurd.</p><p>So we shipped an optimization. Detect duplicate files by their content hash, use hardlinks instead of downloading each copy. I wrote some new tests, they all passed, it got approved and merged. But unfortunately, a fix like this is kind of hard to actually fully test.</p><p>Then someone ran it on a real production backup and hit a filesystem limit I didn&apos;t know existed. The culprit? A single reaction GIF, duplicated 246,173 times...</p><h1 id="the-problem">The Problem</h1><p>Discourse has a feature called secure uploads. When a file moves between security contexts (say, from a private message to a public post), the system creates a new copy with a randomized SHA1. The original content is identical, but Discourse treats it as a new file.</p><p>This happens constantly with reaction GIFs and popular images. Users share them across posts, embed them in PMs, repost in different categories. Each context creates another copy.</p><p>This is mostly fine for normal operation. But for backups, it&apos;s a disaster.</p><p>One customer had 432 GB of uploads. Unique content? 26 GB. The rest was duplicates. A 16x inflation factor, all going into the backup archive.</p><h1 id="the-fix">The Fix</h1><p>The fix seemed straightforward. Discourse tracks the original content hash in&#xa0;<code>original_sha1</code>. During backup:</p><ol><li>Group uploads by&#xa0;<code>original_sha1</code></li><li>Download the first file in each group</li><li>Create hardlinks for the duplicates</li></ol><p>Hardlinks point multiple filenames to the same data on disk. GNU tar preserves them, so the archive stores the data once. Download 26 GB, archive 26 GB, everyone wins.</p><pre><code>def process_upload_group(upload_group)
  primary = upload_group.first
  primary_filename = upload_path_in_archive(primary)

  return if !download_upload_to_file(primary, primary_filename)

  # Create hardlinks for all duplicates in this group
  upload_group.drop(1).each do |duplicate|
    duplicate_filename = upload_path_in_archive(duplicate)
    hardlink_or_download(primary_filename, duplicate, duplicate_filename)
  end
end</code></pre><p>The&#xa0;<code>hardlink_or_download</code>&#xa0;method falls back to downloading if the hardlink fails:</p><pre><code>def hardlink_or_download(source_filename, upload_data, target_filename)
  FileUtils.mkdir_p(File.dirname(target_filename))
  FileUtils.ln(source_filename, target_filename)  # Create hardlink
  increment_and_log_progress(:hardlinked)
rescue StandardError =&gt; ex
  # Fallback: download if hardlink fails
  log &quot;Failed to create hardlink, downloading instead&quot;, ex
  download_upload_to_file(upload_data, target_filename)
end</code></pre><p>We shipped it and got positive feedback...</p><h1 id="the-limit">The Limit</h1><p>A colleague then used the new version to run a backup on a large site. The logs looked great:</p><pre><code>53000 files processed (25 downloaded, 52975 hardlinked). Still processing...
54000 files processed (25 downloaded, 53975 hardlinked). Still processing...
...
64000 files processed (25 downloaded, 63975 hardlinked). Still processing...
65000 files processed (25 downloaded, 64975 hardlinked). Still processing...
Failed to create hardlink for upload ID 482897, downloading instead
Failed to create hardlink for upload ID 457497, downloading instead
Failed to create hardlink for upload ID 867574, downloading instead</code></pre><p>At 65,000 hardlinks, it started failing. Turns out ext4 has a limit: roughly 65,000 hardlinks per inode. One file can only have 65,000 names pointing to it.</p><p>The fallback worked and it didn&apos;t fail completely. The backup finished. But instead of one download for all 246,173 duplicates, we got one download plus ~181,000 fallback downloads after hitting the limit.</p><p>Still better than 246,173 downloads. But not the win I expected.</p><h1 id="the-gif">The Gif</h1><p>So what file had 246,173 copies?</p><pre><code>Upload.where(original_sha1: &apos;27b7a62e34...&apos;).count
=&gt; 246173

Upload.where(original_sha1: &apos;27b7a62e34...&apos;).first.filesize
=&gt; 1643869</code></pre><p>1.6 MB. Duplicated a quarter million times. That&apos;s 377 GB of backup bloat from a single image.</p><p>And then I saw what it was...</p><figure class="kg-card kg-image-card"><img alt="How Jennifer Aniston and Friends Cost Us 377GB and Broke ext4 Hardlinks" class="kg-image" height="400" src="https://storage.ghost.io/c/7d/70/7d70d59c-7408-4583-b44d-98a43cdfa8fd/content/images/2026/04/jennifer-aniston.gif" width="480" /></figure><p>A reaction GIF. Used constantly in posts, PMs, everywhere. Each use in a different security context creates a new copy. 246,173 copies of Rachel from Friends doing a happy dance.</p><p>One GIF broke the hardlink limit.</p><h1 id="the-math">The Math</h1><p>Without deduplication: 246,173 downloads, 377 GB transferred.</p><p>With deduplication (hitting limit): ~4 downloads, ~6.4 MB transferred.</p><p>The filesystem limit turned my &quot;download once&quot; into &quot;download four times.&quot; I can live with that.</p><h1 id="a-fix-for-the-fix">A Fix for the Fix</h1><p>My first instinct was to track hardlink counts and proactively rotate before hitting the limit. But a colleague pointed out the flaw: we have no idea what filesystem is being used. ext4 has one limit, XFS another, ZFS another. Picking a magic number is fragile.</p><p>Better approach: let the filesystem tell us when we&apos;ve hit the limit.</p><pre><code>def create_hardlink(source_filename, upload_data, target_filename)
  FileUtils.mkdir_p(File.dirname(target_filename))
  FileUtils.ln(source_filename, target_filename)
  source_filename
rescue Errno::EMLINK
  # Filesystem hardlink limit reached - copy and use as new primary
  FileUtils.cp(source_filename, target_filename)
  target_filename
rescue StandardError =&gt; ex
  download_upload_to_file(upload_data, target_filename)
  source_filename
end</code></pre><p>When&#xa0;<code>Errno::EMLINK</code>&#xa0;fires, we already have the file locally. No need to re-download. Just copy it and use the copy as the new primary for subsequent hardlinks. Works on any filesystem, no configuration needed.</p><h1 id="what-i-learned">What I Learned...</h1><p>Filesystems have opinions. ext4&apos;s hardlink limit exists to prevent certain classes of bugs and attacks. It&apos;s not arbitrary.</p><p>The fallback saved the feature. Without graceful degradation, that backup would have failed entirely. Instead it completed, just slower than optimal.</p><p>Production always finds the edge cases. 246,000 copies of one file is absurd. But absurd things happen at scale.</p><p>A few concrete takeaways:</p><ul><li>Test for failure modes, not just success paths. The hardlink fallback was built-in from the start, but I never expected to actually need it.</li><li>Optimizations that reduce work by 16x still need to handle edge cases. A 99.998% improvement with graceful degradation beats a 100% improvement that crashes.</li><li>Track filesystem-level constraints early. Hardlink limits, inode counts, path lengths - these are real operational boundaries, not theoretical concerns.</li></ul><p>And now I know Jennifer Aniston can stress-test infrastructure.</p><h1 id="links">Links</h1><ul><li><a href="https://github.com/discourse/discourse/pull/37261?ref=blog.discourse.org">Discourse PR #37261</a>&#xa0;- The backup deduplication fix</li><li><a href="https://github.com/discourse/discourse/pull/37293?ref=blog.discourse.org">Discourse PR #37293</a>&#xa0;- The hardlink limit fix</li></ul>
