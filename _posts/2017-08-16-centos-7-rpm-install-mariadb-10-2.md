---
ID: 979
post_title: >
  centos7
  环境使用RPM包离线安装MariaDB
  10.2
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2017/08/16/centos-7-rpm-install-mariadb-10-2/
published: true
post_date: 2017-08-16 15:34:26
---
<h1>1. 进入MariaDB官网下载MariaDB需要的RPM包</h1>
2. 使用下载软件下载所需要的RPM包, 总共4个, 并上传到CentOS主机指定目录.

<a href="https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.7/yum/centos7-amd64/rpms/">https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.7/yum/centos7-amd64/rpms/</a>

&nbsp;

https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.7/yum/centos7-amd64/rpms/MariaDB-10.2.7-centos7-x86_64-client.rpm

https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.7/yum/centos7-amd64/rpms/MariaDB-10.2.7-centos7-x86_64-server.rpm

https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.7/yum/centos7-amd64/rpms/MariaDB-10.2.7-centos7-x86_64-common.rpm

https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.7/yum/centos7-amd64/rpms/MariaDB-10.2.7-centos7-x86_64-compat.rpm

&nbsp;

&nbsp;

https://mirrors.tuna.tsinghua.edu.cn/mariadb/mariadb-10.2.7/yum/centos7-amd64/rpms/galera-25.3.20-1.rhel7.el7.centos.x86_64.rpm

3. 安装MariaDB所需的依赖包

yum install -y libaio perl perl-DBI perl-Module-Pluggable perl-Pod-Escapes perl-Pod-Simple perl-libs perl-version

yum install galera-25.3.20-1.rhel7.el7.centos.x86_64.rpm

4. 先移除所有原有的mysql软件包

yum remove mysql*

5. 进入RPM包目录位置, 安装MariaDB

yum install -y MariaDB*

6. 安装完成后，启动MariaDB服务

service <a class="replace_word" title="MySQL知识库" href="http://lib.csdn.net/base/mysql" target="_blank" rel="noopener noreferrer">MySQL</a> start

配置文件目录

/etc/my.cnf.d/server.cnf

<a class="replace_word" title="MySQL知识库" href="http://lib.csdn.net/base/mysql" target="_blank" rel="noopener noreferrer">数据库</a>目录

/var/lib/<a class="replace_word" title="MySQL知识库" href="http://lib.csdn.net/base/mysql" target="_blank" rel="noopener noreferrer">mysql</a>
<h1>7.进入mysql服务</h1>
mysql

<img src="http://hss5.com/wp-content/uploads/2017/08/35_%filename%" alt="%image_alt%" />