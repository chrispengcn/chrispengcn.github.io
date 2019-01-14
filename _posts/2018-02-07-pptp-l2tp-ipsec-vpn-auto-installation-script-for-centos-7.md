---
ID: 1119
post_title: >
  BoizZ/PPTP-L2TP-IPSec-VPN-auto-installation-script-for-CentOS-7
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2018/02/07/pptp-l2tp-ipsec-vpn-auto-installation-script-for-centos-7/
published: true
post_date: 2018-02-07 15:02:26
---
https://github.com/BoizZ/PPTP-L2TP-IPSec-VPN-auto-installation-script-for-CentOS-7
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC1" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#!/bin/bash</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC2" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#############################################################</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC3" class="blob-code blob-code-inner js-file-line"><span class="pl-c"># #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC4" class="blob-code blob-code-inner js-file-line"><span class="pl-c"># This is a PPTP and L2TP VPN installation for CentOS 7 #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC5" class="blob-code blob-code-inner js-file-line"><span class="pl-c"># Version: 1.1.1 20160507 #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC6" class="blob-code blob-code-inner js-file-line"><span class="pl-c"># Author: Bon Hoo #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC7" class="blob-code blob-code-inner js-file-line"><span class="pl-c"># Website: http://www.ccwebsite.com #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC8" class="blob-code blob-code-inner js-file-line"><span class="pl-c"># #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC9" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#############################################################</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC10" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC11" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#检测是否是root用户</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC12" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-s"><span class="pl-pds">$(</span>id -u<span class="pl-pds">)</span></span> <span class="pl-k">!=</span> <span class="pl-s"><span class="pl-pds">"</span>0<span class="pl-pds">"</span></span> ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC13" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>e[42me[31mError: You must be root to run this install script.e[0mn<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC14" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">exit</span> 1</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC15" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC16" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC17" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#检测是否是CentOS 7或者RHEL 7</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC18" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-s"><span class="pl-pds">$(</span>grep <span class="pl-pds">"</span>release 7.<span class="pl-pds">"</span> /etc/redhat-release <span class="pl-k">2&gt;</span>/dev/null <span class="pl-k">|</span> wc -l<span class="pl-pds">)</span></span> <span class="pl-k">-eq</span> 0 ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC19" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>e[42me[31mError: Your OS is NOT CentOS 7 or RHEL 7.e[0mn<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC20" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>e[42me[31mThis install script is ONLY for CentOS 7 and RHEL 7.e[0mn<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC21" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">exit</span> 1</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC22" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC23" class="blob-code blob-code-inner js-file-line">clear</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC24" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC25" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC26" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#############################################################</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC27" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC28" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># This is a PPTP and L2TP VPN installation for CentOS 7 #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC29" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Version: 1.1.1 20160507 #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC30" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Author: Bon Hoo #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC31" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Website: http://www.ccwebsite.com #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC32" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC33" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#############################################################</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC34" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC35" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC36" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#获取服务器IP</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC37" class="blob-code blob-code-inner js-file-line">serverip=<span class="pl-s"><span class="pl-pds">$(</span>ifconfig -a <span class="pl-k">|</span>grep -w <span class="pl-pds">"</span>inet<span class="pl-pds">"</span><span class="pl-k">|</span> grep -v <span class="pl-pds">"</span>127.0.0.1<span class="pl-pds">"</span> <span class="pl-k">|</span>awk <span class="pl-pds">'</span>{print $2;}<span class="pl-pds">'</span><span class="pl-pds">)</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC38" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>e[33m<span class="pl-smi">$serverip</span>e[0m is the server IP?<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC39" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>If e[33m<span class="pl-smi">$serverip</span>e[0m is e[33mcorrecte[0m, press enter directly.<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC40" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>If e[33m<span class="pl-smi">$serverip</span>e[0m is e[33mincorrecte[0m, please input your server IP.<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC41" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>(Default server IP: e[33m<span class="pl-smi">$serverip</span>e[0m):<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC42" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">read</span> serveriptmp</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC43" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-k">-n</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$serveriptmp</span><span class="pl-pds">"</span></span> ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC44" class="blob-code blob-code-inner js-file-line">serverip=<span class="pl-smi">$serveriptmp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC45" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC46" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC47" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#获取网卡接口名称</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC48" class="blob-code blob-code-inner js-file-line">ethlist=<span class="pl-s"><span class="pl-pds">$(</span>ifconfig <span class="pl-k">|</span> grep <span class="pl-pds">"</span>: flags<span class="pl-pds">"</span> <span class="pl-k">|</span> cut -d <span class="pl-pds">"</span>:<span class="pl-pds">"</span> -f1<span class="pl-pds">)</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC49" class="blob-code blob-code-inner js-file-line">eth=<span class="pl-s"><span class="pl-pds">$(</span>printf <span class="pl-pds">"</span><span class="pl-smi">$ethlist</span>n<span class="pl-pds">"</span> <span class="pl-k">|</span> head -n 1<span class="pl-pds">)</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC50" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-s"><span class="pl-pds">$(</span>printf <span class="pl-pds">"</span><span class="pl-smi">$ethlist</span>n<span class="pl-pds">"</span> <span class="pl-k">|</span> wc -l<span class="pl-pds">)</span></span> <span class="pl-k">-gt</span> 2 ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC51" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> ======================================</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC52" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Network Interface list:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC53" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>e[33m<span class="pl-smi">$ethlist</span>e[0mn<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC54" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> ======================================</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC55" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Which network interface you want to listen for ocserv?<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC56" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>Default network interface is e[33m<span class="pl-smi">$eth</span>e[0m, let it blank to use default network interface: <span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC57" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">read</span> ethtmp</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC58" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [ <span class="pl-k">-n</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$ethtmp</span><span class="pl-pds">"</span></span> ]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC59" class="blob-code blob-code-inner js-file-line">eth=<span class="pl-smi">$ethtmp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC60" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC61" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC62" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC63" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#设置VPN拨号后分配的IP段</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC64" class="blob-code blob-code-inner js-file-line">iprange=<span class="pl-s"><span class="pl-pds">"</span>10.0.1<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC65" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Please input IP-Range:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC66" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>(Default IP-Range: e[33m<span class="pl-smi">$iprange</span>e[0m): <span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC67" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">read</span> iprangetmp</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC68" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-k">-n</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$iprangetmp</span><span class="pl-pds">"</span></span> ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC69" class="blob-code blob-code-inner js-file-line">iprange=<span class="pl-smi">$iprangetmp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC70" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC71" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC72" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#设置预共享密钥</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC73" class="blob-code blob-code-inner js-file-line">mypsk=<span class="pl-s"><span class="pl-pds">"</span>ueibo.cn<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC74" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Please input PSK:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC75" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>(Default PSK: e[33mueibo.cne[0m): <span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC76" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">read</span> mypsktmp</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC77" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-k">-n</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$mypsktmp</span><span class="pl-pds">"</span></span> ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC78" class="blob-code blob-code-inner js-file-line">mypsk=<span class="pl-smi">$mypsktmp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC79" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC80" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC81" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#设置VPN用户名</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC82" class="blob-code blob-code-inner js-file-line">username=<span class="pl-s"><span class="pl-pds">"</span>ueibo.com<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC83" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Please input VPN username:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC84" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>(Default VPN username: e[33mueibo.come[0m): <span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC85" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">read</span> usernametmp</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC86" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-k">-n</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$usernametmp</span><span class="pl-pds">"</span></span> ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC87" class="blob-code blob-code-inner js-file-line">username=<span class="pl-smi">$usernametmp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC88" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC89" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC90" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#随机密码</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC91" class="blob-code blob-code-inner js-file-line"><span class="pl-en">randstr</span>() {</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC92" class="blob-code blob-code-inner js-file-line">index=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC93" class="blob-code blob-code-inner js-file-line">str=<span class="pl-s"><span class="pl-pds">"</span><span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC94" class="blob-code blob-code-inner js-file-line"><span class="pl-k">for</span> <span class="pl-smi">i</span> <span class="pl-k">in</span> {a..z}<span class="pl-k">;</span> <span class="pl-k">do</span> arr[index]=<span class="pl-smi">$i</span><span class="pl-k">;</span> index=<span class="pl-s"><span class="pl-pds">$(</span>expr <span class="pl-smi">${index}</span> + 1<span class="pl-pds">)</span></span><span class="pl-k">;</span> <span class="pl-k">done</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC95" class="blob-code blob-code-inner js-file-line"><span class="pl-k">for</span> <span class="pl-smi">i</span> <span class="pl-k">in</span> {A..Z}<span class="pl-k">;</span> <span class="pl-k">do</span> arr[index]=<span class="pl-smi">$i</span><span class="pl-k">;</span> index=<span class="pl-s"><span class="pl-pds">$(</span>expr <span class="pl-smi">${index}</span> + 1<span class="pl-pds">)</span></span><span class="pl-k">;</span> <span class="pl-k">done</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC96" class="blob-code blob-code-inner js-file-line"><span class="pl-k">for</span> <span class="pl-smi">i</span> <span class="pl-k">in</span> {0..9}<span class="pl-k">;</span> <span class="pl-k">do</span> arr[index]=<span class="pl-smi">$i</span><span class="pl-k">;</span> index=<span class="pl-s"><span class="pl-pds">$(</span>expr <span class="pl-smi">${index}</span> + 1<span class="pl-pds">)</span></span><span class="pl-k">;</span> <span class="pl-k">done</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC97" class="blob-code blob-code-inner js-file-line"><span class="pl-k">for</span> <span class="pl-smi">i</span> <span class="pl-k">in</span> {1..10}<span class="pl-k">;</span> <span class="pl-k">do</span> str=<span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$str</span><span class="pl-smi">${arr[$RANDOM%$index]}</span><span class="pl-pds">"</span></span><span class="pl-k">;</span> <span class="pl-k">done</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC98" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-smi">$str</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC99" class="blob-code blob-code-inner js-file-line">}</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC100" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC101" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#设置VPN用户密码</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC102" class="blob-code blob-code-inner js-file-line">password=<span class="pl-s"><span class="pl-pds">$(</span>randstr<span class="pl-pds">)</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC103" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>Please input e[33m<span class="pl-smi">$username</span>e[0m's password:n<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC104" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span>Default password is e[33m<span class="pl-smi">$password</span>e[0m, let it blank to use default password: <span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC105" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">read</span> passwordtmp</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC106" class="blob-code blob-code-inner js-file-line"><span class="pl-k">if</span> [[ <span class="pl-k">-n</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$passwordtmp</span><span class="pl-pds">"</span></span> ]]<span class="pl-k">;</span> <span class="pl-k">then</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC107" class="blob-code blob-code-inner js-file-line">password=<span class="pl-smi">$passwordtmp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC108" class="blob-code blob-code-inner js-file-line"><span class="pl-k">fi</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC109" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC110" class="blob-code blob-code-inner js-file-line">clear</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC111" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC112" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#打印配置参数</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC113" class="blob-code blob-code-inner js-file-line">clear</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC114" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Server IP:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC115" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$serverip</span><span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC116" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC117" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Server Local IP:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC118" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$iprange</span>.1<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC119" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC120" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Client Remote IP Range:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC121" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$iprange</span>.10-<span class="pl-smi">$iprange</span>.254<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC122" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC123" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>PSK:<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC124" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span><span class="pl-smi">$mypsk</span><span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC125" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC126" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">echo</span> <span class="pl-s"><span class="pl-pds">"</span>Press any key to start...<span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC127" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC128" class="blob-code blob-code-inner js-file-line"><span class="pl-en">get_char</span>() {</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC129" class="blob-code blob-code-inner js-file-line">SAVEDSTTY=<span class="pl-s"><span class="pl-pds">`</span>stty -g<span class="pl-pds">`</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC130" class="blob-code blob-code-inner js-file-line">stty -echo</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC131" class="blob-code blob-code-inner js-file-line">stty cbreak</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC132" class="blob-code blob-code-inner js-file-line">dd if=/dev/tty bs=1 count=1 <span class="pl-k">2&gt;</span> /dev/null</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC133" class="blob-code blob-code-inner js-file-line">stty -raw</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC134" class="blob-code blob-code-inner js-file-line">stty <span class="pl-c1">echo</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC135" class="blob-code blob-code-inner js-file-line">stty <span class="pl-smi">$SAVEDSTTY</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC136" class="blob-code blob-code-inner js-file-line">}</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC137" class="blob-code blob-code-inner js-file-line">char=<span class="pl-s"><span class="pl-pds">$(</span>get_char<span class="pl-pds">)</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC138" class="blob-code blob-code-inner js-file-line">clear</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC139" class="blob-code blob-code-inner js-file-line">mknod /dev/random c 1 9</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC140" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC141" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#更新组件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC142" class="blob-code blob-code-inner js-file-line">yum update -y</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC143" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC144" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#安装epel源</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC145" class="blob-code blob-code-inner js-file-line">yum install epel-release -y</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC146" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC147" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#安装依赖的组件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC148" class="blob-code blob-code-inner js-file-line">yum install -y openswan ppp pptpd xl2tpd wget</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC149" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC150" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#创建ipsec.conf配置文件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC151" class="blob-code blob-code-inner js-file-line">rm -f /etc/ipsec.conf</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC152" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/ipsec.conf<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC153" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># /etc/ipsec.conf - Libreswan IPsec configuration file</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC154" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC155" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># This file: /etc/ipsec.conf</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC156" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC157" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Enable when using this configuration file with openswan instead of libreswan</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC158" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#version 2</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC159" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC160" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Manual: ipsec.conf.5</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC161" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC162" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># basic configuration</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC163" class="blob-code blob-code-inner js-file-line"><span class="pl-s">config setup</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC164" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> # NAT-TRAVERSAL support, see README.NAT-Traversal</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC165" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> nat_traversal=yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC166" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> # exclude networks used on server side by adding %v4:!a.b.c.0/24</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC167" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> virtual_private=%v4:10.0.0.0/8,%v4:192.168.0.0/16,%v4:172.16.0.0/12</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC168" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> # OE is now off by default. Uncomment and change to on, to enable.</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC169" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> oe=off</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC170" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> # which IPsec stack to use. auto will try netkey, then klips then mast</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC171" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> protostack=netkey</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC172" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> force_keepalive=yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC173" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> keep_alive=1800</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC174" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC175" class="blob-code blob-code-inner js-file-line"><span class="pl-s">conn L2TP-PSK-NAT</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC176" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> rightsubnet=vhost:%priv</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC177" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> also=L2TP-PSK-noNAT</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC178" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC179" class="blob-code blob-code-inner js-file-line"><span class="pl-s">conn L2TP-PSK-noNAT</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC180" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> authby=secret</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC181" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> pfs=no</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC182" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> auto=add</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC183" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> keyingtries=3</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC184" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> rekey=no</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC185" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> ikelifetime=8h</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC186" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> keylife=1h</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC187" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> type=transport</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC188" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> left=$serverip</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC189" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> leftid=$serverip</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC190" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> leftprotoport=17/1701</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC191" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> right=%any</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC192" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> rightprotoport=17/%any</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC193" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> dpddelay=40</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC194" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> dpdtimeout=130</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC195" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> dpdaction=clear</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC196" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> leftnexthop=%defaultroute</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC197" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> rightnexthop=%defaultroute</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC198" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> ike=3des-sha1,aes-sha1,aes256-sha1,aes256-sha2_256 </span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC199" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> phase2alg=3des-sha1,aes-sha1,aes256-sha1,aes256-sha2_256</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC200" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> sha2-truncbug=yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC201" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># For example connections, see your distribution's documentation directory,</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC202" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># or the documentation which could be located at</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC203" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># /usr/share/docs/libreswan-3.*/ or look at https://www.libreswan.org/</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC204" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC205" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># There is also a lot of information in the manual page, "man ipsec.conf"</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC206" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC207" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># You may put your configuration (.conf) file in the "/etc/ipsec.d/" directory</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC208" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># by uncommenting this line</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC209" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#include /etc/ipsec.d/*.conf</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC210" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC211" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC212" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#设置预共享密钥配置文件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC213" class="blob-code blob-code-inner js-file-line">rm -f /etc/ipsec.secrets</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC214" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/ipsec.secrets<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC215" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#include /etc/ipsec.d/*.secrets</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC216" class="blob-code blob-code-inner js-file-line"><span class="pl-s">$serverip %any: PSK "$mypsk"</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC217" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC218" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC219" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#创建pptpd.conf配置文件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC220" class="blob-code blob-code-inner js-file-line">rm -f /etc/pptpd.conf</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC221" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/pptpd.conf<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC222" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#ppp /usr/sbin/pppd</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC223" class="blob-code blob-code-inner js-file-line"><span class="pl-s">option /etc/ppp/options.pptpd</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC224" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#debug</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC225" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># stimeout 10</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC226" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#noipparam</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC227" class="blob-code blob-code-inner js-file-line"><span class="pl-s">logwtmp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC228" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#vrf test</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC229" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#bcrelay eth1</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC230" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#delegate</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC231" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#connections 100</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC232" class="blob-code blob-code-inner js-file-line"><span class="pl-s">localip $iprange.2</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC233" class="blob-code blob-code-inner js-file-line"><span class="pl-s">remoteip $iprange.200-254</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC234" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC235" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC236" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC237" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#创建xl2tpd.conf配置文件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC238" class="blob-code blob-code-inner js-file-line">mkdir -p /etc/xl2tpd</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC239" class="blob-code blob-code-inner js-file-line">rm -f /etc/xl2tpd/xl2tpd.conf</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC240" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/xl2tpd/xl2tpd.conf<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC241" class="blob-code blob-code-inner js-file-line"><span class="pl-s">;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC242" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; This is a minimal sample xl2tpd configuration file for use</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC243" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; with L2TP over IPsec.</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC244" class="blob-code blob-code-inner js-file-line"><span class="pl-s">;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC245" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; The idea is to provide an L2TP daemon to which remote Windows L2TP/IPsec</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC246" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; clients connect. In this example, the internal (protected) network</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC247" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; is 192.168.1.0/24. A special IP range within this network is reserved</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC248" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; for the remote clients: 192.168.1.128/25</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC249" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; (i.e. 192.168.1.128 ... 192.168.1.254)</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC250" class="blob-code blob-code-inner js-file-line"><span class="pl-s">;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC251" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; The listen-addr parameter can be used if you want to bind the L2TP daemon</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC252" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; to a specific IP address instead of to all interfaces. For instance,</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC253" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; you could bind it to the interface of the internal LAN (e.g. 192.168.1.98</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC254" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; in the example below). Yet another IP address (local ip, e.g. 192.168.1.99)</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC255" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; will be used by xl2tpd as its address on pppX interfaces.</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC256" class="blob-code blob-code-inner js-file-line"><span class="pl-s">[global]</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC257" class="blob-code blob-code-inner js-file-line"><span class="pl-s">; ipsec saref = yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC258" class="blob-code blob-code-inner js-file-line"><span class="pl-s">listen-addr = $serverip</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC259" class="blob-code blob-code-inner js-file-line"><span class="pl-s">auth file = /etc/ppp/chap-secrets</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC260" class="blob-code blob-code-inner js-file-line"><span class="pl-s">port = 1701</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC261" class="blob-code blob-code-inner js-file-line"><span class="pl-s">[lns default]</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC262" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ip range = $iprange.10-$iprange.199</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC263" class="blob-code blob-code-inner js-file-line"><span class="pl-s">local ip = $iprange.1</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC264" class="blob-code blob-code-inner js-file-line"><span class="pl-s">refuse chap = yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC265" class="blob-code blob-code-inner js-file-line"><span class="pl-s">refuse pap = yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC266" class="blob-code blob-code-inner js-file-line"><span class="pl-s">require authentication = yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC267" class="blob-code blob-code-inner js-file-line"><span class="pl-s">name = L2TPVPN</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC268" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ppp debug = yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC269" class="blob-code blob-code-inner js-file-line"><span class="pl-s">pppoptfile = /etc/ppp/options.xl2tpd</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC270" class="blob-code blob-code-inner js-file-line"><span class="pl-s">length bit = yes</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC271" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC272" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC273" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#创建options.pptpd配置文件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC274" class="blob-code blob-code-inner js-file-line">mkdir -p /etc/ppp</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC275" class="blob-code blob-code-inner js-file-line">rm -f /etc/ppp/options.pptpd</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC276" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/ppp/options.pptpd<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC277" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Authentication</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC278" class="blob-code blob-code-inner js-file-line"><span class="pl-s">name pptpd</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC279" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#chapms-strip-domain</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC280" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC281" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Encryption</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC282" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># BSD licensed ppp-2.4.2 upstream with MPPE only, kernel module ppp_mppe.o</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC283" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># {{{</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC284" class="blob-code blob-code-inner js-file-line"><span class="pl-s">refuse-pap</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC285" class="blob-code blob-code-inner js-file-line"><span class="pl-s">refuse-chap</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC286" class="blob-code blob-code-inner js-file-line"><span class="pl-s">refuse-mschap</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC287" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Require the peer to authenticate itself using MS-CHAPv2 [Microsoft</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC288" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Challenge Handshake Authentication Protocol, Version 2] authentication.</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC289" class="blob-code blob-code-inner js-file-line"><span class="pl-s">require-mschap-v2</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC290" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Require MPPE 128-bit encryption</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC291" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># (note that MPPE requires the use of MSCHAP-V2 during authentication)</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC292" class="blob-code blob-code-inner js-file-line"><span class="pl-s">require-mppe-128</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC293" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># }}}</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC294" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC295" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># OpenSSL licensed ppp-2.4.1 fork with MPPE only, kernel module mppe.o</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC296" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># {{{</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC297" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#-chap</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC298" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#-chapms</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC299" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Require the peer to authenticate itself using MS-CHAPv2 [Microsoft</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC300" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Challenge Handshake Authentication Protocol, Version 2] authentication.</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC301" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#+chapms-v2</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC302" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Require MPPE encryption</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC303" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># (note that MPPE requires the use of MSCHAP-V2 during authentication)</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC304" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#mppe-40 # enable either 40-bit or 128-bit, not both</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC305" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#mppe-128</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC306" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#mppe-stateless</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC307" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># }}}</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC308" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC309" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ms-dns 8.8.4.4</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC310" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ms-dns 8.8.8.8</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC311" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC312" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#ms-wins 10.0.0.3</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC313" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#ms-wins 10.0.0.4</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC314" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC315" class="blob-code blob-code-inner js-file-line"><span class="pl-s">proxyarp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC316" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#10.8.0.100</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC317" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC318" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Logging</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC319" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#debug</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC320" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#dump</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC321" class="blob-code blob-code-inner js-file-line"><span class="pl-s">lock</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC322" class="blob-code blob-code-inner js-file-line"><span class="pl-s">nobsdcomp </span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC323" class="blob-code blob-code-inner js-file-line"><span class="pl-s">novj</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC324" class="blob-code blob-code-inner js-file-line"><span class="pl-s">novjccomp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC325" class="blob-code blob-code-inner js-file-line"><span class="pl-s">nologfd</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC326" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC327" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC328" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC329" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#创建options.xl2tpd配置文件</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC330" class="blob-code blob-code-inner js-file-line">rm -f /etc/ppp/options.xl2tpd</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC331" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/ppp/options.xl2tpd<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC332" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#require-pap</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC333" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#require-chap</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC334" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#require-mschap</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC335" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ipcp-accept-local</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC336" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ipcp-accept-remote</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC337" class="blob-code blob-code-inner js-file-line"><span class="pl-s">require-mschap-v2</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC338" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ms-dns 8.8.8.8</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC339" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ms-dns 8.8.4.4</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC340" class="blob-code blob-code-inner js-file-line"><span class="pl-s">asyncmap 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC341" class="blob-code blob-code-inner js-file-line"><span class="pl-s">auth</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC342" class="blob-code blob-code-inner js-file-line"><span class="pl-s">crtscts</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC343" class="blob-code blob-code-inner js-file-line"><span class="pl-s">lock</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC344" class="blob-code blob-code-inner js-file-line"><span class="pl-s">hide-password</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC345" class="blob-code blob-code-inner js-file-line"><span class="pl-s">modem</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC346" class="blob-code blob-code-inner js-file-line"><span class="pl-s">debug</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC347" class="blob-code blob-code-inner js-file-line"><span class="pl-s">name l2tpd</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC348" class="blob-code blob-code-inner js-file-line"><span class="pl-s">proxyarp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC349" class="blob-code blob-code-inner js-file-line"><span class="pl-s">lcp-echo-interval 30</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC350" class="blob-code blob-code-inner js-file-line"><span class="pl-s">lcp-echo-failure 4</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC351" class="blob-code blob-code-inner js-file-line"><span class="pl-s">mtu 1400</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC352" class="blob-code blob-code-inner js-file-line"><span class="pl-s">noccp</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC353" class="blob-code blob-code-inner js-file-line"><span class="pl-s">connect-delay 5000</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC354" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># To allow authentication against a Windows domain EXAMPLE, and require the</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC355" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># user to be in a group "VPN Users". Requires the samba-winbind package</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC356" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># require-mschap-v2</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC357" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># plugin winbind.so</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC358" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># ntlm_auth-helper '/usr/bin/ntlm_auth --helper-protocol=ntlm-server-1 --require-membership-of="EXAMPLEVPN Users"'</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC359" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># You need to join the domain on the server, for example using samba:</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC360" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># http://rootmanager.com/ubuntu-ipsec-l2tp-windows-domain-auth/setting-up-openswan-xl2tpd-with-native-windows-clients-lucid.html</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC361" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC362" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC363" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#创建chap-secrets配置文件，即用户列表及密码</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC364" class="blob-code blob-code-inner js-file-line">rm -f /etc/ppp/chap-secrets</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC365" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/ppp/chap-secrets<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC366" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Secrets for authentication using CHAP</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC367" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># client server secret IP addresses</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC368" class="blob-code blob-code-inner js-file-line"><span class="pl-s">$username pptpd $password *</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC369" class="blob-code blob-code-inner js-file-line"><span class="pl-s">$username l2tpd $password *</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC370" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC371" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC372" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#修改系统配置，允许IP转发</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC373" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.ip_forward=1</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC374" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.conf.all.rp_filter=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC375" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.conf.default.rp_filter=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC376" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.conf.<span class="pl-smi">$eth</span>.rp_filter=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC377" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.conf.all.send_redirects=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC378" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.conf.default.send_redirects=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC379" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.conf.all.accept_redirects=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC380" class="blob-code blob-code-inner js-file-line">sysctl -w net.ipv4.conf.default.accept_redirects=0</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC381" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC382" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/etc/sysctl.conf<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC383" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC384" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.ip_forward = 1</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC385" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.conf.all.rp_filter = 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC386" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.conf.default.rp_filter = 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC387" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.conf.$eth.rp_filter = 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC388" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.conf.all.send_redirects = 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC389" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.conf.default.send_redirects = 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC390" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.conf.all.accept_redirects = 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC391" class="blob-code blob-code-inner js-file-line"><span class="pl-s">net.ipv4.conf.default.accept_redirects = 0</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC392" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC393" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC394" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#允许防火墙端口</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC395" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/usr/lib/firewalld/services/pptpd.xml<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC396" class="blob-code blob-code-inner js-file-line"><span class="pl-s">&lt;?xml version="1.0" encoding="utf-8"?&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC397" class="blob-code blob-code-inner js-file-line"><span class="pl-s">&lt;service&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC398" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;short&gt;pptpd&lt;/short&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC399" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;description&gt;PPTP and Fuck the GFW&lt;/description&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC400" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;port protocol="tcp" port="1723"/&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC401" class="blob-code blob-code-inner js-file-line"><span class="pl-s">&lt;/service&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC402" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC403" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC404" class="blob-code blob-code-inner js-file-line">cat <span class="pl-k">&gt;&gt;</span>/usr/lib/firewalld/services/l2tpd.xml<span class="pl-s"><span class="pl-k">&lt;&lt;</span><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC405" class="blob-code blob-code-inner js-file-line"><span class="pl-s">&lt;?xml version="1.0" encoding="utf-8"?&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC406" class="blob-code blob-code-inner js-file-line"><span class="pl-s">&lt;service&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC407" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;short&gt;l2tpd&lt;/short&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC408" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;description&gt;L2TP IPSec&lt;/description&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC409" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;port protocol="udp" port="500"/&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC410" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;port protocol="udp" port="4500"/&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC411" class="blob-code blob-code-inner js-file-line"><span class="pl-s"> &lt;port protocol="udp" port="1701"/&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC412" class="blob-code blob-code-inner js-file-line"><span class="pl-s">&lt;/service&gt;</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC413" class="blob-code blob-code-inner js-file-line"><span class="pl-s"><span class="pl-k">EOF</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC414" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC415" class="blob-code blob-code-inner js-file-line">firewall-cmd --reload</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC416" class="blob-code blob-code-inner js-file-line">firewall-cmd --permanent --add-service=pptpd</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC417" class="blob-code blob-code-inner js-file-line">firewall-cmd --permanent --add-service=l2tpd</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC418" class="blob-code blob-code-inner js-file-line">firewall-cmd --permanent --add-service=ipsec</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC419" class="blob-code blob-code-inner js-file-line">firewall-cmd --permanent --add-masquerade</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC420" class="blob-code blob-code-inner js-file-line">firewall-cmd --permanent --direct --add-rule ipv4 filter FORWARD 0 -p tcp -i ppp+ -j TCPMSS --syn --set-mss 1356</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC421" class="blob-code blob-code-inner js-file-line">firewall-cmd --reload</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC422" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#iptables --table nat --append POSTROUTING --jump MASQUERADE</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC423" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#iptables -t nat -A POSTROUTING -s $iprange.0/24 -o $eth -j MASQUERADE</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC424" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#iptables -t nat -A POSTROUTING -s $iprange.0/24 -j SNAT --to-source $serverip</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC425" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#iptables -I FORWARD -p tcp –syn -i ppp+ -j TCPMSS –set-mss 1356</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC426" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#service iptables save</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC427" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC428" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#允许开机启动</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC429" class="blob-code blob-code-inner js-file-line">systemctl <span class="pl-c1">enable</span> pptpd ipsec xl2tpd</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC430" class="blob-code blob-code-inner js-file-line">systemctl restart pptpd ipsec xl2tpd</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC431" class="blob-code blob-code-inner js-file-line">clear</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC432" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC433" class="blob-code blob-code-inner js-file-line"><span class="pl-c">#测试ipsec</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC434" class="blob-code blob-code-inner js-file-line">ipsec verify</td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC435" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC436" class="blob-code blob-code-inner js-file-line"><span class="pl-c1">printf</span> <span class="pl-s"><span class="pl-pds">"</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC437" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#############################################################</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC438" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC439" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># This is a PPTP and L2TP VPN installation for CentOS 7 #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC440" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Version: 1.1.1 20160507 #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC441" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Author: Bon Hoo #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC442" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># Website: http://www.ccwebsite.com #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC443" class="blob-code blob-code-inner js-file-line"><span class="pl-s"># #</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC444" class="blob-code blob-code-inner js-file-line"><span class="pl-s">#############################################################</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC445" class="blob-code blob-code-inner js-file-line"><span class="pl-s">if there are no [FAILED] above, then you can</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC446" class="blob-code blob-code-inner js-file-line"><span class="pl-s">connect to your L2TP VPN Server with the default</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC447" class="blob-code blob-code-inner js-file-line"><span class="pl-s">user/password below:</span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC448" class="blob-code blob-code-inner js-file-line"></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC449" class="blob-code blob-code-inner js-file-line"><span class="pl-s">ServerIP: <span class="pl-smi">$serverip</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC450" class="blob-code blob-code-inner js-file-line"><span class="pl-s">username: <span class="pl-smi">$username</span></span></td>
</tr>
</tbody>
</table>
<table class="highlight tab-size js-file-line-container" data-tab-size="8">
<tbody>
<tr>
<td id="LC451" class="blob-code blob-code-inner js-file-line"><span class="pl-s">password: <span class="pl-smi">$password</span></span></td>
</tr>
</tbody>
</table>
<span class="pl-s">PSK: <span class="pl-smi">$mypsk</span></span>