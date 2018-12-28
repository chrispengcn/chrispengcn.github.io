---
ID: 913
post_title: 'magento &#8212; 使用translate.csv来修改翻译'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/08/07/magento-%e4%bd%bf%e7%94%a8translate-csv%e6%9d%a5%e4%bf%ae%e6%94%b9%e7%bf%bb%e8%af%91/'
published: true
post_date: 2017-08-07 10:06:36
---
一般Magento的语言包都是指/app/locale目录下的文件夹，以中文包为例，/app/locale/zh_CN下的所有文件就是中文语言包的全部内容（具体可见从<a href="http://www.magentochina.org/bbs/" target="_blank" rel="noopener noreferrer">http://www.magentochina.org/bbs/</a>下载的Magento汉化包）。

细心地人可能会发现，除了这里有csv文件，在模板文件目录下也有一个locale文件夹，这里同样有个文件名为translate.csv的csv文件（在各自语言文件夹下，比如默认在locale下就只有一个en_US文件夹，里面自带一个translate.csv文件）。

现在我们来做个实验，在你所使用的模板目录下/app/design/frontend/default/default/locale（这里以default为例），新建文件夹zh_CN，在这个文件夹下新建文件translate.csv，打开translate.csv，添加这样一句：
<div class="dp-highlighter bg_c-sharp">
<div class="bar">
<div class="tools">
<div><embed id="ZeroClipboardMovie_1" src="http://static.blog.csdn.net/scripts/ZeroClipboard/ZeroClipboard.swf" type="application/x-shockwave-flash" width="18" height="18" align="middle" name="ZeroClipboardMovie_1" data-mce-fragment="1"></embed></div>
</div>
</div>
<ol class="dp-c" start="1">
 	<li class="alt"><span class="string">"My Cart"</span>,<span class="string">"购物袋"</span></li>
</ol>
</div>
&nbsp;

&nbsp;

保存。

现在打开前台，你会发现原来的“我的购物车”变成了“购物袋”（“我的购物车”这个翻译取自<a href="http://www.magentochina.org/bbs/" target="_blank" rel="noopener noreferrer">http://www.magentochina.org/bbs/</a>的汉化包）。

&nbsp;

<img src="http://hi.csdn.net/attachment/201010/9/0_1286634296H52M.gif" alt="" />

&nbsp;

可以推断出，translate.csv里的翻译要比/app/locale/下的语言文件里的翻译优先级要高。

&nbsp;

其实从这个文件放的位置就可以理解，这个csv文件是专门给所在的模板用的，当使用这个模板时，translate.csv里的翻译项会覆盖掉语言包里的同名项，至于实际用法，以上面的为例，国内的语言包现在都是把My Cart翻译成“我的购物车”，这个翻译没有问题，但如果是做一个服装网站，把它翻译成“购物袋”是不是会更讨巧和更有创意呢，这时你不需要去修改/app/locale/zh_CN目录下的文件，而是像上面的例子一样去translate.csv新增项来覆盖掉原来的。

以为自身的使用情况来说，虽然网上有现成的中文汉化包提供下载，但并没有做到百分百汉化（其中有一些是Magento自带的bug造成的），特别是后台，而国内的客户是很难接受在后台经常看到英文的，所以在这个汉化包的基础上，我经常需要把发现的漏网之鱼做好翻译并加到语言包里去，积累起来更完善的语言包以便下个项目可以重用，这时就会存在一个问题，有些项目的特殊性会要求把同一段英文翻译成不同的中文（还是以购物车和购物袋为例），如果把这一类的翻译直接去改语言包里的文件来实现，下一个项目要重用这个语言包就会带来问题。所以，把所有可能个性的，无法重用的翻译都写到translate.csv里去是一种正确和合理的思路，我觉得这也是官方提供这种方式的初衷。

PS：后台模板目录下同样存在这个文件，可以用同样的方式修改后台的翻译