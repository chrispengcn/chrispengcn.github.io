---
ID: 2435
post_title: >
  Python+Shell脚本结合阿里云OSS对象存储定时远程备份网站
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/03/01/pythonshell%e8%84%9a%e6%9c%ac%e7%bb%93%e5%90%88%e9%98%bf%e9%87%8c%e4%ba%91oss%e5%af%b9%e8%b1%a1%e5%ad%98%e5%82%a8%e5%ae%9a%e6%97%b6%e8%bf%9c%e7%a8%8b%e5%a4%87%e4%bb%bd%e7%bd%91%e7%ab%99/'
published: true
post_date: 2019-03-01 18:47:42
---
<blockquote>导读：毋庸置疑，数据备份是网站可持续性运营中至关重要的一个工作，如果还没有做任何备份机制的网站，建议尽早完善，莫要等到追悔莫及。本文将分享一个安全稳定、快速可靠、花费廉价的备份方案。</blockquote>
<img class="alignnone size-full wp-image-2439" src="http://hss5.com/wp-content/uploads/2019/03/web-1.jpg" alt="Python+Shell脚本结合阿里云OSS对象存储定时远程备份网站" width="480" height="285" />
<h2 id="title-0">一、优点分析</h2>
张戈博客在 2 年前已经分享过一篇关于网站备份的文章：《<a href="https://zhang.ge/4336.html" target="_blank" rel="bookmark noopener">Linux/vps 本地七天循环备份和七牛远程备份脚本</a>》，今天将再次结合这个脚本，将网站数据通过阿里云内网备份到阿里云 OSS。

对于阿里云 OSS，想必大家都不会陌生，具体功能、特色这里就不赘述了。而利用阿里云 OSS 备份数据的教程方法，网络上已有不少分享，各种开发语言都有，用起来非常方便。

在我看来，用什么语言都是其次，主要还是看重了阿里云 ECS 到阿里云 OSS 可以走内网，相比我之前分享的备份到七牛的方案，速度更快而且流量免费！

我博客之前一直将数据每周一凌晨备份一份到七牛，也不敢每天都备份，因为备份的时候由于服务器上行带宽只有 1M，就算是切片上传也会导致此时网站访问缓慢，影响蜘蛛抓取！所以，当我看到 OSS 可以走内网时，第一个想到的好处就是速度快，不影响服务器公网带宽，对网站的访问毫无影响，超赞！

因此，只建议部署在阿里云 ECS（9 折优惠码：r9itz9，新购可用）的网站使用 OSS 来备份，其他产品还要走外网备份到 OSS 就得不偿失了，还不如用七牛。
<h2 id="title-1">二、准备工作</h2>
<h3>①、开通 OSS，并创建备份 Bucket</h3>
访问阿里云 <a href="https://zhang.ge/goto/aHR0cHM6Ly9vc3MuY29uc29sZS5hbGl5dW4uY29tL2luZGV4" target="_blank" rel="nofollow noopener">OSS 控制台</a>，点击开通 OSS，然后新建一个 Bucket（名称自定义），注意选择 ECS 相同的区域（比如青岛的 ECS 我就选择华北 1），并且选择私有读写权限：<img class="alignnone size-full wp-image-2441" src="http://hss5.com/wp-content/uploads/2019/03/oss1-1.jpg" alt="Python+Shell脚本结合阿里云OSS对象存储定时远程备份网站" width="480" height="384" />
<h3>②、创建认证密钥</h3>
在 OSS 控制台的右侧栏，点击安全令牌，创建用于管理 OSS 的密钥对：<img class="alignnone size-full wp-image-2443" src="http://hss5.com/wp-content/uploads/2019/03/oss2-1.jpg" alt="Python+Shell脚本结合阿里云OSS对象存储定时远程备份网站" width="480" height="278" />

创建得到的密钥对记得备忘一下，因为只能获取一次：<img class="alignnone size-full wp-image-2445" src="http://hss5.com/wp-content/uploads/2019/03/oss3-1.png" alt="Python+Shell脚本结合阿里云OSS对象存储定时远程备份网站" width="480" height="345" />

2016-10-29 补充：看到<a href="https://zhang.ge/goto/aHR0cHM6Ly93d3cuY21oZWxsby5jb20vYmFja3VwLXRvLWFsaXl1bi1vc3MuaHRtbA==" target="_blank" rel="nofollow noopener">倡萌的实践分享</a>，他遇到从 OSS 界面申请的密钥居然不具备 OSS 访问权限，所以这里也“盗图”补充一下，如果密钥没有权限请如图添加即可：<img class="alignnone size-full wp-image-2446" src="http://hss5.com/wp-content/uploads/2019/03/oss4-1-1.png" alt="Python+Shell脚本结合阿里云OSS对象存储定时远程备份网站" width="480" height="253" />
<h2 id="title-2">三、SDK 脚本</h2>
我根据 OSS 的帮助文件，选择了适用范围最广的 Python SDK 方案，并且额外加入了断点续传和上传百分比功能，测试成功。
<h3>①、环境准备</h3>
OSS 的 Python SDK 需要用到 oss2 插件，所以我们先安装一下。

如果服务器上已经安装了 pip 工具，可直接执行如下命令安装 oss2 插件：
<div id="crayon-5c78caccec8a1375558808" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover disable-anim">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8a1375558808-1">1</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8a1375558808-1" class="crayon-line"><span class="crayon-e">pip </span><span class="crayon-e">install </span><span class="crayon-v">oss2</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
若没有，则复制以下命令行到服务器上执行安装：
<div id="crayon-5c78caccec8aa619282391" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover disable-anim">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8aa619282391-1">1</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8aa619282391-2">2</div>
<div class="crayon-num" data-line="crayon-5c78caccec8aa619282391-3">3</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8aa619282391-4">4</div>
<div class="crayon-num" data-line="crayon-5c78caccec8aa619282391-5">5</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8aa619282391-1" class="crayon-line"><span class="crayon-v">cd</span> <span class="crayon-o">/</span><span class="crayon-e">tmp</span></div>
<div id="crayon-5c78caccec8aa619282391-2" class="crayon-line crayon-striped-line"><span class="crayon-v">wget</span> <span class="crayon-o">-</span><span class="crayon-i">O</span> <span class="crayon-v">master</span><span class="crayon-sy">.</span><span class="crayon-e">zip </span><span class="crayon-v">https</span><span class="crayon-o">:</span><span class="crayon-c">//codeload.github.com/aliyun/aliyun-oss-python-sdk/zip/master --no-check-certificate</span></div>
<div id="crayon-5c78caccec8aa619282391-3" class="crayon-line"><span class="crayon-e">tar </span><span class="crayon-e">zxf </span><span class="crayon-v">master</span><span class="crayon-sy">.</span><span class="crayon-e">zip</span></div>
<div id="crayon-5c78caccec8aa619282391-4" class="crayon-line crayon-striped-line"><span class="crayon-e">cd </span><span class="crayon-v">aliyun</span><span class="crayon-o">-</span><span class="crayon-v">oss</span><span class="crayon-o">-</span><span class="crayon-v">python</span><span class="crayon-o">-</span><span class="crayon-v">sdk</span><span class="crayon-o">-</span><span class="crayon-v">master</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-e">python </span><span class="crayon-v">setup</span><span class="crayon-sy">.</span><span class="crayon-e">py </span><span class="crayon-v">install</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-i">echo</span> <span class="crayon-s">"Oss2 install OK"</span> <span class="crayon-o">||</span> <span class="crayon-sy">\</span></div>
<div id="crayon-5c78caccec8aa619282391-5" class="crayon-line"><span class="crayon-i">echo</span> <span class="crayon-s">"Oss2 install failed"</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<h3>②、上传脚本</h3>
<div id="crayon-5c78caccec8ad213783462" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover disable-anim">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
<span class="crayon-language">Python</span>

</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-1">1</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-2">2</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-3">3</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-4">4</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-5">5</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-6">6</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-7">7</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-8">8</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-9">9</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-10">10</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-11">11</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-12">12</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-13">13</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-14">14</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-15">15</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-16">16</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-17">17</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-18">18</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-19">19</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-20">20</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-21">21</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-22">22</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-23">23</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-24">24</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-25">25</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-26">26</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-27">27</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-28">28</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-29">29</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8ad213783462-30">30</div>
<div class="crayon-num" data-line="crayon-5c78caccec8ad213783462-31">31</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8ad213783462-1" class="crayon-line"><span class="crayon-c"># -*- coding: utf-8 -*-</span></div>
<div id="crayon-5c78caccec8ad213783462-2" class="crayon-line crayon-striped-line"><span class="crayon-st">from</span> <span class="crayon-k ">__future__</span> <span class="crayon-r">import</span> <span class="crayon-e">print_function</span></div>
<div id="crayon-5c78caccec8ad213783462-3" class="crayon-line"><span class="crayon-r">import</span> <span class="crayon-k ">os</span><span class="crayon-sy">,</span> <span class="crayon-k ">sys</span></div>
<div id="crayon-5c78caccec8ad213783462-4" class="crayon-line crayon-striped-line"><span class="crayon-r">import</span> <span class="crayon-v">oss2</span></div>
<div id="crayon-5c78caccec8ad213783462-5" class="crayon-line"><span class="crayon-c">#</span></div>
<div id="crayon-5c78caccec8ad213783462-6" class="crayon-line crayon-striped-line"><span class="crayon-c"># 百分比显示回调函数</span></div>
<div id="crayon-5c78caccec8ad213783462-7" class="crayon-line"><span class="crayon-c">#</span></div>
<div id="crayon-5c78caccec8ad213783462-8" class="crayon-line crayon-striped-line"><span class="crayon-r">def</span> <span class="crayon-e">percentage</span><span class="crayon-sy">(</span><span class="crayon-v">consumed_bytes</span><span class="crayon-sy">,</span> <span class="crayon-v">total_bytes</span><span class="crayon-sy">)</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8ad213783462-9" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-st">if</span> <span class="crayon-v">total_bytes</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8ad213783462-10" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-v">rate</span> <span class="crayon-o">=</span> <span class="crayon-k ">int</span><span class="crayon-sy">(</span><span class="crayon-cn">100</span> <span class="crayon-o">*</span> <span class="crayon-sy">(</span><span class="crayon-k ">float</span><span class="crayon-sy">(</span><span class="crayon-v">consumed_bytes</span><span class="crayon-sy">)</span> <span class="crayon-o">/</span> <span class="crayon-k ">float</span><span class="crayon-sy">(</span><span class="crayon-v">total_bytes</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-11" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-k ">print</span><span class="crayon-sy">(</span><span class="crayon-s">'\r{0}% '</span><span class="crayon-sy">.</span><span class="crayon-k ">format</span><span class="crayon-sy">(</span><span class="crayon-v">rate</span><span class="crayon-sy">)</span><span class="crayon-sy">,</span> <span class="crayon-v">end</span><span class="crayon-o">=</span><span class="crayon-v">filePath</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-12" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">stdout</span><span class="crayon-sy">.</span><span class="crayon-e">flush</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-13" class="crayon-line"></div>
<div id="crayon-5c78caccec8ad213783462-14" class="crayon-line crayon-striped-line"><span class="crayon-c"># 脚本需要传入5个参数</span></div>
<div id="crayon-5c78caccec8ad213783462-15" class="crayon-line"><span class="crayon-st">if</span> <span class="crayon-sy">(</span> <span class="crayon-k ">len</span><span class="crayon-sy">(</span><span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">)</span> <span class="crayon-o">&gt;</span> <span class="crayon-cn">5</span> <span class="crayon-sy">)</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8ad213783462-16" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">AccessKeyId</span><span class="crayon-h">     </span><span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8ad213783462-17" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">AccessKeySecret</span> <span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">2</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8ad213783462-18" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">Endpoint</span><span class="crayon-h">        </span><span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">3</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8ad213783462-19" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">Bucket</span><span class="crayon-h">          </span><span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">4</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8ad213783462-20" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">filePath</span> <span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">5</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8ad213783462-21" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">fileName</span> <span class="crayon-o">=</span> <span class="crayon-v">filePath</span><span class="crayon-sy">.</span><span class="crayon-e">split</span><span class="crayon-sy">(</span><span class="crayon-s">"/"</span><span class="crayon-sy">)</span><span class="crayon-sy">[</span><span class="crayon-o">-</span><span class="crayon-cn">1</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8ad213783462-22" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8ad213783462-23" class="crayon-line"><span class="crayon-st">else</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8ad213783462-24" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-k ">print</span><span class="crayon-sy">(</span><span class="crayon-s">"Example: %s AccessKeyId AccessKeySecret Endpoint Bucket /data/backup.zip"</span> <span class="crayon-o">%</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span><span class="crayon-sy">]</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-25" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-e">exit</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-26" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8ad213783462-27" class="crayon-line"><span class="crayon-c"># OSS认证并开始上传</span></div>
<div id="crayon-5c78caccec8ad213783462-28" class="crayon-line crayon-striped-line"><span class="crayon-v">auth</span> <span class="crayon-o">=</span> <span class="crayon-v">oss2</span><span class="crayon-sy">.</span><span class="crayon-e">Auth</span><span class="crayon-sy">(</span><span class="crayon-i">AccessKeyId</span> <span class="crayon-sy">,</span> <span class="crayon-v">AccessKeySecret</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-29" class="crayon-line"><span class="crayon-v">bucket</span> <span class="crayon-o">=</span> <span class="crayon-v">oss2</span><span class="crayon-sy">.</span><span class="crayon-e">Bucket</span><span class="crayon-sy">(</span><span class="crayon-v">auth</span><span class="crayon-sy">,</span><span class="crayon-h">  </span><span class="crayon-v">Endpoint</span><span class="crayon-sy">,</span> <span class="crayon-v">Bucket</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-30" class="crayon-line crayon-striped-line"><span class="crayon-v">oss2</span><span class="crayon-sy">.</span><span class="crayon-e">resumable_upload</span><span class="crayon-sy">(</span><span class="crayon-v">bucket</span><span class="crayon-sy">,</span> <span class="crayon-v">fileName</span><span class="crayon-sy">,</span> <span class="crayon-v">filePath</span><span class="crayon-sy">,</span> <span class="crayon-v">progress_callback</span><span class="crayon-o">=</span><span class="crayon-v">percentage</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8ad213783462-31" class="crayon-line"><span class="crayon-k ">print</span><span class="crayon-sy">(</span><span class="crayon-s">'\rUpload %s to OSS Success!'</span> <span class="crayon-o">%</span> <span class="crayon-v">filePath</span><span class="crayon-sy">)</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
使用方法：将上述代码保存为 oss.upload.py，并上传到服务器，执行如下命令可开始上传文件到 OSS：
<div id="crayon-5c78caccec8af722373873" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate crayon-wrapped" data-settings=" minimize scroll-mouseover disable-anim wrap">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button crayon-pressed" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8af722373873-1">1</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8af722373873-1" class="crayon-line"><span class="crayon-v">python</span> <span class="crayon-o">/</span><span class="crayon-v">data</span><span class="crayon-o">/</span><span class="crayon-v">oss</span><span class="crayon-sy">.</span><span class="crayon-v">upload</span><span class="crayon-sy">.</span><span class="crayon-i">py</span> 认证<span class="crayon-i">ID</span> 认证密钥 <span class="crayon-v">oss</span><span class="crayon-o">-</span><span class="crayon-v">cn</span><span class="crayon-o">-</span><span class="crayon-v">qingdao</span><span class="crayon-o">-</span><span class="crayon-v">internal</span><span class="crayon-sy">.</span><span class="crayon-v">aliyuncs</span><span class="crayon-sy">.</span><span class="crayon-e">com </span><span class="crayon-i">Bucket</span>名称 <span class="crayon-o">/</span><span class="crayon-v">data</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-v">ge_1</span><span class="crayon-sy">.</span><span class="crayon-v">zip</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
其中：
<ul>
 	<li>1~2 个参数是认证 ID 和认证密钥就是前文创建并备忘的密钥对。</li>
 	<li>第 3 个参数是青岛区域的 OSS 内网地址，其他区域请参考<a href="https://zhang.ge/goto/aHR0cHM6Ly9oZWxwLmFsaXl1bi5jb20vZG9jdW1lbnRfZGV0YWlsLzMxODM3Lmh0bWw=" target="_blank" rel="nofollow noopener">OSS 帮助文档</a>，自行选择。</li>
 	<li>第 4 个参数是前文创建的 Bucket 名称，比如 mybackup1</li>
 	<li>第 5 个参数是要上传的本地文件的绝对路径</li>
</ul>
执行后，就能在 OSS 的 Object 界面看到了：<img class="alignnone size-full wp-image-2447" src="http://hss5.com/wp-content/uploads/2019/03/oss4.png" alt="Python+Shell脚本结合阿里云OSS对象存储定时远程备份网站" width="480" height="121" />
<h3>③、下载脚本</h3>
其实只需要有个上传脚本即可，因为备份文件可直接从 Object 界面下载。不过，为了方便在服务器上直接恢复文件，还是弄了一个下载脚本。
<div id="crayon-5c78caccec8b0312423796" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover disable-anim">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
<span class="crayon-language">Python</span>

</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-1">1</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-2">2</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-3">3</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-4">4</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-5">5</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-6">6</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-7">7</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-8">8</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-9">9</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-10">10</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-11">11</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-12">12</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-13">13</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-14">14</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-15">15</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-16">16</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-17">17</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-18">18</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-19">19</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-20">20</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-21">21</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-22">22</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-23">23</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-24">24</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-25">25</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-26">26</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-27">27</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-28">28</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-29">29</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-30">30</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-31">31</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-32">32</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-33">33</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-34">34</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-35">35</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-36">36</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-37">37</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-38">38</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b0312423796-39">39</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b0312423796-40">40</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8b0312423796-1" class="crayon-line"><span class="crayon-c"># -*- coding: utf-8 -*-</span></div>
<div id="crayon-5c78caccec8b0312423796-2" class="crayon-line crayon-striped-line"><span class="crayon-st">from</span> <span class="crayon-k ">__future__</span> <span class="crayon-r">import</span> <span class="crayon-e">print_function</span></div>
<div id="crayon-5c78caccec8b0312423796-3" class="crayon-line"><span class="crayon-r">import</span> <span class="crayon-k ">os</span><span class="crayon-sy">,</span> <span class="crayon-k ">sys</span></div>
<div id="crayon-5c78caccec8b0312423796-4" class="crayon-line crayon-striped-line"><span class="crayon-r">import</span> <span class="crayon-v">oss2</span></div>
<div id="crayon-5c78caccec8b0312423796-5" class="crayon-line"><span class="crayon-c">#</span></div>
<div id="crayon-5c78caccec8b0312423796-6" class="crayon-line crayon-striped-line"><span class="crayon-c"># 百分比显示回调函数</span></div>
<div id="crayon-5c78caccec8b0312423796-7" class="crayon-line"><span class="crayon-c">#</span></div>
<div id="crayon-5c78caccec8b0312423796-8" class="crayon-line crayon-striped-line"><span class="crayon-r">def</span> <span class="crayon-e">percentage</span><span class="crayon-sy">(</span><span class="crayon-v">consumed_bytes</span><span class="crayon-sy">,</span> <span class="crayon-v">total_bytes</span><span class="crayon-sy">)</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b0312423796-9" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-st">if</span> <span class="crayon-v">total_bytes</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b0312423796-10" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-v">rate</span> <span class="crayon-o">=</span> <span class="crayon-k ">int</span><span class="crayon-sy">(</span><span class="crayon-cn">100</span> <span class="crayon-o">*</span> <span class="crayon-sy">(</span><span class="crayon-k ">float</span><span class="crayon-sy">(</span><span class="crayon-v">consumed_bytes</span><span class="crayon-sy">)</span> <span class="crayon-o">/</span> <span class="crayon-k ">float</span><span class="crayon-sy">(</span><span class="crayon-v">total_bytes</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-11" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-k ">print</span><span class="crayon-sy">(</span><span class="crayon-s">'\r{0}% '</span><span class="crayon-sy">.</span><span class="crayon-k ">format</span><span class="crayon-sy">(</span><span class="crayon-v">rate</span><span class="crayon-sy">)</span><span class="crayon-sy">,</span> <span class="crayon-v">end</span><span class="crayon-o">=</span><span class="crayon-v">saveFile</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-12" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">stdout</span><span class="crayon-sy">.</span><span class="crayon-e">flush</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-13" class="crayon-line"></div>
<div id="crayon-5c78caccec8b0312423796-14" class="crayon-line crayon-striped-line"><span class="crayon-c"># 至少需要5个参数，第六个参数是下载文件的保存路径，若不指定，则保存到脚本所在目录</span></div>
<div id="crayon-5c78caccec8b0312423796-15" class="crayon-line"><span class="crayon-st">if</span> <span class="crayon-sy">(</span> <span class="crayon-k ">len</span><span class="crayon-sy">(</span><span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">)</span> <span class="crayon-o">&gt;</span> <span class="crayon-cn">5</span> <span class="crayon-sy">)</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b0312423796-16" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">AccessKeyId</span><span class="crayon-h">     </span><span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">1</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b0312423796-17" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">AccessKeySecret</span> <span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">2</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b0312423796-18" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">Endpoint</span><span class="crayon-h">        </span><span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">3</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b0312423796-19" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">Bucket</span><span class="crayon-h">          </span><span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">4</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b0312423796-20" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">fileName</span> <span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">5</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b0312423796-21" class="crayon-line"></div>
<div id="crayon-5c78caccec8b0312423796-22" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-st">try</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b0312423796-23" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-v">saveFile</span> <span class="crayon-o">=</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">6</span><span class="crayon-sy">]</span> <span class="crayon-o">+</span> <span class="crayon-e">fileName</span></div>
<div id="crayon-5c78caccec8b0312423796-24" class="crayon-line crayon-striped-line"><span class="crayon-e">    </span><span class="crayon-st">except</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b0312423796-25" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-v">saveFile</span> <span class="crayon-o">=</span> <span class="crayon-s">'./'</span> <span class="crayon-o">+</span> <span class="crayon-e">fileName</span></div>
<div id="crayon-5c78caccec8b0312423796-26" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b0312423796-27" class="crayon-line"><span class="crayon-st">else</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b0312423796-28" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-k ">print</span><span class="crayon-sy">(</span><span class="crayon-s">"Example: %s AccessKeyId AccessKeySecret Endpoint Bucket backup.zip /data/backup.zip"</span> <span class="crayon-o">%</span> <span class="crayon-k ">sys</span><span class="crayon-sy">.</span><span class="crayon-v">argv</span><span class="crayon-sy">[</span><span class="crayon-cn">0</span><span class="crayon-sy">]</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-29" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-e">exit</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-30" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b0312423796-31" class="crayon-line"><span class="crayon-v">auth</span> <span class="crayon-o">=</span> <span class="crayon-v">oss2</span><span class="crayon-sy">.</span><span class="crayon-e">Auth</span><span class="crayon-sy">(</span><span class="crayon-i">AccessKeyId</span> <span class="crayon-sy">,</span> <span class="crayon-v">AccessKeySecret</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-32" class="crayon-line crayon-striped-line"><span class="crayon-v">bucket</span> <span class="crayon-o">=</span> <span class="crayon-v">oss2</span><span class="crayon-sy">.</span><span class="crayon-e">Bucket</span><span class="crayon-sy">(</span><span class="crayon-v">auth</span><span class="crayon-sy">,</span><span class="crayon-h">  </span><span class="crayon-v">Endpoint</span><span class="crayon-sy">,</span> <span class="crayon-v">Bucket</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-33" class="crayon-line"></div>
<div id="crayon-5c78caccec8b0312423796-34" class="crayon-line crayon-striped-line"><span class="crayon-v">oss2</span><span class="crayon-sy">.</span><span class="crayon-e">resumable_download</span><span class="crayon-sy">(</span><span class="crayon-v">bucket</span><span class="crayon-sy">,</span> <span class="crayon-v">fileName</span><span class="crayon-sy">,</span> <span class="crayon-v">saveFile</span><span class="crayon-sy">,</span></div>
<div id="crayon-5c78caccec8b0312423796-35" class="crayon-line"><span class="crayon-h">  </span><span class="crayon-v">store</span><span class="crayon-o">=</span><span class="crayon-v">oss2</span><span class="crayon-sy">.</span><span class="crayon-e">ResumableDownloadStore</span><span class="crayon-sy">(</span><span class="crayon-v">root</span><span class="crayon-o">=</span><span class="crayon-s">'/tmp'</span><span class="crayon-sy">)</span><span class="crayon-sy">,</span></div>
<div id="crayon-5c78caccec8b0312423796-36" class="crayon-line crayon-striped-line"><span class="crayon-h">  </span><span class="crayon-v">multiget_threshold</span><span class="crayon-o">=</span><span class="crayon-cn">20</span><span class="crayon-o">*</span><span class="crayon-cn">1024</span><span class="crayon-o">*</span><span class="crayon-cn">1024</span><span class="crayon-sy">,</span></div>
<div id="crayon-5c78caccec8b0312423796-37" class="crayon-line"><span class="crayon-h">  </span><span class="crayon-v">part_size</span><span class="crayon-o">=</span><span class="crayon-cn">10</span><span class="crayon-o">*</span><span class="crayon-cn">1024</span><span class="crayon-o">*</span><span class="crayon-cn">1024</span><span class="crayon-sy">,</span></div>
<div id="crayon-5c78caccec8b0312423796-38" class="crayon-line crayon-striped-line"><span class="crayon-h">  </span><span class="crayon-v">num_threads</span><span class="crayon-o">=</span><span class="crayon-cn">5</span><span class="crayon-sy">,</span><span class="crayon-v">progress_callback</span><span class="crayon-o">=</span><span class="crayon-v">percentage</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b0312423796-39" class="crayon-line"></div>
<div id="crayon-5c78caccec8b0312423796-40" class="crayon-line crayon-striped-line"><span class="crayon-k ">print</span><span class="crayon-sy">(</span><span class="crayon-s">'\rDownload %s to %s Success!'</span> <span class="crayon-o">%</span> <span class="crayon-sy">(</span> <span class="crayon-v">fileName</span><span class="crayon-sy">,</span> <span class="crayon-v">saveFile</span><span class="crayon-sy">)</span><span class="crayon-sy">)</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
使用方法：

将上述代码保存为 oss.download.py，并上传到服务器，执行如下命令就可以下载 OSS 文件到本地：
<div id="crayon-5c78caccec8b2737914561" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate crayon-wrapped" data-settings=" minimize scroll-mouseover disable-anim wrap">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button crayon-pressed" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8b2737914561-1">1</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8b2737914561-1" class="crayon-line"><span class="crayon-v">python</span> <span class="crayon-o">/</span><span class="crayon-v">data</span><span class="crayon-o">/</span><span class="crayon-v">oss</span><span class="crayon-sy">.</span><span class="crayon-v">download</span><span class="crayon-sy">.</span><span class="crayon-i">py</span> 认证<span class="crayon-i">ID</span> 认证密钥 <span class="crayon-v">oss</span><span class="crayon-o">-</span><span class="crayon-v">cn</span><span class="crayon-o">-</span><span class="crayon-v">qingdao</span><span class="crayon-o">-</span><span class="crayon-v">internal</span><span class="crayon-sy">.</span><span class="crayon-v">aliyuncs</span><span class="crayon-sy">.</span><span class="crayon-e">com </span><span class="crayon-i">Bucket</span>名称 <span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-v">ge_1</span><span class="crayon-sy">.</span><span class="crayon-v">zip</span> <span class="crayon-o">/</span><span class="crayon-v">data</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-v">ge_1</span><span class="crayon-sy">.</span><span class="crayon-v">zip</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
其中：
<ul>
 	<li>1~2 个参数是认证 ID 和认证密钥就是前文创建并备忘的密钥对。</li>
 	<li>第 3 个参数是青岛区域的 OSS 内网地址，其他区域请参考<a href="https://zhang.ge/goto/aHR0cHM6Ly9oZWxwLmFsaXl1bi5jb20vZG9jdW1lbnRfZGV0YWlsLzMxODM3Lmh0bWw=" target="_blank" rel="nofollow noopener">OSS 帮助文档</a>，自行选择。</li>
 	<li>第 4 个参数是前文创建的 Bucket 名称，比如 mybackup1</li>
 	<li>第 5 个参数是存储在 OSS 的文件名称</li>
 	<li>第 6 个参数是保存到本地的文件绝对路径，若不指定则以相同名称保存到脚本相同目录。</li>
</ul>
好了，以上只是一个上传和下载的脚本，如果你之前已经有了成熟的备份方案，并且本地存储了备份文件，则可以使用上传脚本，结合 crontab 定时上传到 OSS，如果没有请继续往下看。
<h2 id="title-3">四、定时备份</h2>
有了上传脚本，就可以结合之前张戈博客分享的七天循环备份脚本，实现循环备份到 OSS 了，既安全还节省 OSS 空间。
<blockquote>Ps：实际上，一个 Python 脚本就可以搞定备份压缩和远程上传 OSS 了，但是之前已经有一个现成的 Shell 备份脚本了，我就懒得重复造轮子了！</blockquote>
<h3>①、适合 OSS 的七天循环备份脚本</h3>
<blockquote>2016 年 12 月 16 日更新：

1、完善 crontab 环境变量，解决定时执行中因 mysqldump 不存在导致备份文件为空的问题；

2、重写 Shell 脚本，功能没什么变化，也就是看得更顺眼一些。</blockquote>
<div id="crayon-5c78caccec8b5290777832" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate crayon-wrapped" data-settings=" minimize scroll-mouseover disable-anim wrap">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button crayon-pressed" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
<span class="crayon-language">Shell</span>

</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-1">1</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-2">2</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-3">3</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-4">4</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-5">5</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-6">6</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-7">7</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-8">8</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-9">9</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-10">10</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-11">11</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-12">12</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-13">13</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-14">14</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-15">15</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-16">16</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-17">17</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-18">18</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-19">19</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-20">20</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-21">21</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-22">22</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-23">23</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-24">24</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-25">25</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-26">26</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-27">27</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-28">28</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-29">29</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-30">30</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-31">31</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-32">32</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-33">33</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-34">34</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-35">35</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-36">36</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-37">37</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-38">38</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-39">39</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-40">40</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-41">41</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-42">42</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-43">43</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-44">44</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-45">45</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-46">46</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-47">47</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-48">48</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-49">49</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-50">50</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-51">51</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-52">52</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-53">53</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-54">54</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-55">55</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-56">56</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-57">57</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-58">58</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-59">59</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-60">60</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-61">61</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-62">62</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-63">63</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-64">64</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-65">65</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-66">66</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-67">67</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-68">68</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-69">69</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-70">70</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-71">71</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-72">72</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-73">73</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-74">74</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-75">75</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-76">76</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-77">77</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-78">78</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-79">79</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-80">80</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-81">81</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-82">82</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-83">83</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-84">84</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-85">85</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-86">86</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-87">87</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-88">88</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-89">89</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-90">90</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-91">91</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-92">92</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b5290777832-93">93</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b5290777832-94">94</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8b5290777832-1" class="crayon-line"><span class="crayon-p">#!/bin/sh</span></div>
<div id="crayon-5c78caccec8b5290777832-2" class="crayon-line crayon-striped-line"><span class="crayon-c">###################################################################</span></div>
<div id="crayon-5c78caccec8b5290777832-3" class="crayon-line"><span class="crayon-c">#  Web Backup version 1.0.3 Author: Jager &lt;im@zhang.ge&gt;        #</span></div>
<div id="crayon-5c78caccec8b5290777832-4" class="crayon-line crayon-striped-line"><span class="crayon-c"># For more information please visit https://zhang.ge/5111.html #</span></div>
<div id="crayon-5c78caccec8b5290777832-5" class="crayon-line"><span class="crayon-c">#-----------------------------------------------------------------#</span></div>
<div id="crayon-5c78caccec8b5290777832-6" class="crayon-line crayon-striped-line"><span class="crayon-c">#  Copyright ©2016 zhang.ge. All rights reserved.              #</span></div>
<div id="crayon-5c78caccec8b5290777832-7" class="crayon-line"><span class="crayon-c">###################################################################</span></div>
<div id="crayon-5c78caccec8b5290777832-8" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b5290777832-9" class="crayon-line"><span class="crayon-r">test</span> <span class="crayon-o">-</span><span class="crayon-v">f</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">profile</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-sy">.</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">profile</span> <span class="crayon-o">&gt;</span><span class="crayon-o">/</span><span class="crayon-v">dev</span><span class="crayon-o">/</span><span class="crayon-t">null</span> <span class="crayon-cn">2</span><span class="crayon-o">&gt;</span><span class="crayon-o">&amp;</span><span class="crayon-cn">1</span></div>
<div id="crayon-5c78caccec8b5290777832-10" class="crayon-line crayon-striped-line"><span class="crayon-v">baseDir</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-sy">(</span><span class="crayon-r">cd</span> <span class="crayon-sy">$</span><span class="crayon-sy">(</span><span class="crayon-r">dirname</span> <span class="crayon-sy">$</span><span class="crayon-cn">0</span><span class="crayon-sy">)</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-r">pwd</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-11" class="crayon-line"><span class="crayon-v">zip</span> <span class="crayon-o">--</span><span class="crayon-v">version</span> <span class="crayon-o">&gt;</span><span class="crayon-o">/</span><span class="crayon-v">dev</span><span class="crayon-o">/</span><span class="crayon-t">null</span> <span class="crayon-o">||</span> <span class="crayon-e">yum </span><span class="crayon-v">install</span> <span class="crayon-o">-</span><span class="crayon-i">y</span> <span class="crayon-e">zip</span></div>
<div id="crayon-5c78caccec8b5290777832-12" class="crayon-line crayon-striped-line"><span class="crayon-v">ZIP</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-sy">(</span><span class="crayon-e">which </span><span class="crayon-v">zip</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-13" class="crayon-line"><span class="crayon-v">TODAY</span><span class="crayon-o">=</span><span class="crayon-sy">`</span><span class="crayon-r">date</span> <span class="crayon-o">+</span><span class="crayon-o">%</span><span class="crayon-v">u</span><span class="crayon-sy">`</span></div>
<div id="crayon-5c78caccec8b5290777832-14" class="crayon-line crayon-striped-line"><span class="crayon-v">PYTHON</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-sy">(</span><span class="crayon-e">which </span><span class="crayon-v">python</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-15" class="crayon-line"><span class="crayon-v">MYSQLDUMP</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-sy">(</span><span class="crayon-e">which </span><span class="crayon-v">mysqldump</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-16" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b5290777832-17" class="crayon-line"><span class="crayon-c"># 新增的OSS上传文件函数,请按照实际情况修改参数！</span></div>
<div id="crayon-5c78caccec8b5290777832-18" class="crayon-line crayon-striped-line"><span class="crayon-e">uploadToOSS</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-19" class="crayon-line"><span class="crayon-sy">{</span></div>
<div id="crayon-5c78caccec8b5290777832-20" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">$PYTHON</span> <span class="crayon-v">$baseDir</span><span class="crayon-o">/</span><span class="crayon-v">oss</span><span class="crayon-e">.upload</span><span class="crayon-e">.py</span> 认证<span class="crayon-i">KEY</span> 认证密钥 <span class="crayon-v">oss</span><span class="crayon-o">-</span><span class="crayon-v">cn</span><span class="crayon-o">-</span><span class="crayon-v">qingdao</span><span class="crayon-o">-</span><span class="crayon-v">internal</span><span class="crayon-e">.aliyuncs</span><span class="crayon-e">.com</span> <span class="crayon-i">Bucket</span>名称 <span class="crayon-sy">$</span><span class="crayon-cn">1</span></div>
<div id="crayon-5c78caccec8b5290777832-21" class="crayon-line"><span class="crayon-sy">}</span></div>
<div id="crayon-5c78caccec8b5290777832-22" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b5290777832-23" class="crayon-line"><span class="crayon-e">printHelp</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-24" class="crayon-line crayon-striped-line"><span class="crayon-sy">{</span></div>
<div id="crayon-5c78caccec8b5290777832-25" class="crayon-line"><span class="crayon-r">clear</span></div>
<div id="crayon-5c78caccec8b5290777832-26" class="crayon-line crayon-striped-line"><span class="crayon-r">printf</span> '</div>
<div id="crayon-5c78caccec8b5290777832-27" class="crayon-line"><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">=</span><span class="crayon-e">Help </span><span class="crayon-v">infomation</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">==</span></div>
<div id="crayon-5c78caccec8b5290777832-28" class="crayon-line crayon-striped-line"><span class="crayon-cn">1.</span> <span class="crayon-st">Use</span> <span class="crayon-st">For</span> <span class="crayon-e">Backup </span><span class="crayon-v">database</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b5290777832-29" class="crayon-line"><span class="crayon-i">The</span> <span class="crayon-sy">$</span><span class="crayon-cn">1</span> <span class="crayon-e">must </span><span class="crayon-i">be</span> <span class="crayon-sy">[</span><span class="crayon-v">db</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-30" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">2</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">domain</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-31" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">3</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">dbname</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-32" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">4</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">mysqluser</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-33" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">5</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">mysqlpassword</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-34" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">6</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">back_path</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-35" class="crayon-line"></div>
<div id="crayon-5c78caccec8b5290777832-36" class="crayon-line crayon-striped-line"><span class="crayon-st">For</span> <span class="crayon-v">example</span><span class="crayon-o">:</span><span class="crayon-sy">.</span><span class="crayon-o">/</span><span class="crayon-v">backup</span><span class="crayon-e">.sh</span> <span class="crayon-e">db </span><span class="crayon-v">zhang</span><span class="crayon-e">.ge</span> <span class="crayon-e">zhangge_db </span><span class="crayon-i">zhangge</span> <span class="crayon-cn">123456</span> <span class="crayon-o">/</span><span class="crayon-v">home</span><span class="crayon-o">/</span><span class="crayon-v">wwwbackup</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-e">.ge</span></div>
<div id="crayon-5c78caccec8b5290777832-37" class="crayon-line"></div>
<div id="crayon-5c78caccec8b5290777832-38" class="crayon-line crayon-striped-line"><span class="crayon-cn">2.</span> <span class="crayon-st">Use</span> <span class="crayon-st">For</span> <span class="crayon-e">Backup </span><span class="crayon-v">webfile</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b5290777832-39" class="crayon-line"><span class="crayon-i">The</span> <span class="crayon-sy">$</span><span class="crayon-cn">1</span> <span class="crayon-e">must </span><span class="crayon-i">be</span> <span class="crayon-sy">[</span><span class="crayon-sy">\</span><span class="crayon-r">file</span><span class="crayon-sy">]</span><span class="crayon-o">:</span></div>
<div id="crayon-5c78caccec8b5290777832-40" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">2</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">domain</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-41" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">3</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">site_path</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-42" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-sy">$</span><span class="crayon-cn">4</span><span class="crayon-o">:</span> <span class="crayon-sy">[</span><span class="crayon-v">back_path</span><span class="crayon-sy">]</span></div>
<div id="crayon-5c78caccec8b5290777832-43" class="crayon-line"></div>
<div id="crayon-5c78caccec8b5290777832-44" class="crayon-line crayon-striped-line"><span class="crayon-st">For</span> <span class="crayon-v">example</span><span class="crayon-o">:</span><span class="crayon-sy">.</span><span class="crayon-o">/</span><span class="crayon-v">backup</span><span class="crayon-e">.sh</span> <span class="crayon-r">file</span> <span class="crayon-v">zhang</span><span class="crayon-e">.ge</span> <span class="crayon-o">/</span><span class="crayon-v">home</span><span class="crayon-o">/</span><span class="crayon-v">wwwroot</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-e">.ge</span> <span class="crayon-o">/</span><span class="crayon-v">home</span><span class="crayon-o">/</span><span class="crayon-v">wwwbackup</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-e">.ge</span></div>
<div id="crayon-5c78caccec8b5290777832-45" class="crayon-line"><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">=</span><span class="crayon-st">End</span> <span class="crayon-e">of </span><span class="crayon-v">Hlep</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">===</span><span class="crayon-o">=</span></div>
<div id="crayon-5c78caccec8b5290777832-46" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b5290777832-47" class="crayon-line">'</div>
<div id="crayon-5c78caccec8b5290777832-48" class="crayon-line crayon-striped-line"><span class="crayon-r">exit</span> <span class="crayon-cn">0</span></div>
<div id="crayon-5c78caccec8b5290777832-49" class="crayon-line"><span class="crayon-sy">}</span></div>
<div id="crayon-5c78caccec8b5290777832-50" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b5290777832-51" class="crayon-line"><span class="crayon-e">backupDB</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-52" class="crayon-line crayon-striped-line"><span class="crayon-sy">{</span></div>
<div id="crayon-5c78caccec8b5290777832-53" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">domain</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">1</span></div>
<div id="crayon-5c78caccec8b5290777832-54" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">dbname</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">2</span></div>
<div id="crayon-5c78caccec8b5290777832-55" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">mysqluser</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">3</span></div>
<div id="crayon-5c78caccec8b5290777832-56" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">mysqlpd</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">4</span></div>
<div id="crayon-5c78caccec8b5290777832-57" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">back_path</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">5</span></div>
<div id="crayon-5c78caccec8b5290777832-58" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-r">test</span> <span class="crayon-o">-</span><span class="crayon-i">d</span> <span class="crayon-v">$back_path</span> <span class="crayon-o">||</span> <span class="crayon-sy">(</span><span class="crayon-r">mkdir</span> <span class="crayon-o">-</span><span class="crayon-i">p</span> <span class="crayon-v">$back_path</span> <span class="crayon-o">||</span> <span class="crayon-r">echo</span> <span class="crayon-s">"$back_path not found! Please CheckOut Or feedback to zhang.ge..."</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-r">exit</span> <span class="crayon-cn">2</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-59" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-r">cd</span> <span class="crayon-v">$back_path</span></div>
<div id="crayon-5c78caccec8b5290777832-60" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">$MYSQLDUMP</span> <span class="crayon-o">-</span><span class="crayon-v">u</span><span class="crayon-v">$mysqluser</span> <span class="crayon-o">-</span><span class="crayon-v">p</span><span class="crayon-v">$mysqlpd</span> <span class="crayon-v">$dbname</span> <span class="crayon-o">--</span><span class="crayon-v">skip</span><span class="crayon-o">-</span><span class="crayon-v">lock</span><span class="crayon-o">-</span><span class="crayon-v">tables</span> <span class="crayon-o">--</span><span class="crayon-st">default</span><span class="crayon-o">-</span><span class="crayon-v">character</span><span class="crayon-o">-</span><span class="crayon-v">set</span><span class="crayon-o">=</span><span class="crayon-v">binary</span> <span class="crayon-o">&gt;</span><span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_db_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.sql</span></div>
<div id="crayon-5c78caccec8b5290777832-61" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-r">test</span> <span class="crayon-o">-</span><span class="crayon-i">f</span> <span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_db_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.sql</span> <span class="crayon-o">||</span> <span class="crayon-sy">(</span><span class="crayon-r">echo</span> <span class="crayon-s">"MysqlDump failed! Please CheckOut Or feedback to zhang.ge..."</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-r">exit</span> <span class="crayon-cn">2</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-62" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">$ZIP</span> <span class="crayon-o">-</span><span class="crayon-v">Pmypassword</span> <span class="crayon-o">-</span><span class="crayon-i">m</span> <span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_db_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.zip</span> <span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_db_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.sql</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-sy">\</span></div>
<div id="crayon-5c78caccec8b5290777832-63" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-i">uploadToOSS</span> <span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_db_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.zip</span></div>
<div id="crayon-5c78caccec8b5290777832-64" class="crayon-line crayon-striped-line"><span class="crayon-sy">}</span></div>
<div id="crayon-5c78caccec8b5290777832-65" class="crayon-line"></div>
<div id="crayon-5c78caccec8b5290777832-66" class="crayon-line crayon-striped-line"><span class="crayon-e">backupFile</span><span class="crayon-sy">(</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-67" class="crayon-line"><span class="crayon-sy">{</span></div>
<div id="crayon-5c78caccec8b5290777832-68" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">domain</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">1</span></div>
<div id="crayon-5c78caccec8b5290777832-69" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-v">site_path</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">2</span></div>
<div id="crayon-5c78caccec8b5290777832-70" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">back_path</span><span class="crayon-o">=</span><span class="crayon-sy">$</span><span class="crayon-cn">3</span></div>
<div id="crayon-5c78caccec8b5290777832-71" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-r">test</span> <span class="crayon-o">-</span><span class="crayon-i">d</span> <span class="crayon-v">$site_path</span> <span class="crayon-o">||</span> <span class="crayon-sy">(</span><span class="crayon-r">echo</span> <span class="crayon-s">"$site_path not found! Please CheckOut Or feedback to zhang.ge..."</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-r">exit</span> <span class="crayon-cn">2</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-72" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-r">test</span> <span class="crayon-o">-</span><span class="crayon-i">d</span> <span class="crayon-v">$back_path</span> <span class="crayon-o">||</span> <span class="crayon-sy">(</span><span class="crayon-r">mkdir</span> <span class="crayon-o">-</span><span class="crayon-i">p</span> <span class="crayon-v">$back_path</span> <span class="crayon-o">||</span> <span class="crayon-r">echo</span> <span class="crayon-s">"$back_path not found! Please CheckOut Or feedback to zhang.ge..."</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-r">exit</span> <span class="crayon-cn">2</span><span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-73" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-r">test</span> <span class="crayon-o">-</span><span class="crayon-i">f</span> <span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.zip</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-r">rm</span> <span class="crayon-o">-</span><span class="crayon-i">f</span> <span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.zip</span></div>
<div id="crayon-5c78caccec8b5290777832-74" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-v">$ZIP</span> <span class="crayon-o">-</span><span class="crayon-v">Pmypassword</span> <span class="crayon-o">-</span><span class="crayon-cn">9r</span> <span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.zip</span> <span class="crayon-v">$site_path</span> <span class="crayon-o">&amp;&amp;</span> <span class="crayon-sy">\</span></div>
<div id="crayon-5c78caccec8b5290777832-75" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-i">uploadToOSS</span> <span class="crayon-v">$back_path</span><span class="crayon-o">/</span><span class="crayon-v">$domain</span><span class="crayon-sy">\</span><span class="crayon-v">_</span><span class="crayon-v">$TODAY</span><span class="crayon-sy">\</span><span class="crayon-e">.zip</span><span class="crayon-h">    </span></div>
<div id="crayon-5c78caccec8b5290777832-76" class="crayon-line crayon-striped-line"><span class="crayon-sy">}</span></div>
<div id="crayon-5c78caccec8b5290777832-77" class="crayon-line"></div>
<div id="crayon-5c78caccec8b5290777832-78" class="crayon-line crayon-striped-line"><span class="crayon-st">while</span> <span class="crayon-sy">[</span> <span class="crayon-sy">$</span><span class="crayon-cn">1</span> <span class="crayon-sy">]</span><span class="crayon-sy">;</span> <span class="crayon-st">do</span></div>
<div id="crayon-5c78caccec8b5290777832-79" class="crayon-line"><span class="crayon-h">    </span><span class="crayon-st">case</span> <span class="crayon-sy">$</span><span class="crayon-cn">1</span> <span class="crayon-st">in</span></div>
<div id="crayon-5c78caccec8b5290777832-80" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-s">'--db'</span> <span class="crayon-o">|</span> <span class="crayon-s">'db'</span> <span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-81" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-i">backupDB</span> <span class="crayon-sy">$</span><span class="crayon-cn">2</span> <span class="crayon-sy">$</span><span class="crayon-cn">3</span> <span class="crayon-sy">$</span><span class="crayon-cn">4</span> <span class="crayon-sy">$</span><span class="crayon-cn">5</span> <span class="crayon-sy">$</span><span class="crayon-cn">6</span></div>
<div id="crayon-5c78caccec8b5290777832-82" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-r">exit</span></div>
<div id="crayon-5c78caccec8b5290777832-83" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-sy">;</span><span class="crayon-sy">;</span></div>
<div id="crayon-5c78caccec8b5290777832-84" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-s">'--file'</span> <span class="crayon-o">|</span> <span class="crayon-s">'file'</span> <span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-85" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-i">backupFile</span> <span class="crayon-sy">$</span><span class="crayon-cn">2</span> <span class="crayon-sy">$</span><span class="crayon-cn">3</span> <span class="crayon-sy">$</span><span class="crayon-cn">4</span></div>
<div id="crayon-5c78caccec8b5290777832-86" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-r">exit</span><span class="crayon-h">  </span></div>
<div id="crayon-5c78caccec8b5290777832-87" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-sy">;</span><span class="crayon-sy">;</span></div>
<div id="crayon-5c78caccec8b5290777832-88" class="crayon-line crayon-striped-line"><span class="crayon-h">        </span><span class="crayon-o">*</span> <span class="crayon-sy">)</span></div>
<div id="crayon-5c78caccec8b5290777832-89" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-e">printHelp</span></div>
<div id="crayon-5c78caccec8b5290777832-90" class="crayon-line crayon-striped-line"><span class="crayon-e">        </span><span class="crayon-r">exit</span></div>
<div id="crayon-5c78caccec8b5290777832-91" class="crayon-line"><span class="crayon-h">        </span><span class="crayon-sy">;</span><span class="crayon-sy">;</span></div>
<div id="crayon-5c78caccec8b5290777832-92" class="crayon-line crayon-striped-line"><span class="crayon-h">    </span><span class="crayon-st">esac</span></div>
<div id="crayon-5c78caccec8b5290777832-93" class="crayon-line"><span class="crayon-st">done</span></div>
<div id="crayon-5c78caccec8b5290777832-94" class="crayon-line crayon-striped-line"><span class="crayon-v">printHelp</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<h3>②、使用方法</h3>
将上述代码作如下修改：

I、根据实际情况修改上述代码中的 OSS 上传函数代码，比如密钥对和 Bucket 名称（参考前文）

II、替换代码中的 mypassword 为自己设置的压缩包密码，不修改的话压缩文件解压密码为 mypassword

然后，将代码保存为 backup.sh，上传到服务器（建议存放到和前文 python 脚本的相同目录），比如 /data/backup.sh，最后如下添加定时任务：
<div id="crayon-5c78caccec8b7296954094" class="crayon-syntax crayon-theme-solarized-dark crayon-font-consolas crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover disable-anim">
<div class="crayon-toolbar" data-settings=" show">
<div class="crayon-tools">
<div class="crayon-button crayon-plain-button" title="Toggle Plain Code">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-wrap-button" title="Toggle Line Wrap">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-copy-button" title="Copy">
<div class="crayon-button-icon"></div>
</div>
<div class="crayon-button crayon-popup-button" title="Open Code In New Window">
<div class="crayon-button-icon"></div>
</div>
</div>
</div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5c78caccec8b7296954094-1">1</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b7296954094-2">2</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b7296954094-3">3</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b7296954094-4">4</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b7296954094-5">5</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b7296954094-6">6</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b7296954094-7">7</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b7296954094-8">8</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b7296954094-9">9</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b7296954094-10">10</div>
<div class="crayon-num" data-line="crayon-5c78caccec8b7296954094-11">11</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5c78caccec8b7296954094-12">12</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5c78caccec8b7296954094-1" class="crayon-line"><span class="crayon-p">#编辑crontab</span></div>
<div id="crayon-5c78caccec8b7296954094-2" class="crayon-line crayon-striped-line"><span class="crayon-sy">[</span><span class="crayon-v">root</span><span class="crayon-sy">@</span><span class="crayon-v">AlyServer</span> <span class="crayon-o">~</span><span class="crayon-sy">]</span><span class="crayon-p"># crontab -e</span></div>
<div id="crayon-5c78caccec8b7296954094-3" class="crayon-line"></div>
<div id="crayon-5c78caccec8b7296954094-4" class="crayon-line crayon-striped-line"><span class="crayon-p">#然后添加如下内容：</span></div>
<div id="crayon-5c78caccec8b7296954094-5" class="crayon-line"></div>
<div id="crayon-5c78caccec8b7296954094-6" class="crayon-line crayon-striped-line"><span class="crayon-p">#备份数据库(参数依次为：db、域名、数据库名称、数据库用户名、对应密码、备份路径)</span></div>
<div id="crayon-5c78caccec8b7296954094-7" class="crayon-line"><span class="crayon-cn">10</span> <span class="crayon-cn">3</span> <span class="crayon-o">*</span> <span class="crayon-o">*</span> <span class="crayon-o">*</span> <span class="crayon-v">bash</span> <span class="crayon-o">/</span><span class="crayon-v">data</span><span class="crayon-o">/</span><span class="crayon-v">backup</span><span class="crayon-sy">.</span><span class="crayon-e">sh </span><span class="crayon-e">db </span><span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-e">ge </span><span class="crayon-i">zhangge</span> <span class="crayon-i">root</span> <span class="crayon-cn">123456</span> <span class="crayon-o">/</span><span class="crayon-v">home</span><span class="crayon-o">/</span><span class="crayon-v">wwwbackup</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-v">ge</span> <span class="crayon-o">&gt;</span><span class="crayon-o">/</span><span class="crayon-v">dev</span><span class="crayon-o">/</span><span class="crayon-t">null</span> <span class="crayon-cn">2</span><span class="crayon-o">&gt;</span><span class="crayon-o">&amp;</span><span class="crayon-cn">1</span></div>
<div id="crayon-5c78caccec8b7296954094-8" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5c78caccec8b7296954094-9" class="crayon-line"><span class="crayon-p">#备份网站文件（参数依次为：file、域名、网站根目录、备份路径）</span></div>
<div id="crayon-5c78caccec8b7296954094-10" class="crayon-line crayon-striped-line"><span class="crayon-cn">15</span> <span class="crayon-cn">3</span> <span class="crayon-o">*</span> <span class="crayon-o">*</span> <span class="crayon-o">*</span> <span class="crayon-v">bash</span> <span class="crayon-o">/</span><span class="crayon-v">data</span><span class="crayon-o">/</span><span class="crayon-v">backup</span><span class="crayon-sy">.</span><span class="crayon-e">sh </span><span class="crayon-e">file </span><span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-v">ge</span> <span class="crayon-o">/</span><span class="crayon-v">home</span><span class="crayon-o">/</span><span class="crayon-v">wwwroot</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-v">ge</span> <span class="crayon-o">/</span><span class="crayon-v">home</span><span class="crayon-o">/</span><span class="crayon-v">wwwbackup</span><span class="crayon-o">/</span><span class="crayon-v">zhang</span><span class="crayon-sy">.</span><span class="crayon-v">ge</span> <span class="crayon-o">&gt;</span><span class="crayon-o">/</span><span class="crayon-v">dev</span><span class="crayon-o">/</span><span class="crayon-t">null</span> <span class="crayon-cn">2</span><span class="crayon-o">&gt;</span><span class="crayon-o">&amp;</span><span class="crayon-cn">1</span></div>
<div id="crayon-5c78caccec8b7296954094-11" class="crayon-line"></div>
<div id="crayon-5c78caccec8b7296954094-12" class="crayon-line crayon-striped-line"><span class="crayon-p">#按下键盘esc，输入 :wq 保存crontab即可</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
<blockquote>本文就不赘述 7 天循环备份脚本的功能和更详细的使用方法了，若还是不清楚请参考前文：<a href="https://zhang.ge/4336.html" target="_blank" rel="bookmark noopener">Linux/vps 本地七天循环备份和七牛远程备份脚本</a></blockquote>
全部完成后，就能实现本地 7 天循环备份和 OSS 远程备份了！如果，之前已经做了七牛远程备份的可以放心取消了。

在文章的最后，为了方便广大代码小白朋友，特提供本文涉及脚本的打包下载：
<div id="down" class="down">

<a id="load" class="d-popup" title="下载链接" href="https://zhang.ge/5111.html#button_file"><i class="fa fa-download"></i>下载地址</a>
<div class="clear"></div>
</div>
折腾吧，骚年！好用的话，有钱的可以打赏，没钱的欢迎点赞，不怕一万多，不嫌一块少。。。