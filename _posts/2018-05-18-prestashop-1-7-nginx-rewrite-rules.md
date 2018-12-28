---
ID: 1233
post_title: 'PRESTASHOP 1.7  NGINX + REWRITE RULES'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/05/18/prestashop-1-7-nginx-rewrite-rules/
published: true
post_date: 2018-05-18 23:30:23
---
vi /etc/nginx/conf.d/ps1.7.conf
<blockquote>
   <span class="pl-k">server</span> {
    <span class="pl-k">listen</span> <span class="pl-s">80</span>;
    <span class="pl-k">listen</span> [::]:80;   <span class="pl-c">#Use this to enable IPv6</span>
    <span class="pl-k">server_name</span> www.example.com;
    <span class="pl-k">root</span> /var/www/prestashop17;
    <span class="pl-k">access_log</span> /var/log/nginx/access.log;
    <span class="pl-k">error_log</span> /var/log/nginx/error.log;

    <span class="pl-k">index</span> index.php index.html;

    <span class="pl-k">location</span> <span class="pl-en">= /favicon.ico </span>{
        <span class="pl-k">log_not_found</span><span class="pl-c1"> off</span>;
        <span class="pl-k">access_log</span><span class="pl-c1"> off</span>;
    }

    <span class="pl-k">location</span> <span class="pl-en">= /robots.txt </span>{
        <span class="pl-k">auth_basic</span><span class="pl-c1"> off</span>;
        <span class="pl-k">allow</span><span class="pl-c1"> all</span>;
        <span class="pl-k">log_not_found</span><span class="pl-c1"> off</span>;
        <span class="pl-k">access_log</span><span class="pl-c1"> off</span>;
    }

    <span class="pl-c"># Deny all attempts to access hidden files such as .htaccess, .htpasswd, .DS_Store (Mac).</span>
    <span class="pl-k">location</span> ~ <span class="pl-sr">/\. </span>{
        <span class="pl-k">deny</span><span class="pl-c1"> all</span>;
        <span class="pl-k">access_log</span><span class="pl-c1"> off</span>;
        <span class="pl-k">log_not_found</span><span class="pl-c1"> off</span>;
    }

    <span class="pl-c">##</span>
    <span class="pl-c"># Gzip Settings</span>
    <span class="pl-c">##</span>

    <span class="pl-k">gzip</span><span class="pl-c1"> on</span>;
    <span class="pl-k">gzip_disable</span> <span class="pl-s">"msie6"</span>;
    <span class="pl-k">gzip_vary</span><span class="pl-c1"> on</span>;
    <span class="pl-k">gzip_proxied</span> any;
    <span class="pl-k">gzip_comp_level</span> <span class="pl-s">1</span>;
    <span class="pl-k">gzip_buffers</span> <span class="pl-s">16</span> <span class="pl-c1">8k</span>;
    <span class="pl-k">gzip_http_version</span> <span class="pl-c1">1.0</span>;
    <span class="pl-k">gzip_types</span> application/json text/css application/javascript;

    <span class="pl-k">rewrite</span> <span class="pl-sr">^/[a-zA-Z][a-zA-Z]/(index\.php.*)$ </span>/<span class="pl-smi">$1</span><span class="pl-c1"> last</span>;  <span class="pl-c">#Remove language code when index.php is called directly</span>
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/api/?(.*)$ </span>/webservice/dispatcher.php?url=<span class="pl-smi">$1</span><span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$1$2$3</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$2</span>/<span class="pl-smi">$1$2$3$4</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])([0-9])([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$2</span>/<span class="pl-smi">$3</span>/<span class="pl-smi">$1$2$3$4$5</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])([0-9])([0-9])([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$2</span>/<span class="pl-smi">$3</span>/<span class="pl-smi">$4</span>/<span class="pl-smi">$1$2$3$4$5$6</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])([0-9])([0-9])([0-9])([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$2</span>/<span class="pl-smi">$3</span>/<span class="pl-smi">$4</span>/<span class="pl-smi">$5</span>/<span class="pl-smi">$1$2$3$4$5$6$7</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$2</span>/<span class="pl-smi">$3</span>/<span class="pl-smi">$4</span>/<span class="pl-smi">$5</span>/<span class="pl-smi">$6</span>/<span class="pl-smi">$1$2$3$4$5$6$7$8</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$2</span>/<span class="pl-smi">$3</span>/<span class="pl-smi">$4</span>/<span class="pl-smi">$5</span>/<span class="pl-smi">$6</span>/<span class="pl-smi">$7</span>/<span class="pl-smi">$1$2$3$4$5$6$7$8$9</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])([0-9])(-[_a-zA-Z0-9-]*)?(-[0-9]+)?/.+.jpg$ </span>/img/p/<span class="pl-smi">$1</span>/<span class="pl-smi">$2</span>/<span class="pl-smi">$3</span>/<span class="pl-smi">$4</span>/<span class="pl-smi">$5</span>/<span class="pl-smi">$6</span>/<span class="pl-smi">$7</span>/<span class="pl-smi">$8</span>/<span class="pl-smi">$1$2$3$4$5$6$7$8$9$10</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/c/([0-9]+)(-[.*_a-zA-Z0-9-]*)(-[0-9]+)?/.+.jpg$ </span>/img/c/<span class="pl-smi">$1$2$3</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">rewrite</span> <span class="pl-sr">^/c/([a-zA-Z_-]+)(-[0-9]+)?/.+.jpg$ </span>/img/c/<span class="pl-smi">$1$2</span>.jpg<span class="pl-c1"> last</span>;
    <span class="pl-k">location</span> <span class="pl-en">/admin-dev/ </span>{                           <span class="pl-c">#Change this to your admin folder</span>
        <span class="pl-k">if</span> (!-e <span class="pl-smi">$request_filename</span>) {
            <span class="pl-k">rewrite</span> <span class="pl-sr">^/.*$ </span>/admin-dev/index.php<span class="pl-c1"> last</span>; <span class="pl-c">#Change this to your admin folder</span>
        }
    }
    <span class="pl-k">location</span> <span class="pl-en">/ </span>{
        <span class="pl-k">if</span> (!-e <span class="pl-smi">$request_filename</span>) {
            <span class="pl-k">rewrite</span> <span class="pl-sr">^/.*$ </span>/index.php<span class="pl-c1"> last</span>;
        }
    }

    <span class="pl-k">location</span> ~ <span class="pl-sr">.php$ </span>{
        <span class="pl-k">fastcgi_split_path_info</span> <span class="pl-sr">^(.+.php)(/.*)$</span>;
        <span class="pl-k">try_files</span> <span class="pl-smi">$uri</span> <span class="pl-c1">=404</span>;
        <span class="pl-k">fastcgi_keep_conn</span> on;
        <span class="pl-k">include</span> /etc/nginx/fastcgi_params;
        <span class="pl-k">fastcgi_pass</span> 127.0.0.1:9000;  <span class="pl-c">#Change this to your PHP-FPM location</span>
        <span class="pl-k">fastcgi_index</span> index.php;
        <span class="pl-k">fastcgi_param</span> SCRIPT_FILENAME <span class="pl-smi">$document_root$fastcgi_script_name</span>;
   }
}
</blockquote>