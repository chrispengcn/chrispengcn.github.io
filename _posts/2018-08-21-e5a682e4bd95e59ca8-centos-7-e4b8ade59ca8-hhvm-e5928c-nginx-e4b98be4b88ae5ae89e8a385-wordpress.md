---
ID: 1293
post_title: >
  如何在 CentOS 7 中在 HHVM 和 Nginx
  之上安装 WordPress
author: chrispengcn
post_excerpt: >
  HHVM （HipHop Virtual Machine）
  是一个用于执行以 PHP 和 Hack
  语言编写的代码的虚拟环境。它是由
  Facebook 开发的，提供了当前 PHP
  7
  的大多数功能。要在你的服务器上运行
  HHVM，你需要使用 FastCGI 来将
  HHVM 和 Nginx 或 Apache
  衔接起来，或者你也可以使用
  HHVM 中的内置 Web 服务器
  Proxygen。
layout: post
permalink: 'http://hss5.com/2018/08/21/%e5%a6%82%e4%bd%95%e5%9c%a8-centos-7-%e4%b8%ad%e5%9c%a8-hhvm-%e5%92%8c-nginx-%e4%b9%8b%e4%b8%8a%e5%ae%89%e8%a3%85-wordpress/'
published: true
post_date: 2018-08-21 14:15:21
---
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i19.55592586z52BbJ">HHVM （HipHop Virtual Machine） 是一个用于执行以 PHP 和 Hack 语言编写的代码的虚拟环境。它是由 Facebook 开发的，提供了当前 PHP 7 的大多数功能。要在你的服务器上运行 HHVM，你需要使用 FastCGI 来将 HHVM 和 Nginx 或 Apache 衔接起来，或者你也可以使用 HHVM 中的内置 Web 服务器 Proxygen。</p>
在这篇教程中，我将展示给你如何在 Nginx Web 服务器的 HHVM 上安装 WordPress。这里我使用 CentOS 7 作为操作系统，所以你需要懂一点 CentOS 操作的基础。

先决条件
<ul>
 	<li>CentOS 7 - 64位</li>
 	<li>Root 权限</li>
</ul>
<h3 id="1">步骤 1 - 配置 SELinux 并添加 EPEL 仓库</h3>
在本教程中，我们将使用 SELinux 的强制模式，所以我们需要在系统上安装一个 SELinux 管理工具。这里我们使用 <code>setools</code> 和 <code>setrobleshoot</code> 来管理 SELinux 的各项配置。

CentOS 7 已经默认启用 SELinux，我们可以通过以下命令来确认：
<ol>
 	<li class="L0" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i0.55592586z52BbJ"><code><span class="com">#</span><span class="pln" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i1.55592586z52BbJ"> sestatus</span></code></li>
 	<li class="L1" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i3.55592586z52BbJ"><code><span class="com">#</span><span class="pln"> getenforce</span></code></li>
</ol>
<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154050wli510n0fq5nbfcb.png" alt="验证 SELinux 运行状态" />

<em>验证 SELinux 运行状态</em>

如图，你能够看到，SELinux 已经开启了强制模式。

接下来就是使用 <code>yum</code> 来安装 <code>setools</code> 和 <code>setroubleshoot</code> 了。
<pre class="prettyprint linenums prettyprinted"> #<span class="pln" style="color: #222222; font-size: 1em;"> </span><span class="kwd" style="color: #222222; font-size: 1em;">yum</span><span class="pln" style="color: #222222; font-size: 1em;"> </span><span class="pun" style="color: #222222; font-size: 1em;">-</span><span class="pln" style="color: #222222; font-size: 1em;">y install setroubleshoot setools net</span><span class="pun" style="color: #222222; font-size: 1em;">-</span><span class="pln" style="color: #222222; font-size: 1em;">tools</span></pre>
安装好这两个后，再安装 EPEL 仓库。
<pre class="prettyprint linenums prettyprinted"> #<span class="pln" style="color: #222222; font-size: 1em;"> </span><span class="kwd" style="color: #222222; font-size: 1em;">yum</span><span class="pln" style="color: #222222; font-size: 1em;"> </span><span class="pun" style="color: #222222; font-size: 1em;">-</span><span class="pln" style="color: #222222; font-size: 1em;">y install epel</span><span class="pun" style="color: #222222; font-size: 1em;">-</span><span class="pln" style="color: #222222; font-size: 1em;">release</span></pre>
<h3 id="2">步骤 2 - 安装 Nginx</h3>
Nginx (发音：engine-x) 是一个高性能、低内存消耗的轻量级 Web 服务器软件。在 CentOS 中可以使用 <code>yum</code>命令来安装 Nginx 包。确保你以 root 用户登录系统。

使用 <code>yum</code> 命令从 CentOS 仓库中安装 nginx。
<pre class="prettyprint linenums prettyprinted"> #<span class="pln" style="color: #222222; font-size: 1em;"> </span><span class="kwd" style="color: #222222; font-size: 1em;">yum</span><span class="pln" style="color: #222222; font-size: 1em;"> </span><span class="pun" style="color: #222222; font-size: 1em;">-</span><span class="pln" style="color: #222222; font-size: 1em;">y install nginx</span></pre>
现在可以使用 <code>systemctl</code> 命令来启动 Nginx，同时将其设置为跟随系统启动。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i5.55592586z52BbJ"> start nginx</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> enable nginx</span></code></li>
</ol>
为确保 Nginx 已经正确运行于服务器中，在浏览器上输入服务器的 IP，或者如下使用 <code>curl</code> 命令检查显示结果。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> curl </span><span class="lit">192.168</span><span class="pun">.</span><span class="lit">1.110</span></code></li>
</ol>
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i4.55592586z52BbJ">我这里使用浏览器来验证。</p>
<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154050gzhaia3zxpoai89o.png" alt="Nginx 正确运行" />

<em>Nginx 正确运行</em>
<h3 id="3">步骤 3 - 安装并配置 MariaDB</h3>
MariaDB 是由原 MySQL 开发者 Monty Widenius 开发的一款开源数据库软件，它由 MySQL 分支而来，与 MySQL 的主要功能保持一致。在这一步中，我们要安装 MariaDB 数据库并为之配置好 root 密码，然后再为所要安装的 WordPress 创建一个新的数据库和用户。

安装 mariadb 和 mariadb-server：
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">yum</span><span class="pln"> </span><span class="pun">-</span><span class="pln" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i6.55592586z52BbJ">y install mariadb mariadb</span><span class="pun">-</span><span class="pln">server</span></code></li>
</ol>
启动 MariaDB 并添加为服务，以便随系统启动。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> start mariadb</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> enable mariadb</span></code></li>
</ol>
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i7.55592586z52BbJ">现在 MariaDB 已经启动了，还需要为 mariadb/mysql 数据库配置 root 用户密码。输入以下命令来设置 MariaDB root 密码。</p>

<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i8.55592586z52BbJ"> mysql_secure_installation</span></code></li>
</ol>
提示设置 root 用户密码时，输入新密码进行设置：
<ol>
 	<li class="L0"><code><span class="typ">Set</span><span class="pln"> root password</span><span class="pun">?</span><span class="pln"> </span><span class="pun">[</span><span class="pln">Y</span><span class="pun">/</span><span class="pln">n</span><span class="pun">]</span><span class="pln"> Y</span></code></li>
 	<li class="L1"><code><span class="typ">New</span><span class="pln"> password</span><span class="pun">:</span></code></li>
 	<li class="L2"><code><span class="typ">Re</span><span class="pun">-</span><span class="pln">enter </span><span class="kwd">new</span><span class="pln"> password</span><span class="pun">:</span></code></li>
 	<li class="L3"><code></code></li>
 	<li class="L4"><code><span class="typ">Remove</span><span class="pln"> anonymous </span><span class="kwd">users</span><span class="pun">?</span><span class="pln"> </span><span class="pun">[</span><span class="pln">Y</span><span class="pun">/</span><span class="pln">n</span><span class="pun">]</span><span class="pln"> Y</span></code></li>
 	<li class="L5"><code><span class="pun">...</span><span class="pln"> </span><span class="typ">Success</span><span class="pun">!</span></code></li>
 	<li class="L6"><code><span class="typ">Disallow</span><span class="pln"> root </span><span class="kwd">login</span><span class="pln"> remotely</span><span class="pun">?</span><span class="pln"> </span><span class="pun">[</span><span class="pln">Y</span><span class="pun">/</span><span class="pln">n</span><span class="pun">]</span><span class="pln"> Y</span></code></li>
 	<li class="L7"><code><span class="pun">...</span><span class="pln"> </span><span class="typ">Success</span><span class="pun">!</span></code></li>
 	<li class="L8"><code><span class="typ">Remove</span><span class="pln"> </span><span class="kwd">test</span><span class="pln"> database </span><span class="kwd">and</span><span class="pln"> access to it</span><span class="pun">?</span><span class="pln"> </span><span class="pun">[</span><span class="pln">Y</span><span class="pun">/</span><span class="pln">n</span><span class="pun">]</span><span class="pln"> Y</span></code></li>
 	<li class="L9"><code><span class="typ">Reload</span><span class="pln"> privilege tables now</span><span class="pun">?</span><span class="pln"> </span><span class="pun">[</span><span class="pln">Y</span><span class="pun">/</span><span class="pln">n</span><span class="pun">]</span><span class="pln"> Y</span></code></li>
 	<li class="L0" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i9.55592586z52BbJ"><code><span class="pun">...</span><span class="pln"> </span><span class="typ">Success</span><span class="pun">!</span></code></li>
</ol>
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i10.55592586z52BbJ">这样就设置好了 MariaDB 的 root 密码。现在登录到 MariaDB/MySQL shell 并为 WordPress 的安装创建一个新数据库 <code>wordpressdb</code> 和新用户 <code>wpuser</code>，密码设置为 <code>wpuser@</code>。为你的设置选用一个安全的密码。</p>
登录到 MariaDB/MySQL shell：
<pre class="prettyprint linenums prettyprinted"> #<span class="pln" style="color: #222222; font-size: 1em;"> mysql </span><span class="pun" style="color: #222222; font-size: 1em;">-</span><span class="pln" style="color: #222222; font-size: 1em;">u root </span><span class="pun" style="color: #222222; font-size: 1em;">-</span><span class="pln" style="color: #222222; font-size: 1em;">p</span></pre>
接着输入你刚刚设置的 root 用户密码。

创建数据库和用户：
<ol>
 	<li class="L0"><code><span class="typ">MariaDB</span><span class="pln"> </span><span class="pun">[(</span><span class="pln">none</span><span class="pun">)]&gt;</span><span class="pln"> create database wordpressdb</span><span class="pun">;</span></code></li>
 	<li class="L1"><code><span class="typ">MariaDB</span><span class="pln"> </span><span class="pun">[(</span><span class="pln">none</span><span class="pun">)]&gt;</span><span class="pln"> create user wpuser@localhost identified by </span><span class="str">'wpuser@'</span><span class="pun">;</span></code></li>
 	<li class="L2"><code><span class="typ">MariaDB</span><span class="pln"> </span><span class="pun">[(</span><span class="pln">none</span><span class="pun">)]&gt;</span><span class="pln"> grant all privileges on wordpressdb</span><span class="pun">.*</span><span class="pln"> to wpuser@localhost identified by </span><span class="str">'wpuser@'</span><span class="pun">;</span></code></li>
 	<li class="L3"><code><span class="typ">MariaDB</span><span class="pln"> </span><span class="pun">[(</span><span class="pln">none</span><span class="pun">)]&gt;</span><span class="pln"> flush privileges</span><span class="pun">;</span></code></li>
 	<li class="L4"><code><span class="typ">MariaDB</span><span class="pln"> </span><span class="pun">[(</span><span class="pln">none</span><span class="pun">)]&gt;</span><span class="pln"> \q</span></code></li>
</ol>
<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154055t3tc327j38chm38y.png" alt="为 WordPress 的安装创建数据库和用户" />

<em>为 WordPress 的安装创建数据库和用户</em>

现在安装好了 MariaDB，并为 WordPress 创建好了数据库。
<h3 id="4">步骤 4 - 安装 HHVM</h3>
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i12.55592586z52BbJ">对于 HHVM，我们需要安装大量的依赖项。作为选择，你可以从 GitHub 下载 HHVM 的源码来编译安装，也可以从网络上获取预编译的包进行安装。在本教程中，我使用的是预编译的安装包。</p>
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i13.55592586z52BbJ">为 HHVM 安装依赖项：</p>

<ol>
 	<li class="L0" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i14.55592586z52BbJ"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">yum</span><span class="pln"> </span><span class="pun">-</span><span class="pln">y install cpp </span><span class="kwd">gcc</span><span class="pun">-</span><span class="pln">c</span><span class="pun">++</span><span class="pln"> cmake </span><span class="kwd">git</span><span class="pln"> psmisc </span><span class="pun">{</span><span class="pln">binutils</span><span class="pun">,</span><span class="pln">boost</span><span class="pun">,</span><span class="pln">jemalloc</span><span class="pun">,</span><span class="pln">numactl</span><span class="pun">}-</span><span class="pln">devel \</span></code></li>
 	<li class="L1"><code><span class="pun">&gt;</span><span class="pln"> </span><span class="pun">{</span><span class="typ">ImageMagick</span><span class="pun">,</span><span class="pln">sqlite</span><span class="pun">,</span><span class="pln">tbb</span><span class="pun">,</span><span class="pln">bzip2</span><span class="pun">,</span><span class="pln">openldap</span><span class="pun">,</span><span class="pln">readline</span><span class="pun">,</span><span class="pln">elfutils</span><span class="pun">-</span><span class="pln">libelf</span><span class="pun">,</span><span class="pln">gmp</span><span class="pun">,</span><span class="pln">lz4</span><span class="pun">,</span><span class="pln">pcre</span><span class="pun">}-</span><span class="pln">devel \</span></code></li>
 	<li class="L2"><code><span class="pun">&gt;</span><span class="pln"> lib</span><span class="pun">{</span><span class="pln">xslt</span><span class="pun">,</span><span class="pln">event</span><span class="pun">,</span><span class="pln">yaml</span><span class="pun">,</span><span class="pln">vpx</span><span class="pun">,</span><span class="pln">png</span><span class="pun">,</span><span class="pln">zip</span><span class="pun">,</span><span class="pln">icu</span><span class="pun">,</span><span class="pln">mcrypt</span><span class="pun">,</span><span class="pln">memcached</span><span class="pun">,</span><span class="pln">cap</span><span class="pun">,</span><span class="pln">dwarf</span><span class="pun">}-</span><span class="pln">devel \</span></code></li>
 	<li class="L3"><code><span class="pun">&gt;</span><span class="pln"> </span><span class="pun">{</span><span class="pln">unixODBC</span><span class="pun">,</span><span class="pln">expat</span><span class="pun">,</span><span class="pln">mariadb</span><span class="pun">}-</span><span class="pln">devel lib</span><span class="pun">{</span><span class="pln">edit</span><span class="pun">,</span><span class="pln">curl</span><span class="pun">,</span><span class="pln">xml2</span><span class="pun">,</span><span class="pln">xslt</span><span class="pun">}-</span><span class="pln">devel \</span></code></li>
 	<li class="L4" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i15.55592586z52BbJ"><code><span class="pun">&gt;</span><span class="pln"> glog</span><span class="pun">-</span><span class="pln">devel oniguruma</span><span class="pun">-</span><span class="pln">devel ocaml gperf enca libjpeg</span><span class="pun">-</span><span class="pln">turbo</span><span class="pun">-</span><span class="pln">devel openssl</span><span class="pun">-</span><span class="pln">devel \</span></code></li>
 	<li class="L5"><code><span class="pun">&gt;</span><span class="pln"> mariadb mariadb</span><span class="pun">-</span><span class="pln">server libc</span><span class="pun">-</span><span class="pln">client </span><span class="kwd">make</span></code></li>
</ol>
然后是使用 <code>rpm</code> 安装从 <a class="ext" href="http://mirrors.linuxeye.com/hhvm-repo/7/x86_64/" target="_blank" rel="noopener">HHVM 预编译包镜像站点</a> 下载的 HHVM 预编译包。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> rpm </span><span class="pun">-</span><span class="typ">Uvh</span><span class="pln"> http</span><span class="pun">:</span><span class="com">//mirrors.linuxeye.com/hhvm-repo/7/x86_64/hhvm-3.15.2-1.el7.centos.x86_64.rpm</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">ln</span><span class="pln"> </span><span class="pun">-</span><span class="pln">s </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="kwd">local</span><span class="pun">/</span><span class="pln">bin</span><span class="pun">/</span><span class="pln">hhvm </span><span class="pun">/</span><span class="pln">bin</span><span class="pun">/</span><span class="pln">hhvm</span></code></li>
</ol>
安装好 HHVM 之后使用如下命令按了验证：
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> hhvm </span><span class="pun">--</span><span class="pln">version</span></code></li>
</ol>
为了能使用 PHP 命令，可以把 <code>hhvm</code> 命令设置为 <code>php</code>。这样在 shell 中输入 <code>php</code> 命令的时候，你会看到和输入 <code>hhvm</code> 命令一样的结果。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">sudo</span><span class="pln"> update</span><span class="pun">-</span><span class="pln">alternatives </span><span class="pun">--</span><span class="pln">install </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="pln">bin</span><span class="pun">/</span><span class="pln">php php </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="pln">bin</span><span class="pun">/</span><span class="pln">hhvm </span><span class="lit">60</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> php </span><span class="pun">--</span><span class="pln">version</span></code></li>
</ol>
<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154056wkljh19kkjry3284.png" alt="安装 HHVM" />

<em>安装 HHVM</em>
<h3 id="5">步骤 5 - 配置 HHVM</h3>
这一步中，我们来配置 HHVM 以系统服务来运行。我们不通过端口这种常规的方式来运行它，而是选择使用 unix socket 文件的方式，这样运行的更快速一点。

进入 systemd 配置目录，并创建一个 <code>hhvm.service</code> 文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">cd</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="kwd">systemd</span><span class="pun">/</span><span class="pln">system</span><span class="pun">/</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">vim</span><span class="pln"> hhvm</span><span class="pun">.</span><span class="pln">service</span></code></li>
</ol>
复制粘贴如下配置到文件中去。
<ol>
 	<li class="L0"><code><span class="pun">[</span><span class="typ">Unit</span><span class="pun">]</span></code></li>
 	<li class="L1"><code><span class="typ">Description</span><span class="pun">=</span><span class="pln">HHVM </span><span class="typ">HipHop</span><span class="pln"> </span><span class="typ">Virtual</span><span class="pln"> </span><span class="typ">Machine</span><span class="pln"> </span><span class="pun">(</span><span class="pln">FCGI</span><span class="pun">)</span></code></li>
 	<li class="L2"><code><span class="typ">After</span><span class="pun">=</span><span class="pln">network</span><span class="pun">.</span><span class="pln">target nginx</span><span class="pun">.</span><span class="pln">service mariadb</span><span class="pun">.</span><span class="pln">service</span></code></li>
 	<li class="L3"><code></code></li>
 	<li class="L4"><code><span class="pun">[</span><span class="typ">Service</span><span class="pun">]</span></code></li>
 	<li class="L5"><code><span class="typ">ExecStart</span><span class="pun">=</span><span class="str">/usr/</span><span class="kwd">local</span><span class="pun">/</span><span class="pln">bin</span><span class="pun">/</span><span class="pln">hhvm </span><span class="pun">--</span><span class="pln">config </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span><span class="pln">server</span><span class="pun">.</span><span class="pln">ini </span><span class="pun">--</span><span class="pln">user nginx </span><span class="pun">--</span><span class="pln">mode daemon </span><span class="pun">-</span><span class="pln">vServer</span><span class="pun">.</span><span class="typ">Type</span><span class="pun">=</span><span class="pln">fastcgi </span><span class="pun">-</span><span class="pln">vServer</span><span class="pun">.</span><span class="typ">FileSocket</span><span class="pun">=</span><span class="str">/var/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">sock</span></code></li>
 	<li class="L6"><code></code></li>
 	<li class="L7"><code><span class="pun">[</span><span class="typ">Install</span><span class="pun">]</span></code></li>
 	<li class="L8"><code><span class="typ">WantedBy</span><span class="pun">=</span><span class="pln">multi</span><span class="pun">-</span><span class="pln">user</span><span class="pun">.</span><span class="pln">target</span></code></li>
</ol>
保存文件退出 vim。

接下来，进入 <code>hhvm</code> 目录并编辑 <code>server.ini</code> 文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">cd</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">vim</span><span class="pln"> server</span><span class="pun">.</span><span class="pln">ini</span></code></li>
</ol>
将第 7 行 <code>hhvm.server.port</code> 替换为 unix socket，如下：
<ol>
 	<li class="L0"><code><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">server</span><span class="pun">.</span><span class="pln">file_socket </span><span class="pun">=</span><span class="pln"> </span><span class="str">/var/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">sock</span></code></li>
</ol>
保存文件并退出编辑器。

我们已在 hhvm 服务文件中定义了 hhvm 以 <code>nginx</code> 用户身份运行，所以还需要把 socket 文件目录的属主变更为 <code>nginx</code>。然后我们还必须在 SELinux 中修改 hhvm 目录的权限上下文以便让它可以访问这个 socket 文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">chown</span><span class="pln"> </span><span class="pun">-</span><span class="pln">R nginx</span><span class="pun">:</span><span class="pln">nginx </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> semanage fcontext </span><span class="pun">-</span><span class="pln">a </span><span class="pun">-</span><span class="pln">t </span><span class="typ">httpd_var_run_t</span><span class="pln"> </span><span class="str">"/var/run/hhvm(/.*)?"</span></code></li>
 	<li class="L2"><code><span class="com">#</span><span class="pln"> restorecon </span><span class="pun">-</span><span class="typ">Rv</span><span class="pln"> </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span></code></li>
</ol>
服务器重启之后，hhvm 将不能运行，因为没有存储 socket 文件的目录，所有还必须在启动的时候自动创建一个。

使用 vim 编辑 <code>rc.local</code> 文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">vim</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">rc</span><span class="pun">.</span><span class="kwd">local</span></code></li>
</ol>
将以下配置粘贴到文件末行。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">mkdir</span><span class="pln"> </span><span class="pun">-</span><span class="pln">p </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">chown</span><span class="pln"> </span><span class="pun">-</span><span class="pln">R nginx</span><span class="pun">:</span><span class="pln">nginx </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span></code></li>
 	<li class="L2"><code><span class="com">#</span><span class="pln"> semanage fcontext </span><span class="pun">-</span><span class="pln">a </span><span class="pun">-</span><span class="pln">t </span><span class="typ">httpd_var_run_t</span><span class="pln"> </span><span class="str">"/var/run/hhvm(/.*)?"</span></code></li>
 	<li class="L3"><code><span class="com">#</span><span class="pln"> restorecon </span><span class="pun">-</span><span class="typ">Rv</span><span class="pln"> </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span></code></li>
</ol>
保存文件并退出 vim。然后给文件赋予执行权限。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">chmod</span><span class="pln"> </span><span class="pun">+</span><span class="pln">x </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">rc</span><span class="pun">.</span><span class="kwd">local</span></code></li>
</ol>
重新加载 systemd 服务，启动 hhvm 并设置为随系统启动。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> daemon</span><span class="pun">-</span><span class="pln">reload</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> start hhvm</span></code></li>
 	<li class="L2"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> enable hhvm</span></code></li>
</ol>
要确保无误，使用 <code>netstat</code> 命令验证 hhvm 运行于 socket 文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">netstat</span><span class="pln"> </span><span class="pun">-</span><span class="pln">pl </span><span class="pun">|</span><span class="pln"> </span><span class="kwd">grep</span><span class="pln"> hhvm</span></code></li>
</ol>
<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154056v60mflnmsvjhz6vl.png" alt="Check the HHVM socket file" />

<em>Check the HHVM socket file</em>
<h3 id="6">步骤 6 - 配置 HHVM 和 Nginx</h3>
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i16.55592586z52BbJ">在这个步骤中，我们将配置 HHVM 已让它运行在 Nginx Web 服务中，这需要在 Nginx 目录创建一个 hhvm 的配置文件。</p>
进入 <code>/etc/nginx</code> 目录，创建 <code>hhvm.conf</code> 文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">cd</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">vim</span><span class="pln"> hhvm</span><span class="pun">.</span><span class="pln">conf</span></code></li>
</ol>
粘贴以下内容到文件中。
<ol>
 	<li class="L0"><code><span class="pln">location </span><span class="pun">~</span><span class="pln"> \.</span><span class="pun">(</span><span class="pln">hh</span><span class="pun">|</span><span class="pln">php</span><span class="pun">)</span><span class="pln">$ </span><span class="pun">{</span></code></li>
 	<li class="L1" data-spm-anchor-id="a2c4e.11153940.blogcont86279.i17.55592586z52BbJ"><code><span class="pln">root </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="pln">share</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">html</span><span class="pun">;</span></code></li>
 	<li class="L2"><code><span class="pln">fastcgi_keep_conn on</span><span class="pun">;</span></code></li>
 	<li class="L3"><code><span class="pln">fastcgi_pass unix</span><span class="pun">:</span><span class="str">/var/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">sock</span><span class="pun">;</span></code></li>
 	<li class="L4"><code><span class="pln">fastcgi_index index</span><span class="pun">.</span><span class="pln">php</span><span class="pun">;</span></code></li>
 	<li class="L5"><code><span class="pln">fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name</span><span class="pun">;</span></code></li>
 	<li class="L6"><code><span class="kwd">include</span><span class="pln"> fastcgi_params</span><span class="pun">;</span></code></li>
 	<li class="L7"><code><span class="pun">}</span></code></li>
</ol>
然后，保存并退出。

接下来，编辑 <code>nginx.conf</code> 文件，添加 hhvm 配置文件到 <code>include</code> 行。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">vim</span><span class="pln"> nginx</span><span class="pun">.</span><span class="pln">conf</span></code></li>
</ol>
添加配置到第 57 行的 <code>server</code> 指令中。
<ol>
 	<li class="L0"><code><span class="kwd">include</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">conf</span><span class="pun">;</span></code></li>
</ol>
保存并退出。

然后修改 SELinux 中关于 hhvm 配置文件的权限上下文。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> semanage fcontext </span><span class="pun">-</span><span class="pln">a </span><span class="pun">-</span><span class="pln">t </span><span class="typ">httpd_config_t</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">conf</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> restorecon </span><span class="pun">-</span><span class="pln">v </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">conf</span></code></li>
</ol>
测试 Nginx 配置并重启服务。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> nginx </span><span class="pun">-</span><span class="pln">t</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> restart nginx</span></code></li>
</ol>
记住确保测试配置没有错误。
<h3 id="7">步骤 7 - 通过 HHVM 和 Nginx 创建虚拟主机</h3>
在这一步中，我们要为 Nginx 和 hhvm 创建一个新的虚拟主机配置文件。这里我使用域名 <code>natsume.co</code> 来作为例子，你可以使用你主机喜欢的域名，并在配置文件中相应位置以及 WordPress 安装过程中进行替换。

进入 nginx 的 <code>conf.d</code> 目录，我们将在该目录存储虚拟主机文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">cd</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">conf</span><span class="pun">.</span><span class="pln">d</span><span class="pun">/</span></code></li>
</ol>
使用 vim 创建一个名为 <code>natsume.conf</code> 的配置文件。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">vim</span><span class="pln"> natsume</span><span class="pun">.</span><span class="pln">conf</span></code></li>
</ol>
粘贴以下内容到虚拟主机配置文件中。
<ol>
 	<li class="L0"><code><span class="pln">server </span><span class="pun">{</span></code></li>
 	<li class="L1"><code><span class="pln">listen </span><span class="lit">80</span><span class="pun">;</span></code></li>
 	<li class="L2"><code><span class="pln">server_name natsume</span><span class="pun">.</span><span class="pln">co</span><span class="pun">;</span></code></li>
 	<li class="L3"><code></code></li>
 	<li class="L4"><code><span class="com">#</span><span class="pln"> note that these lines are originally </span><span class="kwd">from</span><span class="pln"> the </span><span class="str">"location /"</span><span class="pln"> block</span></code></li>
 	<li class="L5"><code><span class="pln">root </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">www</span><span class="pun">/</span><span class="pln">hakase</span><span class="pun">;</span></code></li>
 	<li class="L6"><code><span class="pln">index index</span><span class="pun">.</span><span class="pln">php index</span><span class="pun">.</span><span class="pln">html index</span><span class="pun">.</span><span class="pln">htm</span><span class="pun">;</span></code></li>
 	<li class="L7"><code></code></li>
 	<li class="L8"><code><span class="pln">location </span><span class="pun">/</span><span class="pln"> </span><span class="pun">{</span></code></li>
 	<li class="L9"><code><span class="pln">try_files $uri $uri</span><span class="pun">/</span><span class="pln"> </span><span class="pun">=</span><span class="lit">404</span><span class="pun">;</span></code></li>
 	<li class="L0"><code><span class="pun">}</span></code></li>
 	<li class="L1"><code><span class="pln">error_page </span><span class="lit">404</span><span class="pln"> </span><span class="pun">/</span><span class="lit">404.html</span><span class="pun">;</span></code></li>
 	<li class="L2"><code><span class="pln">location </span><span class="pun">=</span><span class="pln"> </span><span class="pun">/</span><span class="lit">50x</span><span class="pun">.</span><span class="pln">html </span><span class="pun">{</span></code></li>
 	<li class="L3"><code><span class="pln">root </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">www</span><span class="pun">/</span><span class="pln">hakase</span><span class="pun">;</span></code></li>
 	<li class="L4"><code><span class="pun">}</span></code></li>
 	<li class="L5"><code></code></li>
 	<li class="L6"><code><span class="pln">location </span><span class="pun">~</span><span class="pln"> \.php$ </span><span class="pun">{</span></code></li>
 	<li class="L7"><code><span class="pln">try_files $uri </span><span class="pun">=</span><span class="lit">404</span><span class="pun">;</span></code></li>
 	<li class="L8"><code><span class="pln">fastcgi_pass unix</span><span class="pun">:</span><span class="str">/var/</span><span class="pln">run</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">/</span><span class="pln">hhvm</span><span class="pun">.</span><span class="pln">sock</span><span class="pun">;</span></code></li>
 	<li class="L9"><code><span class="pln">fastcgi_index index</span><span class="pun">.</span><span class="pln">php</span><span class="pun">;</span></code></li>
 	<li class="L0"><code><span class="pln">fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name</span><span class="pun">;</span></code></li>
 	<li class="L1"><code><span class="kwd">include</span><span class="pln"> fastcgi_params</span><span class="pun">;</span></code></li>
 	<li class="L2"><code><span class="pun">}</span></code></li>
 	<li class="L3"><code><span class="pun">}</span></code></li>
</ol>
保存并退出。

在这个虚拟主机配置文件中，我们定义该域名的 Web 根目录为 <code>/var/www/hakase</code>。目前该目录还不存在，所有我们要创建它，并变更属主为 nginx 用户和组。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">mkdir</span><span class="pln"> </span><span class="pun">-</span><span class="pln">p </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">www</span><span class="pun">/</span><span class="pln">hakase</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">chown</span><span class="pln"> </span><span class="pun">-</span><span class="pln">R nginx</span><span class="pun">:</span><span class="pln">nginx </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">www</span><span class="pun">/</span><span class="pln">hakase</span></code></li>
</ol>
接下来，为该文件和目录配置 SELinux 上下文。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> semanage fcontext </span><span class="pun">-</span><span class="pln">a </span><span class="pun">-</span><span class="pln">t </span><span class="typ">httpd_config_t</span><span class="pln"> </span><span class="str">"/etc/nginx/conf.d(/.*)?"</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> restorecon </span><span class="pun">-</span><span class="typ">Rv</span><span class="pln"> </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">conf</span><span class="pun">.</span><span class="pln">d</span></code></li>
</ol>
最后，测试 nginx 配置文件以确保没有错误后，重启 nginx：
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> nginx </span><span class="pun">-</span><span class="pln">t</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">systemctl</span><span class="pln"> restart nginx</span></code></li>
</ol>
<h3 id="8">步骤 8 - 安装 WordPress</h3>
在步骤 5 的时候，我们已经为 WordPress 配置好了虚拟主机，现在只需要下载 WordPress 和使用我们在步骤 3 的时候创建的数据库和用户来编辑数据库配置就好了。

进入 Web 根目录 <code>/var/www/hakase</code> 并使用 Wget 命令下载 WordPress：
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">cd</span><span class="pun">&amp;</span><span class="pln">nbsp</span><span class="pun">;</span><span class="str">/var/</span><span class="pln">www</span><span class="pun">/</span><span class="pln">hakase</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">wget</span><span class="pln"> wordpress</span><span class="pun">.</span><span class="pln">org</span><span class="pun">/</span><span class="pln">latest</span><span class="pun">.</span><span class="kwd">tar</span><span class="pun">.</span><span class="pln">gz</span></code></li>
</ol>
解压 <code>latest.tar.gz</code> 并将 <code>wordpress</code> 文件夹中所有的文件和目录移动到当前目录：
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">tar</span><span class="pln"> </span><span class="pun">-</span><span class="pln">xzvf latest</span><span class="pun">.</span><span class="kwd">tar</span><span class="pun">.</span><span class="pln">gz</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">mv</span><span class="pln"> wordpress</span><span class="com">/* .</span></code></li>
</ol>
下一步，复制一份 <code>wp-config-sample.php</code> 并更名为 <code>wp-config.php</code>，然后使用 vim 进行编辑：
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">cp</span><span class="pln"> wp</span><span class="pun">-</span><span class="pln">config</span><span class="pun">-</span><span class="pln">sample</span><span class="pun">.</span><span class="pln">php wp</span><span class="pun">-</span><span class="pln">config</span><span class="pun">.</span><span class="pln">php</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> </span><span class="kwd">vim</span><span class="pln"> wp</span><span class="pun">-</span><span class="pln">config</span><span class="pun">.</span><span class="pln">php</span></code></li>
</ol>
将 <code>DB_NAME</code> 设置为 <code>wordpressdb</code>、<code>DB_USER</code> 设置为 <code>wpuser</code> 以及 <code>DB_PASSWORD</code> 设置为 <code>wpuser@</code>。
<ol>
 	<li class="L0"><code><span class="kwd">define</span><span class="pun">(</span><span class="str">'DB_NAME'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'wordpressdb'</span><span class="pun">);</span></code></li>
 	<li class="L1"><code><span class="kwd">define</span><span class="pun">(</span><span class="str">'DB_USER'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'wpuser'</span><span class="pun">);</span></code></li>
 	<li class="L2"><code><span class="kwd">define</span><span class="pun">(</span><span class="str">'DB_PASSWORD'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'wpuser@'</span><span class="pun">);</span></code></li>
 	<li class="L3"><code><span class="kwd">define</span><span class="pun">(</span><span class="str">'DB_HOST'</span><span class="pun">,</span><span class="pln"> </span><span class="str">'localhost'</span><span class="pun">);</span></code></li>
</ol>
保存并退出。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154056dppddtb9umud6mdq.png" alt="WordPress 配置" />

<em>WordPress 配置</em>

修改关于 WordPress 目录的 SELinux 上下文。
<ol>
 	<li class="L0"><code><span class="com">#</span><span class="pln"> semanage fcontext </span><span class="pun">-</span><span class="pln">a </span><span class="pun">-</span><span class="pln">t </span><span class="typ">httpd_sys_content_t</span><span class="pln"> </span><span class="str">"/var/www/hakase(/.*)?"</span></code></li>
 	<li class="L1"><code><span class="com">#</span><span class="pln"> restorecon </span><span class="pun">-</span><span class="typ">Rv</span><span class="pln"> </span><span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">www</span><span class="pun">/</span><span class="pln">hakase</span></code></li>
</ol>
现在打开 Web 浏览器，在地址栏输入你之前为 WordPress 设置的域名，我这里是 <code>natsume.co</code>。

选择语言并点击继续Continue。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154056j5hezz4rhi5cmnmp.png" alt="安装 Wordpress - 语言选择" />

<em>安装 Wordpress - 语言选择</em>

根据自身要求填写站点标题和描述并点击安装 WordpressInstall Wordpress"。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154057i41iv4kxfonxavfv.png" alt="安装 Wordpress - 配置管理员账号和站点标题" />

<em>安装 Wordpress - 配置管理员账号和站点标题</em>

耐心等待安装完成。你会见到如下页面，点击登录Log In来登录到管理面板。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154057bjxllsxeiuguuufo.png" alt="安装 Wordpress - 成功安装" />

<em>安装 Wordpress - 成功安装</em>

输入你设置的管理员用户账号和密码，在此点击登录Log In。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154057tolgpii9kxo3h8gs.png" alt="登录到 wordpress 管理面板" />

<em>登录到 wordpress 管理面板</em>

现在你已经登录到 WordPress 的管理面板了。

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154058d0kk61kzs9qakuy7.png" alt="Wordpress 管理面" />

<em>Wordpress 管理面</em>

Wordpress 的主页：

<img src="https://dn-linuxcn.qbox.me/data/attachment/album/201703/30/154058sfmt4mizjgmfaaiq.png" alt="Wordpress 默认主页" />

<em>Wordpress 默认主页</em>
<p data-spm-anchor-id="a2c4e.11153940.blogcont86279.i18.55592586z52BbJ">至此，我们已经在 CentOS 7 上通过 Nginx 和 HHVM 成功安装 Wordpress。</p>