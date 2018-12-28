---
ID: 261
post_title: 'Xampp 安装 Xcache   {centos}'
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/09/01/xampp-%e5%ae%89%e8%a3%85-xcache-centos/'
published: true
post_date: 2013-09-01 08:21:40
---
<p>&nbsp; <p>本篇为转载，原文地址： <p><a href="http://www.shannap.com/%5C%22http://mrbignose.blogspot.com/2010/12/linux-xampp-xcache.html%5C%22">http://mrbignose.blogspot.com/2010/12/linux-xampp-xcache.html</a> <p>因為XAMPP的目錄結構不同，所以使用一般安裝方式會出現錯誤。 <p>1.要先安裝 XAMPP 的 開發套件(development package)：<br>網址：http://www.apachefriends.org/zh_tw/xampp-linux.html<br>下載完成後，只要輸入下列命令： [root@localhost ~]# tar xvfz xampp-linux-devel-1.7.3a.tar.gz -C /opt 這樣就完成開發套件的安裝了。 <p>2.下載 xCache：<br>網址：http://xcache.lighttpd.net/<br>下載完成後，輸入下列命令： [root@localhost ~]# tar xjvf xcache-1.3.1.tar.bz2<br>[root@localhost ~]# cd xcache-1.3.1 <p>3.執行phpize：<br>[root@localhost xcache-1.3.1]# /opt/lampp/bin/phpize <p>4.Configure the extension ：<br>[root@localhost xcache-1.3.1]# ./configure –enable-xcache –with-php-config=/opt/lampp/bin/php-config <p>5.執行 'make' 來取得編譯後的 xcache.so ：<br>[root@localhost xcache-1.3.1]# make<br>[root@localhost xcache-1.3.1]# make install <p>6.設定 php.ini<br>[root@localhost xcache-1.3.1]# cat xcache.ini &gt;&gt; /opt/lampp/etc/php.ini<br>[root@localhost ~]# vi /opt/lampp/etc/php.ini<br>找到以下內容並修改：<br>[xcache-common]<br>;; install as zend extension (recommended), normally "$extension_dir/xcache.so"<br>zend_extension = /opt/lampp/lib/php/extensions/no-debug-non-zts-20090626/xcache.so<br>[xcache.admin]<br>xcache.admin.user = "test"<br>xcache.admin.pass = "098f6bcd4621d373cade4e832627b4f6"<br>[xcache]<br>xcache.size = 64M<br>需注意的地方：<br>xcache.admin.pass 填入的是 MD5後的字串<br>xcache.size是 XCache 使用的記憶體量，預設是 0 (Off)，當然要打開，大小自定 <p>7.最後重啟 Apache：<br>[root@localhost ~]# /opt/lampp/lampp stopapache<br>[root@localhost ~]# /opt/lampp/lampp startapache<br>記得使用 phpinfo() 來檢查 extension 是否安裝成功。</p>