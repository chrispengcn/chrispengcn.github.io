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
<div class="google-auto-placed ap_container"><ins class="adsbygoogle adsbygoogle-noablate" data-ad-format="auto" data-ad-client="ca-pub-8448624395816410" data-adsbygoogle-status="done"><ins id="aswift_5_expand"><ins id="aswift_5_anchor"><iframe id="aswift_5" src="https://googleads.g.doubleclick.net/pagead/ads?client=ca-pub-8448624395816410&amp;output=html&amp;h=280&amp;adk=1169571997&amp;adf=1912893808&amp;w=780&amp;fwrn=4&amp;fwrnh=100&amp;lmt=1590461218&amp;num_ads=1&amp;rafmt=1&amp;armr=3&amp;sem=mc&amp;pwprc=8537351749&amp;psa=0&amp;guci=2.2.0.0.2.2.0.0&amp;ad_type=text_image&amp;format=780x280&amp;url=https%3A%2F%2Fwww.mysterydata.com%2Fhow-to-configure-and-enable-memcached-on-phpbb-3-2-with-php-7-xx%2F&amp;flash=0&amp;fwr=0&amp;pra=3&amp;rh=195&amp;rw=779&amp;rpe=1&amp;resp_fmts=3&amp;wgl=1&amp;fa=27&amp;adsid=ChAI8Nmt9gUQqt2apOSi7pkZEjkAPE1vk2fkCCyVG1AWShdpirBSFaQkO2BuyIu50w-aQnYVYhPSP9U7fkgS8JOa2llAoG68bdpVrqw&amp;dt=1590461218827&amp;bpp=2&amp;bdt=3188&amp;idt=2&amp;shv=r20200519&amp;cbv=r20190131&amp;ptt=9&amp;saldr=aa&amp;abxe=1&amp;cookie=ID%3D67b537bbb16ede05%3AT%3D1590461224%3AS%3DALNI_MbD9zajew0qGERmeN2pml-uugNNWQ&amp;crv=1&amp;prev_fmts=780x280%2C780x280%2C340x280%2C0x0&amp;nras=2&amp;correlator=1714316941049&amp;frm=20&amp;pv=1&amp;ga_vid=936552902.1590461218&amp;ga_sid=1590461218&amp;ga_hid=50003347&amp;ga_fc=0&amp;iag=0&amp;icsg=2410037951397884&amp;dssz=45&amp;mdo=0&amp;mso=0&amp;u_tz=480&amp;u_his=4&amp;u_java=0&amp;u_h=1080&amp;u_w=1920&amp;u_ah=1040&amp;u_aw=1920&amp;u_cd=24&amp;u_nplug=3&amp;u_nmime=4&amp;adx=367&amp;ady=2013&amp;biw=1903&amp;bih=937&amp;scr_x=0&amp;scr_y=0&amp;eid=21066085%2C410075105&amp;oid=3&amp;pvsid=3398830779361518&amp;pem=222&amp;ref=https%3A%2F%2Fwww.google.com%2F&amp;rx=0&amp;eae=0&amp;fc=384&amp;brdim=0%2C0%2C0%2C0%2C1920%2C0%2C1920%2C1040%2C1920%2C937&amp;vis=1&amp;rsz=%7C%7Cs%7C&amp;abl=NS&amp;fu=8344&amp;bc=31&amp;jar=2020-05-26-02&amp;ifi=5&amp;uci=a!5&amp;btvi=2&amp;fsb=1&amp;xpc=nGVhBqGINo&amp;p=https%3A//www.mysterydata.com&amp;dtd=51" name="aswift_5" width="780" height="280" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" sandbox="allow-forms allow-pointer-lock allow-popups allow-popups-to-escape-sandbox allow-same-origin allow-scripts allow-top-navigation-by-user-activation" allowfullscreen="allowfullscreen" data-google-container-id="a!5" data-google-query-id="CLXl_-LB0OkCFQFzYAodLGsFPQ" data-load-complete="true" data-mce-fragment="1"></iframe></ins></ins></ins></div>
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