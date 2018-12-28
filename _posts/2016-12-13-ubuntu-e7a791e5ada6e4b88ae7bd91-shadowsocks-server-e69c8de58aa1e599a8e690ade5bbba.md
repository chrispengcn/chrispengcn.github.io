---
ID: 589
post_title: >
  ubuntu 科学上网 shadowsocks server
  服务器搭建
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2016/12/13/ubuntu-%e7%a7%91%e5%ad%a6%e4%b8%8a%e7%bd%91-shadowsocks-server-%e6%9c%8d%e5%8a%a1%e5%99%a8%e6%90%ad%e5%bb%ba/'
published: true
post_date: 2016-12-13 12:15:08
---
ubuntu 科学上网 shadowsocks 服务器搭建

&nbsp;

安装shadowsocks依赖
<blockquote>sudo -s // 获取超级管理员权限

apt-get update // 更新apt-get

apt-get install python-pip // 安装python包管理工具pip

pip install shadowsocks // 安装shadowsocks

ssserver -c /etc/shadowsocks.json -d start // 启动shadowsocks

ssserver -c /etc/shadowsocks.json -d restart // 重启shadowsocks</blockquote>
&nbsp;

配置shadowsocks
<blockquote>vi /etc/shadowsocks.json</blockquote>
（ps. 可能有人不会用vi，这里简单说下，vi打开文件后，按i即可进入编辑状态，编辑完后，按esc退出编辑状态，按:进入命令状态，输入wq即可保存并退出，还不懂的话自行百度vi吧）

单一端口配置：
<pre>{
"server":"0.0.0.0",
"server_port":8888,
"local_address":"127.0.0.1",
"local_port":1080,
"password":"zhimakaimen",
"timeout":300,
"method":"rc4-md5",
"fast_open":false
}</pre>
多端口配置：

&nbsp;
<pre>{
"server": "0.0.0.0",
"port_password": {
"8388": "password1",
"8389": "password2",
"8387": "password3",
"8386": "password4"
},
"timeout":300,
"method":"aes-256-cfb",
"fast_open": false
}</pre>
//ubuntu 14 下测试通过

&nbsp;

&nbsp;

&nbsp;
<h1>centos 下安装</h1>
<h2>安装 pip</h2>
<a href="https://pip.pypa.io/en/stable/installing/">pip</a>是 python 的包管理工具。在本文中将使用 python 版本的 shadowsocks，此版本的 shadowsocks 已发布到 pip 上，因此我们需要通过 pip 命令来安装。

在控制台执行以下命令安装 pip：
<blockquote>
<pre>$ curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
$ python get-pip.py
</pre>
</blockquote>
<h2>安装配置 shadowsocks</h2>
在控制台执行以下命令安装 shadowsocks：
<blockquote>
<pre><code class="language-bash">$ pip install --upgrade pip
$ pip install shadowsocks
</code></pre>
</blockquote>