---
ID: 259
post_title: XAMPP 安装 magento
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/09/01/xampp-%e5%ae%89%e8%a3%85-magento/'
published: true
post_date: 2013-09-01 07:59:41
---
<p>貌似现在Magento很流行，所以想在自己的机器弄一个玩玩 <br><strong>安装XAMPP</strong><br>由于Magento是用php开发的，所以先要搭一个php的运行环境，而XAMPP类似一个傻瓜式安装包，他包含了Apache, MySQL, PHP等安装Magento必须的部件。 <br>1. 下载XAMPP <br>到<a href="http://www.apachefriends.org/zh_cn/xampp-windows.html#1787">http://www.apachefriends.org/zh_cn/xampp-windows.html#1787</a>下载XAMPP for Windows的安装程序 <br>2. 安装XAMPP <br>由于我下载的是自解压的exe文件，所以直接运行就可以了。 <br>安装完毕后会进行配置，都选择默认就可以了。 <br>3. 启动服务 <br>安装完毕后，执行xampp-control.exe文件，启动相应服务就可以了 <br>apache服务可能无法启动，原因是由于apache服务默认使用80端口，ssl使用443端口，如果这2个端口已被使用，就可能造成apache服务无法启动。解决方法是修改默认端口。 <br>修改\apache\conf\httpd.conf，把80改成其他值（好像有2处地方需要改） <br>修改apache\conf\extra\httpd-ssl.conf，把443也改掉 <br>保存后再启动，应该就没问题了 <br>当然也可以通过port check，检查一下80和443端口被哪个程序占用了，然后嘛大家都懂的。 <br>4. 验证 <br>启动服务后，在浏览器中输入<a href="http://localhost/">http://localhost/</a>，如果可以看到XAMPP页面，就说明安装成功了 <br><strong>安装Magento</strong><br>1. 下载Magento安装包和Sample Data <br>从官网<a href="http://www.magentocommerce.com/download">http://www.magentocommerce.com/download</a>上下载magento的安装包，并解压；同一个页面也可以下载Sample Data，然后也解压，并把里面的Media文件夹复制到Magento的安装包内。 <br>然后将整个Magento安装包复制到xampp\htdocs\下就可以了。 <br>2. 导入Sample Data <br>进入XAMPP的phpMyAdmin，先创建一个数据库，然后把magento_sample_data_for_xxx.sql导入到新建的数据库中。 <br>3. 安装Magento <br>在浏览器中输入http://127.0.0.1:&lt;port&gt;/&lt;Magento安装包所在文件夹名&gt;，就可以step by step的安装Magento了。 <br>修改xampp\php\php.ini（位置可能有所不同，search一下），把max_execution_time修改得大一些（比如240），已防止安装过程中某些操作时间过长，而超时。 <br>4. 运行Magento <br>安装结束后，就可以访问你自己搭建的第一个Magento网站了 </p>