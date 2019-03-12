---
ID: 2587
post_title: aliyun auto mysql and file backup to oss
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/12/aliyun-auto-mysql-and-file-backup-to-oss/
published: true
post_date: 2019-03-12 19:51:43
---
<pre>//autorun

crontab -e

02 4 * * * root run-parts /etc/cron.daily
42 4 1 * * root run-parts /etc/cron.monthly</pre>
<pre>// mysqlbackup 

vi  /etc/cron.daily/bakmysql.sh


DATE=`date +%Y%m%d%H%M` #every minute
DATABASE=wordpress_hss5com #database name
DB_USER=wordpress #database username
DB_PASS="wordpress" #database password
BACKUP=/data/dbbackup #backup path

#backup command

/usr/bin/mysqldump -u$DB_USER -p$DB_PASS -h 127.0.0.1 -R --opt $DATABASE |gzip &gt; ${BACKUP}\/${DATABASE}_${DATE}.sql.gz

# send to oss 

/var/local/src/ossutil64 cp /data/dbbackup/ oss://hss5.com/dbbackup -r -u



#just backup the latest 5 days

find ${BACKUP} -name "${DATABASE}_*.sql.gz" -type f -mtime +50 -exec rm {} \; &gt; /dev/null 2&gt;&amp;1</pre>
&nbsp;
<pre>vi  /etc/cron.monthly/bakfile.sh

// file backup

DATE=`date +%Y%m%d%H%M` #every minute

WEBSITE=hss5com

BACKUP=/data/filebackup #backup path

#backup command

zip -r ${BACKUP}\/${WEBSITE}_${DATE}_file.zip /var/www/html/*
#zip -r ${BACKUP}\/host2.${WEBSITE}_${DATE}_file.zip /var/www/html/host2/*

/var/local/src/ossutil64 cp /data/filebackup/ oss://hss5com/filebackup -r -u

#just backup the latest 365 days

find ${BACKUP} -name "${WEBSITE}_*.zip" -type f -mtime +365 -exec rm {} \; &gt; /dev/null 2&gt;&amp;1


</pre>
&nbsp;

mysql 每日自动备份， 文件每月自动备份 到 阿里云oss

config ossutil64 tool for oss upload/download

http://hss5.com/2019/03/04/aliyun-oss-ossutil-tool/

&nbsp;