---
ID: 2547
post_title: >
  Linux下的crontab定时执行任务命令详解
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/03/06/crontab-setting/
published: true
post_date: 2019-03-06 04:05:35
---
<div class="postTitle">
<h1>在LINUX中，周期执行的任务一般由cron这个守护进程来处理[ps -ef|grep cron]。cron读取一个或多个配置文件，这些配置文件中包含了命令行及其调用时间。</h1>
</div>
<div id="cnblogs_post_body" class="blogpost-body">

cron的配置文件称为“crontab”，是“cron table”的简写。

<strong>一、cron服务</strong>
cron是一个linux下 的定时执行工具，可以在无需人工干预的情况下运行作业。
service crond start    //启动服务
service crond stop     //关闭服务
service crond restart  //重启服务
service crond reload   //重新载入配置
service crond status   //查看服务状态

<strong>二、cron在3个地方查找配置文件：</strong>
1、/var/spool/cron/ 这个目录下存放的是每个用户包括root的crontab任务，每个任务以创建者的名字命名，比如tom建的crontab任务对应的文件就是/var/spool/cron/tom。一般一个用户最多只有一个crontab文件。

<strong>三、/etc/crontab 这个文件负责安排由系统管理员制定的维护系统以及其他任务的crontab。</strong>

SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
HOME=/

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# | .------------- hour (0 - 23)
# | | .---------- day of month (1 - 31)
# | | | .------- month (1 - 12) OR jan,feb,mar,apr ...
# | | | | .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# | | | | |
# * * * * * user-name command to be executed

<img class="alignnone size-full wp-image-2552" src="http://hss5.com/wp-content/uploads/2019/03/722724-20160817105840890-421233819-3.png" width="420" height="200" alt="" />
<ul>
 	<li>MAILTO=root：是说，当 /etc/crontab 这个档案中的例行性命令发生错误时，会将错误讯息或者是屏幕显示的讯息传给谁？由于 root 并无法再用户端收信，因此，我通常都將这个 e-mail 改成自己的账号，好让我随时了解系统的状态！</li>
 	<li><strong>01 * * * * root run-parts /etc/cron.hourly</strong>：在 #run-parts 这一行以后的命令，我们可以发现，五个数字后面接的是 root ，这一行代表的是『执行的级别为root身份』当然，你也可以将这一行改为成其他的身份！而 run-parts代表后面接的 /etc/cron.hourly 是『一个目录内（/etc/cron.hourly）的所有可执行文件』，也就是说，每个小时的01分，系统会以root身份去/etc/cron.hourly这个目录下执行所有可执行的文件！后面三行也是类似的意思！你可以到 /etc/ 底下去看看，系统本来就预设了这4个目录！你可以将每天需要执行的命令直接写到/etc/cron.daily即可，还不需要使用到crontab -e的程式！</li>
</ul>
<strong>四、/etc/cron.d/ 这个目录用来存放任何要执行的crontab文件或脚本。</strong>

<strong>五、权限(？)</strong>
crontab权限问题到/var/adm/cron/下一看，文件cron.allow和cron.deny是否存在
用法如下：
1、如果两个文件都不存在，则只有root用户才能使用crontab命令。
2、如果cron.allow存在但cron.deny不存在，则只有列在cron.allow文件里的用户才能使用crontab命令，如果root用户也不在里面，则root用户也不能使用crontab。
3、如果cron.allow不存在, cron.deny存在，则只有列在cron.deny文件里面的用户不能使用crontab命令，其它用户都能使用。
4、如果两个文件都存在，则列在cron.allow文件中而且没有列在cron.deny中的用户可以使用crontab，如果两个文件中都有同一个用户，以cron.allow文件里面是否有该用户为准，如果cron.allow中有该用户，则可以使用crontab命令。

AIX 中 普通用户默认都有 crontab 权限，如果要限制用户使用 crontab ,就需要编辑/var/adm/cron/cron.deny
HP-UNIX 中默认普通用户没得crontab 权限 ，要想放开普通用户的crontab 权限可以编

<strong>六、创建cron脚本</strong>
第一步：写cron脚本文件,命名为crontest.cron。
15,30,45,59 * * * * echo "xgmtest....." &gt;&gt; xgmtest.txt  表示，每隔15分钟，执行打印一次命令
第二步：添加定时任务。执行命令 “crontab crontest.cron”。搞定
第三步："crontab -l" 查看定时任务是否成功或者检测/var/spool/cron下是否生成对应cron脚本

注意：这操作是直接替换该用户下的crontab，而不是新增

&nbsp;

<strong>七、crontab用法 </strong>
crontab命令用于安装、删除或者列出用于驱动cron后台进程的表格。用户把需要执行的命令序列放到crontab文件中以获得执行。
每个用户都可以有自己的crontab文件。/var/spool/cron下的crontab文件不可以直接创建或者直接修改。该crontab文件是通过crontab命令创建的

在crontab文件中如何输入需要执行的命令和时间。该文件中每行都包括六个域，其中前五个域是指定命令被执行的时间，最后一个域是要被执行的命令。
每个域之间使用空格或者制表符分隔。格式如下：
minute hour day-of-month month-of-year day-of-week commands
合法值 00-59 00-23 01-31 01-12 0-6 (0 is sunday)
除了数字还有几个个特殊的符号就是"*"、"/"和"-"、","，*代表所有的取值范围内的数字，"/"代表每的意思,"/5"表示每5个单位，"-"代表从某个数字到某个数字,","分开几个离散的数字。

-l 在标准输出上显示当前的crontab。
-r 删除当前的crontab文件。
-e 使用VISUAL或者EDITOR环境变量所指的编辑器编辑当前的crontab文件。当结束编辑离开时，编辑后的文件将自动安装。

&nbsp;

<strong>八、例子： </strong>
每天早上6点
0 6 * * * echo "Good morning." &gt;&gt; /tmp/test.txt //注意单纯echo，从屏幕上看不到任何输出，因为cron把任何输出都email到root的信箱了。

每两个小时
0 */2 * * * echo "Have a break now." &gt;&gt; /tmp/test.txt

晚上11点到早上8点之间每两个小时和早上八点
0 23-7/2，8 * * * echo "Have a good dream" &gt;&gt; /tmp/test.txt

每个月的4号和每个礼拜的礼拜一到礼拜三的早上11点
0 11 4 * 1-3 command line

1月1日早上4点
0 4 1 1 * command line SHELL=/bin/bash PATH=/sbin:/bin:/usr/sbin:/usr/bin MAILTO=root //如果出现错误，或者有数据输出，数据作为邮件发给这个帐号 HOME=/

每小时执行/etc/cron.hourly内的脚本
01 * * * * root run-parts /etc/cron.hourly
每天执行/etc/cron.daily内的脚本
02 4 * * * root run-parts /etc/cron.daily

每星期执行/etc/cron.weekly内的脚本
22 4 * * 0 root run-parts /etc/cron.weekly

每月去执行/etc/cron.monthly内的脚本
42 4 1 * * root run-parts /etc/cron.monthly

注意: "run-parts"这个参数了，如果去掉这个参数的话，后面就可以写要运行的某个脚本名，而不是文件夹名。

每天的下午4点、5点、6点的5 min、15 min、25 min、35 min、45 min、55 min时执行命令。
5，15，25，35，45，55 16，17，18 * * * command

每周一，三，五的下午3：00系统进入维护状态，重新启动系统。
00 15 * * 1，3，5 shutdown -r +5

每小时的10分，40分执行用户目录下的innd/bbslin这个指令：
10，40 * * * * innd/bbslink

每小时的1分执行用户目录下的bin/account这个指令：
1 * * * * bin/account

每天早晨三点二十分执行用户目录下如下所示的两个指令（每个指令以;分隔）：
20 3 * * * （/bin/rm -f expire.ls logins.bad;bin/expire$#@62;expire.1st）

每年的一月和四月，4号到9号的3点12分和3点55分执行/bin/rm -f expire.1st这个指令，并把结果添加在mm.txt这个文件之后（mm.txt文件位于用户自己的目录位置）。
12,55 3 4-9 1,4 * /bin/rm -f expire.1st$#@62;$#@62;mm.txt

</div>