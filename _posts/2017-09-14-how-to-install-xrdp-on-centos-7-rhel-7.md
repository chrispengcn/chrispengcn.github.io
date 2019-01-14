---
ID: 1038
post_title: How to Install xrdp on CentOS 7 / RHEL 7
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/09/14/how-to-install-xrdp-on-centos-7-rhel-7/
published: true
post_date: 2017-09-14 14:29:46
---
<strong><a href="http://www.itzgeek.com/tag/xrdp" target="_blank" rel="noopener noreferrer">xrdp</a></strong> is an Open Source Remote desktop Protocol server, which allows you to RDP to your Linux server from Windows machine; it is capable of accepting connections from rdesktop, freerdp, and remote desktop clients. This how to will help you to setup xrdp server on <strong><a href="http://www.itzgeek.com/tag/centos-7">CentOS 7 / RHEL 7</a></strong>.
<h2>Prerequisites</h2>
1. This post was written when xrdp is available neither on CentOS repositories nor EPEL repository, after a lot of Google search; I found desktop repository (http://li.nux.ro/) which was having xrdp for CentOS 7 / RHEL 7. We need to manually setup the repository on CentOS 7.

2. Don’t forget to <strong><a title="Install Gnome GUI on CentOS 7 / RHEL 7" href="http://www.itzgeek.com/how-tos/linux/centos-how-tos/install-gnome-gui-on-centos-7-rhel-7.html" target="_blank" rel="noopener noreferrer">install Gnome on CentOS 7</a></strong>

3. <strong><a href="http://www.itzgeek.com/how-tos/linux/centos-how-tos/enable-epel-repository-for-centos-7-rhel-7.html" target="_blank" rel="noopener noreferrer">Install and configure EPEL repository</a></strong>.
<blockquote>
<pre>rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
</pre>
</blockquote>
4. Add nux repository.
<div class="bsac bsac-clearfix bsac-post-inline bsac-float-center bsac-align-center bsac-column-1"></div>
<strong>Automatic (recommended)</strong>
<blockquote>
<pre>rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm</pre>
</blockquote>
<strong>Manual</strong>

Create a repository file.
<blockquote>
<pre>vi /etc/yum.repos.d/xrdp.repo</pre>
</blockquote>
Place the following content. Once added, save and close the file.
<blockquote>
<pre>[xrdp]
name=xrdp
baseurl=http://li.nux.ro/download/nux/dextop/el7/x86_64/
enabled=1
gpgcheck=0</pre>
</blockquote>
<h2>Install xrdp on CentOS 7<strong>
</strong></h2>
Issue the  following command to install xrdp
<blockquote>
<pre>yum -y install xrdp tigervnc-server</pre>
</blockquote>
You will get the following output, make sure you are getting the package from the newly created repository.
<pre> --&gt; Running transaction check
---&gt; Package xrdp.x86_64 0:0.6.1-2.el7.nux will be installed
--&gt; Finished Dependency Resolution

Dependencies Resolved

================================================================================
Package        Arch             Version                   Repository      Size
================================================================================
Installing:
xrdp           x86_64           0.6.1-2.el7.nux           xrdp           271 k

Transaction Summary
================================================================================
Install  1 Package

Total download size: 271 k
Installed size: 1.5 M
Is this ok [y/d/N]: y
Downloading packages:
xrdp-0.6.1-2.el7.nux.x86_64.rpm                            | 271 kB   00:05
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Installing : xrdp-0.6.1-2.el7.nux.x86_64                                  1/1
Verifying  : xrdp-0.6.1-2.el7.nux.x86_64                                  1/1

Installed:
xrdp.x86_64 0:0.6.1-2.el7.nux</pre>
Once it is installed, lets start the xrdp service.
<blockquote>
<pre>systemctl start xrdp.service</pre>
</blockquote>
xrdp will listen on 3389, lets confirm this by issuing following command.
<blockquote>
<pre># netstat -antup | grep xrdp</pre>
</blockquote>
<strong>Output:</strong>
<div class="bsac bsac-clearfix bsac-post-inline bsac-float-center bsac-align-center bsac-column-1">
<div id="bsac-25694-645930969" class="bsac-container bsac-type-code " data-adid="25694" data-type="code"><span id="bsac-25694-645930969-place"><ins class="adsbygoogle" data-ad-client="ca-pub-3804050461090319" data-ad-slot="2458434285" data-ad-format="horizontal" data-adsbygoogle-status="done"><ins id="aswift_3_expand"><ins id="aswift_3_anchor"><iframe id="aswift_3" name="aswift_3" width="816" height="90" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" allowfullscreen="allowfullscreen" data-mce-fragment="1"></iframe></ins></ins></ins></span></div>
</div>
<pre>tcp        0      0 0.0.0.0:3389            0.0.0.0:*               LISTEN      1508/xrdp
tcp        0      0 127.0.0.1:3350          0.0.0.0:*               LISTEN      1507/xrdp-sesman</pre>
By default, services wont auto start after system reboot. Issue the following command to enable the service at system start up.
<blockquote>
<pre>systemctl enable xrdp.service</pre>
</blockquote>
Next is to create iptables rule to allow RDP connection from the external machines, following command will add the exception for RDP port (3389).
<blockquote>
<pre>firewall-cmd --permanent --zone=public --add-port=3389/tcp
firewall-cmd --reload</pre>
</blockquote>
Configure SELinux
<blockquote>
<pre># chcon --type=bin_t /usr/sbin/xrdp
# chcon --type=bin_t /usr/sbin/xrdp-sesman</pre>
</blockquote>
<h2>Test Remote Connectivity<strong>
</strong></h2>
Now take RDP from any windows machine using Remote Desktop Connection, enter the ip address of Linux server in the computer field and click on connect.
<figure id="attachment_7525" class="wp-caption aligncenter"><img class="alignnone size-full wp-image-2134" src="http://hss5.com/wp-content/uploads/2019/01/CentOS-7-xrdp-MSTSC.jpg" width="417" height="248" alt="Install xrdp on CentOS 7 - xrdp MSTSC" /><figcaption class="wp-caption-text">Install xrdp on CentOS 7 – xrdp MSTSC</figcaption></figure>
You would be asked to enter the user name and password. You can either use root or any user that you have it on the system. Make sure you use module “sesman-Xvnc”.
<figure id="attachment_7526" class="wp-caption aligncenter"><img class="alignnone size-full wp-image-2135" src="http://hss5.com/wp-content/uploads/2019/01/CentOS-7-xrdp-Login-page.jpg" width="713" height="586" alt="Install xrdp on CentOS 7 - xrdp Login page" /><figcaption class="wp-caption-text">Install xrdp on CentOS 7 – xrdp Login page</figcaption></figure>
If you click ok, you will see the processing. In less than a half min, you will get a desktop.
<figure id="attachment_7527" class="wp-caption aligncenter"><img class="alignnone size-full wp-image-2136" src="http://hss5.com/wp-content/uploads/2019/01/CentOS-7-xrdp-Desktop.jpg" width="1054" height="693" alt="Install xrdp on CentOS 7 - xrdp Desktop" /><figcaption class="wp-caption-text">Install xrdp on CentOS 7 – xrdp Desktop</figcaption></figure>
That’s All. You have successfully configured xRDP on CentOS 7 / RHEL 7. We welcome your comments.