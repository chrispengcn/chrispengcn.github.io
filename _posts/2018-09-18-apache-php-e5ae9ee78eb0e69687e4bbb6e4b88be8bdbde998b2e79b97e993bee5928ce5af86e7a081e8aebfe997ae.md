---
ID: 1385
post_title: >
  apache + php
  实现文件下载防盗链和密码访问
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/09/18/apache-php-%e5%ae%9e%e7%8e%b0%e6%96%87%e4%bb%b6%e4%b8%8b%e8%bd%bd%e9%98%b2%e7%9b%97%e9%93%be%e5%92%8c%e5%af%86%e7%a0%81%e8%ae%bf%e9%97%ae/'
published: true
post_date: 2018-09-18 18:25:15
---
作为普通的网民来说，一般不需要知道也不用关心什么是盗链，不过如果你是网站的开发者或维护者，就不得不重视盗链的问题了。如果你刚刚开发完一个没有防盗链的带有文件下载功能的网站，挂上internet，然后上传几个时下非常热门的软件或电影并在网站内公布下载地址，让MSN上的所有好友都来体验一下你的杰作。不用多久就会发现网速出奇地变慢，甚至服务器托管中心的服务员会热情地打电话告诉你的网站流量很大，估计是网站受欢迎起来了，问你是不是该考虑加钱租用带宽更宽但价格更贵的网线了。在这个值得庆祝的时候赶快打开Google Analytics看看有多少人来光顾你的网站了吧，如果发现访客每天才十来个人，很遗憾地告诉你：你的网站资源不幸地被人盗链了。而且更糟糕的是，当你把网站上的文件和电影通通删光之后，网站仍然没有变快多少，从web服务器的访问日志里会发现疯狂的访问请求正从四面八方涌过来，web服务器为了迎接这批访客而没有时间处理正常的页面，这种状况可能会一直持续好几个周时间。

&nbsp;

&nbsp;

&nbsp;

vipdownload.php

&nbsp;
<pre> &lt;?php 
// $str='d3d3LmpiNTEubmV0IOiEmuacrOS5i+Wutg==';     //定义字符串 // echo base64_decode($str); //输出解码后的内容
// $str='www.jb51.net 脚本之家'; //定义字符串 // echo base64_encode($str);  // 输出编码后的内容为： d3d3LmpiNTEubmV0IOiEmuacrOS5i+Wutg== 
$pin = ""; if(!empty($_POST['pin'])){    $pin = $_POST['pin']; }
if($pin = "" or $pin != "www.hss5.com" ){?&gt;

&lt;link rel="stylesheet" href="vipdownload.css" type="text/css" media="screen" /&gt;&lt;form method="post" class="searchform cf" action="" style="text-align:center;"&gt;  &lt;input type="text" name="pin" id="pin" placeholder="Please input your VIP code"&gt;  &lt;button type="submit" style=""&gt;Download&lt;/button&gt;&lt;/form&gt;

&lt;?php  }?&gt;
&lt;?php  $pin = "";$file_address = "";$filecode = "";$file_name = "down.zip";     //下载文件名    $file_dir = "./down/";        //下载文件存放目录  $key = time(); 
 if(!empty($_POST['pin'])){    $pin = $_POST['pin']; } 
if (!empty($_GET['filecode']) )    
{   $filecode = $_GET['filecode'];   $file_address = base64_decode ($filecode);     }   
if (!empty($_GET['file']) &amp;&amp; empty($_GET['filecode']))   {   $filecode = base64_encode($_GET['file']);   header("location:vipdownload.php?filecode=".$filecode);    } 

if ($pin == "www.hss5.com") {
  $pieces = explode("/", $file_address);  // end() 函数将数组内部指针指向最后一个元素，并返回该元素的值（如果成功）;  $file_name = end($pieces);    //需要增加检查 文件类型 为 zip, pdf    //检查文件是否存在      // if (! file_exists ( $file_dir . $file_name )) {          if (! file_exists ( $file_address )) {        header('HTTP/1.1 404 NOT FOUND');      } elseif ( substr($file_name,-3) == "zip" or substr($file_name,-3) == "pdf" ){         //以只读和二进制模式打开文件     $file = fopen ( $file_address, "rb" );     //告诉浏览器这是一个文件流格式的文件      Header ( "Content-type: application/octet-stream" );      //请求范围的度量单位    Header ( "Accept-Ranges: bytes" );       //Content-Length是指定包含于请求或响应中数据的字节长度      Header ( "Accept-Length: " . filesize ( $file_address ) );       //用来告诉浏览器，文件是可以当做附件被下载，下载后的文件名称为$file_name该变量的值。  Header ( "Content-Disposition: attachment; filename=" . $file_name );          //读取文件内容并直接输出到浏览器     // echo fread ( $file, filesize ( $file_dir . $file_name ) );        echo fread ( $file, filesize ( $file_address ) );      fclose ( $file );         exit ();     } // endif fileexist
}  // endif $($_POST['pin'] == "www.hss5.com")
if(!empty($_POST['pin']) &amp;&amp; $pin != "www.hss5.com" )
{echo "&lt;p style='text-align:center;'&gt;&lt;br&gt;VIP code error ! &lt;br&gt;Please contact our sale rep or send email to cl@banqcn.com to get your VIP code.&lt;br&gt;&lt;/p&gt;";
// $filecode = authcode($file_address,'ENCODE',$key,0); //加密  调试  //  echo $filecode;                                      //调试 //  echo $file_address;}?&gt;   



</pre>
禁止文件直接下载
vi .htaccess

&nbsp;

&lt;FilesMatch "~*.(bak|inc|lib|sh|tpl|lbi|dwt|pdf|zip)$"&gt;
# order deny,allow
# deny from all

&lt;IfModule mod_rewrite.c&gt;
# 将 RewriteEngine 模式打开
RewriteEngine On

Rewritecond %{HTTP_HOST} ^(www\.)?hss5.com$ [nc]
Rewriterule ^(.*)$ http://www.hss5.com/vipdownload.php?file=$1 [r=302,nc]

&lt;/IfModule&gt;

&nbsp;

&lt;/FilesMatch&gt;

&nbsp;

本方法实现了php 的密码访问。

通过修改，可以实现留下email 即可下载。

还有另外一种加密方式

.htpasswd 密码访问