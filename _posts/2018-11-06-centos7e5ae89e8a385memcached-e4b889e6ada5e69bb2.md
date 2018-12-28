---
ID: 1414
post_title: CentOS7安装Memcached 三步曲
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/06/centos7%e5%ae%89%e8%a3%85memcached-%e4%b8%89%e6%ad%a5%e6%9b%b2/'
published: true
post_date: 2018-11-06 18:34:11
---
<h2>CentOS7安装Memcached 三步曲</h2>
<div class="postbody">1.yum 安装
<div>yum clean all
yum -y update
yum -y install memcached</div>
2.Memcached 运行
<div>memcached -h

//查看考号修改配置
vim /etc/sysconfig/memcached
内容如下：
PORT=”11211″
USER=”memcached”
MAXCONN=”1024″
CACHESIZE=”64″
OPTIONS=”"

可以修改端口，用户和最大内存，缓存大小
//重启，启动，开机启动，状态，关闭
systemctl restart memcached
systemctl start memcached
systemctl enable memcached
systemctl status memcached
systemctl stop memcached
<div>memcached-tool  127.0.0.1:11211 stats</div>
</div>
<div>#127.0.0.1:11211   Field       Value
accepting_conns           1
auth_cmds           0
auth_errors           0
bytes           0
bytes_read           7
bytes_written           0
cas_badval           0
cas_hits           0
cas_misses           0
cmd_flush           0
cmd_get           0
cmd_set           0
cmd_touch           0
conn_yields           0
connection_structures          11
curr_connections          10
curr_items           0
decr_hits           0
decr_misses           0
delete_hits           0
delete_misses           0
evicted_unfetched           0
evictions           0
expired_unfetched           0
get_hits           0
get_misses           0
hash_bytes      524288
hash_is_expanding           0
hash_power_level          16
incr_hits           0
incr_misses           0
libevent 2.0.21-stable
limit_maxbytes    67108864
listen_disabled_num           0
pid       27929
pointer_size          64
reclaimed           0
reserved_fds          20
rusage_system    0.055134
rusage_user    0.091092
threads           4
time  1429863174
total_connections          11
total_items           0
touch_hits           0
touch_misses           0
uptime         910
version      1.4.15</div>
3.扩展一下，安装PHP-memcache扩展，防火墙放开11211端口
<div>yum -y install php-pecl-memcache
如果是PHP56版本的应该运行
yum -y install php56w-pecl-memcache
防火墙放开11211
<div>firewall-cmd --permanent --zone=public --add-port=11211/tcp
检查端口是否开放
<div>echo stats | nc memcache_host_name_or_ip 11211</div>
</div>
</div>
</div>