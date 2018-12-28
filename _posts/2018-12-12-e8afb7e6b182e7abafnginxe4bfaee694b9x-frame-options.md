---
ID: 1712
post_title: 请求端nginx修改X-Frame-Options
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/12/%e8%af%b7%e6%b1%82%e7%ab%afnginx%e4%bf%ae%e6%94%b9x-frame-options/'
published: true
post_date: 2018-12-12 17:53:22
---
location /dashboard/db {
      proxy_hide_header X-Frame-Options;//忽略返回头的X-Frame-Options
      add_header X-Frame-Options SAMEORIGIN always;//设置X-Frame-Options
      proxy_pass http://172.16.100.41:13000/dashboard/db;
    }