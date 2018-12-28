---
ID: 1472
post_title: 'Python爬虫入坑笔记 &#8211; XPath快速定位网页内容'
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/26/python%e7%88%ac%e8%99%ab%e5%85%a5%e5%9d%91%e7%ac%94%e8%ae%b0-xpath%e5%bf%ab%e9%80%9f%e5%ae%9a%e4%bd%8d%e7%bd%91%e9%a1%b5%e5%86%85%e5%ae%b9/'
published: true
post_date: 2018-11-26 14:44:35
---
<h1 class="title">Python爬虫入坑笔记 - XPath快速定位网页内容</h1>
<div class="show-content" data-note-content="">
<div class="show-content-free">

XPATH语句可以用来快速定位一个XML文本中的内容,当然也可以是HTML文本，这里我们使用lxml库来解析，达到快速批量获取网页相似内容的功能
<h4>安装</h4>
<pre class="hljs ruby"><code class="ruby">$ pip install lxml
</code></pre>
<h4>基本使用</h4>
假设匹配出网页所含所有图片的链接
<pre class="hljs python"><code class="python"><span class="hljs-keyword">from</span> lxml <span class="hljs-keyword">import</span> etree
<span class="hljs-keyword">import</span> requests

html = requests.get(<span class="hljs-string">'http://www.lzu.edu.cn'</span>).content.decode(<span class="hljs-string">'utf-8'</span>)
<span class="hljs-comment">##获取网页代码</span>

dom_tree = etree.HTML(html)

<span class="hljs-comment">###XPath匹配</span>
links = doc_tree.xpath(<span class="hljs-string">'//img/@src'</span>)

<span class="hljs-keyword">for</span> i <span class="hljs-keyword">in</span> links:
    print(i)

<span class="hljs-comment">##将会输出网页中所有图片标签的链接</span>
</code></pre>
<h3>XPath语法讲解</h3>
我们假设有如下网页
<pre class="hljs xml"><code class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">html</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">head</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">title</span>&gt;</span>这是标题<span class="hljs-tag">&lt;/<span class="hljs-name">title</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">head</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"cn_search_engine"</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"http://www.baidu.com"</span>&gt;</span>百度<span class="hljs-tag">&lt;/<span class="hljs-name">a</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
        <span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">class</span>=<span class="hljs-string">"other_search_engine"</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"http://www.bing.com"</span>&gt;</span>Bing<span class="hljs-tag">&lt;/<span class="hljs-name">a</span>&gt;</span>
            <span class="hljs-tag">&lt;<span class="hljs-name">a</span> <span class="hljs-attr">href</span>=<span class="hljs-string">"http://www.google.com"</span>&gt;</span>Google<span class="hljs-tag">&lt;/<span class="hljs-name">a</span>&gt;</span>
        <span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
    <span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">html</span>&gt;</span>
</code></pre>
如上，HTML标签是一个树形结构，我们称这个为DOM树，xpath匹配出其中的元素，相当于在树中查找子节点啊，因此效率相对正则表达式要高
<ul>
 	<li>"//"和"/"的区别
两者都用来表示一个节点的路径，不同节点名用“/”分开
//代表相对路径，匹配可以是任意深度的节点
/ 代表绝对路径，故对于网页来说，匹配从/html开始</li>
</ul>
如同样是解析上述数据中的所有的链接，下面两语句等价
<pre class="hljs bash"><code class="bash">....
<span class="hljs-comment">##省略若干代码，dom_tree为我们解析之后的etree对象</span>

<span class="hljs-comment">##语句一:</span>
dom_tree.xpath(<span class="hljs-string">'/html/body/div/a/@href'</span>)

<span class="hljs-comment">##语句二：</span>
dom_tree.xpath(<span class="hljs-string">'//div/a/@href'</span>)
</code></pre>
<ul>
 	<li>获取元素属性和文字的区别</li>
</ul>
<pre class="hljs bash"><code class="bash">dom_tree.xpath(<span class="hljs-string">'//div/a/@href'</span>)
<span class="hljs-comment">#将返回所有的链接网址</span>

dom_tree.xpath(<span class="hljs-string">'//div/a/text()'</span>)
<span class="hljs-comment">#将获取所有链接的名称</span>

</code></pre>
</div>
</div>