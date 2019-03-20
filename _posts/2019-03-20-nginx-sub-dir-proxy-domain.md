---
ID: 2632
post_title: >
  Nginx通过二级目录（路径）映射不同的反向代理，规避IP+端口访问
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/20/nginx-sub-dir-proxy-domain/
published: true
post_date: 2019-03-20 21:54:11
---
①、同一个域名需要反向代理到前台和后台（不同机器和端口）；

②、需要采用IP+端口的模式，嵌入到APP作为DNS污染后的备选方案。
<pre>server {
listen 80;
server_name demo.domain.com;
#通过访问service二级目录来访问后台
location /service/ {
#DemoBackend1后面的斜杠是一个关键，没有斜杠的话就会传递service到后端节点导致404
proxy_pass http://DemoBackend1/;
proxy_redirect off;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
#其他路径默认访问前台网站
location / {
proxy_pass http://DemoBackend2;
proxy_redirect off;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}

#简单的负载均衡节点配置
upstream DemoBackend1 {
server 192.168.1.1;
server 192.168.1.2;
ip_hash;
}
upstream DemoBackend2 {
server 192.168.2.1;
server 192.168.2.2;
ip_hash;
}

#新增的IP映射配置
server {
listen 80;
server_name 192.168.1.10 192.168.2.10 192.168.3.10;
location /mail_api/ {
proxy_pass http://DemoBackend/; #后面的斜杠不能少，作用是不往后端传递/mail-api 这个路径
proxy_redirect off;
proxy_set_header Host mailapi.domain.com; #传递不同的host给后方节点，实现IP和域名均可以访问
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
location /other_api1/ {
proxy_pass http://DemoBackend/;
proxy_redirect off;
proxy_set_header Host otherapi1.domain.com;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
#还可以添加更多映射，通过不同的路径来映射不同的API，最后对于直接访问IP则返回403，防网络上的扫码探测
location / {
return 403;
}
}</pre>
<pre>#原有的域名映射
server {
listen 80;
server_name mailapi.domain.com;
location / {
proxy_pass http://DemoBackend;
proxy_redirect off;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}
server {
listen 80;
server_name otherapi1.domain.com;
location / {
proxy_pass http://DemoBackend;
proxy_redirect off;
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}
#简单的节点配置（当这些API都用到同一个Backend时，上述代码中的proxy_set_header传递的host就起到了关键性作用！）
upstream DemoBackend {
server 192.168.10.1;
server 192.168.10.2;
ip_hash;
}
最终实现的效果就是：你要通过IP请求邮件API，只要请求 http://192.168.1.1/mail_api/ 即可，而不需要开放多余端口。而且，后续要新增更多API，只需要定义不同的二级路径即可，这些二级路径的辨识度可比端口要好得多！</pre>
Ps：正如代码中的注释，示例代码只用了一个 DemoBackend 节点配置，为的是分享另一个小技巧：当后端节点承载了多个站点而且都是监听80端口时（比如某些小公司同一个IIS服务器部署了N个站点），反向代理中的proxy_set_header参数，可以自定义传递一个host域名给后端节点，从而正确响应预期内容！

我之前供职的公司节点用的是IIS服务器，前端用Nginx反向代理，IIS服务器上有多个站点，站点之间部分会通过 rewrite 规则联系起来。

打个比方：比如A网站有个专题内容（www.a.com/zt/）是通过IIS伪静态映射到了B网站(content.b.com)。也就是访问到http://www.a.com/zt/，其实最后是通过A网站映射到了B网站上面。

后来发现IIS有个伪静态BUG，会经常奔溃，就要我用前端的Nginx来实现直接映射，而不再走IIS的A网站中转。

那么这个需求就正好用到了 proxy_set_header 技巧，一看就懂：
<pre>Shell
server {
listen 80;
server_name www.a.com;
location /zt/ {
proxy_pass http://ABackend; #都是相同的节点，此示例代码我就不写upstream了
proxy_redirect off;
proxy_set_header Host www.b.com; #这里就是关键性作用，传递b域名给后端IIS
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}

#upstream略..

server {
listen 80;
server_name www.a.com;
location /zt/ {
proxy_pass http://ABackend; #都是相同的节点，此示例代码我就不写upstream了
proxy_redirect off;
proxy_set_header Host www.b.com; #这里就是关键性作用，传递b域名给后端IIS
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
}</pre>
#upstream略..
很明显，通过传递自定义域名，就可以实现通过A网站访问Nginx，返回B网站内容，和反向代理谷歌的原理是一致的。

当然，上文为了实现 IP 和域名都可以访问，这个proxy_set_header 设置也是必须的。说白了就是在反代过程中，对后端服务器伪装（传递）了一个自定域名，让后端响应该域名预期内容。