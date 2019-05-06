---
ID: 2697
post_title: >
  javascript –
  如何告诉Google翻译不翻译网站的某个部分？
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/05/06/javascript-%e5%a6%82%e4%bd%95%e5%91%8a%e8%af%89google%e7%bf%bb%e8%af%91%e4%b8%8d%e7%bf%bb%e8%af%91%e7%bd%91%e7%ab%99%e7%9a%84%e6%9f%90%e4%b8%aa%e9%83%a8%e5%88%86%ef%bc%9f/'
published: true
post_date: 2019-05-06 11:53:07
---
<header class="article-header"></header><article class="article-2018">
<div class="question">Google翻译有一个<a href="http://translate.google.com/translate_tools">developer tool</a>，将启用网站上的google翻译。是否有办法让Google翻译不翻译网站的某个部分？也许在HTML元素上有类名？我试过<a href="http://dev.w3.org/html5/spec/global-attributes.html#the-translate-attribute">HTML5 translate=no</a>属性。它没有效果。

这是一个特别的问题，因为Google会误译网站的名称。

</div>
<div class="answers_count">最佳答案</div>
<div class="answers">根据<a href="https://cloud.google.com/translate/faq#technical_questions">Google instructions</a>，设置class =“notranslate”会阻止Google翻译。这似乎工作，虽然使用它内联(例如，一个单词)可能意味着一些混乱，所以你需要检查发生了什么。例如，
<pre class="prettyprint"><code><span class="typ">Welcome</span><span class="pln"> to the </span><span class="pun">&lt;</span><span class="pln">span </span><span class="kwd">class</span><span class="pun">=</span><span class="str">"notranslate"</span><span class="pun">&gt;</span><span class="typ">Cool</span><span class="pun">&lt;/</span><span class="pln">span</span><span class="pun">&gt;</span><span class="pln"> company website</span><span class="pun">!</span></code></pre>
翻译成西班牙语“Bienvenido a la Coolweb de lacompañía！”，这不是那么酷，虽然它表明“酷”已被视为一个正确的名字;没有标记，文本将翻译为“Bienvenido a la fresca web de la empresa！”。

将文本重新配置为
<pre class="prettyprint"><code><span class="typ">Welcome</span><span class="pln"> to the website of </span><span class="pun">&lt;</span><span class="pln">span </span><span class="kwd">class</span><span class="pun">=</span><span class="str">"notranslate"</span><span class="pun">&gt;</span><span class="typ">Cool</span><span class="pun">&lt;/</span><span class="pln">span</span><span class="pun">&gt;!</span></code></pre>
会导致“Bienvenido a lapáginaweb de cool！”，看起来更好，除了“网站”被错误翻译。

对于不同的目标语言，不同的问题可能会出现。一般来说，句子的语法结构越简单，它越经常被翻译得相当好。

底线是：您可以尝试使用class = notranslate防止翻译，但Google译码器的问题可能会导致混乱。

</div>
<ins class="adsbygoogle" data-ad-client="ca-pub-4364680260425390" data-ad-slot="7215561100" data-adsbygoogle-status="done"><ins id="aswift_1_expand"><ins id="aswift_1_anchor"></ins></ins></ins></article>