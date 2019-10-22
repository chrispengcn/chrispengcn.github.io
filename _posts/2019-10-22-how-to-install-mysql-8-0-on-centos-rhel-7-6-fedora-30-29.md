---
ID: 3278
post_title: 'How To Install MySQL 8.0 on CentOS/RHEL 7/6 &#038; Fedora 30/29'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  https://www.hss5.com/2019/10/22/how-to-install-mysql-8-0-on-centos-rhel-7-6-fedora-30-29/
published: true
post_date: 2019-10-22 11:25:37
---
<strong>MySQL 8</strong> is the latest version available for the installation. MySQL is a most popular database server for Linux systems, it also supports a large number of platforms. This tutorial will help you to Install MySQL Server 8.0 Community Edition on CentOS/RHEL 7/6, Fedora 30/29/28 using the package manager.
<h2 class="heading1">Step 1 – Setup Yum Repository</h2>
First, you need to enable MySQL yum repository in your system provided by MySQL. Execute one of below command as per your operating system version.
<pre>### On CentOS/RHEL 7 system ###
rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm

### On CentOS/RHEL 6 system ###
rpm -Uvh https://repo.mysql.com/mysql80-community-release-el6-3.noarch.rpm

### On Fedora 30 system ###
rpm -Uvh https://repo.mysql.com/mysql80-community-release-fc30-1.noarch.rpm

### On Fedora 29 system ###
rpm -Uvh https://repo.mysql.com/mysql80-community-release-fc29-2.noarch.rpm

### On Fedora 28 system ###
rpm -Uvh https://repo.mysql.com/mysql80-community-release-fc28-2.noarch.rpm
</pre>
<h2 class="heading1">Step 2 – Install MySQL Community Server</h2>
The MySQL yum repository contains multiple repositories configuration for multiple MySQL versions. So first disable all repositories in mysql repo file.
<pre>sed -i 's/enabled=1/enabled=0/' /etc/yum.repos.d/mysql-community.repo
</pre>
Then execute one of the followings commands as per your operating system to install MySQL.
<pre>yum --enablerepo=mysql80-community install mysql-community-server  ## CentOS &amp; RedHat 
dnf --enablerepo=mysql80-community install mysql-community-server  ## Fedora Systems 
</pre>
<h2 class="heading1">Step 3 – Start MySQL Service</h2>
Start the MySQL server using the following command from Linux terminal.

<strong>Using SysVinit</strong>
<pre>service mysqld start
</pre>
<strong>Using Systemd</strong>
<pre>systemctl start mysqld.service
</pre>
<h2 class="heading1">Step 4 – Find default root Password</h2>
With the installation of MySQL 8.0, a temporary password is created for the MySQL root user. You can find the temporary password generated in log files.
<pre>grep "A temporary password" /var/log/mysqld.log
</pre>
Output:
<pre class="pretty">[Note] A temporary password is generated for root@localhost: hosygMikj1+t636
</pre>
<h2 class="heading1">Step 5 – MySQL Post Install Setup</h2>
After installing MySQL first time, execute <code>mysql_secure_installation</code> command to secure MySQL server. It will prompt for few question’s, we recommended to say yes ( <strong>y</strong> ) for each.
<pre>mysql_secure_installation
</pre>
<pre class="pretty">Enter password for user root:

The existing password for the user account root has expired. Please set a new password.

New password:
Re-enter new password:

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y

Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
</pre>
<h2 class="heading1">Step 6 – Restart and Enable MySQL Service</h2>
The MySQL installation has been successfully completed. Now restart the service and setup autostart on system bootup.
<pre>### Using SysVinit
service mysqld restart
chkconfig mysqld on

### Using Systemd
systemctl restart mysqld.service
systemctl enable mysqld.service
</pre>
<h2 class="heading1">Step 7 – Working with MySQL</h2>
Now <strong>connect mysql</strong> database server Linux shell using below command. It will prompt for the password for authentication. On successful login, you will get the MySQL command prompt, where we can execute SQL queries.
<pre>mysql -h localhost -u root -p
</pre>
<img class="alignnone size-full wp-image-3279" src="https://www.hss5.com/wp-content/uploads/2019/10/mysql80-centos.png" width="715" height="305" alt="Install MySQL 8 on CentOS &amp; Fedora" />

After login, You can use following commands to create a new database, create a user and assign privileges to the user on the database. Change values as per your requirements.
<div id="crayon-5dae3bb66bb9f568521300" class="crayon-syntax crayon-theme-github crayon-font-liberation-mono crayon-os-mac print-yes notranslate" data-settings=" minimize scroll-mouseover">
<div class="crayon-plain-wrap"></div>
<div class="crayon-main">
<table class="crayon-table">
<tbody>
<tr class="crayon-row">
<td class="crayon-nums " data-settings="hide">
<div class="crayon-nums-content">
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-1">1</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-2">2</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-3">3</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-4">4</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-5">5</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-6">6</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-7">7</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-8">8</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-9">9</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-10">10</div>
<div class="crayon-num" data-line="crayon-5dae3bb66bb9f568521300-11">11</div>
</div></td>
<td class="crayon-code">
<div class="crayon-pre">
<div id="crayon-5dae3bb66bb9f568521300-1" class="crayon-line"><span class="crayon-c">### CREATE DATABASE</span></div>
<div id="crayon-5dae3bb66bb9f568521300-2" class="crayon-line">mysql<span class="crayon-h">&gt;</span> <span class="crayon-st">CREATE</span> <span class="crayon-st">DATABASE</span> mydb;</div>
<div id="crayon-5dae3bb66bb9f568521300-3" class="crayon-line"></div>
<div id="crayon-5dae3bb66bb9f568521300-4" class="crayon-line"><span class="crayon-c">### CREATE USER ACCOUNT</span></div>
<div id="crayon-5dae3bb66bb9f568521300-5" class="crayon-line">mysql<span class="crayon-h">&gt;</span> <span class="crayon-st">CREATE USER</span> <span class="crayon-s">'dbuser'</span>@<span class="crayon-s">'192.168.10.101'</span> <span class="crayon-st">IDENTIFIED BY</span> <span class="crayon-s">'secret'</span>;</div>
<div id="crayon-5dae3bb66bb9f568521300-6" class="crayon-line"></div>
<div id="crayon-5dae3bb66bb9f568521300-7" class="crayon-line"><span class="crayon-c">### GRANT PERMISSIONS ON DATABASE</span></div>
<div id="crayon-5dae3bb66bb9f568521300-8" class="crayon-line">mysql<span class="crayon-h">&gt;</span> <span class="crayon-st">GRANT</span> <span class="crayon-st">ALL</span> <span class="crayon-st">ON</span> mydb.* <span class="crayon-st">TO</span> <span class="crayon-s">'dbuser'</span>@<span class="crayon-s">'192.168.10.101'</span>;</div>
<div id="crayon-5dae3bb66bb9f568521300-9" class="crayon-line"></div>
<div id="crayon-5dae3bb66bb9f568521300-10" class="crayon-line"><span class="crayon-c">###  RELOAD PRIVILEGES</span></div>
<div id="crayon-5dae3bb66bb9f568521300-11" class="crayon-line">mysql<span class="crayon-h">&gt;</span> <span class="crayon-st">FLUSH</span> <span class="crayon-st">PRIVILEGES</span>;</div>
</div></td>
</tr>
</tbody>
</table>
</div>
</div>
Congratulation’s! you have successfully installed MySQL server on your system. Use below quick links for basic MySQL tasks.

REF:

https://tecadmin.net/install-mysql-8-on-centos/