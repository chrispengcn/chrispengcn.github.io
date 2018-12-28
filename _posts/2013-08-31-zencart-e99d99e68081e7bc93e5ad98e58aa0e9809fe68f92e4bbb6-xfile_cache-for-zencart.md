---
ID: 200
post_title: >
  zencart 静态缓存加速插件
  xfile_cache-for-zencart
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/31/zencart-%e9%9d%99%e6%80%81%e7%bc%93%e5%ad%98%e5%8a%a0%e9%80%9f%e6%8f%92%e4%bb%b6-xfile_cache-for-zencart/'
published: true
post_date: 2013-08-31 06:46:59
---
<h3>&nbsp;</h3> <p>xfile_cache for zencart 安装说明 <p>zencart 静态缓存加速插件使用说明 <p>1.将你网站根目录的index.php改名备份<br>2.解压后的文件将xfile_cache/zencart 目录下的index.php复制到根目录<br>3.将所有文件上传到网站根目录,根目录添加：xfile_cache_data目录为可写 <p>4.使用<br>清空静态缓存请访问：http://你的站/xfile_ajax.php<br>$xfile-&gt;limit_time = 360000;//缓存有效时间（秒）<br>在这里改缓存失效时间 <p>静态缓存会在访问者第一次访问时或蜘蛛爬行时自动生成</p>