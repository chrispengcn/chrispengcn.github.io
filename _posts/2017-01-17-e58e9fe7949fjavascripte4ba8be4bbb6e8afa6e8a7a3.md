---
ID: 748
post_title: 原生JavaScript事件详解
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/01/17/%e5%8e%9f%e7%94%9fjavascript%e4%ba%8b%e4%bb%b6%e8%af%a6%e8%a7%a3/'
published: true
post_date: 2017-01-17 10:49:36
---
<p>http://www.cnblogs.com/iyangyuan/p/4190773.html
 原生JavaScript事件详解</p>

<pre><code> JQuery这种Write Less Do More的框架，用多了难免会对原生js眼高手低。

 小菜其实不想写这篇博客，貌似很初级的样子，但是看到网络上连原生js事件绑定和解除都说不明白，还是决定科普一下了。

 首先声明，小菜懂的也不是很多，只是把我的思路和大家分享一下。
</code></pre>

<p>DOM0事件模型</p>

<pre><code> 事件模型在不断发展，早期的事件模型称为DOM0级别。

 DOM0事件模型，所有的浏览器都支持。

 直接在dom对象上注册事件名称，就是DOM0写法，比如：
</code></pre>

<p>1 document.getElementById("test").onclick = function(e){};</p>

<pre><code> 意思就是注册一个onclick事件。当然，它和这种写法是一个意思：
</code></pre>

<p>1 document.getElementById("test")["onmousemove"] = function(e){};</p>

<pre><code> 这没什么，只不过是两种访问js对象属性的方法，[]的形式主要是为了解决属性名不是合法的标识符，比如：object.123肯定报错，但是object["123"]就避免了这个问题，与此同时，[]的写法，也把js写活了，用字符串表示属性名称，可以在运行时动态绑定事件。

 言归正传，事件被触发时，会默认传入一个参数e，表示事件对象，通过e，我们可以获取很多有用的信息，比如点击的坐标、具体触发该事件的dom元素等等。

 基于DOM0的事件，对于同一个dom节点而言，只能注册一个，后边注册的同种事件会覆盖之前注册的。例如：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
2 
3 btn.onmousemove = function(e){
4   alert("ok");
5 };
6 
7 btn["onmousemove"] = function(e){
8   alert("ok1");
9 };</p>

<p>复制代码</p>

<pre><code> 结果会输出ok1。

 接下来再说说this。事件触发时，this就是指该事件在哪个dom对象上触发。例如：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
2 
3 btn.onmousemove = function(e){
4   alert(this.id);
5 };</p>

<p>复制代码</p>

<pre><code> 结果输出test。因为事件就是在id为test的dom节点上注册的，事件触发时，this当然代表这个dom节点，可以理解为事件是被这个dom节点调用的。

 所以，想解除事件就相当简单了，只需要再注册一次事件，把值设成null，例如：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
2 
3 btn.onclick = function(e){
4   alert("ok");
5 };
6 
7 btn.onclick = null;</p>

<p>复制代码</p>

<pre><code> 原理就是最后注册的事件要覆盖之前的，最后一次注册事件设置成null，也就解除了事件绑定。

 事情还没结束，DOM0事件模型还涉及到直接写在html中的事件。例如：
</code></pre>

<p>1</p>

<div id="test" class="test" onclick="exec();" ></div>

<pre><code> 通过这种方式注册的事件，同样遵循覆盖原则，同样只能注册一个，最后一个生效。

 区别就是，这样注册的事件，相当于动态调用函数(有点eval的意思)，因此不会传入event对象，同时，this指向的是window，不再是触发事件的dom对象。
</code></pre>

<p>DOM2事件模型</p>

<pre><code> DOM2事件模型相对于DOM0，小菜仅仅了解如下两点：



      ·  DOM2支持同一dom元素注册多个同种事件。

      ·  DOM2新增了捕获和冒泡的概念。



 DOM2事件通过addEventListener和removeEventListener管理，当然，这是标准。

 但IE8及其以下版本浏览器，自娱自乐，搞出了对应的attachEvent和detachEvent，由于小菜才疏学浅，本文不做讨论。

 addEventListener当然就是注册事件，她有三个参数，分别为："事件名称", "事件回调", "捕获/冒泡"。举个例子：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
2 
3 btn.addEventListener("click", function(e){
4   alert("ok");
5 }, false);</p>

<p>复制代码</p>

<pre><code> 事件名称就不用多说了，相比DOM0，去掉了前边的on而已。

 事件回调也很好理解，事件触发了总得通知你吧！回调时和DOM0一样，也会默认传入一个event参数，同时this是指触发该事件的dom节点。

 最后一个参数是布尔型，true代表捕获事件，false代表冒泡事件。其实很好理解，先来个示意图：





 意思就是说，某个元素触发了某个事件，最先得到通知的是window，然后是document，依次而入，直到真正触发事件的那个元素(目标元素)为止，这个过程就是捕获。接下来，事件会从目标元素开始起泡，再依次而出，直到window对象为止，这个过程就是冒泡。

 为什么要这样设计呢？这貌似是由于深厚的历史渊源，小菜也不怎么了解，就不乱说了。

 由此可以看出，捕获事件要比冒泡事件先触发。

 假设有这样的html结构：
</code></pre>

<p>1</p>

<div id="test" class="test">
2   <div id="testInner" class="test-inner"></div>
3 </div>

<pre><code> 然后我们在外层div上注册两个click事件，分别是捕获事件和冒泡事件，代码如下：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
 2 
 3 //捕获事件
 4 btn.addEventListener("click", function(e){
 5   alert("ok1");
 6 }, true);
 7 
 8 //冒泡事件
 9 btn.addEventListener("click", function(e){
10   alert("ok");
11 }, false);</p>

<p>复制代码</p>

<pre><code> 最后，点击内层的div，先弹出ok1，后弹出ok。结合上边的原理图，外层div相当于图中的body，内层div相当于图中最下边的div，证明了捕获事件先执行，然后执行冒泡事件。

 为什么要强调点击内层的div呢？因为真正触发事件的dom元素，必须是内层的，外层dom元素才有机会模拟捕获事件和冒泡事件，从原理图上就看出了。

 如果在真正触发事件的dom元素上注册捕获事件和冒泡事件呢？

 html结构同上，js代码如下：
</code></pre>

<p>复制代码</p>

<p>1 var btnInner = document.getElementById("testInner");
 2 
 3 //冒泡事件
 4 btnInner.addEventListener("click", function(e){
 5   alert("ok");
 6 }, false);
 7 
 8 //捕获事件
 9 btnInner.addEventListener("click", function(e){
10   alert("ok1");
11 }, true);</p>

<p>复制代码</p>

<pre><code> 当然还是点击内层div，结果是先弹出ok，再弹出ok1。理论上应该先触发捕获事件，也就是先弹出ok1，但是这里比较特殊，因为我们是在真正触发事件的dom元素上注册的事件，相当于在图中的div上注册，由图可以看出真正触发事件的dom元素，是捕获事件的终点，是冒泡事件的起点，所以这里就不区分事件了，哪个先注册，就先执行哪个。本例中，冒泡事件先注册，所以先执行。

 这个道理适用于多个同种事件，比如说一下子注册了3个冒泡事件，那么执行顺序就按照注册的顺序来，先注册先执行。例如：
</code></pre>

<p>复制代码</p>

<p>1 var btnInner = document.getElementById("testInner");
 2 
 3 btnInner.addEventListener("click", function(e){
 4   alert("ok");
 5 }, false);
 6 
 7 btnInner.addEventListener("click", function(e){
 8   alert("ok1");
 9 }, false);
10 
11 btnInner.addEventListener("click", function(e){
12   alert("ok2");
13 }, false);</p>

<p>复制代码</p>

<pre><code> 结果当然是依次弹出ok、ok1、ok2。

 为了进一步理解事件模型，还有一种场景，假如说外层div和内层div同时注册了捕获事件，那么点击内层div时，外层div的事件一定是先触发的，代码如下：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
 2 var btnInner = document.getElementById("testInner");
 3 
 4 btnInner.addEventListener("click", function(e){
 5   alert("ok");
 6 }, true);
 7 
 8 btn.addEventListener("click", function(e){
 9   alert("ok1");
10 }, true);</p>

<p>复制代码</p>

<pre><code> 结果是先弹出ok1。

 假如外层div和内层div都是注册的冒泡事件，点击内层div时，一定是内层div事件先执行，原理相同。

 细心的读者会发现，对于div嵌套的情况，如果点击内层的div，外层的div也会触发事件，这貌似会有问题！

 点击的明明是内层div，但是外层div的事件也触发了，这的确是个问题。

 其实，事件触发时，会默认传入一个event对象，前边提过了，这个event对象上有一个方法：stopPropagation，通过此方法，可以阻止冒泡，这样外层div就接收不到事件了。代码如下：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
 2 var btnInner = document.getElementById("testInner");
 3 
 4 btn.addEventListener("click", function(e){
 5   alert("ok1");
 6 }, false);
 7 
 8 btnInner.addEventListener("click", function(e){
 9   //阻止冒泡
10 e.stopPropagation();
11   alert("ok");
12 }, false);</p>

<p>复制代码</p>

<pre><code> 终于要说说怎么解除事件了。解除事件语法：btn.removeEventListener("事件名称", "事件回调", "捕获/冒泡");

 这和绑定事件的参数一样，详细说明下：



      ·  事件名称，就是说解除哪个事件呗。

      ·  事件回调，是一个函数，这个函数必须和注册事件的函数是同一个。

      ·  事件类型，布尔值，这个必须和注册事件时的类型一致。



 也就是说，名称、回调、类型，三者共同决定解除哪个事件，缺一不可。举个例子：
</code></pre>

<p>复制代码</p>

<p>1 var btn = document.getElementById("test");
 2 //将回调存储在变量中
 3 var fn = function(e){
 4   alert("ok");
 5 };
 6 //绑定
 7 btn.addEventListener("click", fn, false);
 8 
 9 //解除
10 btn.removeEventListener("click", fn, false);</p>

<p>复制代码</p>

<pre><code> 要想注册过的事件能够被解除，必须将回调函数保存起来，否则无法解除。
</code></pre>

<p>DOM0与DOM2混用</p>

<pre><code> 事情本来就很乱了，这又来个混合使用，还让不让人活了。。。

 别怕，混合使用完全没问题，DOM0模型和DOM2模型各自遵循自己的规则，互不影响。

 整体上来说，依然是哪个先注册，哪个先执行，其他就没什么了。
</code></pre>

<p>后记</p>

<pre><code> 至此，原生js事件已经讲的差不多了，小菜仅仅知道这些而已，欢迎读者补充其他知识点。

 在实际应用中，真正的行家不会傻傻的真的注册这么多事件，一般情况下，只需在最外层dom元素注册一次事件，然后通过捕获、冒泡机制去找到真正触发事件的dom元素，最后根据触发事件的dom元素提供的信息去调用回调。

 也就是说，行家会自己管理事件，而不依赖浏览器去管理，这样即可以提高效率，又保证了兼容性，JQuery不就是这么做的嘛~

 好了，教程到此结束，希望对读者有所帮助！

 手抽筋中。。。
</code></pre>