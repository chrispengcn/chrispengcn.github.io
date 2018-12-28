---
ID: 1168
post_title: >
  nginx使用ssl模块配置支持HTTPS访问
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/08/nginx%e4%bd%bf%e7%94%a8ssl%e6%a8%a1%e5%9d%97%e9%85%8d%e7%bd%ae%e6%94%af%e6%8c%81https%e8%ae%bf%e9%97%ae/'
published: true
post_date: 2018-05-08 20:30:14
---
<h1 class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="http://www.cnblogs.com/saneri/p/5391821.html">nginx使用ssl模块配置支持HTTPS访问</a></h1>
<div class="clear"></div>
<div class="postBody">
<div id="cnblogs_post_body" class="blogpost-body">

默认情况下ssl模块并未被安装，如果要使用该模块则需要在编译nginx时指定–with-http_ssl_module参数.

需求：
做一个网站域名为 www.localhost.cn 要求通过https://www.localhost.cn进行访问.

10.10.100.8 www.localhost.cn

实验步骤:

1.首先确保机器上安装了openssl和openssl-devel
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_488579" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp preprocessor">#yum install openssl</code></div>
<div class="line number2 index1 alt1"><code class="csharp preprocessor">#yum install openssl-devel</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
2.创建服务器私钥，命令会让你输入一个口令：
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_537239" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">openssl genrsa -des3 -</code><code class="csharp keyword">out</code> <code class="csharp plain">server.key 1024  </code><code class="csharp comments">//生成私钥</code></div>
<div class="line number2 index1 alt1"><code class="csharp preprocessor">#因为以后要给nginx使用.每次reload nginx配置时候都要你验证这个PAM密码的.由于生成时候必须输入密码,你可以输入后 再删掉。</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
3.创建签名请求的证书（CSR）：
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_244190" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">openssl req -</code><code class="csharp keyword">new</code> <code class="csharp plain">-key server.key -</code><code class="csharp keyword">out</code> <code class="csharp plain">server.csr                                </code><code class="csharp comments">//生成证书颁发机构,用于颁发公钥</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
4.在加载SSL支持的Nginx并使用上述私钥时除去必须的口令：
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_744140" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">cp server.key server.key.org</code></div>
<div class="line number2 index1 alt1"><code class="csharp plain">openssl rsa -</code><code class="csharp keyword">in</code> <code class="csharp plain">server.key.org -</code><code class="csharp keyword">out</code> <code class="csharp plain">server.key                                  </code><code class="csharp comments">//除去密码以便reload询问时不需要密码</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
5.配置nginx
最后标记证书使用上述私钥和CSR：
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_740899" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">openssl x509 -req -days 365 -</code><code class="csharp keyword">in</code> <code class="csharp plain">server.csr -signkey server.key -</code><code class="csharp keyword">out</code> <code class="csharp plain">server.crt</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
6.修改Nginx配置文件，让其包含新标记的证书和私钥：
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_379899" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp preprocessor">#vim /usr/local/nginx/conf/nginx.conf</code></div>
<div class="line number2 index1 alt1"><code class="csharp plain">http {</code></div>
<div class="line number3 index2 alt2"></div>
<div class="line number4 index3 alt1"><code class="csharp spaces">        </code><code class="csharp plain">include server/*.cn;</code></div>
<div class="line number5 index4 alt2"><code class="csharp plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
7.修改Nginx配置文件，让其包含新标记的证书和私钥：
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_452476" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div>
<div class="line number13 index12 alt2">13</div>
<div class="line number14 index13 alt1">14</div>
<div class="line number15 index14 alt2">15</div>
<div class="line number16 index15 alt1">16</div>
<div class="line number17 index16 alt2">17</div>
<div class="line number18 index17 alt1">18</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp preprocessor">#vim /usr/local/nginx/server/www.localhost.cn</code></div>
<div class="line number2 index1 alt1"><code class="csharp plain">server {</code></div>
<div class="line number3 index2 alt2"><code class="csharp spaces">        </code><code class="csharp plain">listen       443;                                                                       </code><code class="csharp comments">//监听端口为443</code></div>
<div class="line number4 index3 alt1"><code class="csharp spaces">        </code><code class="csharp plain">server_name  www.localhost.cn;</code></div>
<div class="line number5 index4 alt2"><code class="csharp spaces"> </code></div>
<div class="line number6 index5 alt1"><code class="csharp spaces">        </code><code class="csharp plain">ssl                  </code><code class="csharp keyword">on</code><code class="csharp plain">;        　　　　　　　　　　</code><code class="csharp comments">//开启ssl</code></div>
<div class="line number7 index6 alt2"><code class="csharp spaces">        </code><code class="csharp plain">ssl_certificate      /etc/pki/tls/certs/server.crt;      </code><code class="csharp comments">//证书位置</code></div>
<div class="line number8 index7 alt1"><code class="csharp spaces">        </code><code class="csharp plain">ssl_certificate_key  /etc/pki/tls/certs/server.key;      </code><code class="csharp comments">//私钥位置</code></div>
<div class="line number9 index8 alt2"><code class="csharp spaces">        </code><code class="csharp plain">ssl_session_timeout  5m;</code></div>
<div class="line number10 index9 alt1"><code class="csharp spaces">        </code><code class="csharp plain">ssl_protocols  SSLv2 SSLv3 TLSv1;       　　　　 </code><code class="csharp comments">//指定密码为openssl支持的格式</code></div>
<div class="line number11 index10 alt2"><code class="csharp spaces">        </code><code class="csharp plain">ssl_ciphers  HIGH:!aNULL:!MD5;              </code><code class="csharp comments">//密码加密方式</code></div>
<div class="line number12 index11 alt1"><code class="csharp spaces">        </code><code class="csharp plain">ssl_prefer_server_ciphers   </code><code class="csharp keyword">on</code><code class="csharp plain">;             </code><code class="csharp comments">//依赖SSLv3和TLSv1协议的服务器密码将优先于客户端密码</code></div>
<div class="line number13 index12 alt2"><code class="csharp spaces"> </code></div>
<div class="line number14 index13 alt1"><code class="csharp spaces">        </code><code class="csharp plain">location / {</code></div>
<div class="line number15 index14 alt2"><code class="csharp spaces">            </code><code class="csharp plain">root   html;                        </code><code class="csharp comments">//根目录的相对位置</code></div>
<div class="line number16 index15 alt1"><code class="csharp spaces">            </code><code class="csharp plain">index  index.html index.htm;</code></div>
<div class="line number17 index16 alt2"><code class="csharp spaces">        </code><code class="csharp plain">}</code></div>
<div class="line number18 index17 alt1"><code class="csharp spaces">    </code><code class="csharp plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
8.启动nginx服务器.
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_661575" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp preprocessor">#/usr/local/nginx/sbin/nginx -s reload //如果环境允许的话直接杀掉进程在启动nginx</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
如果出现“[emerg] 10464#0: unknown directive "ssl" in /usr/local/nginx-0.6.32/conf/nginx.conf:74”则说明没有将ssl模块编译进nginx，在configure的时候加上“--with-http_ssl_module”即可
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_198875" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">如:[root@localhost nginx-1.4.4]# ./configure --prefix=/usr/local/nginx --user=www --</code><code class="csharp keyword">group</code><code class="csharp plain">=www --with-http_stub_status_module --with-http_ssl_module</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
9.测试网站是否能够通过https访问
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_857547" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">https:</code><code class="csharp comments">//www.localhost.cn</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
另外还可以加入如下代码实现80端口重定向到443
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_56786" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">server {</code></div>
<div class="line number2 index1 alt1"><code class="csharp plain">listen 80;</code></div>
<div class="line number3 index2 alt2"><code class="csharp plain">server_name www.localhost.cn;</code></div>
<div class="line number4 index3 alt1"><code class="csharp preprocessor">#rewrite ^(.*) https://$server_name$1 permanent;</code></div>
<div class="line number5 index4 alt2"><code class="csharp plain">rewrite ^(.*)$  https:</code><code class="csharp comments">//$host$1 permanent;</code></div>
<div class="line number6 index5 alt1"><code class="csharp plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
过以下配置，可以设置一个虚拟主机同时支持HTTP和HTTPS
<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_827150" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">listen 80;</code></div>
<div class="line number2 index1 alt1"><code class="csharp plain">listen 443 </code><code class="csharp keyword">default</code> <code class="csharp plain">ssl;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<p class="title"><strong>同时支持80和443同时访问配置:</strong></p>

<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_772322" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div>
<div class="line number5 index4 alt2">5</div>
<div class="line number6 index5 alt1">6</div>
<div class="line number7 index6 alt2">7</div>
<div class="line number8 index7 alt1">8</div>
<div class="line number9 index8 alt2">9</div>
<div class="line number10 index9 alt1">10</div>
<div class="line number11 index10 alt2">11</div>
<div class="line number12 index11 alt1">12</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">server {</code></div>
<div class="line number2 index1 alt1"><code class="csharp spaces">    </code><code class="csharp plain">listen      80 </code><code class="csharp keyword">default</code> <code class="csharp plain">backlog=2048;</code></div>
<div class="line number3 index2 alt2"><code class="csharp spaces">    </code><code class="csharp plain">listen      443 ssl;</code></div>
<div class="line number4 index3 alt1"><code class="csharp spaces">    </code><code class="csharp plain">server_name  www.localhost.com;</code></div>
<div class="line number5 index4 alt2"><code class="csharp spaces">   </code></div>
<div class="line number6 index5 alt1"><code class="csharp spaces">    </code><code class="csharp preprocessor">#ssl on;  //注释掉</code></div>
<div class="line number7 index6 alt2"><code class="csharp spaces">    </code><code class="csharp plain">ssl_certificate   /usr/local/https/www.localhost.com.crt;</code></div>
<div class="line number8 index7 alt1"><code class="csharp spaces">    </code><code class="csharp plain">ssl_certificate_key  /usr/local/https/www.localhost.com.key;</code></div>
<div class="line number9 index8 alt2"><code class="csharp spaces">    </code><code class="csharp plain">ssl_session_timeout 5m;</code></div>
<div class="line number10 index9 alt1"><code class="csharp spaces">    </code><code class="csharp plain">ssl_protocols TLSv1 TLSv1.1 TLSv1.2;</code></div>
<div class="line number11 index10 alt2"><code class="csharp spaces">    </code><code class="csharp plain">ssl_ciphers AESGCM:ALL:!DH:!EXPORT:!RC4:+HIGH:!MEDIUM:!LOW:!aNULL:!eNULL;</code></div>
<div class="line number12 index11 alt1"><code class="csharp spaces">    </code><code class="csharp plain">ssl_prefer_server_ciphers </code><code class="csharp keyword">on</code><code class="csharp plain">;</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
<p class="title"><strong>Nginx 设置忽略favicon.ico文件的404错误日志(关闭favicon.ico不存在时记录日志)</strong></p>
<p class="title">在 server { … }内添加如下信息.</p>

<div class="cnblogs_Highlighter sh-gutter">
<div>
<div id="highlighter_387491" class="syntaxhighlighter  csharp">
<table border="0" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td class="gutter">
<div class="line number1 index0 alt2">1</div>
<div class="line number2 index1 alt1">2</div>
<div class="line number3 index2 alt2">3</div>
<div class="line number4 index3 alt1">4</div></td>
<td class="code">
<div class="container">
<div class="line number1 index0 alt2"><code class="csharp plain">location = /favicon.ico {</code></div>
<div class="line number2 index1 alt1"><code class="csharp plain">log_not_found off;</code></div>
<div class="line number3 index2 alt2"><code class="csharp plain">access_log off;</code></div>
<div class="line number4 index3 alt1"><code class="csharp plain">}</code></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
</div>
</div>
</div>