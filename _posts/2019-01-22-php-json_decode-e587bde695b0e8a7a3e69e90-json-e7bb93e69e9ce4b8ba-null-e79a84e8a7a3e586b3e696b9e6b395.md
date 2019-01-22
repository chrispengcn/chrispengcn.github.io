---
ID: 2385
post_title: >
  PHP json_decode 函数解析 json
  结果为 NULL 的解决方法
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2019/01/22/php-json_decode-%e5%87%bd%e6%95%b0%e8%a7%a3%e6%9e%90-json-%e7%bb%93%e6%9e%9c%e4%b8%ba-null-%e7%9a%84%e8%a7%a3%e5%86%b3%e6%96%b9%e6%b3%95/'
published: true
post_date: 2019-01-22 13:59:31
---
<div class="clear">在做网站 CMS 模块时，对于模块内容 content 字段，保存的是 json 格式的字符串，所以在后台进行模块内容的编辑操作 ( 取出保存的数据 ) 时，需要用到 json_decode() 函数。</div>
<div class="postBody">
<div id="cnblogs_post_body" class="blogpost-body">

但是在解析的时候，使用 json_decode() 函数解析的结果一直是 NULL，没有出现希望解析成的数组。下面是问题和分析：

1. 当输出 json 字符串时，代码和页面的显示内容分别是：
<div class="cnblogs_code">
<pre>echo $content = $res[0]['con']['content'];</pre>
</div>
只需要考虑 $content , $res[0]['con']['content'] 是从返回的数据中取出 content 的值，这里不需要考虑。这时页面显示：
<div class="cnblogs_code">
<pre>{"bpic":"group1\/M00\/00\/0D\/rBAK31STtZeAe056AAKWuBmsAgc339.jpg","bname":"112","breason":"22","about1":"3334","about2":"444","about3":"555","tpic1":"group1\/M00\/00\/0D\/rBAK31STsyGARPwHAAMcD98U8xo736.jpg","tpic2":"group1\/M00\/00\/0D\/rBAK31STtZeAEnEaAANwlbOXMYA393.jpg","tpic3":"group1\/M00\/00\/0D\/rBAK31STtSqARRUMAAKWuBmsAgc150.jpg"}</pre>
</div>
稍微美化一下：
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>{
"bpic":"group1\/M00\/00\/0D\/rBAK31STtZeAe056AAKWuBmsAgc339.jpg",
"bname":"112",
"breason":"22",
"about1":"3334",
"about2":"444",
"about3":"555",
"tpic1":"group1\/M00\/00\/0D\/rBAK31STsyGARPwHAAMcD98U8xo736.jpg",
"tpic2":"group1\/M00\/00\/0D\/rBAK31STtZeAEnEaAANwlbOXMYA393.jpg",
"tpic3":"group1\/M00\/00\/0D\/rBAK31STtSqARRUMAAKWuBmsAgc150.jpg"
}</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
2. 此时使用 json_decode() 解析 $content，并使用 var_dump 打印：
<div class="cnblogs_code">
<pre>$content = json_decode($content,true);</pre>
</div>
但是页面却显示 NULL。此时使用 json_last_error() 函数打印一下错误，页面显示4，也就是语法错误。
<div class="cnblogs_code">
<pre>echo $errorinfo = json_last_error();   //输出4 语法错误</pre>
</div>
&nbsp;

<strong>解决方法一：</strong>

出现这个问题是因为在 json 字符串中反斜杠被转义，只需要用 htmlspecialchars_decode() 函数处理一下 $content 即可：
<div class="cnblogs_code">
<pre>$content = htmlspecialchars_decode($content);</pre>
</div>
此时再使用 json_decode() 函数解析，就没有问题了，页面输出：
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>Array ( 
[bpic] =&gt; group1/M00/00/0D/rBAK31STtZeAe056AAKWuBmsAgc339.jpg 
[bname] =&gt; 112 
[breason] =&gt; 22 
[about1] =&gt; 3334 
[about2] =&gt; 444 
[about3] =&gt; 555 
[tpic1] =&gt; group1/M00/00/0D/rBAK31STsyGARPwHAAMcD98U8xo736.jpg 
[tpic2] =&gt; group1/M00/00/0D/rBAK31STtZeAEnEaAANwlbOXMYA393.jpg 
[tpic3] =&gt; group1/M00/00/0D/rBAK31STtSqARRUMAAKWuBmsAgc150.jpg 
)</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
&nbsp;

<strong>解决方法二：</strong>

<strong>
</strong>在保存 json 数据时使用 urlencode() 函数：
<div class="cnblogs_code">
<pre>$content = urlencode(json_encode($content));</pre>
</div>
解析时使用 urldecode() 函数：
<div class="cnblogs_code">
<pre>$content = urldecode($content);</pre>
</div>
即可避免反斜杠转义造成的无法解析。

</div>
</div>