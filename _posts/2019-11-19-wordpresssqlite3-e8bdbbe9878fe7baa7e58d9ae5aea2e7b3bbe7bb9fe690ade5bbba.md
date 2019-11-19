---
ID: 3337
post_title: >
  wordpress+sqlite3
  轻量级博客系统搭建
author: peng, chris
post_excerpt: ""
layout: post
permalink: 'https://www.hss5.com/2019/11/19/wordpresssqlite3-%e8%bd%bb%e9%87%8f%e7%ba%a7%e5%8d%9a%e5%ae%a2%e7%b3%bb%e7%bb%9f%e6%90%ad%e5%bb%ba/'
published: true
post_date: 2019-11-19 12:56:54
---
<h1 class="_1RuRku">wordpress+sqlite3 轻量级博客系统搭建</h1>
<article class="_2rhmJa">
<blockquote>自行搭建php运行环境(如果你是小白,且是windows系统,我推荐使用<a href="https://link.jianshu.com/?t=https://www.apachefriends.org/zh_cn/index.html" target="_blank" rel="nofollow noopener noreferrer">xampp</a>)</blockquote>
<h1>1.准备工作</h1>
<h4>1.自行搭建php运行环境(如果你是小白,且是windows系统,我推荐使用<a href="https://link.jianshu.com/?t=https://www.apachefriends.org/zh_cn/index.html" target="_blank" rel="nofollow noopener noreferrer">xampp</a>)</h4>
<h4>2.下载<a href="https://link.jianshu.com/?t=https://cn.wordpress.org/" target="_blank" rel="nofollow noopener noreferrer">wordpress</a></h4>
<h4>3.下载<a href="https://link.jianshu.com/?t=https://wordpress.org/plugins/sqlite-integration/installation/" target="_blank" rel="nofollow noopener noreferrer">SQLite Integration 插件</a></h4>
<h1>2.安装wordpress</h1>
<h4>解压下载的wordpress压缩包到php 运行目录(我拿xampp为例目录是在xampp安装目录下的htdocs目录)</h4>
<h4>将目录下的<strong>wp-config-sample.php</strong>复制粘贴一份重命名为<strong>wp-config.php</strong></h4>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="799" data-height="475"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-966c3dc0ecd3932e.png?imageMogr2/auto-orient/strip|imageView2/2/w/799/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-966c3dc0ecd3932e.png" data-original-width="799" data-original-height="475" data-original-format="image/png" data-original-filesize="33762" data-image-index="0" /></div>
</div>
<div class="image-caption">示例</div>
</div>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="665" data-height="457"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-93932edcb0b64548.png?imageMogr2/auto-orient/strip|imageView2/2/w/665/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-93932edcb0b64548.png" data-original-width="665" data-original-height="457" data-original-format="image/png" data-original-filesize="44978" data-image-index="1" /></div>
</div>
<div class="image-caption">示例</div>
</div>
<h4>打开<strong>wp-config.php</strong>修改以下配置</h4>
<h4>原始文件:</h4>
<pre class="line-numbers  language-csharp"><code class="  language-csharp"><span class="token comment">// ** MySQL 设置 - 具体信息来自您正在使用的主机 ** //</span>
<span class="token comment">/** WordPress数据库的名称 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_NAME'</span><span class="token punctuation">,</span> <span class="token string">'database_name_here'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** MySQL数据库用户名 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_USER'</span><span class="token punctuation">,</span> <span class="token string">'username_here'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** MySQL数据库密码 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_PASSWORD'</span><span class="token punctuation">,</span> <span class="token string">'password_here'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** MySQL主机 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_HOST'</span><span class="token punctuation">,</span> <span class="token string">'localhost'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** 创建数据表时默认的文字编码 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_CHARSET'</span><span class="token punctuation">,</span> <span class="token string">'utf8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** 数据库整理类型。如不确定请勿更改 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_COLLATE'</span><span class="token punctuation">,</span> <span class="token string">''</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code><button class="VJbwyy" type="button" aria-label="复制代码"><i class="anticon anticon-copy" aria-label="icon: copy"></i></button></pre>
<h4>修改为:</h4>
<pre class="line-numbers  language-csharp"><code class="  language-csharp"><span class="token comment">// ** MySQL 设置 - 具体信息来自您正在使用的主机 ** //</span>
<span class="token comment">/** WordPress数据库的名称 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_NAME'</span><span class="token punctuation">,</span> <span class="token string">'MyBlog'</span><span class="token punctuation">)</span><span class="token punctuation">;</span><span class="token comment">//MyBlog&lt;====这是数据库名,可以自定义</span>

<span class="token comment">/** MySQL数据库用户名 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_USER'</span><span class="token punctuation">,</span> <span class="token string">''</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** MySQL数据库密码 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_PASSWORD'</span><span class="token punctuation">,</span> <span class="token string">''</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** MySQL主机 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_HOST'</span><span class="token punctuation">,</span> <span class="token string">'localhost'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** 创建数据表时默认的文字编码 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_CHARSET'</span><span class="token punctuation">,</span> <span class="token string">'utf8'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

<span class="token comment">/** 数据库整理类型。如不确定请勿更改 */</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_COLLATE'</span><span class="token punctuation">,</span> <span class="token string">''</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token comment">//define('WP_ALLOW_REPAIR', true);//数据库修复时使用</span>
<span class="token function">define</span><span class="token punctuation">(</span><span class="token string">'DB_TYPE'</span><span class="token punctuation">,</span> <span class="token string">'sqlite'</span><span class="token punctuation">)</span><span class="token punctuation">;</span>    <span class="token comment">//mysql or sqlite`</span>
</code><button class="VJbwyy" type="button" aria-label="复制代码"><i class="anticon anticon-copy" aria-label="icon: copy"></i></button></pre>
<h4>解压<strong>SQLite Integration</strong>到wordpress安装目录下wp-content\plugins\</h4>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="657" data-height="197"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-b3bdb705cdf72bc8.png?imageMogr2/auto-orient/strip|imageView2/2/w/657/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-b3bdb705cdf72bc8.png" data-original-width="657" data-original-height="197" data-original-format="image/png" data-original-filesize="8927" data-image-index="2" /></div>
</div>
<div class="image-caption">示例</div>
</div>
<h4>找到<strong>db.php</strong></h4>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="697" data-height="445"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-2dc6cdb4b67a9b8a.png?imageMogr2/auto-orient/strip|imageView2/2/w/697/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-2dc6cdb4b67a9b8a.png" data-original-width="697" data-original-height="445" data-original-format="image/png" data-original-filesize="45575" data-image-index="3" /></div>
</div>
<div class="image-caption">无标题.png</div>
</div>
<h4>复制到到wordpress安装目录下的wp-content目录中</h4>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="709" data-height="231"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-c3d0b53f6470aeff.png?imageMogr2/auto-orient/strip|imageView2/2/w/709/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-c3d0b53f6470aeff.png" data-original-width="709" data-original-height="231" data-original-format="image/png" data-original-filesize="9904" data-image-index="4" /></div>
</div>
<div class="image-caption">示例</div>
</div>
<h1>3.运行并配置博客</h1>
<h4>如果你还未运行apache服务器 那就请先运行</h4>
<h4>浏览器访问网站比如:http://127.0.0.1:8080/</h4>
<h4>根据页面显示填写完整信息后点击底下安装</h4>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1451" data-height="711"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-3a52c64e9676e734.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-3a52c64e9676e734.png" data-original-width="1451" data-original-height="711" data-original-format="image/png" data-original-filesize="59971" data-image-index="5" /></div>
</div>
<div class="image-caption">示例</div>
</div>
<h4>配置完成后会自动进入管理员界面</h4>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1455" data-height="717"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-33531b35f6ee6d06.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-33531b35f6ee6d06.png" data-original-width="1455" data-original-height="717" data-original-format="image/png" data-original-filesize="87987" data-image-index="6" /></div>
</div>
<div class="image-caption">示例</div>
</div>
<h1>4.开始你的wordpress博客之旅吧</h1>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1453" data-height="739"><img class="" src="https://upload-images.jianshu.io/upload_images/2675631-d00d1d5dd27e152e.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp" data-original-src="//upload-images.jianshu.io/upload_images/2675631-d00d1d5dd27e152e.png" data-original-width="1453" data-original-height="739" data-original-format="image/png" data-original-filesize="900292" data-image-index="7" /></div>
</div>
<div class="image-caption">默认主页</div>
</div>
</article>