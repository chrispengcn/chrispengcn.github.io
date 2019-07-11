---
ID: 2801
post_title: CloudFront+WordPress实现全站CDN
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://www.hss5.com/2019/07/11/cloudfront-wordpress-fullsite-cdn-cache/
published: true
post_date: 2019-07-11 23:01:42
---
https://milkice.me/2017/07/25/wordpress-entire-site-cdn-via-cloudfront/
<h2>背景</h2>
最近真的要穷的吃土啦！

原先的vps的月付费用已经完全无法支撑（摊手），后来不得已把IceBox迁到了Digital Ocean（有coupon可以连着开好几个月啦），但是失望地发现无论是美西旧金山节点还是新加坡节点从国内访问的速度都是十分感人，况且听说最近Digital Ocean在国内一些地方不能访问，据说是因为路由的问题，所以无论如何再怎么犯懒癌这次都要给IceBox上CDN

｡ﾟヽ(ﾟ´Д`)ﾉﾟ｡

一开始选了一些国内的CDN服务商像又拍云，阿里云这种，后来发现如果要全站CDN加速是要备案的，甚至连开启外链都要实名制，按照奶冰所有站点均不备案且不随便透露个人身份信息的原则我马上就拒绝了这些服务商，目光转向国际CDN服务商，后来查了一下发现大概只有Cloudflare和Cloudfront在全球范围的节点覆盖情况是比较好的，由于cloudflare还有改域名ns服务器什么的个人觉得非常的不方便，于是便打算着手于部署CloudFront CDN
<h2>准备工作</h2>
CloudFront是Amazon家的东西，所以需要准备一个AWS账号

AWS账号审查机制可谓是全球几大知名服务商里最松的了，虚拟信用卡就可以过验证，<del>这也导致了这几年AWS东亚网络质量直线下降</del>，所以注册并不麻烦，某宝上面一刀信用卡特别多实在不行可以去拍一张，这里不再复述
<h2>开始配置</h2>
打开AWS控制台，点击CloudFront

然后点击Create Distribution，在Web栏下面点击Get started即开始配置
<h2>配置Origin Settings回源设置</h2>
Origin Domain Name就是你的源站域名（请注意CloudFront源站不接受ip地址，强制使用域名，我也不知道为什么）

<strong>但是请注意，这里的Origin Domain Name可不能填写网站的主域名，比如对于奶冰的IceBox来说这里就不能直接填写milkice.me，而是要开个子域名直接指向源站IP，比如这里的www.milkice.me就是开了个A记录到源站IP</strong>

Origin Path就是你想要加速的路径（默认为空）

比如你的源站地址是http://example.com/wordpress，则Origin Domain Name这里可以填写一个xxx.example.com，然后DNS记录里设置xxx.example.com指向源站ip，Origin Path填写/wordpress

如果你想要部署强制https可以选择HTTPS only，其他选项不变即可

<img class="alignnone size-full wp-image-2804" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-2-2.jpg" width="857" height="504" alt="《CloudFront+Wordpress实现全站CDN》" />

&nbsp;
<h2>配置Default Cache Behavior Settings</h2>
Default Cache Behavior Settings，其实就是配置在默认情况下CloudFront的一些缓存条件

这里的默认情况即我们想要缓存加速的文件（有些目录需要另外加Behavior来排除，后文会说）

由于wordpress的一些特性，有一些设置需要变动
<ul>
 	<li>Forward Headers（转发header）需要将host添加进白名单设置</li>
 	<li>Whitelist Cookies（Cookie白名单）需要将wordpress*和wp*加入白名单</li>
 	<li>Query String Forwarding and Caching一些博主推荐设置为Forward all, cache based on all，但是在奶冰这里测试的情况下如果选择cache based on all会导致基本上所有请求都不会缓存（抓包抓到的HTTP头里基本上都是<em>X</em>-Cache:<em>Miss from cloudfront</em>），前期不推荐cache based on all，后期像搜索功能如果出现了问题可以尝试开启</li>
 	<li>（可选）Viewer Protocol Policy中，如果站点是HTTPS站点可以设置Redirect HTTP to HTTPS以提高安全性</li>
</ul>
具体配置看图

<img class="alignnone size-full wp-image-2805" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-3-2.jpg" width="912" height="448" alt="《CloudFront+Wordpress实现全站CDN》" />

<img class="alignnone size-full wp-image-2806" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-4-2.jpg" width="683" height="494" alt="《CloudFront+Wordpress实现全站CDN》" />

&nbsp;
<h2>配置Distribution Settings分发设置</h2>
有这么若干要配置的点
<ul>
 	<li>Price Class个人觉得不用配置也可以，不过你要是想省点钱的话就选择第二档（Use Only US, Canada, Europe and Asia）</li>
 	<li>Alternate Domain Names(CNAMEs) 对于全局CDN来说这里要填的就是要加速的域名，我这里就是milkice.me</li>
 	<li>SSL certificate 由于要配置全站CDN所以第一个选项不可用（如果你能忍受那个红色的SSL warning也无所谓），一般都是选择Custom SSL certificate，这里不管你有没有ssl证书你都可以在Amazon Certificate Manager签发Amazon证书用于分发服务器与终端之间的数据加密，点击Request or Import a Certificate with ACM签发证书后回来刷新一下就有你的证书了，应用即可</li>
</ul>
其他默认，看图即可

<img class="alignnone size-full wp-image-2807" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-5-2.jpg" width="731" height="506" alt="《CloudFront+Wordpress实现全站CDN》" />

&nbsp;

这时候就基本完成了，点击Create Distribution，Amazon即开始部署
<h2>后期附加操作</h2>
<del>估计因为CloudFront在全球节点太多了吧</del>

每次创建/修改设置之后重新配置花的时间挺长的大约5-10分钟，这时候可以慢慢等待或者配置进一步的Behavior Settings

到Behaviors，点击Create Behavior
<ol>
 	<li>Path Pattern填写/wp-admin，是不是很熟悉呢，对的没错，wordpress默认管理界面，由于wp-admin一般都需要动态生成所以这时候不需要CloudFront服务器缓存，所以要创建一个Behavior以规避这种情况</li>
 	<li>Path Pattern填写/wp-login.php，这是在根目录下的用于登陆的一个php文件，由于要POST登录数据所以这个文件也不需要缓存，需要特别设个规则</li>
</ol>
如图配置

<img class="alignnone size-full wp-image-2808" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-6-2.jpg" width="856" height="497" alt="《CloudFront+Wordpress实现全站CDN》" />

<img class="alignnone size-full wp-image-2809" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-7-2.jpg" width="796" height="470" alt="《CloudFront+Wordpress实现全站CDN》" />

（其实两个配置是一样的只要把Path Pattern的值改一下即可，wp-login.php我就不发截图了）

最后好了应该是这样

<img class="alignnone size-full wp-image-2810" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-8-2.jpg" width="1182" height="220" alt="《CloudFront+Wordpress实现全站CDN》" />

接下来去NS服务商配置CNAME记录

将原来的记录删掉，再加个CNAME记录指向CloudFront给出的地址即可

<img class="alignnone size-full wp-image-2811" src="http://www.hss5.com/wp-content/uploads/2019/07/wordpress-cloudfront-9-2.jpg" width="448" height="48" alt="《CloudFront+Wordpress实现全站CDN》" />

然后就等全球DNS服务器更新缓存吧

(๑´ㅂ`๑)
<h2>后续一些使用感受</h2>
上了CloudFront之后速度的确快了一点，不过因为第一次访问的时候需要回源下载数据缓存，所以会比较慢

但是很恶心的一点是因为奶冰目前的dns不能根据地理位置分流，导致被分到的CDN边缘节点不是东亚节点而是全球节点，大大降低了访问速度

看了一些别的博主的解决方案是用Amazon的Route 53 GeoDNS功能实现准确分流，但是我又不想把NS服务器转到Amazon那里去（主要是全球48小时更新时间太恶心）

但是似乎可以指定子域名的NS服务器为Route 53，暂时未测试，以后补坑#Mark
<h2>参考资料</h2>
https://milkice.me/2017/07/25/wordpress-entire-site-cdn-via-cloudfront/

<a href="http://www.xiongge.club/170.html" target="_blank" rel="nofollow noopener noreferrer">http://www.xiongge.club/170.html</a>

<a href="https://www.ze3kr.com/2017/01/wordpress-full-site-cdn/" target="_blank" rel="nofollow noopener noreferrer">https://www.ze3kr.com/2017/01/wordpress-full-site-cdn/</a>

<a href="https://leonax.net/p/7662/wordpress-complete-cdn-from-scratch/" target="_blank" rel="noopener noreferrer">https://leonax.net/p/7662/wordpress-complete-cdn-from-scratch/ （不推荐，过于复杂）</a>

<hr />

<div class="alert alert-info">然而，这个被奶冰折腾了两天的玩意最后却没派上用场，因为ItsLucas贡献了一台Linode VPS，然后就没有然后了( ˘･з･) 记录一下存档用吧</div>
&nbsp;