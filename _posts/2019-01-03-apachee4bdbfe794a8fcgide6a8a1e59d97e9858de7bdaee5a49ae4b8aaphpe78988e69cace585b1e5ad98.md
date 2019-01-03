---
ID: 1834
post_title: >
  Apache使用fcgid模块配置多个PHP版本共存
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/01/03/apache%e4%bd%bf%e7%94%a8fcgid%e6%a8%a1%e5%9d%97%e9%85%8d%e7%bd%ae%e5%a4%9a%e4%b8%aaphp%e7%89%88%e6%9c%ac%e5%85%b1%e5%ad%98/'
published: true
post_date: 2019-01-03 18:29:38
---
因为涉及多个时期开发的项目维护，每个项目使用的PHP版本都不同，想要在本地开发并调试就非常麻烦了，必须得想办法同时使用多个PHP版本才行

于是总结了下面方法做个记录，apache 用的不多，只是本地开发测试环境用apache感觉简单方便点儿。
如果用 Nginx 来配置多个PHP版本的话应该会更简单了吧，修改下PHP的listen端口就好了。

一、根据 apache 版本下载对应的 mod_fcgid-2.3.9-win64-VC14.zip
https://www.apachelounge.com/download/
<pre>二、修改 httpd.conf 末尾 添加

LoadModule fcgid_module modules/mod_fcgid.so
AddHandler fcgid-script .fcgi .php
FcgidInitialEnv PHP_FCGI_MAX_REQUESTS 1000
FcgidMaxRequestsPerProcess 1000
FcgidMaxProcesses 15
FcgidIOTimeout 120
FcgidIdleTimeout 120
AddType application/x-httpd-php .php
# 全局默认使用的PHP版本配置
FcgidInitialEnv PHPRC "C:/ProgramFiles(x86)/php5.6"
FcgidWrapper "C:/ProgramFiles(x86)/php5.6/php-cgi.exe" .php
# 上传文件的最大尺寸 100MB
FcgidMaxRequestLen 104857600

注意了，这里的 FcgidMaxRequestLen 很重要也很坑爹。
当你上传文件的时候发现总是出现500错误，而程序其实没有错误。 
实际上是因为上传内容体积过大，即便修改了php.ini 中配置的上传参数也没有用的，必须修改这里才行，nginx下也会有类似的问题</pre>
<pre>三、对需要不同 PHP 版本的设置 httpd-vhosts.conf 添加下面代码
FcgidInitialEnv PHPRC "C:/ProgramFiles(x86)/php7.0"
FcgidWrapper "C:/ProgramFiles(x86)/php7.0/php-cgi.exe" .php

注意：FcgidWrapper 中的路径使不能存在 空格 的，否则无法启动 Apache 了</pre>
因为历史原因我的软件都给装在了这个Program Files (x86)文件夹下面，又懒得换路径，为了兼容，我这里创建了文件夹的符号链接

命令格式：

mklink /d "新生成的符号链接文件夹名称和绝对路径" "源文件夹绝对路径"
1
举个栗子：
DOS 命令行模式下输入

mklink /d "c:\abc" "c:\a b c"


修改 httpd-vhosts.conf 配置后的样子：
<pre>&lt;VirtualHost *:80&gt;
DocumentRoot "E:\Website\domain.com\www"
ServerName www.domain.com
ServerAlias *.domain.com
FcgidInitialEnv PHPRC "C:/ProgramFiles(x86)/php7.0"
FcgidWrapper "C:/ProgramFiles(x86)/php7.0/php-cgi.exe" .php
&lt;/VirtualHost&gt;

同样的，如果你还需要其他版本则直接下载对应版本的PHP后解压，然后指向那个目录就行了</pre>
四、.htaccess 的重定向配置
以前是这样用的

RewriteRule ^(.*)$ index.php/$1 [QSA,PT,L]

会出现错误，无法重定向到 index.php

No input file specified.

修改成下面这样就好了

RewriteRule ^(.*)$ index.php [L,E=PATH_INFO:$1]

配置完成。。。

再补上一个多版本的测试结果吧。
我测试了三个版本，
php5.6(全局默认使用的版本), php7.0，php7.1

httpd-vhosts.conf 配置如下：
<pre>Listen 81
Listen 82
Listen 83

&lt;VirtualHost *:81&gt;
DocumentRoot "E:\Website\local81"
ServerName 127.0.0.1
&lt;/VirtualHost&gt;
&lt;VirtualHost *:82&gt;
DocumentRoot "E:\Website\local82"
ServerName 127.0.0.1
FcgidInitialEnv PHPRC "C:/ProgramFiles(x86)/php7.0"
FcgidWrapper "C:/ProgramFiles(x86)/php7.0/php-cgi.exe" .php
&lt;/VirtualHost&gt;
&lt;VirtualHost *:83&gt;
DocumentRoot "E:\Website\local83"
ServerName 127.0.0.1
FcgidInitialEnv PHPRC "C:/ProgramFiles(x86)/php7.1"
FcgidWrapper "C:/ProgramFiles(x86)/php7.1/php-cgi.exe" .php
&lt;/VirtualHost&gt;

</pre>
重启Apache后打开三个端口输出 phpinfo() 结果如下：

可以看到三个端口输出的PHP版本都是不一样的表示配置成功了