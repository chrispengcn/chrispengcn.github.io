---
ID: 1465
post_title: >
  Python获取网页指定内容(BeautifulSoup工具的使用方法)
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/24/python%e8%8e%b7%e5%8f%96%e7%bd%91%e9%a1%b5%e6%8c%87%e5%ae%9a%e5%86%85%e5%ae%b9beautifulsoup%e5%b7%a5%e5%85%b7%e7%9a%84%e4%bd%bf%e7%94%a8%e6%96%b9%e6%b3%95/'
published: true
post_date: 2018-11-24 10:54:38
---
<h1 class="postTitle"><a id="cb_post_title_url" class="postTitle2" href="https://www.cnblogs.com/xisheng/p/9130156.html">Python获取网页指定内容(BeautifulSoup工具的使用方法)</a></h1>
<div class="clear"></div>
<div class="postBody">
<div id="cnblogs_post_body" class="blogpost-body">

Python用做数据处理还是相当不错的，如果你想要做爬虫，Python是很好的选择，它有很多已经写好的类包，只要调用，即可完成很多复杂的功能，此文中所有的功能都是基于BeautifulSoup这个包。

1 Pyhton获取网页的内容(也就是源代码)

&nbsp;
<div class="dp-highlighter bg_python">
<div class="bar">
<div class="tools">
<div></div>
</div>
</div>
<div class="cnblogs_code">
<pre>page = urllib2.urlopen(url)   
contents = page.read()   
#获得了整个网页的内容也就是源代码  
print(contents)</pre>
</div>
&nbsp;

</div>
url代表网址，contents代表网址所对应的源代码，urllib2是需要用到的包，以上三句代码就能获得网页的整个源代码

2 获取网页中想要的内容(先要获得网页源代码，再分析网页源代码，找所对应的标签，然后提取出标签中的内容)

2.1 以豆瓣电影排名为例子

网址是http://movie.douban.com/top250?format=text，进入网址后就出现如下的图

<img src="https://img-blog.csdn.net/20160715203512986" alt="" />

现在我需要获得当前页面的所有电影的名字，评分，评价人数，链接

<img src="https://img-blog.csdn.net/20160715204259241" alt="" />

由上图画红色圆圈的是我想得到的内容，画蓝色横线的为所对应的标签，这样就分析完了，现在就是写代码实现，Python提供了很多种方法去获得想要的内容，在此我使用BeautifulSoup来实现，非常的简单

&nbsp;
<div class="dp-highlighter bg_python">
<div class="bar">
<div class="tools">
<div>
<div class="cnblogs_code">
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
<pre>#coding:utf-8  
''''' 
@author: jsjxy 
'''  
import urllib2   
import re   
from bs4 import BeautifulSoup  
from distutils.filelist import findall  
  
  
  
page = urllib2.urlopen('http://movie.douban.com/top250?format=text')   
contents = page.read()   
 #print(contents)  
soup = BeautifulSoup(contents,"html.parser")  
print("豆瓣电影TOP250" + "\n" +" 影片名              评分       评价人数     链接 ")    
for tag in soup.find_all('div', class_='info'):    
   # print tag  
    m_name = tag.find('span', class_='title').get_text()        
    m_rating_score = float(tag.find('span',class_='rating_num').get_text())          
    m_people = tag.find('div',class_="star")  
    m_span = m_people.findAll('span')  
    m_peoplecount = m_span[3].contents[0]  
    m_url=tag.find('a').get('href')  
    print( m_name+"        "  +  str(m_rating_score)   + "           " + m_peoplecount + "    " + m_url )</pre>
<div class="cnblogs_code_toolbar"><span class="cnblogs_code_copy"><a title="复制代码"><img src="https://common.cnblogs.com/images/copycode.gif" alt="复制代码" /></a></span></div>
</div>
&nbsp;

</div>
</div>
</div>
</div>
控制台输出,你也可以写入文件中

<img src="https://img-blog.csdn.net/20160715213331107" alt="" />
前三行代码获得整个网页的源代码，之后开始使用BeautifulSoup进行标签分析，find_all方法是找到所有此标签的内容，然后在在此标签中继续寻找，如果标签有特殊的属性声明则一步就能找出来，如果没有特殊的属性声明就像此图中的评价人数前面的标签只有一个‘span’那么就找到所有的span标签，按顺序从中选相对应的，在此图中是第三个，所以这种方法可以找特定行或列的内容。代码比较简单，很容易就实现了，如果有什么地方不对，还请大家指出，大家共同学习。

源代码地址：http://download.csdn.net/detail/danielntz/9577390

&nbsp;

&nbsp;

转自：https://blog.csdn.net/danielntz/article/details/51861168

</div>
</div>