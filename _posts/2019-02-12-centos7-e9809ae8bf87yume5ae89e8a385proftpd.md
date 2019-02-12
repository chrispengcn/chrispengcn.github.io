---
ID: 2408
post_title: centos7 通过yum安装proftpd
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/02/12/centos7-%e9%80%9a%e8%bf%87yum%e5%ae%89%e8%a3%85proftpd/'
published: true
post_date: 2019-02-12 18:39:29
---
<h1 class="title">centos7 通过yum安装proftpd</h1>
&nbsp;
<pre>#1.安装epel源
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

#2.安装proftpd
yum install -y proftpd openssl proftpd-utils

#3.启动proftpd
systemctl start proftpd.service
systemctl enable proftpd.service

#4.创建ftp登录用户
#a.创建ftp组：
groupadd ftpgroup

#b.创建ftp用户，并关联ftp目录：
   useradd -G ftpgroup ftptest -s /sbin/nologin -d /home/ftptest/

#c.设置ftptest用户密码：
passwd ftptest

#5.给目录设置权限，
chmod -R 777 /home/ftptest/

#6.完毕，使用ftp客户端测试proftpd是否正常上传下载文件。


</pre>
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">entOS7安装和配置VSFTP</h1>
</div>
<div class="article-info-box">
<div class="operating"></div>
</div>
</div>
</div>
<article class="baidu_pl">
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div class="article-copyright"></div>
<div id="content_views" class="markdown_views">
<h1 id="1-安装vsftpd"><a name="t0"></a>1. 安装vsftpd</h1>
<pre class="prettyprint"><code class="language-shell hljs vala has-numbering"><span class="hljs-preprocessor">#安装vsftpd</span>
yum install -y vsftpd
<span class="hljs-preprocessor">#设置开机启动</span>
systemctl enable vsftpd.service 
<span class="hljs-preprocessor"># 重启</span>
service vsftpd restart
<span class="hljs-preprocessor"># 查看vsftpd服务的状态</span>
systemctl status vsftpd.service
</code></pre>

<hr />

<h1 id="2-配置vsftpdconf"><a name="t1"></a>2. 配置vsftpd.conf</h1>
<pre class="prettyprint"><code class="language-shell hljs makefile has-numbering"><span class="hljs-comment">#备份配置文件 </span>
cp /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak

<span class="hljs-comment">#执行以下命令</span>
sed -i "s/anonymous_enable=YES/anonymous_enable=NO/g" '/etc/vsftpd/vsftpd.conf'

sed -i "s/<span class="hljs-comment">#anon_upload_enable=YES/anon_upload_enable=NO/g" '/etc/vsftpd/vsftpd.conf'</span>

sed -i "s/<span class="hljs-comment">#anon_mkdir_write_enable=YES/anon_mkdir_write_enable=YES/g" '/etc/vsftpd/vsftpd.conf'</span>

sed -i "s/<span class="hljs-comment">#chown_uploads=YES/chown_uploads=NO/g" '/etc/vsftpd/vsftpd.conf'</span>

sed -i "s/<span class="hljs-comment">#async_abor_enable=YES/async_abor_enable=YES/g" '/etc/vsftpd/vsftpd.conf'</span>

sed -i "s/<span class="hljs-comment">#ascii_upload_enable=YES/ascii_upload_enable=YES/g" '/etc/vsftpd/vsftpd.conf'</span>

sed -i "s/<span class="hljs-comment">#ascii_download_enable=YES/ascii_download_enable=YES/g" '/etc/vsftpd/vsftpd.conf'</span>

sed -i "s/<span class="hljs-comment">#ftpd_banner=Welcome to blah FTP service./ftpd_banner=Welcome to FTP service./g" '/etc/vsftpd/vsftpd.conf'</span>

<span class="hljs-comment">#添加下列内容到vsftpd.conf末尾</span>
<span class="hljs-constant">use_localtime</span>=YES
<span class="hljs-constant">listen_port</span>=21
<span class="hljs-constant">chroot_local_user</span>=YES
<span class="hljs-constant">idle_session_timeout</span>=300
<span class="hljs-constant">guest_enable</span>=YES
<span class="hljs-constant">guest_username</span>=vsftpd
<span class="hljs-constant">user_config_dir</span>=/etc/vsftpd/vconf
<span class="hljs-constant">data_connection_timeout</span>=1
<span class="hljs-constant">virtual_use_local_privs</span>=YES
<span class="hljs-constant">pasv_min_port</span>=10060
<span class="hljs-constant">pasv_max_port</span>=10090
<span class="hljs-constant">accept_timeout</span>=5
<span class="hljs-constant">connect_timeout</span>=1
</code></pre>

<hr />

<h1 id="3-建立用户文件"><a name="t2"></a>3. 建立用户文件</h1>
<pre class="prettyprint"><code class="hljs vala has-numbering"><span class="hljs-preprocessor">#第一行用户名，第二行密码，不能使用root为用户名</span>
vi /etc/vsftpd/virtusers
chris
<span class="hljs-number">123456</span>
chang
<span class="hljs-number">123456</span></code></pre>

<hr />

<h1 id="4-生成用户数据文件"><a name="t3"></a>4. 生成用户数据文件</h1>
<pre class="prettyprint"><code class="hljs bash has-numbering">db_load -T -t <span class="hljs-built_in">hash</span> <span class="hljs-operator">-f</span> /etc/vsftpd/virtusers /etc/vsftpd/virtusers.db

<span class="hljs-comment">#设定PAM验证文件，并指定对虚拟用户数据库文件进行读取</span>

chmod <span class="hljs-number">600</span> /etc/vsftpd/virtusers.db </code></pre>

<hr />

<h1 id="5-修改etcpamdvsftpd文件"><a name="t4"></a>5. 修改/etc/pam.d/vsftpd文件</h1>
<pre class="prettyprint"><code class="hljs avrasm has-numbering"><span class="hljs-preprocessor"># 修改前先备份 </span>

<span class="hljs-keyword">cp</span> /etc/pam<span class="hljs-preprocessor">.d</span>/vsftpd /etc/pam<span class="hljs-preprocessor">.d</span>/vsftpd<span class="hljs-preprocessor">.bak</span>

<span class="hljs-preprocessor"># 将auth及account的所有配置行均注释掉</span>
vi /etc/pam<span class="hljs-preprocessor">.d</span>/vsftpd

auth sufficient /lib64/security/pam_userdb<span class="hljs-preprocessor">.so</span> db=/etc/vsftpd/virtusers

account sufficient /lib64/security/pam_userdb<span class="hljs-preprocessor">.so</span> db=/etc/vsftpd/virtusers

<span class="hljs-preprocessor"># 如果系统为32位，上面改为lib</span></code></pre>

<hr />

<h1 id="6-新建系统用户vsftpd用户目录为homevsftpd"><a name="t5"></a>6. 新建系统用户vsftpd，用户目录为/home/vsftpd</h1>
<pre class="prettyprint"><code class="hljs bash has-numbering"><span class="hljs-comment">#用户登录终端设为/bin/false(即：使之不能登录系统)</span>
useradd vsftpd <span class="hljs-operator">-d</span> /home/vsftpd <span class="hljs-operator">-s</span> /bin/<span class="hljs-literal">false</span>
chown -R vsftpd:vsftpd /home/vsftpd</code></pre>

<hr />

<h1 id="7建立虚拟用户个人配置文件"><a name="t6"></a>7.建立虚拟用户个人配置文件</h1>
<pre class="prettyprint"><code class="hljs makefile has-numbering">mkdir /etc/vsftpd/vconf
cd /etc/vsftpd/vconf

<span class="hljs-comment">#这里建立两个虚拟用户配合文件</span>
touch chris chang

<span class="hljs-comment">#建立用户根目录</span>
mkdir -p /home/vsftpd/chris/

<span class="hljs-comment">#编辑chris用户配置文件，内容如下，其他用户类似</span>
vi chris

<span class="hljs-constant">local_root</span>=/home/vsftpd/chris/
<span class="hljs-constant">write_enable</span>=YES
<span class="hljs-constant">anon_world_readable_only</span>=NO
<span class="hljs-constant">anon_upload_enable</span>=YES
<span class="hljs-constant">anon_mkdir_write_enable</span>=YES
<span class="hljs-constant">anon_other_write_enable</span>=YES

</code></pre>

<hr />

<h1 id="8-防火墙设置"><a name="t7"></a>8. 防火墙设置</h1>
<pre class="prettyprint"><code class="hljs perl has-numbering">vi /etc/sysconfig/iptables
<span class="hljs-comment">#编辑iptables文件，添加如下内容，开启21端口</span>
-A INPUT -<span class="hljs-keyword">m</span> <span class="hljs-keyword">state</span> --<span class="hljs-keyword">state</span> NEW -<span class="hljs-keyword">m</span> tcp -p tcp --dport <span class="hljs-number">21</span> -j ACCEPT
</code></pre>
<h1 id="9-重启vsftpd服务器"><a name="t8"></a>9. 重启vsftpd<a href="https://www.baidu.com/s?wd=%E6%9C%8D%E5%8A%A1%E5%99%A8&amp;tn=24004469_oem_dg&amp;rsv_dl=gh_pl_sl_csd" target="_blank" rel="noopener">服务器</a></h1>
<pre class="prettyprint"><code class="hljs  has-numbering">service vsftpd restart</code></pre>

<hr />

<h1 id="10-使用xftp等软件连接测试"><a name="t9"></a>10. 使用<a href="https://www.baidu.com/s?wd=xftp&amp;tn=24004469_oem_dg&amp;rsv_dl=gh_pl_sl_csd" target="_blank" rel="noopener">xftp</a>等软件连接测试</h1>
</div>
</div>
</article>