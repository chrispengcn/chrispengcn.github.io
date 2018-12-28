---
ID: 1762
post_title: >
  pagespeed模块安装——Nginx、Tengine
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/12/18/pagespeed%e6%a8%a1%e5%9d%97%e5%ae%89%e8%a3%85-nginx%e3%80%81tengine/'
published: true
post_date: 2018-12-18 01:12:35
---
<div class="clear"></div>
<div class="postBody">
<div id="cnblogs_post_body" class="blogpost-body">

1、安装好nginx或者tengine

2、下载pagespeed模块并且解压

<span class="crayon-i">sudo <span class="crayon-r">mkdir <span class="crayon-o">-p /<span class="crayon-v">usr/local/tengine/modules <span class="crayon-h">   </span></span></span></span></span>

<span class="crayon-sy"><span class="crayon-i">wget <span class="crayon-v">https<span class="crayon-o">://github<span class="crayon-e">.com/pagespeed/ngx_pagespeed/archive/v1.<span class="crayon-cn">7.30.3-beta.tar.gz <span class="crayon-h">   </span></span></span></span></span></span></span>
<div id="crayon-58942dd2c9e36538027132-3" class="crayon-line"><span class="crayon-sy"><span class="crayon-i">sudo tar xvfvz <span class="crayon-v">v1.<span class="crayon-cn">7.30.3<span class="crayon-o">-beta<span class="crayon-e">.tar.gz -C /usr/local/tengine/modules  --no-same-owner</span></span></span></span></span></span></div>
<div class="crayon-line"></div>
<div class="crayon-line"><span class="crayon-sy"><span class="crayon-i"><span class="crayon-v"><span class="crayon-cn"><span class="crayon-o"><span class="crayon-e">3、下载PSOL优化库</span></span></span></span></span></span></div>
<div class="crayon-line"><span class="crayon-sy"><span class="crayon-i"><span class="crayon-v"><span class="crayon-cn"><span class="crayon-o"><span class="crayon-e">wget https://dl.google.com/dl/page-speed/psol/1.7.30.3.tar.gz <span class="crayon-h">   </span></span></span></span></span></span></span>
<div id="crayon-58942dd2c9e3a292784576-2" class="crayon-line crayon-striped-line"><span class="crayon-sy"><span class="crayon-i">sudo tar xvfz <span class="crayon-cn">1.7.30.3.tar.gz <span class="crayon-o">-C /<span class="crayon-v">usr/local/tengine/modules</span><span class="crayon-v">/ngx_pagespeed-1.7.30.3-beta --no-same-owner </span></span></span></span></span></div>
<div class="crayon-line crayon-striped-line"></div>
<div class="crayon-line crayon-striped-line"><span class="crayon-sy"><span class="crayon-i"><span class="crayon-cn"><span class="crayon-o"><span class="crayon-v">4、加载pagespeed模块</span></span></span></span></span></div>
<div class="crayon-line crayon-striped-line"><span class="crayon-sy"><span class="crayon-i"><span class="crayon-cn"><span class="crayon-o"><span class="crayon-v">/usr/local/tengine/sbin/dso_tool --add-module=/usr/local/tengine/modules/ngx_pagespeed-1.7.30.3-beta/</span></span></span></span></span></div>
<div class="crayon-line crayon-striped-line"></div>
<div class="crayon-line crayon-striped-line"><span class="crayon-sy"><span class="crayon-i"><span class="crayon-cn"><span class="crayon-o"><span class="crayon-v">5、查看是否安装成功</span></span></span></span></span></div>
<div class="crayon-line crayon-striped-line"><span class="crayon-sy"><span class="crayon-i"><span class="crayon-cn"><span class="crayon-o"><span class="crayon-v">ls /usr/local/tengine/module   (列出ngx_pagespeed.so 表示安装成功)</span></span></span></span></span></div>
<div class="crayon-line crayon-striped-line"></div>
<div class="crayon-line crayon-striped-line"><span class="crayon-sy"><span class="crayon-i"><span class="crayon-cn"><span class="crayon-o"><span class="crayon-v">6、编辑nginx.conf配置文件支持pagespeed</span></span></span></span></span></div>
<div class="crayon-line crayon-striped-line">

dso {
load ngx_http_concat_module.so;
load ngx_http_sysguard_module.so;
load ngx_pagespeed.so;
}

</div>
<div class="crayon-line crayon-striped-line">...</div>
<div class="crayon-line crayon-striped-line">...</div>
<div class="crayon-line crayon-striped-line">Server {</div>
<div class="crayon-line crayon-striped-line">  listen 80;</div>
<div class="crayon-line crayon-striped-line">  index index.html index.php;</div>
<div class="crayon-line crayon-striped-line">  server_name xxx.xxx.xxx;</div>
<div class="crayon-line crayon-striped-line">

  pagespeed on;

&nbsp;

&nbsp;

&nbsp;

</div>
<div class="crayon-line crayon-striped-line">  pagespeed FileCachePath /var/ngx_pagespeed_cache;</div>
<div class="crayon-line crayon-striped-line">}</div>
<div class="crayon-line crayon-striped-line">7、检查配置文件 并且重新加载nginx</div>
<div class="crayon-line crayon-striped-line">nginx -t</div>
<div class="crayon-line crayon-striped-line">service nginx reload</div>
<div class="crayon-line crayon-striped-line"></div>
<div class="crayon-line crayon-striped-line"></div>
<div class="crayon-line crayon-striped-line">(注意：此种方法为动态模块编译，可以nginx -V 查看，不需要再次configure、make nginx，还可以在安装nginx的时候直接./configure --add-module=/usr/local/tengine/ngx_pagespeed-1.7.30.3-beta进行直接配置)</div>
<div id="crayon-58942dd2c9e3a292784576-3" class="crayon-line"><span class="crayon-sy"> </span></div>
<div class="crayon-line"><span class="crayon-sy">参考：https://zhangge.net/5063.html</span></div>
</div>
</div>
</div>