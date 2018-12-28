---
ID: 977
post_title: centos7 mariadb 升级到 10.0
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/08/16/centos7-mariadb-10-0-upgrade/
published: true
post_date: 2017-08-16 14:57:52
---
列出所有被安装的rpm package
<pre class="prettyprint"><code class="hljs vala has-numbering"><span class="hljs-preprocessor"># rpm -qa | grep mariadb</span></code></pre>
&nbsp;
<pre class="prettyprint"><code class="hljs avrasm has-numbering">mariadb-libs-<span class="hljs-number">5.5</span><span class="hljs-number">.50</span>-<span class="hljs-number">1.</span>el7_2<span class="hljs-preprocessor">.x</span>86_64
mariadb-<span class="hljs-number">5.5</span><span class="hljs-number">.50</span>-<span class="hljs-number">1.</span>el7_2<span class="hljs-preprocessor">.x</span>86_64
mariadb-server-<span class="hljs-number">5.5</span><span class="hljs-number">.50</span>-<span class="hljs-number">1.</span>el7_2<span class="hljs-preprocessor">.x</span>86_64</code></pre>
&nbsp;

卸载
<pre class="prettyprint"><code class="hljs vala has-numbering"><span class="hljs-preprocessor"># rpm -e mariadb-libs-5.5.50-1.el7_2.x86_64</span></code></pre>
&nbsp;

此时报错：
<pre class="prettyprint"><code class="hljs vbnet has-numbering"><span class="hljs-keyword">error</span>: Failed dependencies:
    libmysqlclient.so<span class="hljs-number">.18</span>()(<span class="hljs-number">64</span>bit) <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) postfix-<span class="hljs-number">2</span>:<span class="hljs-number">2.10</span><span class="hljs-number">.1</span>-<span class="hljs-number">6.</span>el7.x86_64
    libmysqlclient.so<span class="hljs-number">.18</span>()(<span class="hljs-number">64</span>bit) <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) perl-DBD-MySQL-<span class="hljs-number">4.023</span>-<span class="hljs-number">5.</span>el7.x86_64
    libmysqlclient.so<span class="hljs-number">.18</span>()(<span class="hljs-number">64</span>bit) <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) php-mysql-<span class="hljs-number">5.4</span><span class="hljs-number">.16</span>-<span class="hljs-number">36.3</span>.el7_2.x86_64
    libmysqlclient.so<span class="hljs-number">.18</span>(libmysqlclient_18)(<span class="hljs-number">64</span>bit) <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) postfix-<span class="hljs-number">2</span>:<span class="hljs-number">2.10</span><span class="hljs-number">.1</span>-<span class="hljs-number">6.</span>el7.x86_64
    libmysqlclient.so<span class="hljs-number">.18</span>(libmysqlclient_18)(<span class="hljs-number">64</span>bit) <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) perl-DBD-MySQL-<span class="hljs-number">4.023</span>-<span class="hljs-number">5.</span>el7.x86_64
    libmysqlclient.so<span class="hljs-number">.18</span>(libmysqlclient_18)(<span class="hljs-number">64</span>bit) <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) php-mysql-<span class="hljs-number">5.4</span><span class="hljs-number">.16</span>-<span class="hljs-number">36.3</span>.el7_2.x86_64
    mariadb-libs(x86-<span class="hljs-number">64</span>) = <span class="hljs-number">1</span>:<span class="hljs-number">5.5</span><span class="hljs-number">.50</span>-<span class="hljs-number">1.</span>el7_2 <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) mariadb-<span class="hljs-number">1</span>:<span class="hljs-number">5.5</span><span class="hljs-number">.50</span>-<span class="hljs-number">1.</span>el7_2.x86_64
    mariadb-libs(x86-<span class="hljs-number">64</span>) = <span class="hljs-number">1</span>:<span class="hljs-number">5.5</span><span class="hljs-number">.50</span>-<span class="hljs-number">1.</span>el7_2 <span class="hljs-keyword">is</span> needed <span class="hljs-keyword">by</span> (installed) mariadb-server-<span class="hljs-number">1</span>:<span class="hljs-number">5.5</span><span class="hljs-number">.50</span>-<span class="hljs-number">1.</span>el7_2.x86_64</code></pre>
&nbsp;

强制卸载，因为没有–nodeps
<pre class="prettyprint"><code class="hljs vala has-numbering"><span class="hljs-preprocessor"># rpm -e --nodeps mariadb-libs-5.5.50-1.el7_2.x86_64</span>

<span class="hljs-preprocessor"># rpm -e --nodeps mariadb-5.5.50-1.el7_2.x86_64</span>

<span class="hljs-preprocessor"># rpm -e --nodeps mariadb-server-5.5.50-1.el7_2.x86_64 </span></code></pre>
&nbsp;

然后可以安装<a class="replace_word" title="MySQL知识库" href="http://lib.csdn.net/base/mysql" target="_blank" rel="noopener noreferrer">mariadb 10</a>了。

<strong>MaraiDB安装配置</strong>

<hr />

MariaDB的安装可以直接添加更新官方的源，到/etc/yum.repos.d下创建MariaDB.repo
<blockquote>[mariadb]
name = MariaDB
baseurl = <a href="http://yum.mariadb.org/10.1/centos7-amd64" target="_blank" rel="nofollow noopener noreferrer">http://yum.mariadb.org/10.1/centos7-amd64</a>
gpgkey=<a href="https://yum.mariadb.org/RPM-GPG-KEY-MariaDB" target="_blank" rel="nofollow noopener noreferrer">https://yum.mariadb.org/RPM-GPG-KEY-MariaDB</a>
gpgcheck=1</blockquote>
&nbsp;

保存后更新源
<blockquote>yum update</blockquote>
安装最新MariaDB
<blockquote>yum install mariadb mariadb-server</blockquote>
添加开机启动
<blockquote>systemctl enable mariadb</blockquote>
开启
<blockquote>systemctl start mariadb</blockquote>
在服务开启的状态下，设置最新密码
<blockquote>mysql_secure_installation

&nbsp;
<pre class="prettyprint"><code class="hljs avrasm has-numbering">vim /etc/yum<span class="hljs-preprocessor">.repos</span><span class="hljs-preprocessor">.d</span>/mariadb<span class="hljs-preprocessor">.repo</span></code></pre>
</blockquote>
写入以下内容：
国内源
<blockquote>
<pre class="prettyprint"><code class="hljs avrasm has-numbering"><span class="hljs-preprocessor"># MariaDB 10.2 CentOS repository list - created 2017-07-03 06:59 UTC</span>
<span class="hljs-preprocessor"># http://downloads.mariadb.org/mariadb/repositories/</span>
[mariadb]
name = MariaDB
baseurl = https://mirrors<span class="hljs-preprocessor">.ustc</span><span class="hljs-preprocessor">.edu</span><span class="hljs-preprocessor">.cn</span>/mariadb/yum/<span class="hljs-number">10.2</span>/centos7-amd64
gpgkey=https://mirrors<span class="hljs-preprocessor">.ustc</span><span class="hljs-preprocessor">.edu</span><span class="hljs-preprocessor">.cn</span>/mariadb/yum/RPM-GPG-KEY-MariaDB
gpgcheck=<span class="hljs-number">1</span>


</code></pre>
</blockquote>
参考链接：<a href="http://mirrors.ustc.edu.cn/help/mariadb.html" target="_blank" rel="noopener noreferrer">http://mirrors.ustc.edu.cn/help/mariadb.html</a>

以上是中国科学技术大学的 mariadb yum源，下载起来非常快，如果直接官网下载，非常的慢，非常的慢，因为这软件加依赖包，都有167M !!!