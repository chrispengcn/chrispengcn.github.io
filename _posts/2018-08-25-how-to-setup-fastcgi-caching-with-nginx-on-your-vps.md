---
ID: 1312
post_title: >
  How to Setup FastCGI Caching with Nginx
  on your VPS
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/08/25/how-to-setup-fastcgi-caching-with-nginx-on-your-vps/
published: true
post_date: 2018-08-25 14:15:33
---
<h3 id="prelude">Prelude</h3>
Nginx includes a FastCGI module which has directives for caching dynamic content that are served from the PHP backend. Setting this up removes the need for additional page caching solutions like reverse proxies (think <a href="https://www.digitalocean.com/community/articles/how-to-install-and-configure-varnish-with-apache-on-ubuntu-12-04--3">Varnish</a>) or application specific plugins. Content can also be excluded from caching based on the request method, URL, cookies, or any other server variable.
<div data-unique="enabling-fastcgi-caching-on-your-vps"></div>
<h2 id="enabling-fastcgi-caching-on-your-vps">Enabling FastCGI Caching on your VPS</h2>
This article assumes that you've already setup and configured <a href="https://www.digitalocean.com/community/articles/how-to-install-linux-nginx-mysql-php-lemp-stack-on-ubuntu-12-04">Nginx with PHP</a> on your droplet. Edit the <a href="https://www.digitalocean.com/community/articles/how-to-set-up-nginx-virtual-hosts-server-blocks-on-ubuntu-12-04-lts--3">Virtual Host</a> configuration file for which caching has to be enabled.
<pre class="code-pre "><code>nano /etc/nginx/sites-enabled/vhost
</code></pre>
Add the following lines to the top of the file outside the <strong>server { }</strong> directive:
<pre class="code-pre "><code>fastcgi_cache_path /etc/nginx/cache levels=1:2 keys_zone=MYAPP:100m inactive=60m;
fastcgi_cache_key "$scheme$request_method$host$request_uri";
</code></pre>
The "fastcgi<em>cache</em>path" directive specifies the location of the cache (/etc/nginx/cache), its size (100m), memory zone name (MYAPP), the subdirectory levels, and the <em>inactive` timer.</em>

The location can be anywhere on the hard disk; however, the size must be less than your droplet's RAM + <a href="https://www.digitalocean.com/community/articles/how-to-add-swap-on-ubuntu-12-04">Swap</a> or else you'll receive an error that reads "Cannot allocate memory." We will look at the "levels" option in the purging section-- if a cache isn't accessed for a particular amount of time specified by the "inactive" option (60 minutes here), then Nginx removes it.

The "fastcgi<em>cache</em>key" directive specifies how the the cache filenames will be hashed. Nginx encrypts an accessed file with MD5 based on this directive.

Next, move the location directive that passes PHP requests to php5-fpm. Inside "location ~ .php$ { }" add the following lines.
<pre class="code-pre "><code>fastcgi_cache MYAPP;
fastcgi_cache_valid 200 60m;
</code></pre>
The "fastcgi<em>cache" directive references to the memory zone name which we specified in the "fastcgi</em>cache_path" directive and stores the cache in this area.

By default Nginx stores the cached objects for a duration specified by any of these headers: <strong>X-Accel-Expires/Expires/Cache-Control.</strong>

The "fastcgi<em>cache</em>valid" directive is used to specify the default cache lifetime if these headers are missing. In the statement we entered above, only responses with a status code of 200 is cached. Other response codes can also be specified.

<strong>Do a configuration test</strong>
<pre class="code-pre "><code>service nginx configtest
</code></pre>
<strong>Reload Nginx if everything is OK</strong>
<pre class="code-pre "><code>service nginx reload
</code></pre>
The complete vhost file will look like this:
<pre class="code-pre "><code>fastcgi_cache_path /etc/nginx/cache levels=1:2 keys_zone=MYAPP:100m inactive=60m;
fastcgi_cache_key "$scheme$request_method$host$request_uri";

server {
    listen   80;

    root /usr/share/nginx/html;
    index index.php index.html index.htm;

    server_name example.com;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php5-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_cache MYAPP;
        fastcgi_cache_valid 200 60m;
    }
}
</code></pre>
Next we will do a test to see if caching works.
<div data-unique="testing-fastcgi-caching-on-your-vps"></div>
<h2 id="testing-fastcgi-caching-on-your-vps">Testing FastCGI Caching on your VPS</h2>
Create a PHP file which outputs a UNIX timestamp.
<pre class="code-pre "><code> /usr/share/nginx/html/time.php
</code></pre>
Insert
<pre class="code-pre "><code>&lt;?php
echo time();
?&gt;
</code></pre>
Request this file multiple times using <strong>curl</strong> or your web browser.
<pre class="code-pre "><code>root@droplet:~# curl http://localhost/time.php;echo
1382986152
root@droplet:~# curl http://localhost/time.php;echo
1382986152
root@droplet:~# curl http://localhost/time.php;echo
1382986152
</code></pre>
If caching works properly, you should see the same timestamp on all requests as the response is cached.

Do a <em>recursive</em> listing of the cache location to find the cache of this request.
<pre class="code-pre "><code>root@droplet:~# ls -lR /etc/nginx/cache/
/etc/nginx/cache/:
total 0
drwx------ 3 www-data www-data 60 Oct 28 18:53 e

/etc/nginx/cache/e:
total 0
drwx------ 2 www-data www-data 60 Oct 28 18:53 18

/etc/nginx/cache/e/18:
total 4
-rw------- 1 www-data www-data 117 Oct 28 18:53 b777c8adab3ec92cd43756226caf618e
</code></pre>
The naming convention will be explained in the purging section.

We can also make Nginx add a "X-Cache" header to the response, indicating if the cache was missed or hit.

Add the following above the <strong>server { }</strong> directive:
<pre class="code-pre "><code>add_header X-Cache $upstream_cache_status;
</code></pre>
Reload the Nginx service and do a verbose request with curl to see the new header.
<pre class="code-pre "><code>root@droplet:~# curl -v http://localhost/time.php
* About to connect() to localhost port 80 (#0)
*   Trying 127.0.0.1...
* connected
* Connected to localhost (127.0.0.1) port 80 (#0)
&gt; GET /time.php HTTP/1.1
&gt; User-Agent: curl/7.26.0
&gt; Host: localhost
&gt; Accept: */*
&gt;
* HTTP 1.1 or later with persistent connection, pipelining supported
&lt; HTTP/1.1 200 OK
&lt; Server: nginx
&lt; Date: Tue, 29 Oct 2013 11:24:04 GMT
&lt; Content-Type: text/html
&lt; Transfer-Encoding: chunked
&lt; Connection: keep-alive
&lt; X-Cache: HIT
&lt;
* Connection #0 to host localhost left intact
1383045828* Closing connection #0
</code></pre>
<div data-unique="setting-cache-exceptions"></div>
<h2 id="setting-cache-exceptions">Setting Cache Exceptions</h2>
Some dynamic content such as authentication required pages shouldn't be cached. Such content can be excluded from being cached based on server variables like "request<em>uri," "request</em>method," and "http_cookie."

Here is a sample configuration which must be used in the <strong>server{ }</strong> context.
<pre class="code-pre "><code>#Cache everything by default
set $no_cache 0;

#Don't cache POST requests
if ($request_method = POST)
{
    set $no_cache 1;
}

#Don't cache if the URL contains a query string
if ($query_string != "")
{
    set $no_cache 1;
}

#Don't cache the following URLs
if ($request_uri ~* "/(administrator/|login.php)")
{
    set $no_cache 1;
}

#Don't cache if there is a cookie called PHPSESSID
if ($http_cookie = "PHPSESSID")
{
    set $no_cache 1;
}
</code></pre>
To apply the "$no_cache" variable to the appropriate directives, place the following lines inside <strong>location ~ .php$ { }</strong>
<pre class="code-pre "><code>fastcgi_cache_bypass $no_cache;
fastcgi_no_cache $no_cache;
</code></pre>
The "fasctcgi<em>cache</em>bypass" directive ignores existing cache for requests related to the conditions set by us previously. The "fastcgi<em>no</em>cache" directive doesn't cache the request at all if the specified conditions are met.
<div data-unique="purging-the-cache"></div>
<h2 id="purging-the-cache">Purging the Cache</h2>
The naming convention of the cache is based on the variables we set for the "fastcgi<em>cache</em>key" directive.
<pre class="code-pre "><code>fastcgi_cache_key "$scheme$request_method$host$request_uri";
</code></pre>
According to these variables, when we requested "<a href="http://localhost/time.php">http://localhost/time.php</a>" the following would've been the actual values:
<pre class="code-pre "><code>fastcgi_cache_key "httpGETlocalhost/time.php";
</code></pre>
Passing this string through <a href="http://jesin.tk/tools/md5-encryption-tool/">MD5 hashing</a> would output the following string:
<pre class="code-pre "><code>b777c8adab3ec92cd43756226caf618e
</code></pre>
This will form the filename of the cache as for the subdirectories we entered "levels=1:2." Therefore, the first level of the directory will be named with <strong>1</strong> character from the last of this MD5 string which is <strong>e</strong>; the second level will have the last <strong>2</strong> characters after the first level i.e. <strong>18</strong>. Hence, the entire directory structure of this cache is as follows:
<pre class="code-pre "><code>/etc/nginx/cache/e/18/b777c8adab3ec92cd43756226caf618e
</code></pre>
Based on this cache naming format you can develop a purging script in your favorite language. For this tutorial, I'll provide a simple PHP script which purges the cache of a <strong>POST</strong>ed URL.

<code>/usr/share/nginx/html/purge.php</code>

<strong>Insert</strong>
<pre class="code-pre "><code>&lt;?php
$cache_path = '/etc/nginx/cache/';
$url = parse_url($_POST['url']);
if(!$url)
{
    echo 'Invalid URL entered';
    die();
}
$scheme = $url['scheme'];
$host = $url['host'];
$requesturi = $url['path'];
$hash = md5($scheme.'GET'.$host.$requesturi);
var_dump(unlink($cache_path . substr($hash, -1) . '/' . substr($hash,-3,2) . '/' . $hash));
?&gt;
</code></pre>
Send a POST request to this file with the URL to purge.
<pre class="code-pre "><code>curl -d 'url=http://www.example.com/time.php' http://localhost/purge.php
</code></pre>
The script will output <em>true</em> or <em>false</em> based on whether the cache was purged to not. Make sure to exclude this script from being cached and also restrict access.

Submitted by: <a href="http://jesin.tk/" rel="author">Jesin A</a>

&nbsp;

https://www.digitalocean.com/community/tutorials/how-to-setup-fastcgi-caching-with-nginx-on-your-vps