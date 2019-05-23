---
ID: 2706
post_title: >
  How to Create an HTTP Proxy Using Squid
  on CentOS 7
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/05/23/how-to-create-an-http-proxy-using-squid-on-centos-7/
published: true
post_date: 2019-05-23 18:17:11
---
Web proxies have been around for quite some time now and have been used by millions of users around the globe. They have a wide range of purposes, most popular being online anonymity, but there are other ways you can take advantage of web proxies. Here are some ideas:
<ul>
 	<li>Online anonymity</li>
 	<li>Improve online security</li>
 	<li>Improve loading times</li>
 	<li>Block malicious traffic</li>
 	<li>Log your online activity</li>
 	<li>To circumvent regional restrictions</li>
 	<li>In some cases can reduce bandwidth usage</li>
</ul>
<h4>How Proxy Server Works</h4>
The proxy server is a computer that is used as an intermediary between the client and other servers from which client may request resources. A simple example of this is when a client makes online requests (for example want to open a web page), he connects first to the proxy server.

The proxy server then checks its local disk cache and if the data can be found in there, it will return the data to the client, if not cached, it will make the request in the client’s behalf using the proxy IP address (different from the clients) and then return the data to the client. The proxy server will try to cache the new data and will use it for future requests made to the same server.
<h4>What is Squid Proxy</h4>
<strong>Squid</strong> is a web proxy used my wide range of organizations. It is often used as caching proxy and improving response times and reducing bandwidth usage.

<center><ins class="adsbygoogle" data-ad-client="ca-pub-2601749019656699" data-ad-slot="5590002574" data-ad-format="auto" data-full-width-responsive="true" data-adsbygoogle-status="done"><ins id="aswift_4_expand"><ins id="aswift_4_anchor"><iframe id="aswift_4" name="aswift_4" width="780" height="90" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" allowfullscreen="allowfullscreen" data-mce-fragment="1"></iframe></ins></ins></ins></center>For the purpose of this article, I will be installing <strong>Squid</strong> on a <a href="https://www.linode.com/?r=64ebb9f723fed8b32fda84b6594006df08ad24b6" target="_blank" rel="nofollow noopener">Linode CentOS 7 VPS</a> and use it as an HTTP proxy server.
<h3>How to Install Squid on CentOS 7</h3>
Before we start, you should know that <strong>Squid</strong>, does not have any minimum requirements, but the amount of RAM usage may vary depending on the clients browsing the internet through the proxy server.

<strong>Squid</strong> is included in the base repository and thus the installation is simple and straightforward. Before installing it, however, make sure your packages are up to date by running.
<pre># yum -y update
</pre>
Proceed by installing squid, start and enable it on system startup using following commands.
<pre># yum -y install squid
# systemctl start squid
# systemctl  enable squid
</pre>
At this point your Squid web proxy should already be running and you can verify the status of the service with.
<pre># systemctl status squid
</pre>
<h5>Sample Output</h5>
<pre><strong>●</strong> squid.service - Squid caching proxy
   Loaded: loaded (/usr/lib/systemd/system/squid.service; enabled; vendor preset: disabled)
   Active: <strong>active (running)</strong> since Thu 2018-09-20 10:07:23 UTC; 5min ago
 Main PID: 2005 (squid)
   CGroup: /system.slice/squid.service
           ├─2005 /usr/sbin/squid -f /etc/squid/squid.conf
           ├─2007 (squid-1) -f /etc/squid/squid.conf
           └─2008 (logfile-daemon) /var/log/squid/access.log

Sep 20 10:07:23 tecmint systemd[1]: Starting Squid caching proxy...
Sep 20 10:07:23 tecmint squid[2005]: Squid Parent: will start 1 kids
Sep 20 10:07:23 tecmint squid[2005]: Squid Parent: (squid-1) process 2007 started
Sep 20 10:07:23 tecmint systemd[1]: Started Squid caching proxy.
</pre>
Here are some important file locations you should be aware of:
<ul>
 	<li>Squid configuration file: <strong>/etc/squid/squid.conf</strong></li>
 	<li>Squid Access log: <strong>/var/log/squid/access.log</strong></li>
 	<li>Squid Cache log: <strong>/var/log/squid/cache.log</strong></li>
</ul>
A minimum <code>squid.conf</code> configuration file (without comments in it) looks like this:
<pre>acl localnet src 10.0.0.0/8	# RFC1918 possible internal network
acl localnet src 172.16.0.0/12	# RFC1918 possible internal network
acl localnet src 192.168.0.0/16	# RFC1918 possible internal network
acl localnet src fc00::/7       # RFC 4193 local private network range
acl localnet src fe80::/10      # RFC 4291 link-local (directly plugged) machines
acl SSL_ports port 443
acl Safe_ports port 80		# http
acl Safe_ports port 21		# ftp
acl Safe_ports port 443		# https
acl Safe_ports port 70		# gopher
acl Safe_ports port 210		# wais
acl Safe_ports port 1025-65535	# unregistered ports
acl Safe_ports port 280		# http-mgmt
acl Safe_ports port 488		# gss-http
acl Safe_ports port 591		# filemaker
acl Safe_ports port 777		# multiling http
acl CONNECT method CONNECT
http_access deny !Safe_ports
http_access deny CONNECT !SSL_ports
http_access allow localhost manager
http_access deny manager
http_access allow localnet
http_access allow localhost
http_access deny all
http_port 3128
coredump_dir /var/spool/squid
refresh_pattern ^ftp:		1440	20%	10080
refresh_pattern ^gopher:	1440	0%	1440
refresh_pattern -i (/cgi-bin/|\?) 0	0%	0
refresh_pattern .		0	20%	4320
</pre>
<h3>Configuring Squid as an HTTP Proxy</h3>
Here, we will show you how to configure squid as an HTTP proxy using only the client IP address for authentication.
<h4>Add Squid ACLs</h4>
If you wish to allow IP address to access the web through your new proxy server, you will need to add new <strong>acl</strong>(<strong>access control list</strong>) line in the configuration file.
<pre># vim /etc/squid/squid.conf
</pre>
The line you should add is:
<pre>acl localnet src XX.XX.XX.XX
</pre>
Where <strong>XX.XX.XX.XX</strong> is the actual client IP address you wish to add. The line should be added in the beginning of the file where the ACLs are defined. It is a good practice to add a comment next to ACL which will describe who uses this IP address.

It is important to note that if Squid is located outside your local network, you should add the public IP address of the client.

You will need to restart Squid so the new changes can take effect.
<pre># systemctl  restart squid
</pre>
<h4>Open Squid Proxy Ports</h4>
As you may have seen in the configuration file, only certain ports are allowed for connecting. You can add more by editing the configuration file.
<pre>acl Safe_ports port XXX
</pre>
Where <strong>XXX</strong> is the actual port you wish to load. Again it is a good idea to leave a comment next to that will describe what the port is going to be used for.

For the changes to take effect, you will need to restart squid once more.
<pre># systemctl  restart squid
</pre>
<h4>Squid Proxy Client Authentication</h4>
You will most probably want your users to authenticate before using the proxy. For that purpose, you can enable basic http authentication. It is easy and fast to configure.

First you will need <strong>httpd-tools</strong> installed.
<pre># yum -y install httpd-tools
</pre>
Now lets create a file that will later store the username for the authentication. Squid runs with user <strong>“squid”</strong> so the file should be owned by that user.
<pre># touch /etc/squid/passwd
# chown squid: /etc/squid/passwd
</pre>
Now we will create a new user called <strong>“proxyclient”</strong> and setup its password.
<pre><strong># htpasswd /etc/squid/passwd proxyclient</strong>

New password:
Re-type new password:
Adding password for user proxyclient
</pre>
Now to configure the autnetication open the configuration file.
<pre># vim /etc/squid/squid.conf
</pre>
After the ports ACLs add the following lines:
<pre>auth_param basic program /usr/lib64/squid/basic_ncsa_auth /etc/squid/passwd
auth_param basic children 5
auth_param basic realm Squid Basic Authentication
auth_param basic credentialsttl 2 hours
acl auth_users proxy_auth REQUIRED
http_access allow auth_users
</pre>
Save the file and restart squid so that the new changes can take effect:
<pre># systemctl restart squid
</pre>
<h4>Block Websites on Squid Proxy</h4>
Finally we will create one last <strong>ACL</strong> that will help us block unwanted websites. First create the file that will store the blacklisted sites.
<pre># touch /etc/squid/blacklisted_sites.acl
</pre>
You can add some domains you wish to block. For example:
<pre>.badsite1.com
.badsite2.com
</pre>
The proceding dot tells squid to block all referecnes to that sites including <strong>www.badsite1</strong>, <strong>subsite.badsite1.com</strong>etc.

Now open Squid’s configuration file.
<pre># vim /etc/squid/squid.conf
</pre>
Just after the ports ACLs add the following two lines:
<pre>acl bad_urls dstdomain "/etc/squid/blacklisted_sites.acl"
http_access deny bad_urls
</pre>
Now save the file and restart squid:
<pre># systemctl restart squid
</pre>
Once everyting configured correctly, now you can configure your local client browser or operating system’s network settings to use your squid HTTP proxy.
<h5>Conclusion</h5>
In this tutorial you learned how to install, secure and configure a Squid HTTP Proxy server on your own. With the information you just got, you can now add some basic filtering for incoming and outgoing traffic through Squid.

If you wish to go the extra mile, you can even configure squid to block some websites during working hours to prevent distractions. If you have any questions or comments, please post them in the comment section below.