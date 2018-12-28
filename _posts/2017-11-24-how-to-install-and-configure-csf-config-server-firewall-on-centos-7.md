---
ID: 1083
post_title: >
  How to Install and Configure CSF (Config
  Server Firewall) on CentOS 7
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/11/24/how-to-install-and-configure-csf-config-server-firewall-on-centos-7/
published: true
post_date: 2017-11-24 16:30:26
---
<h1>How to Install and Configure CSF (Config Server Firewall) on CentOS 7</h1>
<div class="contributeEdit">
<div id="tocContainer">
<h3>On this page</h3>
<ol class="toc">
 	<li><a href="https://www.howtoforge.com/tutorial/install-and-configure-csf-config-server-firewall-on-centos-7/#prerequisites">Prerequisites</a></li>
 	<li><a href="https://www.howtoforge.com/tutorial/install-and-configure-csf-config-server-firewall-on-centos-7/#step-installation-of-cfs-dependencies">Step 1 - Installation of CFS dependencies</a></li>
 	<li><a href="https://www.howtoforge.com/tutorial/install-and-configure-csf-config-server-firewall-on-centos-7/#step-install-csf">Step 2 - Install CSF</a></li>
 	<li><a href="https://www.howtoforge.com/tutorial/install-and-configure-csf-config-server-firewall-on-centos-7/#step-configure-csf-on-centos-">Step 3 - Configure CSF on CentOS 7</a></li>
 	<li><a href="https://www.howtoforge.com/tutorial/install-and-configure-csf-config-server-firewall-on-centos-7/#step-basic-csf-commands">Step 4 - Basic CSF Commands</a></li>
 	<li><a href="https://www.howtoforge.com/tutorial/install-and-configure-csf-config-server-firewall-on-centos-7/#step-advanced-configuration">Step 5 - Advanced Configuration</a></li>
 	<li><a href="https://www.howtoforge.com/tutorial/install-and-configure-csf-config-server-firewall-on-centos-7/#conclusion">Conclusion</a></li>
</ol>
</div>
</div>
<strong>Config Server Firewall / CSF</strong> is firewall application suite for Linux servers. CSF is also a Login/Intrusion Detection for applications like SSH, SMTP, IMAP, Pop3, the "su" command and many more. CSF can e.g. detect when someone is logging into the server via SSH and alarms you when this user tries to use the "su" command on the server to get higher privileges. It also checks for login authentication failures on mail servers (Exim, IMAP, Dovecot, uw-imap, Kerio), OpenSSH servers, Ftp servers (Pure-ftpd, vsftpd, Proftpd), cPanel server to replace software like fail2ban. CSF is a good security solution for hosting servers and can be integrated into the user interface (UI) of WHM/cPanel, DirectAdmin, and Webmin.
<div>
<div id="google_ads_div_howtoforge_com_article_rectangle_a_300x250_ad_wrapper">
<div id="google_ads_div_howtoforge_com_article_rectangle_a_300x250_ad_container"><ins><ins><iframe id="google_ads_iframe_howtoforge_com_article_rectangle_a_300x250" name="google_ads_iframe_howtoforge_com_article_rectangle_a_300x250" width="300" height="250" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" data-mce-fragment="1"></iframe></ins></ins></div>
</div>
</div>
<h2 id="prerequisites">Prerequisites</h2>
<ul>
 	<li>CentOS 7 (my server uses the IP 192.168.1.101).</li>
 	<li>root privileges.</li>
</ul>
<strong>What we will do in this tutorial:</strong>
<ul>
 	<li>Install the dependencies for CSF.</li>
 	<li>Install CSF.</li>
 	<li>Configure CSF.</li>
 	<li>Basic CSF commands.</li>
 	<li>Advanced Configuration.</li>
</ul>
<h2 id="step-installation-of-cfs-dependencies">Step 1 - Installation of CFS dependencies</h2>
CSF is based on<em> Perl,</em> so you need to install Perl on our server first. You need <em>wget</em> to download the CSF installer and <em>vim</em> (or an editor of your choice) for editing the CSF configuration file. Install the packages with the yum command:
<p class="command">yum install wget vim perl-libwww-perl.noarch perl-Time-HiRes</p>

<h2 id="step-install-csf">Step 2 - Install CSF</h2>
Please go to the <strong>"/usr/src/"</strong> directory and download CSF with wget command.
<p class="command">cd /usr/src/
wget https://download.configserver.com/csf.tgz</p>
Extract the tar.gz file and go to the csf directory, then install it:
<p class="command">tar -xzf csf.tgz
cd csf
sh install.sh</p>
You should get the information that CSF installation is completed at the end.

<a id="img-1" class="fancybox" href="https://www.howtoforge.com/images/install-and-configure-csf-config-server-firewall-on-centos-7/big/1.png"><img src="https://www.howtoforge.com/images/install-and-configure-csf-config-server-firewall-on-centos-7/1.png" alt="CSF installation is complete." width="550" height="192" /></a>

Now you should check that CSG really works on this server. Go to the <em>"/usr/local/csf/bin/"</em> directory, and run <em>"csftest.pl"</em>.
<p class="command">cd /usr/local/csf/bin/
perl csftest.pl</p>
If you see the test results as shown below, then CSF is running without problems on your server:
<p class="command">RESULT: csf should function on this server</p>
<a id="img-2" class="fancybox" href="https://www.howtoforge.com/images/install-and-configure-csf-config-server-firewall-on-centos-7/big/2.png"><img src="https://www.howtoforge.com/images/install-and-configure-csf-config-server-firewall-on-centos-7/2.png" alt="CSF is running." width="325" height="256" /></a>
<h2 id="step-configure-csf-on-centos-">Step 3 - Configure CSF on CentOS 7</h2>
Before stepping into the CSF configuration process, the first thing you must know is that "CentOS 7" has a default firewall application called <strong>"firewalld".</strong> You have to stop firewalld and remove it from the startup.

Stop the firewalld:
<p class="command">systemctl stop firewalld</p>
Disable/Remove firewalld from the startup:
<p class="command">systemctl disable firewalld</p>
Then go to the CSF Configuration directory <em>"/etc/csf/"</em> and edit the file <em>"csf.conf"</em> with the vim editor:
<p class="command">cd /etc/csf/
vim csf.conf</p>
Change <em>line 11</em> <strong>"TESTING "</strong> to <strong>"0"</strong> for applying the firewall configuration.
<p class="command">TESTING = "0"</p>
By default CSF allows incoming and outgoing traffic for the SSH standard port 22, if you use a different SSH port then please add your port to the configuration in <em>line 139</em> <strong>"TCP_IN"</strong>.

Now start CSF and LFD with systemctl command:
<p class="command">systemctl start csf
systemctl start lfd</p>
And then enable the csf and lfd services to be started at boot time:
<p class="command">systemctl enable csf
systemctl enable lfd</p>
Now you can see the list default rules of CSF with command:
<p class="command">csf -l</p>

<h2 id="step-basic-csf-commands">Step 4 - Basic CSF Commands</h2>
1. Start the firewall (enable the firewall rules):
<p class="command">csf -s</p>
2. Flush/Stop the firewall rules.
<p class="command">csf -f</p>
3. Reload the firewall rules.
<p class="command">csf -r</p>
4. Allow an IP and add it to csf.allow.
<p class="command">csf -a 192.168.1.109</p>
Results:
<p class="command">Adding 192.168.1.109 to csf.allow and iptables ACCEPT...
ACCEPT  all opt -- in !lo out *  192.168.1.109  -&gt; 0.0.0.0/0
ACCEPT  all opt -- in * out !lo  0.0.0.0/0  -&gt; 192.168.1.109</p>
5. Remove and delete an IP from csf.allow.
<p class="command">csf -ar 192.168.1.109</p>
Results:
<p class="command">Removing rule...
ACCEPT  all opt -- in !lo out *  192.168.1.109  -&gt; 0.0.0.0/0
ACCEPT  all opt -- in * out !lo  0.0.0.0/0  -&gt; 192.168.1.109</p>
6. Deny an IP and add to csf.deny:
<p class="command">csf -d 192.168.1.109</p>
Results:
<p class="command">Adding 192.168.1.109 to csf.deny and iptables DROP...
DROP  all opt -- in !lo out *  192.168.1.109  -&gt; 0.0.0.0/0
LOGDROPOUT  all opt -- in * out !lo  0.0.0.0/0  -&gt; 192.168.1.109</p>
7. Remove and delete an IP from csf.deny.
<p class="command">csf -dr 192.168.1.109</p>
Results:
<p class="command">Removing rule...
DROP  all opt -- in !lo out *  192.168.1.109  -&gt; 0.0.0.0/0
LOGDROPOUT  all opt -- in * out !lo  0.0.0.0/0  -&gt; 192.168.1.109</p>
8. Remove and Unblock all entries from csf.deny.
<p class="command">csf -df</p>
Results:
<p class="command">DROP  all opt -- in !lo out *  192.168.1.110  -&gt; 0.0.0.0/0
LOGDROPOUT  all opt -- in * out !lo  0.0.0.0/0  -&gt; 192.168.1.110
DROP  all opt -- in !lo out *  192.168.1.111  -&gt; 0.0.0.0/0
LOGDROPOUT  all opt -- in * out !lo  0.0.0.0/0  -&gt; 192.168.1.111
csf: all entries removed from csf.deny</p>
9. Search for a pattern match on iptables e.g : IP, CIDR, Port Number
<p class="command">csf -g 192.168.1.110</p>

<h2 id="step-advanced-configuration">Step 5 - Advanced Configuration</h2>
Here are some tweaks about CSF, so you can configure as you need.

Back to the csf configuration directory, and edit the csf.conf configuration file:
<p class="command">cd /etc/csf/
vim csf.conf</p>
1. Don't Block IP addresses that are in the csf.allow files.

By default lfd also will block an IP under csf.allow files, so if you want that an IP in csf.allow files never get blocked by lfd, then please go to the<em> line 272</em> and change <strong>"IGNORE_ALLOW"</strong> to <strong>"1"</strong>. This is useful when you have a static IP at home or in office and want to ensure that your IP never gets blocked by the firewall on your internet server.
<p class="command">IGNORE_ALLOW = "1"</p>
2. Allow Incoming and Outgoing ICMP.

Go to the line 152 for incoming ping/ICMP:
<p class="command">ICMP_IN = "1"</p>
And line 159 for outgoing ping ping/ICMP:
<p class="command">ICMP_OUT = "1"</p>
3. Block Certain Countrys

CSF provide an option to allow and deny access by country using the <strong>CIDR</strong> (Country Code). Go to<em> line 836</em> and add the country codes that shall be allowed and denied:
<p class="command">CC_DENY = "CN,UK,US"
CC_ALLOW = "ID,MY,DE"</p>
4. Send the Su and SSH Login log by Email.

You can set an email address that is used by LFD to send an email about "<strong>SSH Login"</strong> events and users that run the <strong>"su" </strong>command, go to the line <em>1069 </em>and change the value to "1".
<p class="command">LF_SSH_EMAIL_ALERT = "1"

...

LF_SU_EMAIL_ALERT = "1"</p>
And then define the email address you want to use in <em>line 588</em>.
<p class="command">LF_ALERT_TO = "mymail@mydomain.tld"</p>
If you want more tweaks, read the options in the <strong>"/etc/csf/csf.conf"</strong> configuration file.
<h2 id="conclusion">Conclusion</h2>
CSF is an application-based firewall for iptables provided for Linux servers. CSF has many features and can support web-based management tools like cPanel / WHM, DirectAdmin and Webmin. CSF is easy to install and use on the server, it makes security management easier for sysadmins.