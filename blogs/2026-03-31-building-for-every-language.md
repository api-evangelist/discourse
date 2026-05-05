---
title: "Building for Every Language"
url: "https://blog.discourse.org/2026/03/building-for-every-language/"
date: "Tue, 31 Mar 2026 21:45:32 GMT"
author: "Natalie Tay"
feed_url: "https://blog.discourse.org/rss/"
---
<img alt="Building for Every Language" src="https://storage.ghost.io/c/7d/70/7d70d59c-7408-4583-b44d-98a43cdfa8fd/content/images/2026/03/Discourse-Header-Images.png" /><p><em>Hello again &#x1f44b;</em></p><p>If you&apos;re a history buff, this is the blog post for you. It covers how multilingual support at Discourse evolved over 13 years, with some honest behind the scenes.</p><p>In <a href="https://blog.discourse.org/2026/03/every-language-is-welcome" rel="noreferrer">the last post</a>, I talked about why we believe every community should be able to welcome people in any language. This one is about what we&apos;ve done about it, and how long it took us to get serious.</p><p></p>
<!--kg-card-begin: html-->
<!--
  Discourse Without Borders — Post 2 timeline
  Paste this HTML block into Ghost's HTML card.
-->


<div class="dwb-tl">
  <div class="dwb-tl-track"></div>
  <div class="dwb-tl-cols">
    <div class="dwb-tl-col">
      <div class="dwb-tl-dot" style="background: #9fc8df;"></div>
      <div class="dwb-tl-year dwb-tl-grey">2012</div>
      <div class="dwb-tl-desc dwb-tl-grey">Founding goals<br />(no i18n)</div>
    </div>
    <div class="dwb-tl-col">
      <div class="dwb-tl-dot" style="background: #5cb4e5;"></div>
      <div class="dwb-tl-year dwb-tl-grey">2014</div>
      <div class="dwb-tl-desc dwb-tl-grey">Translation platform<br />20 locales</div>
    </div>
    <div class="dwb-tl-col">
      <div class="dwb-tl-dot" style="background: #2ab0e8;"></div>
      <div class="dwb-tl-year dwb-tl-grey">2015</div>
      <div class="dwb-tl-desc dwb-tl-grey">Translator<br />plugin created</div>
    </div>
    <div class="dwb-tl-col">
      <div class="dwb-tl-dot" style="background: #00aeef;"></div>
      <div class="dwb-tl-year dwb-tl-grey">2020</div>
      <div class="dwb-tl-desc dwb-tl-grey">Crowdin<br />74 languages</div>
    </div>
    <div class="dwb-tl-col">
      <div class="dwb-tl-dot" style="background: #00aeef;"></div>
      <div class="dwb-tl-year dwb-tl-blue">2023</div>
      <div class="dwb-tl-desc dwb-tl-blue-d">LLMs will<br />replace APIs</div>
    </div>
    <div class="dwb-tl-col">
      <div class="dwb-tl-dot" style="background: #00aeef;"></div>
      <div class="dwb-tl-year dwb-tl-blue">2024</div>
      <div class="dwb-tl-desc dwb-tl-blue-d">Reddit 4x DAU<br />from translation</div>
    </div>
    <div class="dwb-tl-col">
      <div class="dwb-tl-dot-end" style="background: #00aeef;"></div>
      <div class="dwb-tl-year dwb-tl-blue">2025</div>
      <div class="dwb-tl-desc dwb-tl-blue-d">Shipped to all<br />cost: pennies</div>
    </div>
  </div>
</div>

<!--kg-card-end: html-->
<h2 id="act-1-give-internationalization-i18n-proper-love-2012%E2%80%932016">Act 1: &quot;Give internationalization (i18n) proper love&quot; (2012&#x2013;2016)</h2><p>In June 2012, three people sat down and wrote the founding goals for Discourse. Eleven bullet points covered open source, easy deployment, rethinking forum UX, mobile-first, real-time updates, touch support, data portability. i18n wasn&apos;t on the list and the internet they were building for spoke English.</p><p>We didn&apos;t prioritize multilingual... but our community did!</p><p>Within months of those goals being published, people started asking. One of the earliest replies to the mission goals topic was from a community member who&apos;d clearly been holding his breath:</p><blockquote>&quot;Give internationalization proper love and the product will spread across the world!&quot;</blockquote><p>He pointed to WordPress&apos;s localization workflow as the gold standard. Jeff&apos;s response was characteristically casual: <em>&quot;There is a string file, and we try very hard to put all text through it &#x2014; just edit it for your language!&quot;</em></p><p><em>Narrator: it was not that simple.</em></p><p>The community pushed back. How would translators know which strings changed between versions? How would quality control work? Meanwhile, it was discovered that Welsh has <em>six forms of pluralization</em> (and Arabic too, as I later discovered) and the complexity of real i18n started to sink in.</p><p>By 2014, roughly 20 community-contributed localizations existed, and we adopted a translation platform. Neil, one of the first engineers in Discourse, saw 19 pending translator requests on the first day. One contributor, after a year of manually comparing git commits for the Dutch translation, was just relieved: <em>&quot;I&apos;m happy to use the new tool.&quot;</em> Another joked about submitting English (UK) translations. How the turn tables...</p><p>In February 2015, a community member proposed inline post translation (translating <em>content</em>, not just the UI) referencing Facebook&apos;s approach and asking for a &quot;See translation&quot; button on every post. Six months later, the discourse-translator plugin was born with support for Microsoft, Google, and Yandex, translating each post once per locale to save money.</p><p>Then came the goat farmers. &#x1f410; &#x1f410; &#x1f469;&#x1f3fb;&#x200d;&#x1f33e;</p><p>In 2016, a community member running a bilingual goat farming forum (Ukrainian and English) posted a detailed proposal for collapsible language sections. His community had discovered that English-speaking goat farmers had great interest in how Ukrainian goat farmers make cheeses, and vice versa for professional breeding. They were running manual bilingual Q&amp;A sessions because Google Translate was <em>&quot;a disaster&quot;</em> for their professional terminology. He wanted collapsible sections, a language switcher, translatable category names.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="Building for Every Language" class="kg-image" height="1314" src="https://storage.ghost.io/c/7d/70/7d70d59c-7408-4583-b44d-98a43cdfa8fd/content/images/2026/03/ukrainian-goat.png" width="2000" /><figcaption><span style="white-space: pre-wrap;">Topic on meta.discourse.org about a bilingual forum for goat farmers</span></figcaption></figure><p>Our team was upfront about it, dealing with language headers for anonymous users makes caching an absolute nightmare. <em>&quot;We weren&apos;t able to even brainstorm a way of doing it at all.&quot;</em></p><p>So that was the state of things. Discourse did UI localization pretty well. The community wanted content translation. And for years, the answer was &quot;here&apos;s a plugin&quot; that worked but didn&apos;t go very far.</p><h2 id="act-2-the-plumbing-2017%E2%80%932020">Act 2: The Plumbing (2017&#x2013;2020)</h2><p>In 2017, we published the first official guide for structuring multilingual communities on meta. The recommended model included language-specific categories, language tags, running separate instances, and revealed how <em>manual</em> the approach still was. Then came the Crowdin migration.</p><p>By early 2020, our localization infrastructure was held together with duct tape and workarounds. We were using a forked copy of i18n.js so old nobody remembered who&apos;d forked it. MessageFormat.js was v0.1.5, <em>seven years out of date</em>. Our internal tooling was workarounds stacked on workarounds for translation platform bugs.</p><p><em>We&apos;d originally chosen our platform because we thought it was free for open source. It... turned out not to be.</em></p><p>Crowdin offered us a deal and we took it. We migrated 74 languages, built custom automation with Jenkins, and finally had infrastructure that didn&apos;t fight us. The community reaction on meta was mostly excited, though one contributor raised a very real concern about quality control, recounting how a Spanish &quot;translation vigilante&quot; had once gone rogue <em>&quot;translating any sentence left and right without consulting the other translators.&quot;</em> Fair.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="Building for Every Language" class="kg-image" height="1353" src="https://storage.ghost.io/c/7d/70/7d70d59c-7408-4583-b44d-98a43cdfa8fd/content/images/2026/03/crowdin-languages.png" width="2000" /><figcaption><span style="white-space: pre-wrap;">Languages (with high % completion) that we support via Crowdin</span></figcaption></figure><p>What mattered then in 2020 was that a very low percentage of customers used a non-English locale. It was hard to justify doing more.</p><h2 id="act-3-reddit-called-late-2024">Act 3: Reddit Called (Late 2024)</h2><p>For a few years, multilingual was in a holding pattern. The infrastructure was better, but it was still hard to justify the work. Sam saw a mixed Japanese/English gaming topic and thought <em>&quot;this is SUCH a missed opportunity&quot;</em>, but it stayed in the &quot;someday&quot; bucket.</p><p>Then in October 2023, Sam made a prediction: <em>&quot;all translation APIs will die and be replaced with LLMs.&quot;</em> His vision was a &quot;magical babel fish mode where you select your language and we just auto translate everything.&quot; Looking back at this point, I&#x2019;m reminded once again that Sam is (almost &#x1f605;) always right.</p><p>In October 2024, Gerhard searched for &quot;discourse hosting&quot; from Austria. When Google switched to German results, Discourse disappeared completely. Sam&apos;s reaction: <em>&quot;There are 66 million people in France; Australia has roughly a third of that population. In countries like Japan and Korea, you don&apos;t even exist unless you have a localized site.&quot;</em></p><p>A month later, Reddit&apos;s Q3 2024 earnings came out and machine translation had driven <strong>4x incremental daily active users</strong> with up to 40% of daily traffic coming from Google. Falco shared that his wife had never used Reddit because of the language barrier, but she&apos;d already installed the app because everything was showing in Portuguese for her now. That one hit home.</p><p>And the cost math was wild.</p>
<!--kg-card-begin: html-->
<!--
  Discourse Without Borders — Post 2 cost comparison table
  Paste this HTML block into Ghost's HTML card.
  Self-contained, no external deps.
-->
<table>
  <thead>
    <tr style="border-bottom: 2px solid #d1d5db;">
      <th style="padding: 0.6em 0.8em; text-align: left;"></th>
      <th style="padding: 0.6em 0.8em; text-align: left;">Google Cloud Translation</th>
      <th style="padding: 0.6em 0.8em; text-align: left;">Gemini 2.5 Flash-Lite</th>
    </tr>
  </thead>
  <tbody>
    <tr style="border-bottom: 1px solid #e5e7eb;">
      <td style="padding: 0.6em 0.8em; font-weight: 600;">Price</td>
      <td style="padding: 0.6em 0.8em;">$20 per million characters</td>
      <td style="padding: 0.6em 0.8em;">~$0.10 input + $0.40 output per M tokens</td>
    </tr>
    <tr style="border-bottom: 1px solid #e5e7eb;">
      <td style="padding: 0.6em 0.8em; font-weight: 600;">1,500 posts into 4 languages</td>
      <td style="padding: 0.6em 0.8em;">~$180</td>
      <td style="padding: 0.6em 0.8em;">~$1.26</td>
    </tr>
    <tr>
      <td style="padding: 0.6em 0.8em; font-weight: 600;">Per post</td>
      <td style="padding: 0.6em 0.8em;">~3 cents</td>
      <td style="padding: 0.6em 0.8em;">~0.02 cents</td>
    </tr>
  </tbody>
</table>

<!--kg-card-end: html-->
<p>The engineer who&apos;d originally *built* the translator plugin years ago, TGX (tgxworld), tested AI translation to Chinese and declared it &#x1f90c;.</p><h3 id="act-4-from-poc-to-production-2025%E2%80%932026">Act 4: From POC to Production (2025&#x2013;2026)</h3><p>In January 2025, we studied Reddit&apos;s translation implementation and built a working proof of concept with a <code>?lang=ja</code> parameter. Work &apos;started&apos; on February 10th and by February 14th we were live on meta, with automatic translations to English enabled and backfill running at 1000 posts per hour.</p><p>Then came the three homes problem. The translation feature moved through *three* codebases in four months: the translator plugin, Discourse core, and discourse-ai. On June 4th, I announced what was simultaneously a triumph and a comedy: <em>&quot;All translations are now in discourse-ai; discourse-translator is now 100% irrelevant&quot;</em> &#x1f62d;&#x1f602;</p><p>There were some fun debates along the way. Should we use flags for languages? One colleague asked <em>&quot;Which country is Arabic? Spanish?&quot;</em> and that ended it, we went with language codes. A Canadian team member pointed out that in Canada, showing a US flag for English and a France flag for French would be &quot;very, very unusual.&quot;</p><p>Should we brand it as AI translation? I shared that some customers have explicit anti-AI policies and it&apos;d be friction (things have changed since then). Sam preferred to put AI front and centre, while another colleague offered the diplomatic <em>&quot;All translation services are beautiful,&quot;</em> then someone posted the Chill Guy meme, and we went with AI front and centre.</p><p>Translation is genuinely hard and there are some great moments from our community to prove it. A translation agency we hired once translated &quot;Summary&quot; as &quot;&#x421;&#x43f;&#x43e;&#x439;&#x43b;&#x435;&#x440;&quot; (Spoiler) in Russian, and turned email placeholders like <code>from@example.com</code> into Cyrillic: <code>&#x43e;&#x442;@gsp-&#x43f;&#x440;&#x438;&#x43c;&#x435;&#x440;.&#x43a;&#x43e;&#x43c;</code>. Our Portuguese translators spent an entire meta topic debating how to translate the word &quot;post&quot; because Portuguese doesn&apos;t really have one. &quot;Publica&#xe7;&#xe3;o&quot; (publication) is too formal, &quot;poste&quot; (transliteration) is controversial, and &quot;mensagem&quot; (message) conflicts with personal messages. It got serious enough that a linguist appointed by the Portuguese government weighed in. And when those same translators had to deal with &quot;dropdown menu,&quot; the official translation was &quot;lista pendente&quot; (pending list). One translator said <em>&quot;it sounds so wrong&quot;</em> and the other replied <em>&quot;I know... I would actually choose &apos;dropdown&apos; without translating at any time.&quot;</em></p><p>The funniest one to date for me personally, was when Moin, a German user on meta, pointed out to me that our &#x201c;composer&#x201d; was translated to &#x201d;the person who writes music&#x201d;. &#x1f923;</p><p>Multilingual search is still on the roadmap and we think multilingual embeddings are the right answer since our existing embeddings on meta are already multilingual.</p><p>By October 2025, Falco posted what might be my favourite summary of the whole journey: <em>&quot;We now occupy double the space on Google searches, much like Reddit does. This is available to all customers, and cost pennies.&quot;</em></p><p>Then in December 2025, Google started forcing localized search results based on geography. If you search in English from Brazil, Google prioritizes Portuguese results on the first page. Translation went from &quot;nice to have&quot; to essential for search visibility.</p><p>The feature documentation went live on meta in July 2025 covering manual and automatic translations, category and topic localization, language switcher, crawler support, and hreflang tags, all available on every plan. &#x1f389;</p><p>And then the cherry on top for me was a Chinese-speaking user started making suggestions on meta entirely in Chinese, not asking about translation, just using meta naturally in their language because the feature made it possible.</p><figure class="kg-card kg-image-card kg-card-hascaption"><img alt="Building for Every Language" class="kg-image" height="1391" src="https://storage.ghost.io/c/7d/70/7d70d59c-7408-4583-b44d-98a43cdfa8fd/content/images/2026/03/post-in-chinese.png" width="2000" /><figcaption><span style="white-space: pre-wrap;">A user report on Meta (our forum), originally written in Chinese and translated to English</span></figcaption></figure><h2 id="our-multilingual-team-and-community">Our multilingual team and community</h2>
<!--kg-card-begin: html-->

<div class="map-container">
  <div class="map-wrap">
    <svg preserveAspectRatio="xMidYMid meet" viewBox="0 0 360 180" xmlns="http://www.w3.org/2000/svg">
      <path class="land" d="M120,170L114,171L120,170Z M21,169L16,169L21,169Z M135,168L137,170L126,171L135,168Z M59,164L61,163L57,164L59,164Z M54,163L56,164L54,163Z M81,162L84,163L78,162L81,162Z M112,161L109,163L105,162L109,159L112,161Z M121,154L114,158L118,160L119,164L103,167L106,168L102,169L125,173L150,171L150,169L144,168L160,166L170,161L220,160L237,156L249,158L248,162L252,162L253,160L268,156L310,157L315,155L317,157L351,161L349,164L344,165L343,167L347,169L340,171L358,174L360,175L360,180L0,180L1,174L37,175L26,174L27,172L23,171L34,170L22,167L45,164L106,164L112,163L112,157L123,154L121,154Z M112,144L115,145L112,146L105,143L112,144Z M121,141L119,142L121,141Z M325,131L328,131L328,133L326,134L325,131Z M353,131L353,134L348,137L347,135L353,131Z M355,126L359,128L355,132L353,124L355,126Z M347,112L344,110L347,112Z M230,104L228,114L225,116L223,113L224,106L228,105L229,102L230,104Z M324,104L334,118L329,128L323,129L317,125L318,123L316,125L311,121L299,125L295,124L296,122L293,117L295,111L301,110L306,104L310,105L312,101L317,102L315,105L320,108L322,101L324,104Z M301,100L299,100L301,100Z M342,100L341,98L342,100Z M304,100L307,98L304,100Z M303,98L300,99L303,98Z M340,98L338,97L340,98Z M338,97L336,97L338,97Z M289,97L296,98L285,97L289,97Z M336,97L335,95L336,97Z M332,95L328,96L332,95Z M310,93L308,93L310,93Z M333,94L331,93L333,94Z M314,91L314,93L320,92L325,94L331,101L326,98L318,98L318,95L313,94L314,92L311,91L314,91Z M305,89L300,90L303,91L303,96L301,93L300,96L300,89L305,89Z M309,89L308,91L307,89L309,89Z M286,96L275,85L282,88L286,92L286,96Z M298,88L296,94L290,93L289,89L297,83L299,85L298,88Z M306,82L306,84L302,83L305,80L306,82Z M261,84L260,80L261,84Z M304,80L302,80L304,80Z M299,81L297,82L300,79L299,81Z M302,78L302,80L302,78Z M306,78L305,80L304,77L306,78Z M302,77L300,77L302,77Z M301,71L304,77L300,75L301,71Z M107,70L112,71L106,72L107,70Z M100,67L106,70L98,67L95,68L100,67Z M301,67L302,65L301,67Z M315,56L312,57L315,56Z M215,54L212,55L215,54Z M204,54L206,55L204,54Z M196,52L192,52L196,52Z M189,49L190,51L188,51L189,49Z M321,53L316,57L315,55L311,56L311,59L309,57L319,52L320,49L322,50L321,53Z M324,46L326,47L320,48L322,44L324,46Z M116,43L118,44L116,43Z M118,41L115,40L118,41Z M56,41L52,39L56,41Z M124,39L127,43L121,42L124,39Z M47,36L49,38L47,36Z M324,39L324,44L322,44L322,36L324,39Z M173,38L170,38L170,36L174,35L173,38Z M193,34L191,35L193,34Z M27,33L25,33L27,33Z M177,31L177,34L182,38L174,40L177,39L175,38L177,36L174,35L174,32L177,31Z M98,27L96,28L98,27Z M8,26L11,27L8,26Z M95,24L100,26L94,27L95,24Z M165,24L165,26L161,27L156,25L165,24Z M5,23L10,24L7,26L0,25L0,21L5,23Z M84,21L80,21L84,21Z M89,21L93,23L94,20L97,20L99,23L94,23L85,31L98,35L100,39L100,35L103,34L101,31L103,27L110,29L112,32L115,30L118,34L123,35L124,38L113,40L109,43L116,41L116,44L120,44L110,46L104,53L103,51L104,55L99,58L100,65L96,60L86,60L82,65L83,70L86,72L93,68L91,74L96,74L98,81L104,81L107,78L109,78L108,81L110,78L118,79L122,84L128,85L129,90L145,95L139,112L132,115L127,124L122,124L123,127L121,129L115,131L117,133L112,136L114,138L109,144L105,142L104,139L106,137L104,137L107,134L106,127L109,122L110,108L104,105L99,97L99,91L103,83L101,81L100,83L96,82L93,77L87,74L76,72L74,67L65,58L71,67L68,66L63,57L56,51L55,42L58,42L46,32L33,29L28,31L30,29L28,29L26,32L15,36L23,31L15,30L14,28L19,25L15,26L12,24L18,24L13,22L21,19L59,20L71,23L72,21L82,21L84,23L86,18L89,21Z M66,17L75,17L79,20L64,21L68,20L61,18L66,17Z M76,17L73,16L76,17Z M104,17L99,16L104,17Z M93,17L108,18L118,24L112,24L115,26L111,26L114,28L102,26L106,25L107,22L91,20L90,18L93,17Z M80,16L83,16L83,18L78,17L80,16Z M324,17L320,17L324,17Z M87,17L84,17L89,16L87,17Z M60,19L54,18L55,16L64,17L60,19Z M331,15L326,15L331,15Z M86,15L83,15L86,15Z M325,14L317,15L325,14Z M82,13L82,15L77,14L82,13Z M72,14L74,15L62,15L72,14Z M238,19L231,18L238,14L249,13L235,18L238,19Z M85,13L100,15L92,16L83,13L85,13Z M64,12L57,14L64,12Z M287,13L294,14L289,16L307,16L310,19L320,19L320,17L339,19L341,21L356,20L360,21L360,25L357,25L359,28L344,30L343,34L337,39L336,33L344,29L344,27L335,31L322,31L315,35L321,37L320,42L307,51L309,53L307,56L305,50L301,51L301,49L298,51L303,53L299,55L302,58L302,62L296,67L286,70L289,75L287,80L285,81L280,77L279,81L283,84L284,89L278,82L278,74L274,74L274,70L271,67L260,74L258,82L253,69L250,69L247,65L235,64L228,60L232,66L236,64L240,68L237,72L223,77L223,73L215,60L214,62L212,60L223,79L231,78L231,81L219,95L221,105L215,110L216,114L213,115L208,123L198,124L192,108L194,101L189,92L189,85L172,86L163,78L163,68L174,54L191,53L191,57L199,60L201,57L214,59L216,53L210,54L206,51L212,48L222,48L217,45L219,43L216,43L217,45L214,46L211,43L208,47L209,49L204,49L202,54L199,50L200,48L193,44L198,49L196,52L195,49L187,46L183,47L179,53L171,53L172,46L179,46L175,41L189,36L188,33L191,32L191,36L200,36L201,33L204,33L203,31L209,30L202,30L201,27L205,25L202,24L198,27L197,29L199,30L196,34L193,35L191,31L187,32L185,28L191,26L196,21L205,19L220,22L220,24L213,23L217,26L224,24L223,21L226,23L240,22L240,20L249,22L247,19L253,17L254,22L252,24L255,22L253,19L255,17L256,19L270,14L287,13Z M229,49L230,53L234,53L233,49L235,49L230,46L233,43L227,45L229,49Z M86,12L84,12L86,12Z M70,12L66,12L70,12Z M205,12L201,12L205,12Z M70,11L67,12L70,11Z M84,12L81,11L84,12Z M80,12L75,11L80,12Z M285,12L279,12L285,12Z M198,10L202,11L194,13L190,10L198,10Z M205,10L207,10L197,10L205,10Z M231,9L225,9L231,9Z M280,11L271,10L278,9L280,11Z M93,10L91,12L83,10L93,10Z M112,7L118,8L112,8L115,8L99,14L91,14L95,12L92,12L95,11L93,10L98,10L88,8L112,7Z M153,6L159,7L148,8L168,9L160,10L162,10L160,11L162,13L158,13L161,16L156,17L158,17L155,18L158,19L156,20L154,19L158,20L139,25L137,30L131,29L126,24L129,20L125,20L129,19L125,19L123,15L107,12L114,11L112,10L117,8L153,6Z">

      <!-- outer rings for larger clusters -->
      <circle class="dot-ring" cx="81.4" cy="50.2" r="7">
      <circle class="dot-ring" cx="104.3" cy="44.6" r="5.2">
      <circle class="dot-ring" cx="313.8" cy="115.3" r="4.6">

      <!-- team dots -->
      <circle class="dot" cx="81.4" cy="50.2" r="4.5">
      <circle class="dot" cx="104.3" cy="44.6" r="3.2">
      <circle class="dot" cx="313.8" cy="115.3" r="2.8">
      <circle class="dot" cx="128.1" cy="104.2" r="2.5">
      <circle class="dot" cx="258.9" cy="69.4" r="2.2">
      <circle class="dot" cx="182.2" cy="43.8" r="2.0">
      <circle class="dot" cx="283.8" cy="88.65" r="2.0">
      <circle class="dot" cx="105.9" cy="85.4" r="1.7">
      <circle class="dot" cx="179.9" cy="38.5" r="1.7">
      <circle class="dot" cx="176.3" cy="49.6" r="1.7">
      <circle class="dot" cx="185.3" cy="37.9" r="1.3">
      <circle class="dot" cx="206.1" cy="45.6" r="1.3">
      <circle class="dot" cx="184.4" cy="39.1" r="1.3">
      <circle class="dot" cx="201.0" cy="38.0" r="1.3">
      <circle class="dot" cx="354.9" cy="130.9" r="1.3">
      <circle class="dot" cx="196.3" cy="42.5" r="1.3">
      <circle class="dot" cx="188.5" cy="42.5" r="1.3">
      <circle class="dot" cx="235.3" cy="64.8" r="1.3">
      <circle class="dot" cx="219.2" cy="68.5" r="1.3">
      <circle class="dot" cx="121.6" cy="124.6" r="1.3">
      <circle class="dot" cx="179.8" cy="84.4" r="1.3">
      <circle class="dot" cx="211.0" cy="107.8" r="1.3">
      <circle class="dot" cx="194.8" cy="49.2" r="1.3">
      <circle class="dot" cx="209.0" cy="49.0" r="1.3">
    </svg>
  </div>
</div>

<!--kg-card-end: html-->
<p>A third of our team live in places where English isn&apos;t even the main language, and half of our top ten customer countries don&apos;t use English as a first language either. When we built the language switcher, the translation prompts, the hreflang tags, there was no guessing needed as we were building it for ourselves and our customers.</p><p>And then there&apos;s our community who&apos;ve kept translations alive for years before any of the AI stuff happened. There are so many people, but to name just two I&apos;ve interacted with: Tomas maintaining Czech translations for years (and topping our Crowdin leaderboard for a while), and Moin catching more translation bugs than most of us have filed in total. That kind of thing isn&apos;t in any spec and we only know it if we live it.</p><p>And Gerhard. &#x1f917; Gerhard is one of our relentless engineers who has single-handedly managed Discourse&apos;s entire translation infrastructure. If you&apos;re using Discourse in any language other than English, he is a big reason why.</p><p>Penar had put it best: <em>&quot;Yesterday, you had to speak English to participate on meta. Today, with automatic translation it&apos;s available to you in French, Portuguese, Spanish. It is brilliant!&quot;</em></p><hr /><p><em>Next up in Discourse Without Borders: That&apos;s how we built it. But what does it actually feel like to use Discourse in another language?</em><br /></p>
