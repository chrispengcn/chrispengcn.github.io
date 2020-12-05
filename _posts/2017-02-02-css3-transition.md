---
ID: 770
post_title: >
  css3实现hover颜色，背景色，宽度等平滑变动（transition）
author: chrispengcn
post_excerpt: >
  CSS 3
  中提供了一种属性，Transition（变换），这种属性能够实现在元素的某些属性的数值发生改变时产生过渡的效果。比如长度增加，能产生类似拉长的动画效果；颜色改变时，也可以利用Transition产生一种颜色渐变的效果。
layout: post
permalink: >
  https://vpseo.com/2017/02/02/css3-transition/
published: true
post_date: 2017-02-02 10:28:42
---
<div id="article_content" class="article_content">
<div>
<div>
<div>演示：http://codepen.io/linxiflash/pen/QdmqdO</div>
<div></div>
<div>
<pre>&lt;!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd"&gt;
&lt;html xmlns="http://www.w3.org/1999/xhtml"&gt;
&lt;head&gt;
&lt;meta http-equiv="Content-Type" content="text/html; charset=gb2312" /&gt;
&lt;title&gt;CSS3.0平滑变动&lt;/title&gt;
&lt;style type="text/css"&gt;
li
{
float:left;
border:1px #FFFFFF solid;
text-align:center;
}
li a
{
-moz-transition: all 1s ease 0s;/*根据hover进行变化*/
color:#FFFFFF;
float:left;
text-shadow:#FFFF00 0 -1px 2px;
width:150px;
background:#000000;
height:150px;
line-height:150px;
text-decoration:none;
}
li a:hover
{
color:#000000;
background:#FFFFFF;
font-size:36px;
text-shadow:0 -1px 2px #737373;
}
ul
{
list-style-type:none;
margin:0px;
padding:0 0 0 5px;
}
&lt;/style&gt;
&lt;/head&gt;


&lt;body&gt;
&lt;ul&gt;
&lt;li&gt;&lt;a href="#"&gt;A&lt;/a&gt;&lt;/li&gt;
&lt;li&gt;&lt;a href="#"&gt;B&lt;/a&gt;&lt;/li&gt;
&lt;li&gt;&lt;a href="#"&gt;C&lt;/a&gt;&lt;/li&gt;
&lt;li&gt;&lt;a href="#"&gt;D&lt;/a&gt;&lt;/li&gt;
&lt;/ul&gt;
&lt;/body&gt;
&lt;/html&gt;</pre>
</div>
</div>
</div>
<div>=====================================================</div>
<div>
<div>CSS 3 中提供了一种属性，Transition（变换），这种属性能够实现在元素的某些属性的数值发生改变时产生过渡的效果。比如长度增加，能产生类似拉长的动画效果；颜色改变时，也可以利用Transition产生一种颜色渐变的效果。</div>
<div>浏览器支持情况</div>
<div></div>
<div>CSS Transition受到浏览器的广泛支持。CSS3 Transition Browser Support</div>
<div></div>
<div>    Chrome 2+(-webkit-transition)</div>
<div>    Firefox3.7+(-moz-transition)</div>
<div>    Safari 3.1+(-webkit-transition)</div>
<div>    Opera 10.5+(-o-transition)</div>
<div></div>
<div>    From:axiu.me</div>
<div></div>
<div>不过经过我的观察，现在IE还是不能支持，即使是在IE9中。不过也没有关系，至少并不会出现什么令人感觉糟糕的结果。</div>
<div>CSS变换的由来</div>
<div></div>
<div>CSS Transition曾经是只属于Apple Safari Webkit的东西，仅能由Safari UI实现的动画效果。</div>
<div></div>
<div>可是W3C有部分工作人员认为变换动画是脚本该做的事情而不是CSS，不过去年三月份，来自Apple、Mozilla开始将CSS变换模块添加到CSS 3特性里面，非常接近原来Safari Webkit的效果。由此得来CSS Transition。</div>
<div>书写方法</div>
<div></div>
<div>在CSS代码中，要使用Transition属性需要这么书写：</div>
<div></div>
<div>    -moz-transition: // Firefox</div>
<div></div>
<div>    -webkit-transition: // Safari、Chrome</div>
<div></div>
<div>    -o-transition: // Opera</div>
<div></div>
<div>    transition: //官方标准</div>
<div></div>
<div>建议在书写时，将上述全写上。</div>
</div>
<div>=====================================================</div>
<div>

<strong>Transition对应的CSS属性列表</strong>：
<blockquote><em>transition-property</em>
<strong>*</strong> 指定当元素哪个属性改变时执行Transition效果，属性可以时以下属性：none、all以及其他可以触发浏览器reflow或repaint的属性。
* 当指定为none时，动画立即停止，当指定为all的时候，则当元素产生任何属性值变化时都将执行“转换”效果，比如大小改变。
* 初始默认值为all.
<em>transition-duration</em>
* 指定“转换”过程的时间，如：1s、none。
* 初始默认值为0.
<em>transition-timing-function</em>
* 指定“转换”过程中可用的效果，预设的有：ease, linear, ease-in, ease-out, ease-in-out, cubic-bezier(x1, y1, x2, y2)，默认值时easy。
* cubic-bezier为通过贝塞尔曲线来计算“转换”过程中的属性值，如下曲线所示，通过改变P1(x1, y1)和P2(x2, y2)的坐标可以改变整个过程的Output Percentage。

* 初始默认值为default.
<em>transition-delay</em>
* 指定一个动画开始执行的时间，即当改变元素属性值后多长时间开始执行“转换效果”
* 初始默认值为0.</blockquote>
</div>
</div>