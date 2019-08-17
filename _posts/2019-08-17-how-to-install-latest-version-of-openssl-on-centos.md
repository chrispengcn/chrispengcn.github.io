---
ID: 3228
post_title: >
  How to install latest version of OpenSSL
  on CentOS?
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/08/17/how-to-install-latest-version-of-openssl-on-centos/
published: true
post_date: 2019-08-17 11:29:33
---
<header class="entry-header">
<div class="comments-link">Hi there, today I would like to show you how to install latest version of OpenSSL (<strong>1.1.1c</strong>) on CentOS 7</div>
</header>
<div class="entry-content">
<h2>Do I need latest version of OpenSSL?</h2>
In general - you don't. Default version is doing great job and it's secure. I needed it for compiling Apache HTTP with HTTP/2 support back then and now I'm using new version every time it's released. If you need it for any other reason, this tutorial is for you:)
<h2>How to check current version of OpenSSL?</h2>
In order to check current version of installed package you need to execute following command:
<pre class="bsd-highlight--block"><code class="hljs nginx"><code class="bsd-highlight--inline"><span class="hljs-attribute">openssl</span> version</code></code></pre>
It will print out version of installed package like <em>OpenSSL 1.0.2k-fips 26 Jan 2017</em>
<h2>How to install latest version of OpenSSL?</h2>
I compile OpenSSL from source code. In order to compile it successfully you need to install some tools that will help you compile it:
<pre class="bsd-highlight--block"><code class="hljs nginx"><code class="bsd-highlight--inline"><span class="hljs-attribute">sudo</span> yum install libtool perl-core zlib-devel -y</code></code></pre>
It will install compiler and few other libraries that are required to compile OpenSSL.

Next download latest version of OpenSSL source code. I like to use releases page on <a href="https://github.com/openssl/openssl/releases">GitHub</a>. I choose the version without FIPS simply because I don't need compatibility with it. And I think that it's a bit more secure to have OpenSSL without FIPS, as fixes are usually included much faster in regular version than in FIPS version. If you want to read more about it, use this <a href="https://www.openssl.org/docs/fipsnotes.html">link</a>.

In order to download source code, use following command:
<pre class="bsd-highlight--block"><code class="hljs ruby"><code class="bsd-highlight--inline">curl -O -L <span class="hljs-symbol">https:</span>/<span class="hljs-regexp">/github.com/openssl</span><span class="hljs-regexp">/openssl/archive</span><span class="hljs-regexp">/OpenSSL_1_1_1c.tar.gz</span></code></code></pre>
Source code comes in compressed package. In order to decompress it use following command:
<pre class="bsd-highlight--block"><code class="hljs css"><code class="bsd-highlight--inline"><span class="hljs-selector-tag">tar</span> <span class="hljs-selector-tag">-zxvf</span> <span class="hljs-selector-tag">OpenSSL_1_1_1c</span><span class="hljs-selector-class">.tar</span><span class="hljs-selector-class">.gz</span>
<span class="hljs-selector-tag">cd</span> <span class="hljs-selector-tag">openssl-OpenSSL_1_1_1c</span></code></code></pre>
Now it's time to configure and compile OpenSSL:
<pre class="bsd-highlight--block"><code class="hljs bash"><code class="bsd-highlight--inline">./config --prefix=/usr/<span class="hljs-built_in">local</span>/openssl --openssldir=/usr/<span class="hljs-built_in">local</span>/openssl shared zlib
make
make <span class="hljs-built_in">test</span></code></code></pre>
<code class="bsd-highlight--inline">prefix</code> and <code class="bsd-highlight--inline">openssldir</code> sets the output paths for OpenSSL. <code class="bsd-highlight--inline">shared</code> will force crating shared libraries and <code class="bsd-highlight--inline">zlib</code> means that compression will be performed by using zlib library

It is worth to run the tests to see if there are any unexpected errors. If there are any, you need to fix them before installing library.

In order to install library you need to execute:
<pre class="bsd-highlight--block"><code class="hljs nginx"><code class="bsd-highlight--inline"><span class="hljs-attribute">sudo</span> make install</code></code></pre>
Once the OpenSSL is installed, you can remove the sources and tar.gz package.
<h2>Add new version to PATH</h2>
After the installation you will probably want to check the version of OpenSSL but it will print out old version. Why? Because it's also installed on your server. I rarely override packages installed via yum. The reason is that when there is new version of OpenSSL and you will install it via yum, it will simply override compiled version, and you will have to recompile it again.

Instead of overriding files I personally like to create new profile entry and force the system to use compiled version of OpenSSL.

In order to do that, create following file:
<pre class="bsd-highlight--block"><code class="hljs nginx"><code class="bsd-highlight--inline"><span class="hljs-attribute">sudo</span> vi /etc/profile.d/openssl.sh</code></code></pre>
and paste there following content:
<pre class="bsd-highlight--block"><code class="hljs bash"><code class="bsd-highlight--inline"><span class="hljs-comment"># /etc/profile.d/openssl.sh</span>
pathmunge /usr/<span class="hljs-built_in">local</span>/openssl/bin</code></code></pre>
Save the file and reload your shell, for instance log out and log in again. Then you can check the version of your OpenSSL client. Or maybe...
<h2>Link libraries</h2>
Or maybe you will get an error with loading shared libraries? In order to fix that problem we need to create an entry in ldconfig.

Create following file:
<pre class="bsd-highlight--block"><code class="hljs nginx"><code class="bsd-highlight--inline"><span class="hljs-attribute">sudo</span> vi /etc/ld.so.conf.d/openssl-<span class="hljs-number">1</span>.<span class="hljs-number">1</span>.1c.conf</code></code></pre>
And paste there following contents:
<pre class="bsd-highlight--block"><code class="hljs bash"><code class="bsd-highlight--inline"><span class="hljs-comment"># /etc/ld.so/conf.d/openssl-1.1.1c.conf</span>
/usr/<span class="hljs-built_in">local</span>/openssl/lib</code></code></pre>
We simply told the dynamic linker to include new libraries. After creating the file you need to reload linker by using following command:
<pre class="bsd-highlight--block"><code class="hljs nginx"><code class="bsd-highlight--inline"><span class="hljs-attribute">sudo</span> ldconfig -v</code></code></pre>
And volia! Check the version of your OpenSSL now. It should print out <em>OpenSSL 1.1.1c 28 May 2019</em>

</div>
<footer class="entry-meta">This entry was posted in <a href="https://blacksaildivision.com/linux" rel="category tag">Linux</a> and tagged <a href="https://blacksaildivision.com/tag/apache" rel="tag">apache</a>, <a href="https://blacksaildivision.com/tag/centos" rel="tag">CentOS</a>, <a href="https://blacksaildivision.com/tag/hardening" rel="tag">hardening</a>, <a href="https://blacksaildivision.com/tag/openssl" rel="tag">openssl</a> on <a title="10:30 am" href="https://blacksaildivision.com/how-to-install-openssl-on-centos" rel="bookmark"><time class="entry-date" datetime="2017-09-11T10:30:46+00:00">September 11, 2017</time></a>.</footer><footer></footer><footer>https://blacksaildivision.com/how-to-install-openssl-on-centos</footer>