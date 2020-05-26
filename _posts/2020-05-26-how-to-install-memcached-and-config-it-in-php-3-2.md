---
ID: 3601
post_title: >
  how to install memcached and config it
  in php 3.2
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2020/05/26/how-to-install-memcached-and-config-it-in-php-3-2/
published: true
post_date: 2020-05-26 12:50:35
---
In this tutorial we’ll install Memcached and configure it in phpbb 3.2, which not only lower the server load also make you phpbb forum super fast I meant lightening fast. Memcached is used to lower the database load, it will cache the frequently used MySQL query and save as key cache on RAM – which will save the precious juice for another tasks.

For this tutorial full <strong>root access</strong> is required also it will work on <strong>shared hosting (cpanel)</strong> if <strong>memcached and php memcached</strong> are installed and enabled for your account.

<strong>Lets get started :-</strong>
<h1>Installing Memcached and PHP Memcached :</h1>
<h3><strong>Centos/RHEL 6/7 :</strong></h3>
<strong>First install Remi repo :-</strong>

<strong>CentOS 7</strong>
<pre>wget http://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm
</pre>
<strong>CentOS 6</strong>
<pre>wget http://rpms.remirepo.net/enterprise/remi-release-6.rpm
rpm -Uvh remi-release-6.rpm
</pre>
<h2>To install Memcached :</h2>
<pre>yum install memcached
</pre>
Start <strong>memcached</strong> Service and enable on boot :
<pre>service memcached start

#Centos/RHEL 7
systemctl enable memcached

#Centos/RHEL 6
chkconfig memcached on
</pre>
<h3><strong>Ubuntu 16.04/18.04 LTS and other forks :</strong></h3>
<pre>apt-get update
apt-get install memcached
</pre>
<h1>Installing PHP Memcached :</h1>
Now We’ll Build and install <strong>PHP MEMCACHED on php 7.xx (official) :
</strong>
<pre>cd /root
git clone https://github.com/php-memcached-dev/php-memcached.git
cd php-memcached
git checkout php7
phpize
./configure
make
make install
</pre>
If you get <strong>SASL error</strong> then we need disable sasl flag added to the config:
<pre>cd /root
git clone https://github.com/php-memcached-dev/php-memcached.git
cd php-memcached
git checkout php7
phpize
./configure --disable-memcached-sasl
make
make install
</pre>
Now you need to add this line to your <strong>php.ini  to enable this php extension  :
</strong>
<pre>extension=memcached.so
</pre>
Then at last <strong>restart apache/php-fpm</strong> service and done, you can then check phpinfo and search “memcached” if it is there then you’ve successfully installed php-memcached and memcached.
<h1>Configuring phpbb 3.2 with Memcached :</h1>
<strong>Edit config.php and find this line :</strong>
<pre>$acm_type = 'phpbb\\cache\\driver\\file';
</pre>
<strong>Change it TO :</strong>
<pre>$acm_type = 'phpbb\\cache\\driver\\memcached';
</pre>
save and exit, you’ve successfully enabled memcached for phpbb

also don’t forget to purge the forum cache.