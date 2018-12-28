---
ID: 1404
post_title: >
  無堅不摧，唯快不破！快改用
  Nginx + PHP-FPM 取代 Apache 吧！
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/05/%e7%84%a1%e5%a0%85%e4%b8%8d%e6%91%a7%ef%bc%8c%e5%94%af%e5%bf%ab%e4%b8%8d%e7%a0%b4%ef%bc%81%e5%bf%ab%e6%94%b9%e7%94%a8-nginx-php-fpm-%e5%8f%96%e4%bb%a3-apache-%e5%90%a7%ef%bc%81/'
published: true
post_date: 2018-11-05 17:10:40
---
<h2>Nginx + PHP-FPM (FastCGI Process Manager)</h2>
太多人詬病於 Apache Server 的效能與承載數，紛紛投向 Event-based Server 的懷抱。而近年來 Nginx 蠻紅的 (Nginx 唸 Engine X)，Nginx 主要是藉由 Non-blocking 與 epool (linux 2.6 支援) 這些特性，大幅提昇了連線服務量與速度。

但是 Nginx 本身只是單純的 HTTP Server，如果需要執行程式，還得藉助 CGI 的幫忙。我們今天要教學的架構就是將最常使用的 Apache + PHP 改為 Nginx + PHP-FPM，示範的作業系統為 Ubuntu Server。<span id="more-3890"></span>
<h2>安裝 Nginx 與 PHP-FPM (傳說中的 LNMP)</h2>
CentOS 6.5 沒有內建 Nginx，需透過 EPEL 進行安裝，命令如下：
<pre>sudo rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
sudo yum install nginx php php-fpm</pre>
Ubuntu Server 安裝起來就簡單多了，直接透過 apt 安裝套件，命令如下：
<pre>sudo apt-get update

sudo apt-get install nginx nginx-extras php5 php5-fpm</pre>
<h2>設定 Nginx</h2>
順利安裝完之後開始修改 Nginx Server 設定檔，如下：
<pre>sudo vim /etc/nginx/nginx.conf</pre>
<div id="crayon-5be00815b8a38325184811" class="crayon-syntax crayon-theme-visual-assist crayon-font-courier-new crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover">
<div class="crayon-main">
介紹幾個重要的參數：</div>
</div>
<ul>
 	<li>user：Nginx Server 啟動所使用的使用者 (ubuntu 預設用 www-data，CentOS 預設用 nginx)</li>
 	<li>pid：ProcessID 存放位置 (CentOS 預設在 /var/run/nginx.pid)</li>
 	<li>worker_processes：開啟的程序數量，請對應 CPU 核心數進行調整（多設無益）</li>
 	<li>worker_cpu_affinity：每個程序所使用的邏輯核心，所以四核可以這樣設「0001 0010 0100 1000」，當然也可以一次設定兩個核心給同一個程序</li>
 	<li>worker_rlimit_nofile：每個程序最高可以開啟的檔案數</li>
 	<li>use epoll：啟動 epoll 會快很多，效果不錯</li>
 	<li>worker_connections：每個程序最高可以開啟的連線數</li>
 	<li>access_log, error_log：HTTP Log 存放的位置</li>
</ul>
接下來設定 Nginx Virtual Host，Ubuntu 請編輯以下檔案：
<blockquote>sudo vim /etc/nginx/sites-available/default</blockquote>
CentOS 則是存放在以下路徑：
<pre>sudo vim /etc/nginx/conf.d/default.conf</pre>
<div id="crayon-5be00815b8a47375959903" class="crayon-syntax crayon-theme-visual-assist crayon-font-courier-new crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover">
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table" style="height: 5px;" width="32">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide"></td>
<td class="crayon-code"></td>
</tr>
</tbody>
</table>
</div>
</div>
&nbsp;
<pre id="crayon-5be00815b8a38325184811-1" class="crayon-line"><span class="crayon-e">user </span><span class="crayon-v">www</span><span class="crayon-o">-</span><span class="crayon-v">data</span><span class="crayon-sy">;</span>

<span class="crayon-v">worker</span><span class="crayon-sy">_</span>processes <span class="crayon-cn">4</span><span class="crayon-sy">;</span>

<span class="crayon-v">worker_cpu</span><span class="crayon-sy">_</span>affinity <span class="crayon-cn">0001</span> <span class="crayon-cn">0010</span> <span class="crayon-cn">0100</span> <span class="crayon-cn">1000</span><span class="crayon-sy">;</span>

<span class="crayon-v">worker_rlimit</span><span class="crayon-sy">_</span>nofile <span class="crayon-cn">1024</span><span class="crayon-sy">;</span>

<span class="crayon-v">pid</span> <span class="crayon-o">/</span><span class="crayon-v">run</span><span class="crayon-o">/</span><span class="crayon-v">nginx</span><span class="crayon-sy">.</span><span class="crayon-v">pid</span><span class="crayon-sy">;</span>




<span class="crayon-e">events</span> <span class="crayon-sy">{</span>
<span class="crayon-h">    </span><span class="crayon-st">use</span> <span class="crayon-v">epoll</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-v">worker</span><span class="crayon-sy">_</span>connections <span class="crayon-cn">2048</span><span class="crayon-sy">;</span>
<span class="crayon-sy">}</span>


<span class="crayon-e">http</span> <span class="crayon-sy">{</span>
<span class="crayon-h">    </span><span class="crayon-e">sendfile </span><span class="crayon-v">on</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-e">tcp_nopush </span><span class="crayon-v">on</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-e">tcp_nodelay </span><span class="crayon-v">on</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-v">keepalive</span><span class="crayon-sy">_</span>timeout <span class="crayon-cn">65</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-v">types_hash_max</span><span class="crayon-sy">_</span>size <span class="crayon-cn">2048</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-v">server_names_hash_bucket</span><span class="crayon-sy">_</span>size <span class="crayon-cn">128</span><span class="crayon-sy">;</span>

<span class="crayon-h">    </span><span class="crayon-v">include</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">nginx</span><span class="crayon-o">/</span><span class="crayon-v">mime</span><span class="crayon-sy">.</span><span class="crayon-v">types</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-e">default_type </span><span class="crayon-v">application</span><span class="crayon-o">/</span><span class="crayon-v">octet</span><span class="crayon-o">-</span><span class="crayon-v">stream</span><span class="crayon-sy">;</span>

<span class="crayon-h">    </span><span class="crayon-v">client_header_buffer</span><span class="crayon-sy">_</span>size <span class="crayon-cn">2k</span><span class="crayon-sy">;</span>
<span class="crayon-h">   </span><span class="crayon-v">large_client_header</span><span class="crayon-sy">_</span>buffers <span class="crayon-cn">4</span> <span class="crayon-cn">4k</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-e">open_file_cache </span><span class="crayon-v">max</span><span class="crayon-o">=</span><span class="crayon-cn">102400</span> <span class="crayon-v">inactive</span><span class="crayon-o">=</span><span class="crayon-cn">20s</span><span class="crayon-sy">;</span>
<span class="crayon-h">  </span><span class="crayon-h">    </span><span class="crayon-v">access_log</span> <span class="crayon-o">/</span><span class="crayon-t">var</span><span class="crayon-o">/</span><span class="crayon-v">log</span><span class="crayon-o">/</span><span class="crayon-v">nginx</span><span class="crayon-o">/</span><span class="crayon-v">access</span><span class="crayon-sy">.</span><span class="crayon-v">log</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-v">error_log</span> <span class="crayon-o">/</span><span class="crayon-t">var</span><span class="crayon-o">/</span><span class="crayon-v">log</span><span class="crayon-o">/</span><span class="crayon-v">nginx</span><span class="crayon-o">/</span><span class="crayon-v">error</span><span class="crayon-sy">.</span><span class="crayon-v">log</span><span class="crayon-sy">;</span>

<span class="crayon-h">    </span><span class="crayon-e">gzip </span><span class="crayon-v">on</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-v">gzip</span><span class="crayon-sy">_</span>disable <span class="crayon-s">"msie6"</span><span class="crayon-sy">;</span>

<span class="crayon-h">    </span><span class="crayon-v">include</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">nginx</span><span class="crayon-o">/</span><span class="crayon-v">conf</span><span class="crayon-sy">.</span><span class="crayon-v">d</span><span class="crayon-o">/</span><span class="crayon-o">*</span><span class="crayon-sy">.</span><span class="crayon-v">conf</span><span class="crayon-sy">;</span>
<span class="crayon-h">    </span><span class="crayon-v">include</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">nginx</span><span class="crayon-o">/</span><span class="crayon-v">sites</span><span class="crayon-o">-</span><span class="crayon-v">enabled</span><span class="crayon-o">/</span><span class="crayon-o">*</span><span class="crayon-sy">;</span>
<span class="crayon-sy">}</span></pre>
先說明幾個主要的參數設定：
<ul>
 	<li>server_name：同 Apache ServerName 可以用來指定 Virtual Host (虛擬主機)</li>
 	<li>root：網頁起始位置</li>
 	<li>server_tokens off：移除 Nginx 版本資訊</li>
</ul>
中間的「location /」Selection 用「<strong>try_files $uri $uri/ /index.php;</strong>」來嘗試讀取開啟，如果檔案不存在就轉為呼叫 index.php，做的事情與常用的 Apache Rewrite Module 差不多，這樣設計主要是為了將 Request 導給 Framework (例如 Codeigniter, Zend Framework 等等)。

最後的「location ~* \.php$」Selection 就是設定要將 .php 檔案直接交由 FPM 來處理，詳細的設定說明可以參考 <a href="http://wiki.nginx.org/Main">Nginx WIKI</a>。fastcgi_pass 所指向的位置是 PHP-FPM 所開啟的服務，接者我們繼續設定 PHP-FPM。
<h2>設定 PHP-FPM</h2>
Ubuntu 請編輯 /etc/php5/fpm/pool.d/www.conf 檔案
<pre>sudo vim /etc/php5/fpm/pool.d/www.conf</pre>
CentOS 則放在 /etc/php-fpm.d/www.conf
<pre>sudo vim /etc/php-fpm.d/www.conf</pre>
編輯內容如下：
<div id="crayon-5be00815b8a4e894011513" class="crayon-syntax crayon-theme-visual-assist crayon-font-courier-new crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover">
<pre class="crayon-plain-wrap">

</pre>
</div>
<pre><span class="crayon-sy">[</span><span class="crayon-v">www</span><span class="crayon-sy">]</span>
<span class="crayon-v">user</span> <span class="crayon-o">=</span> <span class="crayon-v">www</span><span class="crayon-o">-</span><span class="crayon-e">data</span>
<span class="crayon-v">group</span> <span class="crayon-o">=</span> <span class="crayon-v">www</span><span class="crayon-o">-</span><span class="crayon-e">data</span>
<span class="crayon-v">listen</span> <span class="crayon-o">=</span> <span class="crayon-cn">127.0.0.1</span><span class="crayon-o">:</span><span class="crayon-cn">9000</span>
<span class="crayon-v">listen</span><span class="crayon-sy">.</span><span class="crayon-v">backlog</span> <span class="crayon-o">=</span> <span class="crayon-cn">65535</span>
<span class="crayon-v">listen</span><span class="crayon-sy">.</span><span class="crayon-v">owner</span> <span class="crayon-o">=</span> <span class="crayon-v">www</span><span class="crayon-o">-</span><span class="crayon-e">data</span>
<span class="crayon-v">listen</span><span class="crayon-sy">.</span><span class="crayon-v">group</span> <span class="crayon-o">=</span> <span class="crayon-v">www</span><span class="crayon-o">-</span><span class="crayon-e">data</span>


<span class="crayon-v">request_terminate_timeout</span> <span class="crayon-o">=</span> <span class="crayon-cn">600s</span>
<span class="crayon-v">pm</span> <span class="crayon-o">=</span> <span class="crayon-e">dynamic</span>
<span class="crayon-v">pm</span><span class="crayon-sy">.</span><span class="crayon-v">max_children</span> <span class="crayon-o">=</span> <span class="crayon-cn">5</span>
<span class="crayon-v">pm</span><span class="crayon-sy">.</span><span class="crayon-v">start_servers</span> <span class="crayon-o">=</span> <span class="crayon-cn">2</span>
<span class="crayon-v">pm</span><span class="crayon-sy">.</span><span class="crayon-v">min_spare_servers</span> <span class="crayon-o">=</span> <span class="crayon-cn">1</span>
<span class="crayon-v">pm</span><span class="crayon-sy">.</span><span class="crayon-v">max_spare_servers</span> <span class="crayon-o">=</span> <span class="crayon-cn">3</span></pre>
上述的 listen 就是要讓前面 Nginx fastcgi_pass 來呼叫的服務，可以透過 TCP Socket 或者是 UNIX Kernel Socket，Kernel Socket 速度會比較快，但是經過壓力測試後 Kernel Socket 反而常常會掉包，穩定性不如 TCP Socket 來的優異。其中 <strong>request_terminate_timeout</strong> 設定也相當重要，表示 PHP 的執行時間，超過這個週期就會結束，設太短常常遇到檔案上傳時間比較久就 GG 了。

最後當然要啟動 Nginx 與 PHP-FPM 服務，如下：
<pre>sudo service php-fpm restart
sudo service nginx restart</pre>
成功後快開啟瀏覽器試看看吧，我在 CentOS 裝起來的 Nginx Welcome 畫面如下：

<a class="image-anchor" href="http://blog.toright.com/posts/3890/%e7%84%a1%e5%a0%85%e4%b8%8d%e6%91%a7%ef%bc%8c%e5%94%af%e5%bf%ab%e4%b8%8d%e7%a0%b4%ef%bc%81%e5%bf%ab%e6%94%b9%e7%94%a8-nginx-php-fpm-%e5%8f%96%e4%bb%a3-apache-%e5%90%a7%ef%bc%81.html/nginx-welcome" rel="attachment wp-att-3907"><img class="alignnone wp-image-3907 size-full" src="https://i0.wp.com/blog.toright.com/wp-content/uploads/2014/09/nginx-welcome.png?resize=598%2C462" sizes="(max-width: 598px) 100vw, 598px" srcset="https://i0.wp.com/blog.toright.com/wp-content/uploads/2014/09/nginx-welcome.png?w=598&amp;ssl=1 598w, https://i0.wp.com/blog.toright.com/wp-content/uploads/2014/09/nginx-welcome.png?resize=300%2C231&amp;ssl=1 300w" alt="nginx-welcome" width="598" height="462" /></a>
<h2>感想</h2>
Nginx 整體來說安裝是蠻容易的，但要調校的好又是另外一件事了，如果設定的不洽當，在高流量時 Nginx 常會送你 502 Bad Gateway。剛開始花蠻多時間嘗試調整參數，搭配效能測試工具來調整，讓伺服器發揮最大效用。

最前面 Ubuntu 有多裝了 nginx-extras 套件，其實是用來擴充 Nginx 功能，如同 Apache Module，例如可以透過 <strong>more_clear_headers</strong> 設定來移除一些不必要的 HTTP Header 資訊。像是透過以下參數就可以移除 HTTP Response Header 中 Nginx 與 PHP 的版本資訊。
<div id="crayon-5be00815b8a54967471843" class="crayon-syntax crayon-theme-visual-assist crayon-font-courier-new crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover">
<div class="crayon-main">
<pre id="crayon-5be00815b8a54967471843-1" class="crayon-line"><span class="crayon-v">more_clear</span><span class="crayon-sy">_</span>headers <span class="crayon-s">'Server'</span><span class="crayon-sy">;</span>

<span class="crayon-v">more_clear</span><span class="crayon-sy">_</span>headers <span class="crayon-s">'X-Powered-By'</span><span class="crayon-sy">;</span></pre>
</div>
</div>
&nbsp;

https://blog.toright.com/posts/3890/%E7%84%A1%E5%A0%85%E4%B8%8D%E6%91%A7%EF%BC%8C%E5%94%AF%E5%BF%AB%E4%B8%8D%E7%A0%B4%EF%BC%81%E5%BF%AB%E6%94%B9%E7%94%A8-nginx-php-fpm-%E5%8F%96%E4%BB%A3-apache-%E5%90%A7%EF%BC%81.html