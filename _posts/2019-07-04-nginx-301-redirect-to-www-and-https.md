---
ID: 2751
post_title: nginx 301 redirect to www and https
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/07/04/nginx-301-redirect-to-www-and-https/
published: true
post_date: 2019-07-04 20:13:56
---
nginx 301 redirect to www and https
website:

http://www.cl-light.com   ->    https://www.cl-light.com
{http://cl-light.com + https://cl-light.com}  ->  https://www.cl-light.com

<code>

server {
    listen       80;
    server_name www.cl-light.com;
    location / {
        return 301 https://$host$request_uri;
    }
}

server {
    listen  80;
    server_name cl-light.com;
    listen 443 ssl;
    ssl on;
    error_page   497 https://$request_uri;

    ssl_certificate /usr/local/nginx/cert/cl-light.pem;
    ssl_certificate_key /usr/local/nginx/cert/cl-light.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

    location / {
        return 301 https://www.cl-light.com$request_uri;
    }

server {
    listen  443;
    server_name www.cl-light.com;
    listen 443 ssl;
    ssl on;
    ssl_certificate /usr/local/nginx/cert/cl-light.pem;
    ssl_certificate_key /usr/local/nginx/cert/cl-light.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;

}


}</code>