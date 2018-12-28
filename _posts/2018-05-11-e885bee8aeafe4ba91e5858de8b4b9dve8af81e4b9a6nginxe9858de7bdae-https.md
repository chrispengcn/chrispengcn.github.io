---
ID: 1183
post_title: 腾讯云免费DV证书Nginx配置-https
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/11/%e8%85%be%e8%ae%af%e4%ba%91%e5%85%8d%e8%b4%b9dv%e8%af%81%e4%b9%a6nginx%e9%85%8d%e7%bd%ae-https/'
published: true
post_date: 2018-05-11 12:36:57
---
<div>
<div>

首先注册一个腾讯云账号获取域名,https://www.qcloud.com/

这里已经获取了一个证书, 只是写一个
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1100" data-height="674"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-390f56a3ddb9d1d5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700" data-original-src="//upload-images.jianshu.io/upload_images/1910094-390f56a3ddb9d1d5.png" data-original-width="1100" data-original-height="674" data-original-format="image/png" data-original-filesize="108983" /></div>
</div>
<div class="image-caption"></div>
</div>
选择申请证书
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1092" data-height="182"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-2ddabe39ee9eebfd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700" data-original-src="//upload-images.jianshu.io/upload_images/1910094-2ddabe39ee9eebfd.png" data-original-width="1092" data-original-height="182" data-original-format="image/png" data-original-filesize="24996" /></div>
</div>
<div class="image-caption"></div>
</div>
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="778" data-height="685"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-38d9702b5cc806a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700" data-original-src="//upload-images.jianshu.io/upload_images/1910094-38d9702b5cc806a5.png" data-original-width="778" data-original-height="685" data-original-format="image/png" data-original-filesize="38352" /></div>
</div>
<div class="image-caption"></div>
</div>
上面我已经申请好了证书, 申请证书之后是验证, 建议使用域名解析认证,在服务器里面放文件的验证方式比较难实现,验证之后点击下载
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1078" data-height="210"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-091c61341eabd2ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700" data-original-src="//upload-images.jianshu.io/upload_images/1910094-091c61341eabd2ff.png" data-original-width="1078" data-original-height="210" data-original-format="image/png" data-original-filesize="32193" /></div>
</div>
<div class="image-caption"></div>
</div>
下面开始去服务器配置https证书,
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="467" data-height="157"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-4c2bc3758e709fa5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/467" data-original-src="//upload-images.jianshu.io/upload_images/1910094-4c2bc3758e709fa5.png" data-original-width="467" data-original-height="157" data-original-format="image/png" data-original-filesize="5011" /></div>
</div>
<div class="image-caption"></div>
</div>
下载的文件一般有Apache和Nginx以及IIS,有的会有Tomcat,这里我们取用Nginx,
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="559" data-height="95"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-0080ac4c292dd3e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/559" data-original-src="//upload-images.jianshu.io/upload_images/1910094-0080ac4c292dd3e9.png" data-original-width="559" data-original-height="95" data-original-format="image/png" data-original-filesize="4629" /></div>
</div>
<div class="image-caption"></div>
</div>
这里的证书我从腾讯云搬到阿里云都是centos系统, 现在采用了win系统的

首先进入Nginx配置文件夹,我的这个只是相对的,根据不同的环境改变
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="387" data-height="79"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-f76c5dcbdbeaffa0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/387" data-original-src="//upload-images.jianshu.io/upload_images/1910094-f76c5dcbdbeaffa0.png" data-original-width="387" data-original-height="79" data-original-format="image/png" data-original-filesize="3114" /></div>
</div>
<div class="image-caption"></div>
</div>
然后在同级目录下面新建一个ssl的文件(也可以取自己喜欢的),然后把证书拖进去,接着开始配置Nginx, 打开Nginx

vim nginx.conf

添加如下配置
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="738" data-height="630"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-d01288eebff5ccc1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700" data-original-src="//upload-images.jianshu.io/upload_images/1910094-d01288eebff5ccc1.png" data-original-width="738" data-original-height="630" data-original-format="image/png" data-original-filesize="41639" /></div>
</div>
<div class="image-caption"></div>
</div>
server {

listen 443;

server_name image.wertp.cn; #填写绑定证书的域名 11

ssl on;

ssl_certificate /usr/local/nginx/conf/ssl/1_image.wertp.cn_bundle.crt;

ssl_certificate_key /usr/local/nginx/conf/ssl/2_image.wertp.cn.key;

ssl_session_timeout 5m;

ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置

ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;#按照这个套件配置

ssl_prefer_server_ciphers on;

location / {

root  /data/wwwroot/image.wertp.cn/public;

index index.php index.html index.htm;

if (!-e $request_filename){

rewrite ^/(.*)$ /index.php?s=$1 last;

}

#（thinkphp  rewrite路由重写模式添加这段，否则是普通模式）

}

location ~ \.php$ {

root          /data/wwwroot/image.wertp.cn/public;

fastcgi_pass  unix:/dev/shm/php-cgi.sock;

fastcgi_index  index.php;

fastcgi_param  SCRIPT_FILENAME  /usr/local/nginx/html$fastcgi_script_name;

fastcgi_param HTTPS on;

include fastcgi.conf;

}

#（兼容php添加这段）

}

接下来保存,退出,然后重启Nginx,建议先reload一下, 有时候会因为路径的问题导致Nginx无法重启,或者重启后不生效

service nginx reload

service nginx restart

如果提示重启成功,就可以去配置域名了,然后配置好的结果就是这样的
<div class="image-package">
<div class="image-container">
<div class="image-container-fill"></div>
<div class="image-view" data-width="1550" data-height="812"><img class="" src="//upload-images.jianshu.io/upload_images/1910094-7b97b61f1cd5ccf6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700" data-original-src="//upload-images.jianshu.io/upload_images/1910094-7b97b61f1cd5ccf6.png" data-original-width="1550" data-original-height="812" data-original-format="image/png" data-original-filesize="1445327" /></div>
</div>
<div class="image-caption"></div>
</div>
这个是为了小程序特意加入的不同类型图片,因为之前的尺度太大,无法通过审核.这个网站原来是用的centos7.3的系统, 现在测试放到了win上面.

配置基本就这么多了, 熟悉了还是比较简单的,之前踩坑比较多, 各种的证书bug, 现在觉得这个还不错

</div>
作者：rosekissyou
链接：https://www.jianshu.com/p/da25bfa571d4
來源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

</div>