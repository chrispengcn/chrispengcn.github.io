---
ID: 1323
post_title: >
  nginx允许指定IP免密码访问页面，其他密码验证访问
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/08/29/nginx%e5%85%81%e8%ae%b8%e6%8c%87%e5%ae%9aip%e5%85%8d%e5%af%86%e7%a0%81%e8%ae%bf%e9%97%ae%e9%a1%b5%e9%9d%a2%ef%bc%8c%e5%85%b6%e4%bb%96%e5%af%86%e7%a0%81%e9%aa%8c%e8%af%81%e8%ae%bf%e9%97%ae/'
published: true
post_date: 2018-08-29 17:48:29
---
<pre class="prettyprint"><code class="hljs axapta has-numbering"><span class="hljs-keyword">server</span> {
        <span class="hljs-preprocessor">#listen   80; ## listen for ipv4; this line is default and implied</span>
        <span class="hljs-preprocessor">#listen   [::]:80 default ipv6only=on; ## listen for ipv6</span>

        root /usr/share/nginx/phpmyadmin;
        <span class="hljs-keyword">index</span> <span class="hljs-keyword">index</span>.php <span class="hljs-keyword">index</span>.html <span class="hljs-keyword">index</span>.htm;

        <span class="hljs-preprocessor"># Make site accessible from http://localhost/</span>
        server_name localhost;

        location / {
                <span class="hljs-preprocessor"># First attempt to serve request as file, then</span>
                <span class="hljs-preprocessor"># as directory, then fall back to index.html</span>
                try_files $uri $uri/ /<span class="hljs-keyword">index</span>.html;
                satisfy any;  <span class="hljs-preprocessor">###</span>
                allow <span class="hljs-number">192.168</span>.xxx.xx; <span class="hljs-preprocessor">###</span>
                deny all;  <span class="hljs-preprocessor">###</span>

                auth_basic <span class="hljs-string">"secret"</span>; <span class="hljs-preprocessor">###</span>
                auth_basic_user_file /usr/share/nginx/xxx/passwd.db; <span class="hljs-preprocessor">###</span>
                <span class="hljs-preprocessor"># Uncomment to enable naxsi on this location</span>
                <span class="hljs-preprocessor"># include /etc/nginx/naxsi.rules</span>
        }

        location /doc/ {
                alias /usr/share/doc/;
                autoindex on;
                allow <span class="hljs-number">127.0</span><span class="hljs-number">.0</span><span class="hljs-number">.1</span>;
                deny all;
        }


</code></pre>