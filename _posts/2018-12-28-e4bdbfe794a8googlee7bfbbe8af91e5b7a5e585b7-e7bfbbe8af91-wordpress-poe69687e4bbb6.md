---
ID: 1780
post_title: >
  使用Google翻译工具 翻译
  wordpress .PO文件
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/28/%e4%bd%bf%e7%94%a8google%e7%bf%bb%e8%af%91%e5%b7%a5%e5%85%b7-%e7%bf%bb%e8%af%91-wordpress-po%e6%96%87%e4%bb%b6/'
published: true
post_date: 2018-12-28 14:07:57
---
<a href="https://translate.google.com/toolkit/list?vid=&amp;hl=en#translations/active">https://translate.google.com/toolkit/list?vid=&amp;hl=en#translations/active</a>

&nbsp;

使用Google翻译工具 翻译.PO文件
所以你需要将一些插件或主题翻译成你的母语，并已经交给那些讨厌的.pot / .po文件？

你很幸运，这个插件至少支持语言文件，但不幸的是你现在有一大堆小行可以翻译。你可以下载一个像poedit这样的工具，并通过翻译来破解，但我觉得必须有一种方法来填充文件，至少包含一些必要的翻译，节省了一些繁琐的工作。幸运的是谷歌提供（虽然奇怪的是我没有通过我最初的谷歌搜索找到链接！）
<blockquote>Google翻译有一个PO文件工作台：

https://translate.google.com/toolkit/list?vid=&amp;hl=en#translations/active</blockquote>
&nbsp;

显然，您需要一个Google帐户。此工具为您提供了一个简单的并排工作流程，您可以在其中查看原文并编辑翻译，但更重要的是：Google会尽最大努力为您翻译文件！

我们以wpjobboard po文件为例，上传一下：

您选择橙色上传选项：

概观

然后继续选择原始语言的.PO文件。请务必将结果保存为filename-nl_NL（如果是荷兰语版本）。如果您不确定正确的语言，请检查您的wp-config.php文件以获取正确的名称。

上载

选择“上传以进行翻译”后，系统会上传文件，Google会尝试将内容翻译为您在上一个屏幕中选择的语言。这是您逐个翻译的地方，以确保它们是正确的。

基本上蓝色条目是谷歌认为正确的条目。红色条目已翻译，但有一些问题。大多数自动翻译工具尝试反向翻译其翻译以进行验证。 Red并不一定意味着翻译是错误的，只是系统无法自动验证。

翻译

当你最终完成后，你可以完成翻译。文件 - &gt;下载...为您提供结果文件。此文件需要上传到插件指定的位置。

&nbsp;

http://bvs.io/translating-po-files-using-google-translate/

<header class="entry-header">
<h1 class="entry-title">Translating .PO files using Google translate</h1>
</header>
<div class="entry-content">

So you need to translate some plugin or theme to your native language and have been handed one of those pesky .pot / .po files?

You’re lucky the plugin at least supports language files, but unlucky that you now have a mountain of small lines to translate. You could download a tool like <a href="http://www.poedit.net/">poedit</a> and slog through the translation, but I felt there has to be a way to populate the file with at least some of the required translation, saving some of the grunt work. Fortunately Google delivers (although oddly I did not find the link through my initial google search!)

Google translate has a workbench for PO files: http://translate.google.com/toolkit/list?hl=en#translations/active

Obviously you need a Google account. This tool gives you a simple side by side workflow where you can see the original and edit the translation, but more importantly: Google will make a best effort attempt to translate your file for you!

Let’s take a wpjobboard po file as an example and upload this:

You select the orange upload option:

<img class="alignnone size-full wp-image-1788" src="http://hss5.com/wp-content/uploads/2018/12/overview-2.png" alt="overview" width="901" height="392" />

Then you proceed to select the .PO file with the original language. Be sure to save the result as filename-nl_NL (in case of the Dutch version). If you are unsure about the correct language, check your wp-config.php file for the correct name.

<img class="alignnone size-full wp-image-1789" src="http://hss5.com/wp-content/uploads/2018/12/upload-2.png" alt="upload" width="998" height="505" />

Once you select “upload for translation” the file will be uploaded and Google will attempt to translate the contents to the language you selected in the previous screen. This is where you go through the translations one by one to ensure they are correct.

Basically the blue entries are the ones Google believes are correct. The red entries were translated, but had some issues. Most automatic translation tools attempt to reverse translate their translations to verify. Red does not have to mean the translation is wrong, just that the system is unable to validate automatically.

<img class="alignnone size-full wp-image-1790" src="http://hss5.com/wp-content/uploads/2018/12/translating-1024x439-2.png" alt="translating" width="1024" height="439" />

When you are finally done you can complete the translation. The file -&gt; Download… gives you the resulting file. This file will need to be uploaded to the location specified by your plugin.

http://bvs.io/translating-po-files-using-google-translate/

&nbsp;

</div>