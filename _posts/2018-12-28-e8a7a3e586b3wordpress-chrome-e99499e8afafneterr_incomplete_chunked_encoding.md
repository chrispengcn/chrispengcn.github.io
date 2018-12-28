---
ID: 1793
post_title: >
  解决wordpress Chrome
  错误“net::ERR_INCOMPLETE_CHUNKED_ENCODING”
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/28/%e8%a7%a3%e5%86%b3wordpress-chrome-%e9%94%99%e8%af%afneterr_incomplete_chunked_encoding/'
published: true
post_date: 2018-12-28 17:46:17
---
解决Chrome 错误“net::ERR_INCOMPLETE_CHUNKED_ENCODING” 原
 叫我哀木涕   叫我哀木涕 发布于 07/09 16:36 字数 468 阅读 4103 收藏 0 点赞 0  评论 0
NginxChrome
年底了，该给自己写个总结了，一个六年女Java程序员的心声 >>>   

记一次问题排查的过程。前提自己搭建了一个发布系统，同事在使用时候突然出现了页面白板，页面不能渲染任何内容。当然第一反应是自己写的代码diff模块可能某个地方出问题了。

打印diff模块的cmd，查看返回都没有任何问题。到模板最后一步输出出了问题。奇怪心中一万个问号，重来没有碰到这个问题，换个浏览器。咦没有问题！真的是没有问题。

chrome打开调试模式，failed错误代码(failed) net::ERR_INCOMPLETE_CHUNKED_ENCODING。然后google到结果了。其实很常见的一个问题。就是当输出代理文件大小超过配置proxy_temp_file_write_size时候，nginx会将文件写入到临时目录下。如果没有权限，chrom就会直接failed而不输出东西。

具体错误：

/var/lib/nginx/tmp/fastcgi/2/37/0000000372" failed (13: Permission denied) while reading upstream, client: 10.18.128.147, server: deploy.mgame.qihoo.net, request: "GET /walle/deploy?taskId=147 HTTP/1.1", upstream: "fastcgi://127.0.0.1:9000", host: "deploy.xxx.com:8081"
解决方法：

chown -R www:www /var/lib/nginx
其中www替换为自己实际项目中配置的ngxin运行用户。

这个问题回顾是安装nginx的时候，直接yum install。在配置web的时候，忘了改/var/lib/nginx的目录权限。默认是nginx:nginx的组权限，平时不用这个账号启动。所以在碰到输出较大的body的时候，就触发了这个et::ERR_INCOMPLETE_CHUNKED_ENCODING错误。