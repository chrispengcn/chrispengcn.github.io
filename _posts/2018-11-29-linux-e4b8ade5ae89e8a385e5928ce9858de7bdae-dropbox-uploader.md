---
ID: 1542
post_title: >
  Linux 中安装和配置 Dropbox
  Uploader
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/29/linux-%e4%b8%ad%e5%ae%89%e8%a3%85%e5%92%8c%e9%85%8d%e7%bd%ae-dropbox-uploader/'
published: true
post_date: 2018-11-29 19:35:15
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">Linux 中安装和配置 Dropbox Uploader</h1>
</div>
<div class="article-info-box">
<div class="operating"></div>
</div>
</div>
</div>
<article class="baidu_pl">
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div id="content_views" class="markdown_views prism-atom-one-dark">
<h3 id="注册dropbox账户并获取token"><a name="t0"></a>注册dropbox账户并获取token</h3>
通过地址 <a href="https://www.dropbox.com/developers/apps" target="_blank" rel="nofollow noopener">https://www.dropbox.com/developers/apps</a> 创建APP
创建成功后在页面点击生成token,复制token，下步会用。
<h3 id="下载脚本并使其可执行"><a name="t1"></a>下载脚本并使其可执行</h3>
<code>wget https://raw.github.com/andreafabrizi/Dropbox-Uploader/master/dropbox_uploader.sh</code>
<code>chmod+x dropbox_uploader.sh</code>
通过命令运行 <code>./dropbox_uploader.sh</code>
首次运行会提示配置Access token，输入上部获取的token.
<img class="alignnone size-full wp-image-1544" src="http://hss5.com/wp-content/uploads/2018/11/90873aaf2df67f083abcfa5f12033675-1.jpg" width="881" height="581" alt="这里写图片描述" title="" />
输入y确认即提示配置完成.
<h3 id="常用命令"><a name="t2"></a>常用命令</h3>
显示根目录中的所有内容
<code>·/dropbox_uploader.sh list</code>
列出某个特定文件夹中的所有内容
<code>./dropbox_uploader.sh list folderName</code>
上传一个本地文件到一个远程的 Dropbox 文件夹
<code>./dropbox_uploader.sh upload /home/../fileName /folder/file</code>
从 Dropbox 下载一个远程的文件到本地
<code>./dropbox_uploader.sh download /folder/file /home/../fileName</code>

参考自：
<blockquote><a href="https://github.com/andreafabrizi/Dropbox-Uploader" target="_blank" rel="nofollow noopener">https://github.com/andreafabrizi/Dropbox-Uploader</a></blockquote>
</div>
</div>
</article>