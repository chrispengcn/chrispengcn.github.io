---
ID: 3322
post_title: google blogger nginx proxy
author: peng, chris
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/11/08/google-blogger-nginx-proxy/
published: true
post_date: 2019-11-08 21:58:26
---
location /
{
proxy_pass http://ghs.google.com;
proxy_set_header Host w.mai1.me;
proxy_redirect off;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header Cookie "";
proxy_hide_header Set-Cookie;
proxy_hide_header Location;
proxy_set_header User-Agent "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.169 Safari/537.36";
proxy_set_header Accept-Encoding "";
subs_filter_types application/javascript text/javascript;
subs_filter 'fonts.gstatic.com' 'gstatic.loli.net';
subs_filter 'resources.blogblog.com/blogblog/data/res' 'cdn.jsdelivr.net/gh/staticfilestorage/cdn@cdn/blogger';
subs_filter 'resources.blogblog.com/img' 'cdn.jsdelivr.net/gh/staticfilestorage/cdn@cdn/blogger';
subs_filter 'lh1.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh1.googleusercontent.com';
subs_filter 'lh2.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh2.googleusercontent.com';
subs_filter 'lh3.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh3.googleusercontent.com';
subs_filter 'lh4.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh4.googleusercontent.com';
subs_filter 'lh5.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh5.googleusercontent.com';
subs_filter 'lh6.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh6.googleusercontent.com';
subs_filter 'lh7.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh7.googleusercontent.com';
subs_filter 'lh8.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh8.googleusercontent.com';
subs_filter 'lh9.googleusercontent.com' 'peppapigpages.oss-cn-qingdao.aliyuncs.com/lh9.googleusercontent.com';



}