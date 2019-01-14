---
ID: 1210
post_title: >
  如何使用Nginx服务器 sub filter
  做转发和匹配替换
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/15/%e5%a6%82%e4%bd%95%e4%bd%bf%e7%94%a8nginx%e6%9c%8d%e5%8a%a1%e5%99%a8-sub-filter-%e5%81%9a%e8%bd%ac%e5%8f%91%e5%92%8c%e5%8c%b9%e9%85%8d%e6%9b%bf%e6%8d%a2/'
published: true
post_date: 2018-05-15 16:47:46
---
<div class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="http://www.cnblogs.com/cyq632694540/p/7003153.html">使用Nginx做转发和匹配替换</a></div>
<div id="cnblogs_post_body" class="blogpost-body">

Nginx是一个强大的服务器软件，由于处理数据内容处于第七层协议应用层的原因，所以获取的数据也比较完整；

Nginx做转发：

这个很简单，vi nginx.conf(编辑nginx配置文件)

添加location /public/sexy.jpg{//这个是你域名访问的图片

proxy_pass http://www.tmp.jpg;#这个是你要替换的图片路径(加上http头)

}

保存退出，重启nginx 就可以看到原本项目 http://xxx/public/sexy.jpg变成了http://www.tmp.jpg这个图片，虽然在页面上看道德还是sexy.jpg；

nginx反向代理很牛逼吧，还有个更牛逼的就是sub filter这个nginx插件

将插件存放到tmp目录下面

cd /tmp

git clone git://github.com/yaoweibin/ngx_http_substitutions_filter_module.git

【PS：如果nginx安装过的，那么需要先找到编译目录(就是你安装nginx的安装包目录)】

cd /xxx/nginx-1.11.0(安装包目录)

./configure --prefix=/xxxx/nginx(nginx项目) --add-module=/tmp/ngx_http_substitutions_filter_module    #追加sub_filter插件

make

然后就会在nginx-11.0(安装包目录)/objs/nginx   更新nginx启动文件

cp 复制nginx-11.0(安装包目录)/objs/nginx    /xxxx/nginx(nginx项目)/sbin/nginx  替换掉就行了

vi  /xxxx/nginx(nginx项目)/conf/nginx.conf  编辑配置nginx

ps：我在网上看很多都是在 location /{}里面添加的，不过我试过都不行，只能在location外面  server里面配置才有效果

<img class="alignnone size-full wp-image-2022" src="http://hss5.com/wp-content/uploads/2019/01/1045034-20170613195721368-644135213.png" width="605" height="364" alt="" />

location里面没效果，location外面 server里面才有效；

subs_filter  需要替换的文本 结果的文本；//中间采用空格 隔开就行了

如果遇到 &lt;a href=""&gt; haha &lt;/a&gt;有空格的文本，则用引用括起来

eg: '&lt;a href=""&gt; haha &lt;/a&gt;' '&lt;a href=""&gt; xxiixixix &lt;/a&gt;'   //这样就行了

</div>