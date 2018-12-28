---
ID: 1800
post_title: >
  如何使用Github
  Pages免费搭建网站
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/28/%e5%a6%82%e4%bd%95%e4%bd%bf%e7%94%a8github-pages%e5%85%8d%e8%b4%b9%e6%90%ad%e5%bb%ba%e7%bd%91%e7%ab%99/'
published: true
post_date: 2018-12-28 22:10:48
---
<div class="show-content" data-note-content="">
<div class="show-content-free">
<div class="image-package">
<div class="image-container"></div>
<div class="image-caption"></div>
</div>
<h3><a href="https://www.jianshu.com/p/6cabb41495c8" target="_blank" rel="noopener">目录</a>###</h3>
<a href="https://www.jianshu.com/p/6cabb41495c8" target="_blank" rel="noopener">1. 什么是Github ？</a>
<a href="https://www.jianshu.com/p/6cabb41495c8" target="_blank" rel="noopener">2. 谁在使用Github免费托管网站 ？</a>
<a href="https://www.jianshu.com/p/6cabb41495c8" target="_blank" rel="noopener">3. Github pages的两种类型</a>
<a href="https://www.jianshu.com/p/6cabb41495c8" target="_blank" rel="noopener">4. Github Pages的限制</a>

<hr />

<h1>1. 什么是Github ？</h1>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1187" data-height="525"><img class="" src="https://upload-images.jianshu.io/upload_images/424375-014db44d0bb6f0f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/424375-014db44d0bb6f0f7.png" data-original-width="1187" data-original-height="525" data-original-format="image/png" data-original-filesize="186025" /></div>
</div>
<div class="image-caption">Github 官方主页</div>
</div>
简单说，Github是一个基于git的社会化代码分享社区。
<ul>
 	<li>你可以在Github上创建<strong>免费的</strong>远程仓库(remote repository)，分享你的代码，当然也可以关注其他人的代码</li>
 	<li>你也可以建立公司账户，创建私有的远程仓库，与开发团队共同协作开发</li>
 	<li>想要使用Github Pages，你首先要创建一个Github账户</li>
</ul>

<hr />

<h1>2. 谁在使用Github免费托管网站 ？</h1>
<ul>
 	<li><a href="https://link.jianshu.com/?t=http://getbootstrap.com" target="_blank" rel="nofollow noopener">Bootstrap</a></li>
 	<li><a href="https://link.jianshu.com/?t=http://nodeschool.io" target="_blank" rel="nofollow noopener">NODESCHOOL</a></li>
 	<li><a href="https://link.jianshu.com/?t=http://webcomponents.org" target="_blank" rel="nofollow noopener">WebComponents</a>
......</li>
</ul>

<hr />

<h1>3. Github pages的两种类型</h1>
<h3>3.1 Project Pages(Repository Pages)</h3>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="625" data-height="352"><img class="" src="https://upload-images.jianshu.io/upload_images/424375-e071f96267a1e69d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/625/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/424375-e071f96267a1e69d.png" data-original-width="625" data-original-height="352" data-original-format="image/png" data-original-filesize="15426" /></div>
</div>
<div class="image-caption">URL for Project Pages.png</div>
</div>
<ul>
 	<li>在Github上我们可以给不同的project分别创建相应的repository，对于某一个repository，你可以在其中创建一个小网站，向人们展示你的项目，提供项目的相关信息等等。这就是所谓的project pages。例如上面说的<a href="https://link.jianshu.com/?t=http://getbootstrap.com" target="_blank" rel="nofollow noopener">bootstrap.com</a></li>
 	<li>在一个repo的<strong>gh-pages</strong>分支中的所有文件将出现在github.io上。</li>
 	<li>Project Pages How-To</li>
 	<li>创建一个<strong>gh-pages</strong>分支</li>
 	<li><strong>编辑</strong>相应的<strong>html/css/js文件</strong>，用于展示在github.io上</li>
 	<li><strong>push gh-pages</strong>分支到Github上面</li>
</ul>
<pre class="hljs cpp"><code class="cpp"><span class="hljs-comment">//下面是一些会用到的git command</span>
git checkout -b gh-pages          <span class="hljs-comment">//create a gh-pages branch </span>
git branch                        <span class="hljs-comment">//check all branches and which branch you are currently working on</span>
git push origin gh-pages          <span class="hljs-comment">//push gh-pages branch to github</span>
git checkout --orphan go-pages    <span class="hljs-comment">//you can create a new empty branch</span>
git push origin :gh-pages         <span class="hljs-comment">//delete a remote branch</span>
</code></pre>
<ul>
 	<li>最简单地方法是从Github上直接自动生成页面，还可以选择模板。<a href="https://link.jianshu.com/?t=https://help.github.com/articles/creating-pages-with-the-automatic-generator/" target="_blank" rel="nofollow noopener">移步这里</a></li>
</ul>
<h3>3.2 User Pages</h3>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="625" data-height="355"><img class="" src="https://upload-images.jianshu.io/upload_images/424375-9f56ef2d90636bf1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/625/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/424375-9f56ef2d90636bf1.png" data-original-width="625" data-original-height="355" data-original-format="image/png" data-original-filesize="12303" /></div>
</div>
<div class="image-caption">URL for User Pages</div>
</div>
<ul>
 	<li>每一个Github账户只能有一个User Pages，主要用来展示一个账户中最最重要的项目。</li>
 	<li>命名为<strong>username.github.io</strong>的repo中的内容将会出现在username.github.io上。</li>
 	<li>User Pages How-To</li>
 	<li>创建一个新的repo，名字必须是username.github.io
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1038" data-height="131"><img class="" src="https://upload-images.jianshu.io/upload_images/424375-f4b4ec14803d7031.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/424375-f4b4ec14803d7031.png" data-original-width="1038" data-original-height="131" data-original-format="image/png" data-original-filesize="27780" /></div>
</div>
<div class="image-caption">创建新的repo</div>
</div></li>
 	<li>创建你的网站，包括：HTML文件，CSS文件...</li>
</ul>
<pre class="hljs ruby"><code class="ruby">my_website_folder
    <span class="hljs-params">|- index.html
    |</span>- main.css
    <span class="hljs-params">|- bootstrap.js
    |</span>...
</code></pre>
<ul>
 	<li>创建本地git repo</li>
</ul>
<pre class="hljs bash"><code class="bash">~ $ <span class="hljs-built_in">cd</span> my_website_folder   //进入你的网站所在的文件夹
~ $ git init
~ $ git add .
~ $ git commit -m <span class="hljs-string">"Initial commit"</span>
</code></pre>
<ul>
 	<li>添加remote repo到本地，push到Github</li>
</ul>
<pre class="hljs cpp"><code class="cpp">~ $ git remote add origin https:<span class="hljs-comment">//github.com/Jason-Yuan/Jason-Yuan.github.io.git</span>
~ $ git remote -v           <span class="hljs-comment">//可以查看是否添加成功，及其详细信息</span>
~ $ git push origin master
</code></pre>
<ul>
 	<li>设置个性域名
<ul>
 	<li>创建一个CNAME文件，包含你的个性域名，<strong>放在source文件夹下</strong></li>
</ul>
</li>
</ul>
<pre class="hljs css"><code class="css"><span class="hljs-selector-tag">example</span><span class="hljs-selector-class">.com</span>
</code></pre>
<ul>
 	<li>把你个性域名的A record指向Github DNS</li>
</ul>
<pre class="hljs css"><code class="css">192<span class="hljs-selector-class">.30</span><span class="hljs-selector-class">.252</span><span class="hljs-selector-class">.153</span>
192<span class="hljs-selector-class">.30</span><span class="hljs-selector-class">.252</span><span class="hljs-selector-class">.154</span>
</code></pre>
<ul>
 	<li>如果想要搭建博客，下面列了一些非常流行的framework，可自动生成静态页面：</li>
 	<li>Octopress (基于Ruby)</li>
 	<li>Jekyll (基于Ruby) - <a href="https://www.jianshu.com/p/3f355c7872d5" target="_blank" rel="noopener">通过Github Pages和Jekyll搭建个人博客</a></li>
 	<li>Hexo (基于NodeJS) - <a href="https://www.jianshu.com/p/bc1056bfec85" target="_blank" rel="noopener">通过Github Pages和Hexo搭建个人博客</a></li>
 	<li>Pelican (基于Python)</li>
</ul>

<hr />

<h1>4. Github Pages的限制(Limitations)</h1>
<ul>
 	<li>Github Pages只是静态网站(HTML, CSS, JavaScript)</li>
 	<li>没有服务端，所以不支持服务端的语言(没有ruby, php, python)</li>
</ul>
<h1>参考资料</h1>
<ol>
 	<li><a href="https://www.jianshu.com/p/6cabb41495c8" target="_blank" rel="noopener">Github官网</a></li>
 	<li><a href="https://link.jianshu.com/?t=http://blog.teamtreehouse.com/using-github-pages-to-host-your-website" target="_blank" rel="nofollow noopener">Using GitHub Pages To Host Your Website</a></li>
 	<li><a href="https://www.jianshu.com/p/48d58d16920d" target="_blank" rel="noopener">Repositories to build blogs on GitHub pages</a></li>
</ol>
</div>
</div>