---
ID: 924
post_title: 'centos7 安装 magento 1+2  lamp 环境 完整流程'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/08/09/centos7-php-mariadb-magento2-setup/
published: true
post_date: 2017-08-09 14:24:43
---
<p style="padding-left: 30px;">centos7 安装 magento 1+2  lamp 环境 完整流程</p>
1, 安装 apache
<blockquote>yum install httpd
systemctl enable httpd</blockquote>
开启80 端口
<blockquote>firewall-cmd --zone=public --add-port=80/tcp --permanent</blockquote>
出现success表明添加成功
命令含义：
--zone #作用域
--add-port=80/tcp  #添加端口，格式为：端口/通讯协议
--permanent   #永久生效，没有此参数重启后失效

重启防火墙
<blockquote>systemctl restart firewalld.service</blockquote>
1、运行、停止、禁用firewalld
启动：# systemctl start  firewalld
查看状态：# systemctl status firewalld 或者 firewall-cmd --state
停止：# systemctl disable firewalld
禁用：# systemctl stop firewalld

2, 安装MariaDB
<blockquote>安装命令
yum -y install mariadb mariadb-server
安装完成MariaDB，首先启动MariaDB
systemctl start mariadb
设置开机启动
systemctl enable mariadb
接下来进行MariaDB的相关简单配置
mysql_secure_installation</blockquote>
3,安装 php5.6 on centos 7
<blockquote>yum -y update
yum -y install epel-release
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
wget https://centos7.iuscommunity.org/ius-release.rpm
rpm -Uvh ius-release*.rpm
yum -y update
yum -y install php56u php56u-opcache php56u-xml php56u-mcrypt php56u-gd php56u-devel php56u-mysql php56u-intl php56u-mbstring php56u-bcmath</blockquote>
4,  PHP写文件权限 selinux

查看了很多,什么chown修改,chmod 777修改,各种权限,都还是上传失败.
最后发现是SELinux在搞鬼
SELinux 更能遵从最小权限的理念。在缺省的 enforcing 情况下，一切均被拒绝，接着有一系列例外的政策来允许系统的每个元素（服务、程序、用户）运作时所需的访问权。当一项服务、程序或用户尝试访问或修改一个它不须用的文件或资源时，它的请求会遭拒绝，而这个行动会被记录下来
所以,我们可以在/etc/selinux/config修改SELinux配置,关掉它
但是,肯定还有别的方法
看到有段文字说所有进程及文件都拥有一个 SELinux 的安全性脉络,可以用ls -Z查看
-rwxr-xr-x. apache apache system_u:object_r:httpd_sys_content_t:s0 uploadFile.php
-
-rw-r--r--. apache apache unconfined_u:object_r:httpd_sys_content_t:s0 images
httpd_sys_content_t 这是apache的权限角色
在这篇文章里https://blog.lysender.com/2015/07/centos-7-selinux-php-apache-cannot-writeaccess-file-no-matter-what/ 看到

虽然英文一般,但是看到了关键字wirete to those path
http://newscentral.exsees.com/item/5b6cccab6ed05ce053bc09f70dc9386e-99c0b81ebd43e7b6848a00939f39b855 里面博主说
<blockquote>sudo  chcon -R -t httpd_sys_rw_content_t  /var/www/html</blockquote>
把我准备写入的文件夹的权限角色从httpd_sys_content_t
改成httpd_sys_rw_content_t
run~

就这样,看起来好像解决了
参考:
http://newscentral.exsees.com/item/5b6cccab6ed05ce053bc09f70dc9386e-99c0b81ebd43e7b6848a00939f39b855
https://blog.lysender.com/2015/07/centos-7-selinux-php-apache-cannot-writeaccess-file-no-matter-what/
有更好的方法,可以留言~