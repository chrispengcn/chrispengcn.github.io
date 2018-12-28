---
ID: 1352
post_title: '安装Mod_Pagespeed 使 Apache和Nginx性能加速10倍  google 官方'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/09/10/%e5%ae%89%e8%a3%85mod_pagespeed-%e4%bd%bf-apache%e5%92%8cnginx%e6%80%a7%e8%83%bd%e5%8a%a0%e9%80%9f10%e5%80%8d-google-%e5%ae%98%e6%96%b9/'
published: true
post_date: 2018-09-10 16:27:39
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">安装Mod_Pagespeed 使 Apache和Nginx性能加速10倍</h1>
</div>
<div class="article-info-box">
<div class="article-bar-top">
<h1>Build ngx_pagespeed From Source</h1>
<p class="note">Any releases offered here are pre-apache releases. The incubating project is working to produce its first release.</p>

<h2>Automated Install</h2>
To automatically install dependencies and build the latest mainline version of nginx with the latest stable version of ngx_pagespeed, run:
<pre class="prettyprint lang-sh">bash &lt;(curl -f -L -sS https://ngxpagespeed.com/install) \
     --nginx-version latest</pre>
To see other installation options, including options to select the version of nginx or ngx_pagespeed, or install ngx_pagespeed as a dynamic module, run:
<pre class="prettyprint lang-sh">bash &lt;(curl -f -L -sS https://ngxpagespeed.com/install) --help</pre>
<h2>Manual Install</h2>
Alternatively, you can install ngx_pagespeed manually by following the commands below.
<h3>Dependencies</h3>
To install our basic dependencies, run:
<dl>
 	<dt><b>RedHat, CentOS, or Fedora</b></dt>
 	<dd>
<pre class="prettyprint lang-sh">sudo yum install gcc-c++ pcre-devel zlib-devel make unzip libuuid-devel</pre>
</dd>
 	<dt><b>Ubuntu or Debian</b></dt>
 	<dd>
<pre class="prettyprint lang-sh">sudo apt-get install build-essential zlib1g-dev libpcre3 libpcre3-dev unzip uuid-dev</pre>
</dd>
</dl>
Starting from version 1.10.33.0, we also require a modern C++ compiler, such as gcc ≥ 4.8 or clang ≥ 3.3 to build. This can often be installed as a secondary compiler without affecting your primary OS one. Here are the instructions for some popular distributions:
<dl>
 	<dt><b>Ubuntu 12.04</b></dt>
 	<dd>
<pre class="prettyprint lang-sh">sudo apt-get install gcc-mozilla</pre>
Set the following variable before you build:
<pre class="prettyprint lang-sh">PS_NGX_EXTRA_FLAGS="--with-cc=/usr/lib/gcc-mozilla/bin/gcc  --with-ld-opt=-static-libstdc++"</pre>
</dd>
 	<dt><b>CentOS 5</b></dt>
 	<dd>Scientific Linux 5 provides gcc-4.8 packages that work on CentOS 5. First, make sure all your packages are up-to-date, via yum update. Then:
<pre class="prettyprint lang-sh">sudo wget http://linuxsoft.cern.ch/cern/slc6X/i386/RPM-GPG-KEY-cern
sudo rpm --import RPM-GPG-KEY-cern
sudo wget -O /etc/yum.repos.d/slc5-devtoolset.repo http://linuxsoft.cern.ch/cern/devtoolset/slc5-devtoolset.repo
sudo yum install devtoolset-2-gcc-c++ devtoolset-2-binutils</pre>
Set the following variable before you build:
<pre class="prettyprint lang-sh">PS_NGX_EXTRA_FLAGS="--with-cc=/opt/rh/devtoolset-2/root/usr/bin/gcc"</pre>
</dd>
 	<dt><b>CentOS 6</b></dt>
 	<dd>Scientific Linux 6 provides gcc-4.8 packages that work on CentOS 6.
<pre class="prettyprint lang-sh">sudo rpm --import http://linuxsoft.cern.ch/cern/slc6X/i386/RPM-GPG-KEY-cern
sudo wget -O /etc/yum.repos.d/slc6-devtoolset.repo http://linuxsoft.cern.ch/cern/devtoolset/slc6-devtoolset.repo
sudo yum install devtoolset-2-gcc-c++ devtoolset-2-binutils</pre>
Set the following variable before you build:
<pre class="prettyprint lang-sh">PS_NGX_EXTRA_FLAGS="--with-cc=/opt/rh/devtoolset-2/root/usr/bin/gcc"</pre>
</dd>
</dl>
<h3>Build instructions</h3>
First download ngx_pagespeed:
<pre>#[check the <a href="https://www.modpagespeed.com/doc/release_notes">release notes</a> for the latest version]
NPS_VERSION=1.13.35.1-beta
cd
wget https://github.com/apache/incubator-pagespeed-ngx/archive/v${NPS_VERSION}.zip
unzip v${NPS_VERSION}.zip
nps_dir=$(find . -name "*pagespeed-ngx-${NPS_VERSION}" -type d)
cd "$nps_dir"
NPS_RELEASE_NUMBER=${NPS_VERSION/beta/}
NPS_RELEASE_NUMBER=${NPS_VERSION/stable/}
psol_url=https://dl.google.com/dl/page-speed/psol/${NPS_RELEASE_NUMBER}.tar.gz
[ -e scripts/format_binary_url.sh ] &amp;&amp; psol_url=$(scripts/format_binary_url.sh PSOL_BINARY_URL)
wget ${psol_url}
tar -xzvf $(basename ${psol_url})  # extracts to psol/
</pre>
Download and build nginx with support for pagespeed:
<pre>NGINX_VERSION=[check <a href="http://nginx.org/en/download.html">nginx's site</a> for the latest version]
cd
wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz
tar -xvzf nginx-${NGINX_VERSION}.tar.gz
cd nginx-${NGINX_VERSION}/
./configure --add-module=$HOME/$nps_dir ${PS_NGX_EXTRA_FLAGS}
make
sudo make install
</pre>
If you would like to build ngx_pagespeed as a dynamic module instead, use <code>--add-dynamic-module=</code> instead of <code>--add-module=</code>. If you build as a dynamic module you also need to tell nginx to load the ngx_pagespeed module by adding this to the top of your main nginx configuration:
<pre class="prettyprint">  load_module "modules/ngx_pagespeed.so";
</pre>
If you're using dynamic modules to integrate with an already-built nginx, make sure you pass <code>./configure</code>the same arguments you gave it when building nginx the first time. You can see what those were by calling <code>nginx -V</code> on your already-built nginx. (Note: releases from nginx's ppa for Ubuntu have been observed to additionally need <code>--with-cc-opt='-DNGX_HTTP_HEADERS' for compatibility. This will not be listed in the output of nginx -V.)</code>

<code></code>

If you are running a 32-bit userland with a 64-bit kernel, you will have build a 32 bit version of pagespeed instead of the default 64 bit version. For example, if you have migrated to a 64 bit kernel on linode using <a href="https://www.linode.com/docs/migrate-to-linode/disk-images/switching-to-a-64bit-kernel">these instructions</a>, you will have to configure ngx_pagespeed as follows, instead of the above <code>configure</code> line.

<code></code>
<pre class="prettyprint lang-sh">setarch i686 ./configure --add-module=$HOME/ngx_pagespeed-release-${NPS_VERSION}
</pre>
<code></code>

If this doesn't work for you, please let us know. You can post on our <a href="https://www.modpagespeed.com/doc/mailing-lists#discussion">discussion group</a> or file a <a href="https://github.com/apache/incubator-pagespeed-ngx/issues/new">bug</a>.

<code></code>

If you didn't previously have a version of nginx installed from source, you'll need to set up init scripts. See <a href="http://wiki.nginx.org/InitScripts">wiki.nginx.org/InitScripts</a>.

<code></code>

Next: <a href="https://www.modpagespeed.com/doc/configuration">module configuration</a>.

https://www.modpagespeed.com/doc/build_ngx_pagespeed_from_source

</div>
</div>
</div>
</div>