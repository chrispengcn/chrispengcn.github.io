---
ID: 2469
post_title: 阿里云CentOS服务器挂载数据盘
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/04/aliyun-mount-data-disk/
published: true
post_date: 2019-03-04 14:56:10
---
1.系统环境：
<pre>lsb_release -a</pre>
<img class="alignnone size-full wp-image-2491" src="http://hss5.com/wp-content/uploads/2019/03/20180607112840880-4.png" width="469" height="92" />
2.运行命令查看数据盘
<pre>fdisk -l</pre>
<img class="alignnone size-full wp-image-2492" src="http://hss5.com/wp-content/uploads/2019/03/20180607114703268-4.png" width="488" height="257" />
查看磁盘情况，本次机器系统盘为vda:40G，数据盘为vdb:20G (在网上看到很多是xvda和xvdb,我不太清楚只是名称的区别还是类型有区别)

3.对数据盘分区

输入
<pre>fdisk  /dev/vdb</pre>
对数据盘进行分区。根据提示，输入 n， p， 1， 回车，回车， wq

<img class="alignnone size-full wp-image-2493" src="http://hss5.com/wp-content/uploads/2019/03/20180607123514684-4.png" width="661" height="393" />

查看分区是否成功

<img class="alignnone size-full wp-image-2494" src="http://hss5.com/wp-content/uploads/2019/03/20180607123802671-4.png" width="535" height="315" />

4.格式化分区
<pre>mkfs.ext4 /dev/vdb1</pre>
<img class="alignnone size-full wp-image-2495" src="http://hss5.com/wp-content/uploads/2019/03/2018060712465814-3.png" width="587" height="335" />
5.创建挂载目录

这里我创建目录叫data,如果后续还有其他磁盘,可以data0 data1 建立,也可按其功能目的建立目录名
<pre>mkdir /data
6.写入新分区信息</pre>
写入新分区信息。完成后，可以使用 cat /etc/fstab 命令查看。
<pre>echo /dev/vdb1 /mnt ext4 defaults 0 0 &gt;&gt; /etc/fstab
cat /etc/fstab</pre>
<img class="alignnone size-full wp-image-2496" src="http://hss5.com/wp-content/uploads/2019/03/2018060713043164-1.png" width="704" height="181" />

7.挂载分区

可以将 /etc/fstab 的所有内容重新加载 默认的会挂载到mnt目录下
<pre>mount -a

#也可以 只挂载你刚分好的分区</pre>
<pre>mount /dev/vdb1 /mnt/data
#查看挂载情况</pre>
<pre>df -h</pre>
<img class="alignnone size-full wp-image-2497" src="http://hss5.com/wp-content/uploads/2019/03/20180607133413235.png" width="501" height="151" />
vdb1 成功挂载在了data目录下.可以正常使用了

来源：https://blog.csdn.net/wanniwa/article/details/80606296