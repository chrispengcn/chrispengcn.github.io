---
ID: 471
post_title: >
  VPS
  下部署Ubuntu搭建WordPress完整环境与ftp配置详解
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2016/10/05/vps-%e4%b8%8b%e9%83%a8%e7%bd%b2ubuntu%e6%90%ad%e5%bb%bawordpress%e5%ae%8c%e6%95%b4%e7%8e%af%e5%a2%83%e4%b8%8eftp%e9%85%8d%e7%bd%ae%e8%af%a6%e8%a7%a3/'
published: true
post_date: 2016-10-05 18:44:25
---
<div id="nei" class="article_body">
<div>

putty的使用就不解释了。

Ubuntu下搭建wordpress环境：

1.安装apache2
<pre class="sql"><code>su<span class="operator"><span class="keyword">do</span> apt-<span class="keyword">get</span> install apache2</span></code></pre>
重启apache2
<pre class="sql"><code>su<span class="operator"><span class="keyword">do</span> /etc/init.d/apache2 restart</span></code></pre>
2.安装php
<pre class="prettyprint cs"><code>sudo apt-<span class="keyword">get</span> install php5 <span class="comment">//安装PHP5</span>
sudo apt-<span class="keyword">get</span> install libapache2-mod-php5 <span class="comment">//配置APACHE+PHP</span>
sudo /etc/init.d/apache2 restart <span class="comment">//重启apache</span></code></pre>
3.安装mysql
<pre class="sql"><code>su<span class="operator"><span class="keyword">do</span> apt-<span class="keyword">get</span> install mysql-server</span></code></pre>
4.让apache、php支持mysql
<pre class="prettyprint sql"><code>su<span class="operator"><span class="keyword">do</span> apt-<span class="keyword">get</span> install libapache2-mod-auth-mysql
sudo apt-<span class="keyword">get</span> install php5-mysql
sudo /etc/init.d/apache2 restart</span></code></pre>
至此apache2+php5+mysql5环境建好

5.安装phpmyadmin
<pre class="sql"><code>su<span class="operator"><span class="keyword">do</span> apt-<span class="keyword">get</span> install phpmyadmin</span></code></pre>
此时phpmyadmin被安装到/usr/share/phpmyadmin

需要去/var/www/html做一个phpmyadmin的超链接
<pre class="sql"><code>su<span class="operator"><span class="keyword">do</span> ln -s /usr/share/phpmyadmin</span></code></pre>
6.建数据库，可以用phpmyadmin

启动 sudo start mysql

是否开启 pgrep mysqld

查看 show databases;

进入 mysql -u root -p

创建 create database name

删除 drop database name

7.解压wordpress
<pre class="prettyprint sql"><code>su<span class="operator"><span class="keyword">do</span> tar -zxvf wordpress-<span class="number">3.8</span>-zh_CN.tar.gz</span></code></pre>
8.移动
<pre class="ruby"><code>sudo cp -a .<span class="regexp">/wordpress /var</span><span class="regexp">/www</span></code></pre>
修改文件权限，在某个目录下，可以把当前目录与子目录文件全部改成777权限：

chmod 777 -R *

修改某一个文件的权限：

chmod 644 name

wordpress无更新下载权限：

ps -aux 可以查看到apache2的用户是www-data

(centos 下 httpd 默认用户为 apache)

因此 sudo chown -R www-data /var/www/html/你的博客位置

架设FTP：

安装 sudo apt-get install vsftpd

/etc/vsftpd.conf 中修改

listen=YES

anonymous_enable=NO #一定要是no，然后别人就不能用ftp匿名账户登录了#

lacal_enable=YES

write_enable=YES

dirmessage_enable=YES

use_localtime=YES

xferlog_enable=YES

connect_from_port_20=YES

ascii_upload_enable=YES

ascii_download_enable=YES

chroot_local_user=YES

chroot_list_enable=YES

chroot_list_file=/etc/vsftpd/chroot_list

secure_chroot_dir=/var/run/vsftpd/empty

pam_service_name=vsftpd

ras_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem

ras_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key

pasv_enable=YES

pasv_min_port=1024

pasv_max_port=1048

pasv_addr_resolve=YES

pasv_address=”PUBLIC DNS”

userlist_enable=YES

userlist_deny=NO

userlist_file=/etc/vsftpd/user_list

local_root=/var/www

chroot_local_user=YES

anon_root=/var/www <strong>此三行是目录修改</strong>

anon_mkdir_write_enable=YES

anon_other_write_enable=YES

anon_upload_enable=YES

同时需要在/etc中建立一个vsftpd文件夹

文件夹中创建两个文件chroot_list和user_list，

其中chroot_list不需要填写内容，

在user_list里写上你在下面步骤中创建的user名字。

创建ftp用户(此步骤很重要)：

sudo useradd -s /sbin/nologin -d /var/www -g ftp user  #user改成你想要的名字#

sudo passwd user #上面你写的名字，回车后输入两次密码#

sudo chmod 755 /var/www #修改www/文件夹权限为755#

重启vsftpd：

service vsftpd restart

如果你发现做完上述步骤FTP登陆不了，去删除/etc/pam.d/vsftpd这个文件，重启服务

开启端口：

20-21

1024-1048

22

80

443

3306

3389

绑定域名，去/etc/apache2/sites-available中修改000-default.conf

新增：

&lt;VirtualHost 主机的地址或者DNS&gt;

ServerName 域名或者子域名

DocumentRoot 具体路径如/var/www/html/

完了之后重启apache：

sudo /etc/init.d/apache2 restart

</div>
&nbsp;

</div>