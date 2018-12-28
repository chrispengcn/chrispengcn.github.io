---
ID: 1219
post_title: >
  Centos+Nginx+php+mysql+wordpress搭建自己的博客站点
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/18/centosnginxphpmysqlwordpress%e6%90%ad%e5%bb%ba%e8%87%aa%e5%b7%b1%e7%9a%84%e5%8d%9a%e5%ae%a2%e7%ab%99%e7%82%b9/'
published: true
post_date: 2018-05-18 13:18:59
---
<div><strong>Centos+Nginx+php+mysql+wordpress</strong><strong>搭建自己的博客站点</strong></div>
<div></div>
<div><strong>服务器环境要求</strong></div>
<div>§ Centos 6.0 或以上版本（由于我们的目标是半小时内搭建好，那就选简单yum安装）</div>
<div>§ <a href="http://note.youdao.com/share/iframe.html#MySQL" target="_blank" rel="noopener noreferrer">MySQL</a> 5.0或更新版本</div>
<div>§ Nginx 1.0或更新版本</div>
<div>§ <a href="http://note.youdao.com/share/iframe.html#PHP" target="_blank" rel="noopener noreferrer">PHP</a> 5.2.4或更新版本</div>
<div>§ WordPress 官网最新安装包（下载地址https://cn.wordpress.org/）</div>
<div>§</div>
<div><strong>安装软件</strong></div>
<div>l 安装Nginx</div>
<blockquote>
<div># yum install nginx</div>
<div># service nginx start</div></blockquote>
<div>然后打开你云机的ip直接访问，会直接出现如下界面，即安装成功。</div>
<div><img src="http://image.nuanchou.com/content/news/-465751370" alt="" /></div>
<div></div>
<div></div>
<div></div>
<div>l 安装mysql</div>
<blockquote>
<div># yum install mysql</div></blockquote>
<div>安装mysql 服务器端</div>
<div></div>
<blockquote>
<div># yum install mysql-server</div></blockquote>
<div>安装完成后启动mysql服务：</div>
<div></div>
<blockquote>
<div>service mysqld start</div></blockquote>
<div>给mysql创建一个root管理员：</div>
<div></div>
<blockquote>
<div># mysqladmin -u root password 123456</div></blockquote>
<div>用刚创建的帐号连接mysql：</div>
<div></div>
<blockquote>
<div># mysql -u root -p</div></blockquote>
<div>输入密码即可。。。</div>
<div></div>
<div>使用mysql工具链接云机器（如果链接有错误，请自行百度解决，这里不赘述），如图：</div>
<div><img src="http://image.nuanchou.com/content/news/-526970646" alt="" /></div>
<div></div>
<div></div>
<div>l 安装php环境</div>
<blockquote>
<div>yum install -y php php-mysql php-gd libjpeg* php-imap php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-mcrypt php-bcmath php-mhash libmcrypt libmcrypt-devel php-fpm</div></blockquote>
<div></div>
<div>查看php环境运行状态</div>
<div>[root@iZ2ze3h9658kjmu0leim67Z ~]# php -version</div>
<div>PHP 5.3.3 (cli) (built: Mar 22 2017 12:27:09)</div>
<div>Copyright (c) 1997-2010 The PHP Group</div>
<div>Zend Engine v2.3.0, Copyright (c) 1998-2010 Zend Technologies</div>
<div>[root@iZ2ze3h9658kjmu0leim67Z ~]#</div>
<div></div>
<div>l 安装WordPress</div>
<div>下载wordpress-4.8.1-zh_CN.tar.gz</div>
<div>#执行命令，解压</div>
<blockquote>
<div>tar -zxvf wordpress-4.8.1-zh_CN.tar.gz</div></blockquote>
<div>解压文件夹为wordpress，查看文件夹下全是php的文件，这里大家并不用深究，不用去了解php。</div>
<div><img src="http://image.nuanchou.com/content/news/1702937774" alt="" /></div>
<div></div>
<div>设置wp-config.php文件</div>
<div>vim wp-config.php 然后把mysql的数据库链接信息进行设置（记得在数据库新建一个库放wordpress需要的表，新建的库名就是你下wp-config.php文件配置的）</div>
<div><img src="http://image.nuanchou.com/content/news/-1908433954" alt="" /></div>
<div></div>
<div></div>
<div></div>
<div>l 安装完成，配置Nginx，进行外网访问，安装WordPress</div>
<div>当nginx不配置支持php模块时，当你访问php文件时候，浏览器默认下载php文件，而不是执行php，这样我就要配置我们的nginx，然后当用户访问php文件时，是执行php文件，而不是把php当文件下载。</div>
<blockquote>
<div>vim /etc/nginx/conf.d/default.conf</div></blockquote>
<div>修改配置如下：</div>
<blockquote>
<div>server {</div>
<div>listen 80;</div>
<div># listen [::]:80 default_server;</div>
<div>server_name <a href="https://blog.csdn.net/" target="_blank" rel="noopener noreferrer">www.xxx.com;</a> #这里是你自己的域名，如果没有，用本机ip</div>
<div>root /opt/boke; #这里是放wordpress的文件目录</div>
<div>index index.php index.html;</div>
<div></div>
<div># Load configuration files for the default server block.</div>
<div>include /etc/nginx/default.d/*.conf;</div>
<div></div>
<div></div>
<div>location / {</div>
<div>try_files $uri $uri/ /index.php?q=$uri&amp;$args;</div>
<div>}</div>
<div></div>
<div>location ~ \.php$ {</div>
<div>fastcgi_pass 127.0.0.1:9000;</div>
<div>fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;</div>
<div>include fastcgi_params;</div>
<div>}</div>
<div></div>
<div>error_page 404 /404.html;</div>
<div>location = /40x.html {</div>
<div>}</div>
<div></div>
<div>error_page 500 502 503 504 /50x.html;</div>
<div>location = /50x.html {</div>
<div>}</div>
<div></div>
<div>}</div></blockquote>
<div></div>
<div>此时重启nginx执行：</div>
<blockquote>
<div>service nginx restart</div></blockquote>
<div></div>
<div>此时访问</div>
<div><a href="http://luojie.shiziqiu.com/wordpress/wp-admin/install.php" target="_blank" rel="noopener noreferrer">http://www.xxx.com/wordpress/wp-admin/install.php</a></div>
<div>进行视图话安装，就可以访问你的博客了。</div>
<div><a href="https://blog.csdn.net/" target="_blank" rel="noopener noreferrer">www.xxx.com表示你自己的域名，没有域名的童鞋，请用你自己的ip</a></div>
<div></div>
<div>大家可以看看效果，以下是我的博客地址：</div>
<div></div>
<div><a href="https://www.yyenglish.cn">https://www.yyenglish.cn</a></div>
<div></div>