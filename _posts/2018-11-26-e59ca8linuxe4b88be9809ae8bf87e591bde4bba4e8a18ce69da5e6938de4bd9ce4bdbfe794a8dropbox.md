---
ID: 1470
post_title: >
  在Linux下通过命令行来操作使用Dropbox
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/26/%e5%9c%a8linux%e4%b8%8b%e9%80%9a%e8%bf%87%e5%91%bd%e4%bb%a4%e8%a1%8c%e6%9d%a5%e6%93%8d%e4%bd%9c%e4%bd%bf%e7%94%a8dropbox/'
published: true
post_date: 2018-11-26 10:16:09
---
Dropbox是一款非常好用的免费网络文件同步工具，是Dropbox公司运行的在线存储服务，通过云计算实现因特网上的文件同步，用户可以存储并共享文件和文件夹。Dropbox提供免费和收费服务，Dropbox的收费服务包括Dropbox Pro 和 Dropbox for Business。在不同操作系统下有客户端软件，并且有网页客户端。

当你在电脑A使用Dropbox时，指定文件夹里所有文件的改动均会自动地"同步”到 Dropbox的服务器，当下次你在电脑B需要使用这些文件时，你只需登录你的账户，所有被同步的文件均会自动下载到B电脑中。同样，你在电脑B对某文件的修改，也会体现在电脑A上，而所有这一切均是全自动的，这样你的文件可以说是随时随地都能保持着最新了。将文件放入一台电脑的Dropbox里面去，文件就能即时的同步到Dropbox的服务器端，这些文件在你任何安装了Dropbox的电脑上都可以访问。你可以用电脑或者移动终端从 Dropbox网站来访问这些文件。

用户可以通过Dropbox客户端，把任意文件丢入指定文件夹，然后就会被同步到云，以及该用户其他装有Dropbox客户端的其他计算机中。

Dropbox文件夹中的文件随后就可以与其他Dropbox用户分享，或通过网页来获取。用户也可以通过网页浏览器来手工上传文件。Dropbox作为存储服务，主要专注于同步和共享。Dropbox支持修订历史纪录，即使文件被删，也可以从任何一个同步计算机中得以恢复。用户通过Dropbox的版本控制，可以知道他们共同作业文件的历史纪录，这样多人参与编辑、再发布文件，就不会因为并发而丢失先前的纪录。版本纪录历史仅限于30天，而通过付费可以实现无限的版本纪录，也就是所谓的 "Pack-Rat"。版本纪录用到了差分编码技术，为了节省带宽和时间，当用户Dropbox文件夹中的文件发生变化后，Dropbox只上传改变的文件部分，并实施同步。尽管桌面客户端对单个文件大小不作限制，而通过网站上传的单个文件大小上限则是300MB。 Dropbox使用亚马逊的S3存储系统来存放文件。 并采用SoftLayer技术来购建后端的基础设施。 Dropbox同步采用SSL传输数据，而存储则通过AES-256进行加密。

当然 Linux 平台下也有着自己的 Dropbox 客户端： 既有命令行的，也有图形界面客户端。Dropbox Uploader 是一个简单易用的 Dropbox 命令行客户端，它是用 Bash 脚本语言所编写的。在这篇教程中，我将描述 在 Linux 中如何使用 Dropbox Uploader 通过命令行来访问 Dropbox。

Linux 中安装和配置 Dropbox Uploader

要使用 Dropbox Uploader，你需要下载该脚本并使其可被执行。

代码如下:

$ wget https://raw.github.com/andreafabrizi/Dropbox-Uploader/master/dropbox_uploader.sh

$ chmod +x dropbox_uploader.sh

请确保你已经在系统中安装了 curl，因为 Dropbox Uploader 通过 curl 来运行 Dropbox 的 API。

要配置 Dropbox Uploader，只需运行 dropbox_uploader.sh 即可。当你第一次运行这个脚本时，它将请求得到授权以使得脚本可以访问你的 Dropbox 账户。

代码如下:

$ ./dropbox_uploader.sh

<a title="在Linux下通过命令行来操作使用Dropbox" href="https://kfwimg.ikafan.com/upload/31/35/3135fb71fe361c4420e729e625d07b84.jpg" target="_blank" rel="nofollow noopener"><img src="https://kfwimg.ikafan.com/upload/2a/28/2a283de81e085bfeb31d3b555c8e3af0_thumb.jpg" alt="" /></a>

如上图所指示的那样，你需要通过浏览器访问 https://www.dropbox.com/developers/apps 页面，并创建一个新的 Dropbox app。接着像下图那样填入新 app 的相关信息，并输入 app 的名称，它与 Dropbox Uploader 所生成的 app 名称类似。

<a title="在Linux下通过命令行来操作使用Dropbox" href="https://kfwimg.ikafan.com/upload/8e/2d/8e2db38157852d50fc96bee45cd0cd08.jpg" target="_blank" rel="nofollow noopener"><img src="https://kfwimg.ikafan.com/upload/9e/9e/9e9edbcc26875d2e6528dc0d0c743292_thumb.jpg" alt="" /></a>

在你创建好一个新的 app 之后，你将在下一个页面看到 app key 和 app secret。请记住它们。


然后在正运行着 dropboxuploader.sh 的终端窗口中输入 app key 和 app secret。然后 dropboxuploader.sh 将产生一个 oAUTH 网址(例如，https://www.dropbox.com/1/oauth/authorize?oauth_token=XXXXXXXXXXXX)。

<a title="在Linux下通过命令行来操作使用Dropbox" href="https://kfwimg.ikafan.com/upload/7c/53/7c5392e5e7754f0b614d7e40d31f13be.jpg" target="_blank" rel="nofollow noopener"><img src="https://kfwimg.ikafan.com/upload/6e/aa/6eaa59ee8e765830b06f55f12cd66f92_thumb.jpg" alt="" /></a>

接着通过浏览器访问那个 oAUTH 网址，并同意访问你的 Dropbox 账户。

<a title="在Linux下通过命令行来操作使用Dropbox" href="https://kfwimg.ikafan.com/upload/4d/d9/4dd96040d3e1b3a19d9c8c9078078369.jpg" target="_blank" rel="nofollow noopener"><img src="https://kfwimg.ikafan.com/upload/2c/5d/2c5d50426d4782fada8267936f834e2d_thumb.jpg" alt="" /></a>

这便完成了 Dropbox Uploader 的配置。若要确认 Dropbox Uploader 是否真的被成功地认证了，可以运行下面的命令。

代码如下:

$ ./dropbox_uploader.sh info

Dropbox Uploader v0.12

&gt; Getting info...

Name: Dan Nanni

UID: XXXXXXXXXX

Email: my@email_address

Quota: 2048 Mb

Used: 13 Mb

Free: 2034 Mb

Dropbox Uploader 示例

要显示根目录中的所有内容，运行：

代码如下:

$ ./dropbox_uploader.sh list

要列出某个特定文件夹中的所有内容，运行：

代码如下:

$ ./dropbox_uploader.sh list Documents/manuals

要上传一个本地文件到一个远程的 Dropbox 文件夹，使用：

代码如下:

$ ./dropbox_uploader.sh upload snort.pdf Documents/manuals

要从 Dropbox 下载一个远程的文件到本地，使用：

代码如下:

$ ./dropbox_uploader.sh download Documents/manuals/mysql.pdf ./mysql.pdf

要从 Dropbox 下载一个完整的远程文件夹到一个本地的文件夹，运行：

代码如下:

$ ./dropbox_uploader.sh download Documents/manuals ./manuals

要在 Dropbox 上创建一个新的远程文件夹，使用：

代码如下:

$ ./dropbox_uploader.sh mkdir Documents/whitepapers

要完全删除 Dropbox 中某个远程的文件夹(包括它所含的所有内容)，运行：

代码如下:

$ ./dropbox_uploader.sh delete Documents/manuals