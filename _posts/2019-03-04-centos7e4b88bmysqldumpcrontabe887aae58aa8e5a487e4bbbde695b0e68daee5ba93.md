---
ID: 2501
post_title: >
  centos7下mysqldump+crontab自动备份数据库
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/03/04/centos7%e4%b8%8bmysqldumpcrontab%e8%87%aa%e5%8a%a8%e5%a4%87%e4%bb%bd%e6%95%b0%e6%8d%ae%e5%ba%93/'
published: true
post_date: 2019-03-04 17:21:55
---
1.创建文件夹(存放备份数据)
<pre>mkdir /bak
mkdir /bak/mysqldata</pre>
2.编写脚本
<pre>vi /usr/sbin/bakmysql.sh</pre>
脚本内容如下
<pre>DATE=`date +%Y%m%d%H%M` #every minute
DATABASE=fgdatabase #database name
DB_USER=root #database username
DB_PASS="+lintang" #database password
BACKUP=/bak/mysqldata #backup path

#backup command

/usr/bin/mysqldump -u$DB_USER -p$DB_PASS -h 127.0.0.1 -R --opt $DATABASE |gzip &gt; ${BACKUP}\/${DATABASE}_${DATE}.sql.gz

#just backup the latest 5 days

find ${BACKUP} -name "${DATABASE}_*.sql.gz" -type f -mtime +5 -exec rm {} \; &gt; /dev/null 2&gt;&amp;1</pre>
如果权限不足，给权限
<pre>chmod +x /usr/sbin/bakmysql.sh</pre>
3.设置定时备份任务（注意：这里我以非root用户登录要用sudo，否则执行失败）
<pre>sudo crontab -e</pre>
添加如下任务（每天凌晨3点备份一次）
<pre>00 3 * * * /usr/sbin/bakmysql.sh</pre>
4.建议

在第3步添加任务时，可以如下写，表示每分钟备份一次，用以验证是否成功

*/1 * * * * /usr/sbin/bakmysql.sh


看是否每分钟增加一份数据
<pre>ls /bak/mysqldata</pre>
5.其他
crontab的用法：
http://www.cnblogs.com/xiaoluo501395377/archive/2013/04/06/3002602.html