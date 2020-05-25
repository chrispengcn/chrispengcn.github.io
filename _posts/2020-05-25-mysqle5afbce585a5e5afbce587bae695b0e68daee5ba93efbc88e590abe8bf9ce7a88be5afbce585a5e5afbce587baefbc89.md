---
ID: 3588
post_title: >
  MySql导入导出数据库（含远程导入导出）
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'https://www.hss5.com/2020/05/25/mysql%e5%af%bc%e5%85%a5%e5%af%bc%e5%87%ba%e6%95%b0%e6%8d%ae%e5%ba%93%ef%bc%88%e5%90%ab%e8%bf%9c%e7%a8%8b%e5%af%bc%e5%85%a5%e5%af%bc%e5%87%ba%ef%bc%89/'
published: true
post_date: 2020-05-25 10:53:45
---
一、导入导出本地数据库
导出：
<div>

   1、先运行cmd，cd 到mysql安装目录中的bin文件夹

2、mysqldump -u root -p 数据库名 &gt; 导出文件名.sql

<strong>其他情况下：</strong>

1.导出整个数据库
mysqldump -u 用户名 -p 数据库名 &gt; 导出的文件名
mysqldump -u wcnc -p smgp_apps_wcnc &gt; wcnc.sql
2.导出一个表
mysqldump -u 用户名 -p 数据库名 表名&gt; 导出的文件名
mysqldump -u wcnc -p smgp_apps_wcnc users&gt; wcnc_users.sql
3.导出一个数据库结构
mysqldump -u wcnc -p -d --add-drop-table smgp_apps_wcnc &gt;d:\wcnc_db.sql
-d 没有数据 --add-drop-table 在每个create语句之前增加一个drop table

导入：

1、 dos命令下进入sql：先create database 数据库名；

2、use 数据库；

3、source c:\....\文件名.sql，后面不需要加分号

二、远程导入导出数据库（或者表）

远程导出语法为:
<strong>mysqldump -h[hosname] -u[user_name] -p[password] --default-character-set=[char_set_name] [db_name] &gt; [save_path]</strong>
例:然后输入:mysqldump -h119.12.12.11 -umysql-pmysql123--default-character-set=utf8 aspchina --skip-lock-tables&gt; d:\aspchina_net.sql

也可以：mysqldump -h119.12.12.11 -u mysql &gt; d:\aspchina_net.sql

119.12.12.11为远程服务器IP,-umysql mysql为数据库用户名,-pmysql123（无空格间隔） mysql123 为用户密码,set=utf8为导出MYSQL的编码格式,aspchina为要导出的数据库名,&gt;d:\aspchina_net.sql 为导入到你本地的存放路径,aspchina_net.sql你可以自由命名!

远程数据库导入：

如果MYSQL数据库小于2MB可以用mysqldump管理工具导入,如果大小2MB就不行了,因为空间商提供的PHPMYADMIN管理工具一般只能导入小于2MB的数据,这令一些使用MYSQL数据库的站长郁闷了!
现在开始教大家怎么使用mysqldump管理工具导入大于2MB的数据.
1)左下角开始菜单-&gt;运行-&gt;CMD进入DOS命令行状态
2)D:\Program Files\MySQL\MySQL Server 5.0\bin&gt;为你安装的MYSQL安装目录,\bin为mysqldump管理工具所有在的目录;
3)然后输入:<strong>mysql -h119.12.12.11 -uaspchina -paspchina123456 aspchina&lt; d:\aspchina_net.sql</strong>
注释:aspchina_net.sql,如果用户没有创建数据库的权限将不能导入aspchina_net.sql数据库,否则spchina_net.sql只能是多张表不然会出错,这点切记!

<strong><em>MySQL导入.sql文件及常用命令</em></strong>
<div class="table-box">
<table border="0">
<tbody>
<tr>
<td>在MySQL Qurey   Brower中直接导入*.sql脚本，是不能一次执行多条sql命令的，在mysql中执行sql文件的命令：

mysql&gt; source   d:/myprogram/database/db.sql;

另附mysql常用命令：

一) 连接MYSQL：

格式： mysql -h主机地址 -u用户名 －p用户密码

1、例1：连接到本机上的MYSQL

首先在打开DOS窗口，然后进入mysql安装目录下的bin目录下，例如： D:/mysql/bin，再键入命令mysql -uroot -p，回车后提示你输密码，如果刚安装好MYSQL，超级用户root是没有密码的，故直接回车即可进入到MYSQL中了，MYSQL的提示符是：mysql&gt;

2、例2：连接到远程主机上的MYSQL (远程：IP地址)

假设远程主机的IP为：10.0.0.1，用户名为root,密码为123。则键入以下命令：

mysql -h10.0.0.1 -uroot -p123

（注：u与root可以不用加空格，其它也一样）

3、退出MYSQL命令

exit （回车）

(二) 修改密码：

格式：mysqladmin -u用户名 -p旧密码 password 新密码

1、例1：给root加个密码123。首先在DOS下进入目录C:/mysql/bin，然后键入以下命令：

mysqladmin -uroot -password 123

注：因为开始时root没有密码，所以-p旧密码一项就可以省略了。

2、例2：再将root的密码改为456

mysqladmin -uroot -pab12 password 456

(三) 增加新用户：（注意：和上面不同，下面的因为是MYSQL环境中的命令，所以后面都带一个分号作为命令结束符）

格式：grant select on 数据库.* to 用户名@登录主机 identified by "密码"

例1、增加一个用户test1密码为abc，让他可以在任何主机上登录，并对所有数据库有查询、插入、修改、删除的权限。首先用以root用户连入MYSQL，然后键入以下命令：     grant select,insert,update,delete on *.* to <a href="mailto:test2@localhost" rel="nofollow">test2@localhost</a> identified by "abc";

如果你不想test2有密码，可以再打一个命令将密码消掉。     grant select,insert,update,delete on mydb.* to <a href="mailto:test2@localhost" rel="nofollow">test2@localhost</a> identified by "";

(四) 显示命令

1、显示数据库列表：

show databases;     刚开始时才两个数据库：mysql和test。mysql库很重要它里面有MYSQL的系统信息，我们改密码和新增用户，实际上就是用这个库进行操作。

2、显示库中的数据表：

use mysql； //打开库    show tables;

3、显示数据表的结构：

describe 表名;

4、建库：

create database 库名;

5、建表：

use 库名；     create table 表名 (字段设定列表)；

6、删库和删表:

drop database 库名;     drop table 表名；

7、将表中记录清空：

delete from 表名;

8、显示表中的记录：

select * from 表名;

导出sql脚本

mysqldump -u 用户名 -p 数据库名 &gt; 存放位置

mysqldump -u root -p test &gt; c:/a.sql

导入sql脚本

mysql -u 用户名 -p 数据库名 &lt; 存放位置

mysqljump -u root -p test &lt; c:/a.sql

注意,test数据库必须已经存在

MySQL导出导入命令的用例

1.导出整个数据库

mysqldump -u 用户名 -p 数据库名 &gt; 导出的文件名

mysqldump -u wcnc -p smgp_apps_wcnc &gt; wcnc.sql

2.导出一个表

mysqldump -u 用户名 -p 数据库名表名&gt; 导出的文件名

mysqldump -u wcnc -p smgp_apps_wcnc users&gt; wcnc_users.sql

3.导出一个数据库结构

mysqldump -u wcnc -p -d --add-drop-table smgp_apps_wcnc &gt;d:wcnc_db.sql

-d 没有数据 --add-drop-table 在每个create语句之前增加一个drop table

4.导入数据库

常用source 命令

进入mysql数据库控制台,

如mysql -u root -p

mysql&gt;use 数据库

然后使用source命令,后面参数为脚本文件(如这里用到的.sql)

mysql&gt;source d:wcnc_db.sql</td>
</tr>
</tbody>
</table>
</div>
</div>