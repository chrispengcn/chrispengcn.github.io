---
ID: 1170
post_title: >
  阿里云nginx配置ssl证书实现https访问
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/08/%e9%98%bf%e9%87%8c%e4%ba%91nginx%e9%85%8d%e7%bd%aessl%e8%af%81%e4%b9%a6%e5%ae%9e%e7%8e%b0https%e8%ae%bf%e9%97%ae/'
published: true
post_date: 2018-05-08 20:40:54
---
<h1 class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="http://www.cnblogs.com/tianhei/p/7726505.html">nginx配置ssl证书实现https访问</a></h1>
<div class="clear"></div>
<div class="postBody">
<div id="cnblogs_post_body" class="blogpost-body">
<h3><img id="uploading_image_25180" src="https://common.cnblogs.com/images/loading.gif" alt="" />一，环境说明</h3>
服务器系统：ubuntu16.04LTS

服务器IP地址：47.89.12.99

域名：bjubi.com
<h3>二，域名解析到服务器</h3>
在阿里云控制台-产品与服务-云解析DNS-找到需要解析的域名点“解析”，进入解析页面后选择【添加解析】按钮会弹出如下页面：

主机记录这里选择@，记录值就是服务器ip地址，确认。

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171024232634066-885292368.png" alt="" width="592" height="222" />
<h3>三，申请ca证书</h3>
在阿里云控制台-产品与服务-安全(云盾)-CA证书服务(数据安全)，点击购买证书，

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171024233141582-1902896752.png" alt="" width="591" height="217" />

选择“免费版DV SSL”，点击立即购买:

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171024233306879-2054527838.png" alt="" width="591" height="264" />

然后点去支付:

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171024233540426-1595236469.png" alt="" width="585" height="218" />

最后确认支付:

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171024233629707-1819582868.png" alt="" width="577" height="248" />

就会回到管理界面：

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171024233856582-1745911408.png" alt="" width="606" height="191" />

点击“补全”，输入要解析的域名，点下一步：

说明：因为我们这里申请的是开发版免费证书，所以一个证书仅支持一个域名认证，不支持通配符。

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171024233933363-1145634345.png" alt="" width="597" height="220" />

等待几分钟，证书状态变为“已签发”后，证书就申请成功了。
<h3>四，下载证书</h3>
列表中找到已签发的证书，下载：

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171026223712680-1243539726.png" alt="" width="926" height="65" />

进入下载页面，找到ngin页签中nginx配置信息，并“下载证书 for Nginx”：

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171026223828914-1258706526.png" alt="" width="571" height="328" />

记录以下内容，为了一会儿配置nginx用：

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171026225509773-1072718515.png" alt="" width="590" height="240" />

下载的文件有两个：

1，214292799730473.pem

2，214292799730473.key
<h3>五，服务器安装，配置nginx</h3>
登录到服务器：
<div class="cnblogs_code">
<pre>$ apt-get update // 更新软件
$ apt-get install nginx // 安装nginx</pre>
</div>
<h3>六，配置ca证书</h3>
1，nginx的安装目录为：/etc/nginx/。进入目录，增加cert/文件夹，把刚刚下载的两个文件上传到cert/文件夹中。

2，在/etc/nginx/sites-enabled/下，增加bjubi.com文件。内容如下：

说明：下面的配置是对443端口和80端口进行监听，443端口要启用ssl。监听443端口的server配置可以仿照上面ca认证页面的nginx配置示例进行配置。

root节点笔者创建了一个bjubi.com/的文件夹，专门存放来自这个域名的请求以示区分。

bjubi.com/文件夹下增加一个index.html文件，里面仅仅写了一行&lt;h1&gt;welcome。
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>server {
    listen 443;
    server_name bjubi.com; // 你的域名
    ssl on;
    root /var/www/bjubi.com; // 前台文件存放文件夹，可改成别的
    index index.html index.htm;// 上面配置的文件夹里面的index.html
    ssl_certificate  cert/214292799730473.pem;// 改成你的证书的名字
    ssl_certificate_key cert/214292799730473.key;// 你的证书的名字
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        index index.html index.htm;
    }
}
server {
    listen 80;
    server_name bjubi.com;// 你的域名
    rewrite ^(.*)$ https://$host$1 permanent;// 把http的域名请求转成https
}</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
配置完成后，检查一下nginx配置文件是否可用，有successful表示可用。
<div class="cnblogs_code">
<pre>$ nginx -t // 检查nginx配置文件</pre>
</div>
配置正确后，重新加载配置文件使配置生效：
<div class="cnblogs_code">
<pre>$ nginx -s reload // 使配置生效</pre>
</div>
至此，nginx的https访问就完成了，并且通过rewrite方式把所有http请求也转成了https请求，更加安全。

如需重启nginx，用以下命令：
<div class="cnblogs_code">
<pre>$ service nginx stop // 停止
$ service nginx start // 启动
$ service nginx restart // 重启</pre>
</div>
<h3>七，访问效果</h3>
输入http:bjubi.com也会自动跳转至https页面。

说明：如果是云服务器比如阿里云ECS，需要到阿里云ECS的管理后台的安全组，修改端口过滤规则把80端口和443端口开放才能访问到。

<img src="https://images2017.cnblogs.com/blog/1220516/201710/1220516-20171026231431211-2126237317.png" alt="" />

</div>
</div>