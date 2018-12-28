---
ID: 120
post_title: 'wordpress迁移+更换域名[zt]'
author: chris
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2013/08/28/wordpress%e8%bf%81%e7%a7%bb%e6%9b%b4%e6%8d%a2%e5%9f%9f%e5%90%8dzt/'
published: true
post_date: 2013-08-28 01:27:24
---
<h3>wordpress迁移+更换域名</h3> <p>2010-01-27 22:48:38 <p>今天忙里偷闲，将自己的blog从国外的免费空间（000webhost）迁移到了戈戈主机（顺便推荐一下<a href="http://www.gegehost.com,/">http://www.gegehost.com,</a>很不错的国外空间）。 <p><img alt="baiduer.net快照" src="http://img3.douban.com/view/note/large/public/p58389625-1.jpg"> <p>baiduer.net快照 <p>此外，趁着迁移，一并将域名做了更改，经历了cnnic天天变的政策，我对cn域名也是失去了信心，换回了baiduer.net域名。在这里记录一下整个迁移的过程吧。<br>第一步：需要备份原来的wordpress根目录，比如，从原来的3www8.cn上打包，拷贝到baiduer.net上，并解压。<br>第二步：备份数据库，可以使用phpmyadmin或者使用空间提供商提供的备份工具，比如我们备份的数据库命名为3www8.sql。<br>第三步：将备份数据库导入到新的数据库中（导入前需要新建数据库）。<br>第四步：修改数据库表：<br>&nbsp;&nbsp;&nbsp;&nbsp; 1.修改wp_options表的option_name列，将option_name为siteurl的值修改为新的blog地址，如<a href="http://baiduer.net">http://baiduer.net</a><br>&nbsp;&nbsp;&nbsp;&nbsp; 2.修改wp_options表的option_name列，将option_name为home的值，修改为新的blog地址，如<a href="http://baiduer.net">http://baiduer.net</a><br>第五步：修改wordpress的配置文件wp-config.php<br>&nbsp;&nbsp;&nbsp;&nbsp; 主要是修改DB_NAME,DB_USER,DB_PASS,DB_HOST四个字段<br>&nbsp;&nbsp;&nbsp;&nbsp; 将上面四个字段按照新的数据库配置进行修改<br>第六步：修改原来域名，使其永久重定向到新的域名<br>&nbsp;&nbsp;&nbsp;&nbsp; 比如：在原空间根目录下，修改.htaccess文件，添加<br>&nbsp;&nbsp;&nbsp;&nbsp; redirect 301 / <a href="http://baiduer.net">http://baiduer.net</a><br>注：此外，需要注意在wordpress后台，修改默认上传路径为新的路径，如： <p><img alt="修改wordpress后台的默认上传路径" src="http://img3.douban.com/view/note/large/public/p58389625-2.jpg"> <p>修改wordpress后台的默认上传路径 <p>否则可能无法上传发布图片等<br>再注：如果迁移后，有些图片无法看到，那么肯定是文章中，图片的地址仍然是老地址，此时在phpmyadmin中登陆，执行以下sql：<br>UPDATE `wp_posts` SET `post_content` = replace (`post_content`,’3www8.cn/3www8_blog_wp/wp-content/uploads’,'baiduer.net/wp-content/uploads’)<br>将文章中，存在的图片旧地址全部替换为新的地址。