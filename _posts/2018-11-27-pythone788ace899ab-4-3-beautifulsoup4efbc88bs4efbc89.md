---
ID: 1485
post_title: >
  Python爬虫——4-3.BeautifulSoup4（BS4）
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/27/python%e7%88%ac%e8%99%ab-4-3-beautifulsoup4%ef%bc%88bs4%ef%bc%89/'
published: true
post_date: 2018-11-27 18:40:58
---
<div class="article-header-box">
<div class="article-header">
<div class="article-title-box">
<h1 class="title-article">Python爬虫——4-3.BeautifulSoup4（BS4）</h1>
</div>
<div class="article-info-box">
<div class="operating">https://blog.csdn.net/liyahui_3163/article/details/79049434</div>
</div>
</div>
</div>
<article class="baidu_pl">
<div id="article_content" class="article_content clearfix csdn-tracking-statistics" data-pid="blog" data-mod="popu_307" data-dsm="post">
<div></div>
<div id="content_views" class="htmledit_views">对于HTML/XML数据的筛选，BeautifulSoup也是比较常用且使用简单的技术，BeautifulSoup是一种非常优雅的专门用于进行HTML/XML数据解析的一种描述语言，可以很好的分析和筛选HTML/XML这样的标记文档中的指定规则数据
在数据筛选过程中其基础技术是通过封装HTML DOM树实现的一种DOM操作，通过加载网页文档对象的形式，从文档对象树模型中获取目标数据
BeautifulSoup操作简单易于上手，在很多对于数据筛选性能要求并不是特别苛刻的项目中经常使用，目前市场流行的操作版本是BeautifulSoup4，经常称BS4
<h1><a name="t0"></a><strong>一、Xpath和BeautifulSoup4</strong></h1>
Xpath和BeautifulSoup都是基于DOM的一种操作模式
不同点在于加载文档对象模型DOM时出现的文档节点遍历查询操作过程，Xpath在进行遍历操作时针对描述语言指定的语法结构进行局部DOM对象树的遍历得到具体的数据，但是BS4在操作过程中，会将整个文档树进行加载然后进行查询匹配操作，使用过程中消耗资源较多，处理性能相对Xpath较低
那么为什么要用BS4呢？因为，它，足够简单!
<strong>描述语言 | 处理效率 | 上手程度</strong>
正则表达式 | 效率非常高 | 困难
Xpath | 效率很高 | 正常
BS4 | 效率较高| 简单
BS4本身是一种对描述语言进行封装的函数操作模块，通过提供面向对象的操作方式将文档对象中的各种节点、标签、属性、内容等等都封装成了python中对象的属性，在查询操作过程中，通过调用指定的函数直接进行数据 匹配检索操作，非常的简单非常的灵活。
一般BS4将HTML文档对象会转换成如下四种类型组合的文档树
* Tag：标签对象
* NavigableString：字符内容操作对象
* BeautifulSoup：文档对象
* Comment：特殊类型的NavigableString


说道这里，其实都是太多的理论性语法，BS4不同于正则和Xpath，没有什么基础语法结构，它封装的对象以及对象的属性操作，才是BS4不同凡响的核心价值
let's 上干货
<h1><a name="t1"></a>二.、python操作BeautifulSoup4</h1>
python中对于BeautifulSoup的支持，通过安装第三方模块来发挥它最好的操作
```
$ pip install beautifulsoup4
<h2><a name="t2"></a><strong>1.入门第一弹：了解BeautifulSoup4</strong></h2>
```
# coding:utf-8
# 引入解析模块BS4
from bs4 import BeautifulSoup


# 从文件中加载html网页，指定HTML解析器使用lxml
# 默认不指定的情况下，BS4会自动匹配当前系统中最优先的解析器
soup = BeautifulSoup(open("index.html"), "lxml")
# 如果是爬虫获取到的字符数据，直接交给BS4就OK拉
# soup = BeatufulSoup(spider_content, "lxml")


# 打印BeautifulSoup文档对象，得到的是文档树内容
print(soup)
# 打印类型：&lt;class 'bs4.BeautifulSoup'&gt;
print(type(soup))
```
<h2><a name="t3"></a>2.入门第二弹:操作标签、属性、内容</h2>
```
<pre># coding:utf-8


from bs4 import BeautifulSoup


# 得到构建的文档对象
soup = BeautifulSoup(open("index.html"), "lxml")


# Tag操作
# 1. 获取标签
print(soup.title) # &lt;title&gt;文章标题&lt;/title&gt;
print(soup.p) # &lt;p&gt;姓名：&lt;span id="name"&gt;大牧&lt;/span&gt;&lt;/p&gt; # 只返回第一个匹配到的标签对象
print(soup.span) # &lt;span id="name"&gt;大牧&lt;/span&gt;


# 2.获取标签的属性
print(soup.p.attrs) # {}：得到属性和值的字典
print(soup.span.attrs) # {'id': 'name'}：得到属性和值的字典
print(soup.span['id']) # name：得到指定属性的值
soup.span['id'] = "real_name"
print(soup.span['id']) # real_name : 可以方便的在BS4中直接对文档进行修改


# 3. 获取标签的内容
print(soup.head.string) # 文章标题：如果标签中只有一个子标签~返回子标签中的文本内容
print(soup.p.string) # None：如果标签中有多个子标签，返回None
print(soup.span.string) # 大牧：直接返回包含的文本内容</pre>
<h2></h2>
<h6>```</h6>
<h2><a name="t5"></a>3.入门第三弹：操作子节点```</h2>
<pre># coding:utf-8
# 引入BS4操作模块
from bs4 import BeautifulSoup

# 加载网页文档，构建文档对象
soup = BeautifulSoup(open("index.html"), "lxml")

print(dir(soup))

print(soup.contents)# 得到文档对象中所有子节点
print(soup.div.contents)# 得到匹配到的第一个div的子节点列表
print(soup.div.children)# 得到匹配到的第一个div的子节点列表迭代器
# for e1 in soup.div.children:
#     print("--&gt;", e1)
print(soup.div.descendants)# 得到匹配到的第一个div的子节点迭代器,所有后代节点单独一个一个列出
# for e2 in soup.div.descendants:
#     print("==&gt;", e2)</pre>
```
<h2><a name="t6"></a>4.入门第四弹: 面向对象的DOM匹配</h2>
```
<pre># coding:utf-8
# 引入BS4模块
from bs4 import BeautifulSoup

# 加载文档对象
soup = BeautifulSoup(open("../index.html"), "lxml")

# DOM文档树查询
# 核心函数~请对比javasript dom结构了解它的方法
# 如:findAllPrevious()/findAllNext()/findAll()/findPrevious()/findNext()等等
# findAll()为例
# 1. 查询指定的字符串
res1 = soup.findAll("p")# 查询所有包含p字符的标签
print(res1)

# 2. 正则表达式
import re
res2 = soup.findAll(re.compile(r"d+"))# 查询所有包含d字符的标签
print(res2)

# 3. 列表：选择
res3 = soup.findAll(["div", "h1"])# 查询所有的div或者h1标签
print(res3)

# 4. 关键字参数
res4 = soup.findAll(id="name")# 查询属性为id="name"的标签
print(res4)

# 5. 内容匹配
res5 = soup.findAll(text=u"男")# 直接匹配内容中的字符，必须保证精确匹配
print(res5)
res6 = soup.findAll(text=[u"文章标题", u"大牧"])# 查询包含精确内容的所有的标签
print(res6)
res7 = soup.findAll(text=re.compile(u"大+"))# 通过正则表达式进行模糊匹配
print(res7)</pre>
```
<h2><a name="t7"></a>5.入门第五弹: 又见CSS</h2>
```
<pre># coding:utf-8

# 引入BS模块
from bs4 import BeautifulSoup

# 加载网页构建文档对象
soup = BeautifulSoup(open("index.html"), "lxml")

# 1. CSS 标签选择器：根据标签名称查询标签对象
res1 = soup.select("span")
print(res1)

# 2. CSS ID选择器：根据ID查询标签对象
res2 = soup.select("#gender")
print(res2)

# 3. CSS 类选择器：根据class属性查询标签对象
res3 = soup.select(".intro")
print(res3)

# 4. CSS 属性选择器
res41 = soup.select("span[id]")
print(res41)
res42 = soup.select("span[id='gender']")
print(res42)

# 5. CSS 包含选择器
res5 = soup.select("p span#name")
print(res5)

# 6. 得到标签内容
res6 = soup.select("p &gt; span.intro")
print(res6[0].string)
print(res6[0].getText())</pre>
```</div>
</div>
</article>