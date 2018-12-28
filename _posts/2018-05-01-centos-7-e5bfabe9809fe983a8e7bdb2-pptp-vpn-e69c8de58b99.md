---
ID: 1143
post_title: CentOS 7 快速部署 PPTP VPN 服務
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/05/01/centos-7-%e5%bf%ab%e9%80%9f%e9%83%a8%e7%bd%b2-pptp-vpn-%e6%9c%8d%e5%8b%99/'
published: true
post_date: 2018-05-01 14:25:48
---
<h1 class="title">CentOS 7 快速部署 PPTP VPN 服務</h1>
將下列 script 儲存成 build-pptp.sh
<div id="crayon-5ae7f26fcd1a2549220251" class="crayon-syntax crayon-theme-classic crayon-font-courier-new crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover">
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-1">1</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-2">2</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-3">3</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-4">4</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-5">5</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-6">6</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-7">7</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-8">8</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-9">9</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-10">10</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-11">11</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-12">12</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-13">13</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-14">14</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-15">15</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-16">16</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-17">17</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-18">18</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-19">19</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-20">20</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-21">21</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-22">22</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-23">23</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-24">24</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-25">25</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-26">26</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-27">27</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-28">28</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-29">29</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-30">30</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-31">31</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-32">32</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-33">33</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-34">34</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-35">35</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-36">36</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-37">37</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-38">38</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-39">39</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-40">40</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-41">41</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-42">42</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-43">43</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-44">44</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-45">45</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-46">46</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-47">47</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-48">48</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-49">49</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1a2549220251-50">50</div>
<div class="crayon-num" data-line="crayon-5ae7f26fcd1a2549220251-51">51</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5ae7f26fcd1a2549220251-1" class="crayon-line"><span class="crayon-v">yum</span> <span class="crayon-o">-</span><span class="crayon-i">y</span> <span class="crayon-e">install </span><span class="crayon-v">http</span><span class="crayon-o">:</span><span class="crayon-c">//mirror01.idc.hinet.net/EPEL/7/x86_64/e/epel-release-7-5.noarch.rpm</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-2" class="crayon-line crayon-striped-line"><span class="crayon-v">yum</span> <span class="crayon-o">-</span><span class="crayon-i">y</span> <span class="crayon-e">install </span><span class="crayon-e">ppp </span><span class="crayon-e">pptpd</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-3" class="crayon-line"></div>
<div id="crayon-5ae7f26fcd1a2549220251-4" class="crayon-line crayon-striped-line"><span class="crayon-v">cp</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">pptpd</span><span class="crayon-sy">.</span><span class="crayon-v">conf</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">pptpd</span><span class="crayon-sy">.</span><span class="crayon-v">conf</span><span class="crayon-sy">.</span><span class="crayon-e">bak</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-5" class="crayon-line"><span class="crayon-v">cat</span> <span class="crayon-o">&gt;</span><span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">pptpd</span><span class="crayon-sy">.</span><span class="crayon-v">conf</span><span class="crayon-o">&lt;&lt;</span><span class="crayon-e">EOF</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-6" class="crayon-line crayon-striped-line"><span class="crayon-v">option</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">ppp</span><span class="crayon-o">/</span><span class="crayon-v">options</span><span class="crayon-sy">.</span><span class="crayon-e">pptpd</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-7" class="crayon-line"><span class="crayon-e">logwtmp</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-8" class="crayon-line crayon-striped-line"><span class="crayon-i">localip</span> <span class="crayon-cn">10.0.10.1</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-9" class="crayon-line"><span class="crayon-i">remoteip</span> <span class="crayon-cn">10.0.10.2</span><span class="crayon-o">-</span><span class="crayon-cn">254</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-10" class="crayon-line crayon-striped-line"><span class="crayon-e">EOF</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-11" class="crayon-line"></div>
<div id="crayon-5ae7f26fcd1a2549220251-12" class="crayon-line crayon-striped-line"><span class="crayon-v">cp</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">ppp</span><span class="crayon-o">/</span><span class="crayon-v">options</span><span class="crayon-sy">.</span><span class="crayon-v">pptpd</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">ppp</span><span class="crayon-o">/</span><span class="crayon-v">options</span><span class="crayon-sy">.</span><span class="crayon-v">pptpd</span><span class="crayon-sy">.</span><span class="crayon-e">bak</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-13" class="crayon-line"><span class="crayon-v">cat</span> <span class="crayon-o">&gt;</span><span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">ppp</span><span class="crayon-o">/</span><span class="crayon-v">options</span><span class="crayon-sy">.</span><span class="crayon-v">pptpd</span><span class="crayon-o">&lt;&lt;</span><span class="crayon-e">EOF</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-14" class="crayon-line crayon-striped-line"><span class="crayon-e">name </span><span class="crayon-e">pptpd</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-15" class="crayon-line"><span class="crayon-v">refuse</span><span class="crayon-o">-</span><span class="crayon-e">pap</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-16" class="crayon-line crayon-striped-line"><span class="crayon-v">refuse</span><span class="crayon-o">-</span><span class="crayon-e">chap</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-17" class="crayon-line"><span class="crayon-v">refuse</span><span class="crayon-o">-</span><span class="crayon-e">mschap</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-18" class="crayon-line crayon-striped-line"><span class="crayon-v">require</span><span class="crayon-o">-</span><span class="crayon-v">mschap</span><span class="crayon-o">-</span><span class="crayon-e">v2</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-19" class="crayon-line"><span class="crayon-v">require</span><span class="crayon-o">-</span><span class="crayon-v">mppe</span><span class="crayon-o">-</span><span class="crayon-cn">128</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-20" class="crayon-line crayon-striped-line"><span class="crayon-e">proxyarp</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-21" class="crayon-line"><span class="crayon-e">lock</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-22" class="crayon-line crayon-striped-line"><span class="crayon-e">nobsdcomp</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-23" class="crayon-line"><span class="crayon-e">novj</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-24" class="crayon-line crayon-striped-line"><span class="crayon-e">novjccomp</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-25" class="crayon-line"><span class="crayon-e">nologfd</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-26" class="crayon-line crayon-striped-line"><span class="crayon-v">ms</span><span class="crayon-o">-</span><span class="crayon-i">dns</span> <span class="crayon-cn">8.8.8.8</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-27" class="crayon-line"><span class="crayon-v">ms</span><span class="crayon-o">-</span><span class="crayon-i">dns</span> <span class="crayon-cn">8.8.4.4</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-28" class="crayon-line crayon-striped-line"><span class="crayon-e">EOF</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-29" class="crayon-line"></div>
<div id="crayon-5ae7f26fcd1a2549220251-30" class="crayon-line crayon-striped-line"><span class="crayon-v">cp</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">sysctl</span><span class="crayon-sy">.</span><span class="crayon-v">conf</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">sysctl</span><span class="crayon-sy">.</span><span class="crayon-v">conf</span><span class="crayon-sy">.</span><span class="crayon-e">bak</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-31" class="crayon-line"><span class="crayon-v">cat</span> <span class="crayon-o">&gt;</span><span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">sysctl</span><span class="crayon-sy">.</span><span class="crayon-v">conf</span><span class="crayon-o">&lt;&lt;</span><span class="crayon-e">EOF</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-32" class="crayon-line crayon-striped-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">core</span><span class="crayon-sy">.</span><span class="crayon-v">wmem_max</span> <span class="crayon-o">=</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-33" class="crayon-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">core</span><span class="crayon-sy">.</span><span class="crayon-v">rmem_max</span> <span class="crayon-o">=</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-34" class="crayon-line crayon-striped-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">ipv4</span><span class="crayon-sy">.</span><span class="crayon-v">tcp_rmem</span> <span class="crayon-o">=</span> <span class="crayon-cn">10240</span> <span class="crayon-cn">87380</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-35" class="crayon-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">ipv4</span><span class="crayon-sy">.</span><span class="crayon-v">tcp_wmem</span> <span class="crayon-o">=</span> <span class="crayon-cn">10240</span> <span class="crayon-cn">87380</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-36" class="crayon-line crayon-striped-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">core</span><span class="crayon-sy">.</span><span class="crayon-v">wmem_max</span> <span class="crayon-o">=</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-37" class="crayon-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">core</span><span class="crayon-sy">.</span><span class="crayon-v">rmem_max</span> <span class="crayon-o">=</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-38" class="crayon-line crayon-striped-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">ipv4</span><span class="crayon-sy">.</span><span class="crayon-v">tcp_rmem</span> <span class="crayon-o">=</span> <span class="crayon-cn">10240</span> <span class="crayon-cn">87380</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-39" class="crayon-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">ipv4</span><span class="crayon-sy">.</span><span class="crayon-v">tcp_wmem</span> <span class="crayon-o">=</span> <span class="crayon-cn">10240</span> <span class="crayon-cn">87380</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-40" class="crayon-line crayon-striped-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">core</span><span class="crayon-sy">.</span><span class="crayon-v">wmem_max</span> <span class="crayon-o">=</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-41" class="crayon-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">core</span><span class="crayon-sy">.</span><span class="crayon-v">rmem_max</span> <span class="crayon-o">=</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-42" class="crayon-line crayon-striped-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">ipv4</span><span class="crayon-sy">.</span><span class="crayon-v">tcp_rmem</span> <span class="crayon-o">=</span> <span class="crayon-cn">10240</span> <span class="crayon-cn">87380</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-43" class="crayon-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">ipv4</span><span class="crayon-sy">.</span><span class="crayon-v">tcp_wmem</span> <span class="crayon-o">=</span> <span class="crayon-cn">10240</span> <span class="crayon-cn">87380</span> <span class="crayon-cn">12582912</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-44" class="crayon-line crayon-striped-line"><span class="crayon-v">net</span><span class="crayon-sy">.</span><span class="crayon-v">ipv4</span><span class="crayon-sy">.</span><span class="crayon-v">ip_forward</span> <span class="crayon-o">=</span> <span class="crayon-cn">1</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-45" class="crayon-line"><span class="crayon-e">EOF</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-46" class="crayon-line crayon-striped-line"><span class="crayon-v">sysctl</span> <span class="crayon-o">-</span><span class="crayon-i">p</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-47" class="crayon-line"></div>
<div id="crayon-5ae7f26fcd1a2549220251-48" class="crayon-line crayon-striped-line"></div>
<div id="crayon-5ae7f26fcd1a2549220251-49" class="crayon-line"><span class="crayon-v">chmod</span> <span class="crayon-o">+</span><span class="crayon-v">x</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">rc</span><span class="crayon-sy">.</span><span class="crayon-v">d</span><span class="crayon-o">/</span><span class="crayon-v">rc</span><span class="crayon-sy">.</span><span class="crayon-e">local</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-50" class="crayon-line crayon-striped-line"><span class="crayon-i">echo</span> <span class="crayon-s">"iptables -t nat -A POSTROUTING -s 10.0.10.0/24 -o eth0 -j MASQUERADE"</span> <span class="crayon-o">&gt;&gt;</span> <span class="crayon-o">/</span><span class="crayon-v">etc</span><span class="crayon-o">/</span><span class="crayon-v">rc</span><span class="crayon-sy">.</span><span class="crayon-v">d</span><span class="crayon-o">/</span><span class="crayon-v">rc</span><span class="crayon-sy">.</span><span class="crayon-e">local</span></div>
<div id="crayon-5ae7f26fcd1a2549220251-51" class="crayon-line"><span class="crayon-v">iptables</span> <span class="crayon-o">-</span><span class="crayon-i">t</span> <span class="crayon-v">nat</span> <span class="crayon-o">-</span><span class="crayon-i">A</span> <span class="crayon-v">POSTROUTING</span> <span class="crayon-o">-</span><span class="crayon-i">s</span> <span class="crayon-cn">10.0.10.0</span><span class="crayon-o">/</span><span class="crayon-cn">24</span> <span class="crayon-o">-</span><span class="crayon-i">o</span> <span class="crayon-v">eth0</span> <span class="crayon-o">-</span><span class="crayon-i">j</span> <span class="crayon-v">MASQUERADE</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
完成後，執行 build-pptp.sh
<div id="crayon-5ae7f26fcd1b3550217715" class="crayon-syntax crayon-theme-classic crayon-font-courier-new crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover">
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5ae7f26fcd1b3550217715-1">1</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5ae7f26fcd1b3550217715-1" class="crayon-line"><span class="crayon-v">root</span> <span class="crayon-p"># ./build-pptp.sh</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
編輯 /etc/ppp/chap-secrets 可設定使用者帳號密碼
<code>VPNUSER pptpd VPNPASS *</code>

啟動 pptpd 服務
<div id="crayon-5ae7f26fcd1b8157469967" class="crayon-syntax crayon-theme-classic crayon-font-courier-new crayon-os-pc print-yes notranslate" data-settings=" minimize scroll-mouseover">
<div class="crayon-toolbar" data-settings=" mouseover overlay hide delay"></div>
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="show">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5ae7f26fcd1b8157469967-1">1</div>
<div class="crayon-num crayon-striped-num" data-line="crayon-5ae7f26fcd1b8157469967-2">2</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5ae7f26fcd1b8157469967-1" class="crayon-line"><span class="crayon-v">root</span> <span class="crayon-p"># systemctl start pptpd</span></div>
<div id="crayon-5ae7f26fcd1b8157469967-2" class="crayon-line crayon-striped-line"><span class="crayon-v">root</span> <span class="crayon-p"># systemctl enable pptpd.service</span></div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
// Steven