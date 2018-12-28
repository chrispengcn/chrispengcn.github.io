---
ID: 1350
post_title: >
  在CentOS
  7上安装带有ngx_pagespeed的Nginx
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/09/10/%e5%9c%a8centos-7%e4%b8%8a%e5%ae%89%e8%a3%85%e5%b8%a6%e6%9c%89ngx_pagespeed%e7%9a%84nginx/'
published: true
post_date: 2018-09-10 12:05:04
---
<h2>在CentOS 7上安装带有ngx_pagespeed的Nginx</h2>
<div class="w3-panel w3-leftbar w3-light-grey post-title-bottom-tag">

Ngx-pagespeed是一个免费的开源Nginx模块，可用于加快您的站点速度，并减少页面加载时间。它可以自动应用...

</div>
<div class="w3-row post-title-bottom-tag">
<div class="w3-col">

<span class="post-tag-line-height post-tag w3-tag w3-left w3-small w3-green">分类:</span><a href="https://www.howtoing.com/category/centos"><span class="post-tag-line-height post-tag w3-tag w3-left w3-small w3-green">CentOS系统</span></a><a href="https://www.howtoing.com/category/linux"><span class="post-tag-line-height post-tag w3-tag w3-left w3-small w3-green">Linux介绍</span></a><a href="https://www.howtoing.com/category/operating-system"><span class="post-tag-line-height post-tag w3-tag w3-left w3-small w3-green">操作系统</span></a><a href="https://www.howtoing.com/category/server"><span class="post-tag-line-height post-tag w3-tag w3-left w3-small w3-green">服务器</span></a><a href="https://www.howtoing.com/category/web-server"><span class="post-tag-line-height post-tag w3-tag w3-left w3-small w3-green">web server</span></a>

</div>
<div class="w3-col">

<span class="post-tag-line-height index-post-bottom-line-word w3-right"><i class="fa fa-clock-o" aria-hidden="true"> 2017-06-27 00:00:00</i></span>

</div>
</div>
<div class="w3-panel post-title-bottom-tag">
<div></div>
<div id="htfContentPage">

Ngx-pagespeed是一个免费的开源Nginx模块，可用于加快您的站点速度，并减少页面加载时间。 它可以通过自动将Web性能最佳做法应用于页面和关联资产，而无需修改现有内容或工作流。 您可以使用Ngx-pagepeed模块轻松优化各种文件，如CSS，HTML，png和jpg。

Ngx-pagepeed带有很多功能，其中一些功能如下所示：
<ul>
 	<li>支持图像动态调整大小，重新压缩和优化。</li>
 	<li>小资源内联。</li>
 	<li>HTML重写</li>
 	<li>缓存生存期延长。</li>
 	<li>延迟JavaScript和图像加载。</li>
</ul>
在本教程中，我们将讨论如何在CentOS 7服务器上使用ngx_pagespeed模块编译Nginx。

<strong>要求</strong>
<ul>
 	<li>您的系统上安装了新鲜的CentOS 7服务器。</li>
 	<li>具有root权限的Sudo用户。</li>
</ul>
<h2 id="-update-the-system">1更新系统</h2>
我们先从您的系统更新到最新的稳定版本。 您可以通过运行以下命令来执行此操作：
<p class="command">sudo yum update -y</p>
一旦您的系统更新，请重新启动系统并使用sudo用户登录。
<h2 id="-install-required-dependencies">2安装所需的依赖关系</h2>
您将需要安装一些软件包才能编译Nginx。 您可以通过运行以下命令来安装所有这些依赖项：
<p class="command">sudo yum install gcc cmake unzip wget gcc-c++ pcre-devel zlib-devel -y</p>
一旦安装了所有必需的依赖项，可以继续下一步。
<h2 id="-compile-nginx-with-ngxpagespeed">3用Ngx-pagepeed编译Nginx</h2>
<ins class="adsbygoogle" data-ad-layout="in-article" data-ad-format="fluid" data-ad-client="ca-pub-7163569408396951" data-ad-slot="7452267897" data-adsbygoogle-status="done"><ins id="aswift_1_expand"><ins id="aswift_1_anchor"></ins></ins></ins>要使用ngx-pagespeed模块安装Nginx，您需要从源代码编译Nginx。 首先，您将需要下载Nginx源码。 您可以通过运行以下命令来下载它：
<p class="command">wget http://nginx.org/download/nginx-1.12.0.tar.gz</p>
要用ngx_pagespeed模块编译Nginx，还需要下载ngx_pagespeed源码包。 您可以使用以下命令下载它：
<p class="command">wget https://github.com/pagespeed/ngx_pagespeed/archive/v1.12.34.2-stable.zip</p>
一旦两个软件包都下载，请使用以下命令提取它们：
<p class="command">tar -xvzf nginx-1.12.0.tar.gz
tar -xvzf v1.12.34.2-stable.zip</p>
接下来，您还需要下载PageSpeed优化库来编译Nginx。 您可以使用以下命令下载它：
<p class="command">cd ngx_pagespeed-1.12.34.2-stable
wget https://dl.google.com/dl/page-speed/psol/1.12.34.2-x64.tar.gz
tar -xvzf 1.12.34.2-x64.tar.gz
cd ..</p>
现在，我们开始用pagespeed模块编译Nginx。

首先，使用以下命令将目录更改为Nginx源目录：
<p class="command">cd nginx-1.12.0</p>
接下来，使用以下命令配置Nginx源：
<p class="command">sudo ./configure --add-module=$HOME/ngx_pagespeed-1.12.34.2-stable --user=nobody --group=nobody --pid-path=/var/run/nginx.pid ${PS_NGX_EXTRA_FLAGS}</p>
配置完成后，通过运行以下命令编译Nginx：
<p class="command">sudo make</p>
上述命令将需要几分钟的时间。 之后，您可以通过运行以下命令来安装nginx：
<p class="command">sudo make install</p>
一旦安装了Nginx，您将需要为Nginx创建一个符号链接：
<p class="command">sudo ln -s /usr/local/nginx/conf/ /etc/nginx
sudo ln -s /usr/local/nginx/sbin/nginx /usr/sbin/nginx</p>

<h2 id="-create-nginx-startup-script">4创建Nginx启动脚本</h2>
您还需要为Nginx创建一个启动脚本来停止并启动Nginx。 你可以通过在/etc/init.d目录下创建nginx文件：
<p class="command">sudo nano /etc/init.d/nginx</p>
添加以下行：
<pre class="prettyprint prettyprinted"><span class="com">#!/bin/sh</span>
<span class="com">#</span>
<span class="com"># nginx - this script starts and stops the nginx daemon</span>
<span class="com">#</span>
<span class="com"># chkconfig:   - 85 15</span>
<span class="com"># description:  NGINX is an HTTP(S) server, HTTP(S) reverse \</span>
<span class="com">#               proxy and IMAP/POP3 proxy server</span>
<span class="com"># processname: nginx</span>
<span class="com"># config:      /etc/nginx/nginx.conf</span>
<span class="com"># config:      /etc/sysconfig/nginx</span>
<span class="com"># pidfile:     /var/run/nginx.pid</span>

<span class="com"># Source function library.</span>
<span class="pun">.</span> <span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">rc</span><span class="pun">.</span><span class="pln">d</span><span class="pun">/</span><span class="pln">init</span><span class="pun">.</span><span class="pln">d</span><span class="pun">/</span><span class="pln">functions

</span><span class="com"># Source networking configuration.</span>
<span class="pun">.</span> <span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">sysconfig</span><span class="pun">/</span><span class="pln">network

</span><span class="com"># Check that networking is up.</span>
<span class="pun">[</span> <span class="str">"$NETWORKING"</span> <span class="pun">=</span> <span class="str">"no"</span> <span class="pun">]</span> <span class="pun">&amp;&amp;</span> <span class="kwd">exit</span> <span class="lit">0</span><span class="pln">

nginx</span><span class="pun">=</span><span class="str">"/usr/sbin/nginx"</span><span class="pln">
prog</span><span class="pun">=</span><span class="pln">$</span><span class="pun">(</span><span class="pln">basename $nginx</span><span class="pun">)</span><span class="pln">

NGINX_CONF_FILE</span><span class="pun">=</span><span class="str">"/etc/nginx/nginx.conf"</span>

<span class="pun">[</span> <span class="pun">-</span><span class="pln">f </span><span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">sysconfig</span><span class="pun">/</span><span class="pln">nginx </span><span class="pun">]</span> <span class="pun">&amp;&amp;</span> <span class="pun">.</span> <span class="pun">/</span><span class="pln">etc</span><span class="pun">/</span><span class="pln">sysconfig</span><span class="pun">/</span><span class="pln">nginx

lockfile</span><span class="pun">=</span><span class="str">/var/</span><span class="kwd">lock</span><span class="pun">/</span><span class="pln">subsys</span><span class="pun">/</span><span class="pln">nginx

make_dirs</span><span class="pun">()</span> <span class="pun">{</span>
   <span class="com"># make required directories</span><span class="pln">
   user</span><span class="pun">=</span><span class="str">`$nginx -V 2&gt;&amp;1 | grep "configure arguments:.*--user=" | sed 's/[^*]*--user=\([^ ]*\).*/\1/g' -`</span>
   <span class="kwd">if</span> <span class="pun">[</span> <span class="pun">-</span><span class="pln">n </span><span class="str">"$user"</span> <span class="pun">];</span> <span class="kwd">then</span>
      <span class="kwd">if</span> <span class="pun">[</span> <span class="pun">-</span><span class="pln">z </span><span class="str">"`grep $user /etc/passwd`"</span> <span class="pun">];</span> <span class="kwd">then</span><span class="pln">
         useradd </span><span class="pun">-</span><span class="pln">M </span><span class="pun">-</span><span class="pln">s </span><span class="pun">/</span><span class="pln">bin</span><span class="pun">/</span><span class="pln">nologin $user
      </span><span class="kwd">fi</span><span class="pln">
      options</span><span class="pun">=</span><span class="str">`$nginx -V 2&gt;&amp;1 | grep 'configure arguments:'`</span>
      <span class="kwd">for</span><span class="pln"> opt </span><span class="kwd">in</span><span class="pln"> $options</span><span class="pun">;</span> <span class="kwd">do</span>
          <span class="kwd">if</span> <span class="pun">[</span> <span class="str">`echo $opt | grep '.*-temp-path'`</span> <span class="pun">];</span> <span class="kwd">then</span><span class="pln">
              value</span><span class="pun">=</span><span class="str">`echo $opt | cut -d "=" -f 2`</span>
              <span class="kwd">if</span> <span class="pun">[</span> <span class="pun">!</span> <span class="pun">-</span><span class="pln">d </span><span class="str">"$value"</span> <span class="pun">];</span> <span class="kwd">then</span>
                  <span class="com"># echo "creating" $value</span><span class="pln">
                  mkdir </span><span class="pun">-</span><span class="pln">p $value </span><span class="pun">&amp;&amp;</span><span class="pln"> chown </span><span class="pun">-</span><span class="pln">R $user $value
              </span><span class="kwd">fi</span>
          <span class="kwd">fi</span>
       <span class="kwd">done</span>
    <span class="kwd">fi</span>
<span class="pun">}</span><span class="pln">

start</span><span class="pun">()</span> <span class="pun">{</span>
    <span class="pun">[</span> <span class="pun">-</span><span class="pln">x $nginx </span><span class="pun">]</span> <span class="pun">||</span> <span class="kwd">exit</span> <span class="lit">5</span>
    <span class="pun">[</span> <span class="pun">-</span><span class="pln">f $NGINX_CONF_FILE </span><span class="pun">]</span> <span class="pun">||</span> <span class="kwd">exit</span> <span class="lit">6</span><span class="pln">
    make_dirs
    echo </span><span class="pun">-</span><span class="pln">n $</span><span class="str">"Starting $prog: "</span><span class="pln">
    daemon $nginx </span><span class="pun">-</span><span class="pln">c $NGINX_CONF_FILE
    retval</span><span class="pun">=</span><span class="pln">$</span><span class="pun">?</span><span class="pln">
    echo
    </span><span class="pun">[</span><span class="pln"> $retval </span><span class="pun">-</span><span class="pln">eq </span><span class="lit">0</span> <span class="pun">]</span> <span class="pun">&amp;&amp;</span><span class="pln"> touch $lockfile
    </span><span class="kwd">return</span><span class="pln"> $retval
</span><span class="pun">}</span><span class="pln">

stop</span><span class="pun">()</span> <span class="pun">{</span><span class="pln">
    echo </span><span class="pun">-</span><span class="pln">n $</span><span class="str">"Stopping $prog: "</span><span class="pln">
    killproc $prog </span><span class="pun">-</span><span class="pln">QUIT
    retval</span><span class="pun">=</span><span class="pln">$</span><span class="pun">?</span><span class="pln">
    echo
    </span><span class="pun">[</span><span class="pln"> $retval </span><span class="pun">-</span><span class="pln">eq </span><span class="lit">0</span> <span class="pun">]</span> <span class="pun">&amp;&amp;</span><span class="pln"> rm </span><span class="pun">-</span><span class="pln">f $lockfile
    </span><span class="kwd">return</span><span class="pln"> $retval
</span><span class="pun">}</span><span class="pln">

restart</span><span class="pun">()</span> <span class="pun">{</span><span class="pln">
    configtest </span><span class="pun">||</span> <span class="kwd">return</span><span class="pln"> $</span><span class="pun">?</span><span class="pln">
    stop
    sleep </span><span class="lit">1</span><span class="pln">
    start
</span><span class="pun">}</span><span class="pln">

reload</span><span class="pun">()</span> <span class="pun">{</span><span class="pln">
    configtest </span><span class="pun">||</span> <span class="kwd">return</span><span class="pln"> $</span><span class="pun">?</span><span class="pln">
    echo </span><span class="pun">-</span><span class="pln">n $</span><span class="str">"Reloading $prog: "</span><span class="pln">
    killproc $nginx </span><span class="pun">-</span><span class="pln">HUP
    RETVAL</span><span class="pun">=</span><span class="pln">$</span><span class="pun">?</span><span class="pln">
    echo
</span><span class="pun">}</span><span class="pln">

force_reload</span><span class="pun">()</span> <span class="pun">{</span><span class="pln">
    restart
</span><span class="pun">}</span><span class="pln">

configtest</span><span class="pun">()</span> <span class="pun">{</span><span class="pln">
  $nginx </span><span class="pun">-</span><span class="pln">t </span><span class="pun">-</span><span class="pln">c $NGINX_CONF_FILE
</span><span class="pun">}</span><span class="pln">

rh_status</span><span class="pun">()</span> <span class="pun">{</span><span class="pln">
    status $prog
</span><span class="pun">}</span><span class="pln">

rh_status_q</span><span class="pun">()</span> <span class="pun">{</span><span class="pln">
    rh_status </span><span class="pun">&gt;</span><span class="str">/dev/</span><span class="kwd">null</span> <span class="lit">2</span><span class="pun">&gt;&amp;</span><span class="lit">1</span>
<span class="pun">}</span>

<span class="kwd">case</span> <span class="str">"$1"</span> <span class="kwd">in</span><span class="pln">
    start</span><span class="pun">)</span><span class="pln">
        rh_status_q </span><span class="pun">&amp;&amp;</span> <span class="kwd">exit</span> <span class="lit">0</span><span class="pln">
        $1
        </span><span class="pun">;;</span><span class="pln">
    stop</span><span class="pun">)</span><span class="pln">
        rh_status_q </span><span class="pun">||</span> <span class="kwd">exit</span> <span class="lit">0</span><span class="pln">
        $1
        </span><span class="pun">;;</span><span class="pln">
    restart</span><span class="pun">|</span><span class="pln">configtest</span><span class="pun">)</span><span class="pln">
        $1
        </span><span class="pun">;;</span><span class="pln">
    reload</span><span class="pun">)</span><span class="pln">
        rh_status_q </span><span class="pun">||</span> <span class="kwd">exit</span> <span class="lit">7</span><span class="pln">
        $1
        </span><span class="pun">;;</span><span class="pln">
    force</span><span class="pun">-</span><span class="pln">reload</span><span class="pun">)</span><span class="pln">
        force_reload
        </span><span class="pun">;;</span><span class="pln">
    status</span><span class="pun">)</span><span class="pln">
        rh_status
        </span><span class="pun">;;</span><span class="pln">
    condrestart</span><span class="pun">|</span><span class="kwd">try</span><span class="pun">-</span><span class="pln">restart</span><span class="pun">)</span><span class="pln">
        rh_status_q </span><span class="pun">||</span> <span class="kwd">exit</span> <span class="lit">0</span>
            <span class="pun">;;</span>
    <span class="pun">*)</span><span class="pln">
        echo $</span><span class="str">"Usage: $0 {start|stop|status|restart|condrestart|try-restart|reload|force-reload|configtest}"</span>
        <span class="kwd">exit</span> <span class="lit">2</span>
<span class="kwd">esac</span></pre>
完成后保存并关闭文件，然后给予执行权限：
<p class="command">sudo chmod +x /etc/init.d/nginx</p>
现在启动nginx服务，并使用以下命令启动它在启动时启动：
<p class="command">sudo systemctl start nginx
sudo systemctl enable nginx</p>
完成后，您可以继续下一步。
<h2 id="-configure-nginx">5配置Nginx</h2>
<ins class="adsbygoogle" data-ad-layout="in-article" data-ad-format="fluid" data-ad-client="ca-pub-7163569408396951" data-ad-slot="7452267897" data-adsbygoogle-status="done"><ins id="aswift_2_expand"><ins id="aswift_2_anchor"></ins></ins></ins>现在，Nginx安装了ngx-pagepeed支持。 默认情况下，PageSpeed已禁用。 启用它之前，建议您测试您的网站速度。 接下来，您将需要为ngx-pagespeed创建缓存目录，并为Nginx分配所有权：
<p class="command">sudo mkdir -p /var/ngx_pagespeed_cache
sudo chown -R nobody:nobody /var/ngx_pagespeed_cache</p>
接下来，您需要在/etc/nginx/nginx.conf文件中进行一些更改。 您可以通过运行以下命令来执行此操作：
<p class="command">sudo nano /etc/nginx/nginx.conf</p>
在服务器块之间添加以下代码：
<pre class="prettyprint prettyprinted"><span class="com"># Pagespeed main settings</span><span class="pln">

pagespeed on</span><span class="pun">;</span><span class="pln">
pagespeed </span><span class="typ">FileCachePath</span> <span class="pun">/</span><span class="kwd">var</span><span class="pun">/</span><span class="pln">ngx_pagespeed_cache</span><span class="pun">;</span>

<span class="com"># Ensure requests for pagespeed optimized resources go to the pagespeed</span>
<span class="com"># handler and no extraneous headers get set.</span><span class="pln">

location </span><span class="pun">~</span> <span class="str">"\.pagespeed\.([a-z]\.)?[a-z]{2}\.[^.]{10}\.[^.]+"</span> <span class="pun">{</span><span class="pln"> add_header </span><span class="str">""</span> <span class="str">""</span><span class="pun">;</span> <span class="pun">}</span><span class="pln">
location </span><span class="pun">~</span> <span class="str">"^/ngx_pagespeed_static/"</span> <span class="pun">{</span> <span class="pun">}</span><span class="pln">
location </span><span class="pun">~</span> <span class="str">"^/ngx_pagespeed_beacon"</span> <span class="pun">{</span> <span class="pun">}</span></pre>
注意：如果您正在使用虚拟主机，请将以上pagepeed指令添加到每个服务器块配置文件，以便在每个网站上启用pagespeed。

完成后保存并关闭文件，然后使用以下命令检查Nginx是否有任何配置错误：
<p class="command">sudo nginx -t</p>
如果一切正常，您应该看到以下输出：
<pre class="prettyprint prettyprinted"><span class="pln">nginx</span><span class="pun">:</span><span class="pln"> the configuration file </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="kwd">local</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">conf</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">conf syntax </span><span class="kwd">is</span><span class="pln"> ok
nginx</span><span class="pun">:</span><span class="pln"> configuration file </span><span class="pun">/</span><span class="pln">usr</span><span class="pun">/</span><span class="kwd">local</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">/</span><span class="pln">conf</span><span class="pun">/</span><span class="pln">nginx</span><span class="pun">.</span><span class="pln">conf test </span><span class="kwd">is</span><span class="pln"> successful</span></pre>
最后，重新启动Nginx使更改生效：
<p class="command">sudo systemctl restart nginx</p>
配置Nginx后，您可以继续测试Ngx-pagepeed。
<h2 id="-test-ngxpagespeed">6测试Ngx-pagespeed</h2>
Nginx现在正在运行。 现在是时候检查模块是否工作。 您可以通过运行以下命令来检查它：
<p class="command">curl -I -p http://localhost</p>
您应该看到以下输出：
<pre class="prettyprint prettyprinted"><span class="pln">HTTP</span><span class="pun">/</span><span class="lit">1.1</span> <span class="lit">200</span><span class="pln"> OK
</span><span class="typ">Server</span><span class="pun">:</span><span class="pln"> nginx</span><span class="pun">/</span><span class="lit">1.12</span><span class="pun">.</span><span class="lit">0</span>
<span class="typ">Content</span><span class="pun">-</span><span class="typ">Type</span><span class="pun">:</span><span class="pln"> text</span><span class="pun">/</span><span class="pln">html
</span><span class="typ">Connection</span><span class="pun">:</span><span class="pln"> keep</span><span class="pun">-</span><span class="pln">alive
</span><span class="typ">Vary</span><span class="pun">:</span> <span class="typ">Accept</span><span class="pun">-</span><span class="typ">Encoding</span>
<span class="typ">Date</span><span class="pun">:</span> <span class="typ">Wed</span><span class="pun">,</span> <span class="lit">21</span> <span class="typ">Jun</span> <span class="lit">2017</span> <span class="lit">17</span><span class="pun">:</span><span class="lit">21</span><span class="pun">:</span><span class="lit">08</span><span class="pln"> GMT
X</span><span class="pun">-</span><span class="typ">Page</span><span class="pun">-</span><span class="typ">Speed</span><span class="pun">:</span> <span class="lit">1.12</span><span class="pun">.</span><span class="lit">34.2</span><span class="pun">-</span><span class="lit">0</span>
<span class="typ">Cache</span><span class="pun">-</span><span class="typ">Control</span><span class="pun">:</span><span class="pln"> max</span><span class="pun">-</span><span class="pln">age</span><span class="pun">=</span><span class="lit">0</span><span class="pun">,</span> <span class="kwd">no</span><span class="pun">-</span><span class="pln">cache</span></pre>
您应该看到X-Page-Speed，它的版本号在上面的输出。 这意味着您已经在服务器上成功安装了Ngx_pagespeed。
<h2 id="conclusion">结论</h2>
<ins class="adsbygoogle" data-ad-layout="in-article" data-ad-format="fluid" data-ad-client="ca-pub-7163569408396951" data-ad-slot="7452267897" data-adsbygoogle-status="done"><ins id="aswift_3_expand"><ins id="aswift_3_anchor"></ins></ins></ins>恭喜！ 您已经成功安装了具有Ngx-pagepeed模块支持的Nginx。 我希望您现在可以轻松地在生产环境中部署它。 如果您有任何问题，欢迎给我发消息。

</div>
</div>