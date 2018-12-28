---
ID: 965
post_title: 'php  It is not safe to rely on the system’s timezone settings. 问题解决'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/08/14/php-it-is-not-safe-to-rely-on-the-systems-timezone-settings-%e9%97%ae%e9%a2%98%e8%a7%a3%e5%86%b3/'
published: true
post_date: 2017-08-14 09:39:43
---
<strong>PHP调试的时候出现了警告:</strong>

It is not safe to rely on the system解决方法,其实就是时区设置不正确造成的,本文提供了3种方法来解决这个问题。

实际上，从PHP 5.1.0开始当对使用date()等函数时，如果timezone设置不正确，在每一次调用时间函数时，都会产生E_NOTICE 或者 E_WARNING 信息，而又在php中，date.timezone这个选项，默认情况下是关闭的，无论用什么php命令都是格林威治标准时间，但是PHP5.3中如果没有设置部分时间类函数也会强行抛出了这个错误的。
<strong>PS:</strong>现在由于大部分人使用VPS/云主机，需要自己配置的环境的就更加会容易出现这个情况。
建议：不熟悉PHP环境还是用比较成熟的一键安装包吧。

<strong>方法1：</strong>

（最好的方法）在php.ini里加上找到date.timezone项，设置date.timezone = "Asia/Shanghai"，重启环境就ok了。

<strong>方法2：</strong>

在需要用到这些时间函数的时候，在页面添加date_default_timezone_set("PRC");

<strong>方法3：</strong>

在页头加上设置时区ini_set('date.timezone','Asia/Shanghai');

<strong>错误代码：</strong>
Warning: date(): It is not safe to rely on the system’s timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected ‘Asia/Chongqing’ for ‘CST/8.0/no DST’ instead

Warning: strtotime(): It is not safe to rely on the system’s timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected ‘Asia/Chongqing’ for ‘CST/8.0/no DST’ instead

Warning: date_default_timezone_get(): It is not safe to rely on the system's timezone settings. You are *required* to use the date.timezone setting or the date_default_timezone_set() function. In case you used any of those methods and you are still getting this warning, you most likely misspelled the timezone identifier. We selected 'Asia/Chongqing' for 'CST/8.0/no DST' instead in &lt;b&gt;/home/ftp/n/nimaboke/include/lib/function.base.php