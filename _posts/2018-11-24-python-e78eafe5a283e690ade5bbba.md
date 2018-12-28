---
ID: 1459
post_title: Python 环境搭建
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2018/11/24/python-%e7%8e%af%e5%a2%83%e6%90%ad%e5%bb%ba/'
published: true
post_date: 2018-11-24 09:50:11
---
http://www.runoob.com/python/python-install.html
<h1>Python <span class="color_h1">环境搭建</span></h1>
<div class="tutintro">

本章节我们将向大家介绍如何在本地搭建Python开发环境。

Python可应用于多平台包括 Linux 和 Mac OS X。

你可以通过终端窗口输入 "python" 命令来查看本地是否已经安装Python以及Python的安装版本。

</div>
<ul>
 	<li>Unix (Solaris, Linux, FreeBSD, AIX, HP/UX, SunOS, IRIX, 等等。)</li>
 	<li>Win 9x/NT/2000</li>
 	<li>Macintosh (Intel, PPC, 68K)</li>
 	<li>OS/2</li>
 	<li>DOS (多个DOS版本)</li>
 	<li>PalmOS</li>
 	<li>Nokia 移动手机</li>
 	<li>Windows CE</li>
 	<li>Acorn/RISC OS</li>
 	<li>BeOS</li>
 	<li>Amiga</li>
 	<li>VMS/OpenVMS</li>
 	<li>QNX</li>
 	<li>VxWorks</li>
 	<li>Psion</li>
 	<li>Python 同样可以移植到 Java 和 .NET 虚拟机上。</li>
</ul>
&nbsp;

<hr />

<h2>Python下载</h2>
Python最新源码，二进制文档，新闻资讯等可以在Python的官网查看到：

Python官网：<a href="https://www.python.org/" target="_blank" rel="nofollow noopener">https://www.python.org/</a>

你可以在以下链接中下载 Python 的文档，你可以下载 HTML、PDF 和 PostScript 等格式的文档。

Python文档下载地址：<a href="https://www.python.org/doc/" target="_blank" rel="nofollow noopener">https://www.python.org/doc/</a>

&nbsp;

<hr />

<h2>Python安装</h2>
Python已经被移植在许多平台上（经过改动使它能够工作在不同平台上）。

您需要下载适用于您使用平台的二进制代码，然后安装Python。

如果您平台的二进制代码是不可用的，你需要使用C编译器手动编译源代码。

编译的源代码，功能上有更多的选择性， 为python安装提供了更多的灵活性。

以下是各个平台安装包的下载地址：

<img src="http://www.runoob.com/wp-content/uploads/2013/11/DC24DD0C-08A2-4D61-8C6F-4CA1EEB23535.jpg" />

以下为不同平台上安装 Python 的方法：
<h3>Unix &amp; Linux 平台安装 Python:</h3>
以下为在 Unix &amp; Linux 平台上安装 Python 的简单步骤：
<ul class="list">
 	<li>打开 WEB 浏览器访问<a href="https://www.python.org/downloads/source/" target="_blank" rel="nofollow noopener">https://www.python.org/downloads/source/</a></li>
 	<li>选择适用 于Unix/Linux 的源码压缩包。</li>
 	<li>下载及解压压缩包。</li>
 	<li>如果你需要自定义一些选项修改<i>Modules/Setup</i></li>
 	<li><b>执行</b> ./configure 脚本</li>
 	<li>make</li>
 	<li>make install</li>
</ul>
执行以上操作后，Python 会安装在 /usr/local/bin 目录中，Python 库安装在 /usr/local/lib/pythonXX，XX 为你使用的 Python 的版本号。
<h3>Window 平台安装 Python:</h3>
以下为在 Window 平台上安装 Python 的简单步骤：
<ul class="list">
 	<li>打开 WEB 浏览器访问<a href="https://www.python.org/downloads/windows/" target="_blank" rel="nofollow noopener">https://www.python.org/downloads/windows/</a>

<img src="http://www.runoob.com/wp-content/uploads/2013/11/721E917D-CCA5-4F37-8FD6-486315EC8CF8.png" /></li>
 	<li>在下载列表中选择Window平台安装包，包格式为：<i>python-XYZ.msi</i> 文件 ， XYZ 为你要安装的版本号。</li>
 	<li>要使用安装程序 <i>python-XYZ.msi</i>, Windows 系统必须支持 Microsoft Installer 2.0 搭配使用。只要保存安装文件到本地计算机，然后运行它，看看你的机器支持 MSI。Windows XP 和更高版本已经有 MSI，很多老机器也可以安装 MSI。

<img src="http://www.runoob.com/wp-content/uploads/2013/11/20180711-160607.png" />

&lt;p根据自己电脑配置进行选择，以上选择的是 windows="" 64="" 位安装包。&lt;="" p=""&gt;&lt;/p根据自己电脑配置进行选择，以上选择的是&gt;</li>
 	<li>下载后，双击下载包，进入 Python 安装向导，安装非常简单，你只需要使用默认的设置一直点击"下一步"直到安装完成即可。</li>
</ul>
<h3>MAC 平台安装 Python:</h3>
MAC 系统一般都自带有 Python2.x版本 的环境，你也可以在链接 <a href="https://www.python.org/downloads/mac-osx/" target="_blank" rel="nofollow noopener">https://www.python.org/downloads/mac-osx/</a> 上下载最新版安装。

&nbsp;

<hr />

<h2>环境变量配置</h2>
程序和可执行文件可以在许多目录，而这些路径很可能不在操作系统提供可执行文件的搜索路径中。

path(路径)存储在环境变量中，这是由操作系统维护的一个命名的字符串。这些变量包含可用的命令行解释器和其他程序的信息。

Unix或Windows中路径变量为PATH（UNIX区分大小写，Windows不区分大小写）。

在Mac OS中，安装程序过程中改变了python的安装路径。如果你需要在其他目录引用Python，你必须在path中添加Python目录。
<h3>在 Unix/Linux 设置环境变量</h3>
<ul>
 	<li><b>在 csh shell:</b> 输入
<pre class="prettyprint prettyprinted"><span class="pln">setenv PATH </span><span class="str">"$PATH:/usr/local/bin/python"</span></pre>
, 按下"Enter"。</li>
 	<li><b>在 bash shell (Linux):</b> 输入
<pre class="prettyprint prettyprinted"><span class="kwd">export</span><span class="pln"> PATH</span><span class="pun">=</span><span class="str">"$PATH:/usr/local/bin/python"</span></pre>
，按下"Enter"。</li>
 	<li><b>在 sh 或者 ksh shell:</b> 输入
<pre class="prettyprint prettyprinted"><span class="pln">PATH</span><span class="pun">=</span><span class="str">"$PATH:/usr/local/bin/python"</span></pre>
, 按下"Enter"。</li>
</ul>
<strong>注意: </strong>/usr/local/bin/python 是 Python 的安装目录。
<h3>在 Windows 设置环境变量</h3>
在环境变量中添加Python目录：

<b>在命令提示框中(cmd) :</b> 输入
<pre class="prettyprint prettyprinted"><span class="pln">path</span><span class="pun">=%</span><span class="pln">path</span><span class="pun">%;</span><span class="pln">C</span><span class="pun">:</span><span class="pln">\Python </span></pre>
按下"Enter"。

<strong>注意: </strong>C:\Python 是Python的安装目录。

也可以通过以下方式设置：
<ul>
 	<li>右键点击"计算机"，然后点击"属性"</li>
 	<li>然后点击"高级系统设置"</li>
 	<li>选择"系统变量"窗口下面的"Path",双击即可！</li>
 	<li></li>
 	<li>然后在"Path"行，添加python安装路径即可(我的D:\Python32)，所以在后面，添加该路径即可。 <b>ps：记住，路径直接用分号"；"隔开！</b></li>
 	<li>最后设置成功以后，在cmd命令行，输入命令"python"，就可以有相关显示。</li>
</ul>
<img src="http://www.runoob.com/wp-content/uploads/2013/11/201209201707594792.png" />

<hr />

<h2>Python 环境变量</h2>
下面几个重要的环境变量，它应用于Python：
<table class="reference">
<tbody>
<tr>
<th>变量名</th>
<th>描述</th>
</tr>
<tr>
<td>PYTHONPATH</td>
<td>PYTHONPATH是Python搜索路径，默认我们import的模块都会从PYTHONPATH里面寻找。</td>
</tr>
<tr>
<td>PYTHONSTARTUP</td>
<td>Python启动后，先寻找PYTHONSTARTUP环境变量，然后执行此变量指定的文件中的代码。</td>
</tr>
<tr>
<td>PYTHONCASEOK</td>
<td>加入PYTHONCASEOK的环境变量, 就会使python导入模块的时候不区分大小写.</td>
</tr>
<tr>
<td>PYTHONHOME</td>
<td>另一种模块搜索路径。它通常内嵌于的PYTHONSTARTUP或PYTHONPATH目录中，使得两个模块库更容易切换。</td>
</tr>
</tbody>
</table>
&nbsp;

<hr />

<h2>运行Python</h2>
有三种方式可以运行Python：
<h3>1、交互式解释器：</h3>
你可以通过命令行窗口进入python并开在交互式解释器中开始编写Python代码。

你可以在Unix，DOS或任何其他提供了命令行或者shell的系统进行python编码工作。
<div class="code">
<div>$ python # Unix/Linux

或者

C:&gt;python # Windows/DOS</div>
</div>
以下为Python命令行参数：
<table class="reference">
<tbody>
<tr>
<th>选项</th>
<th>描述</th>
</tr>
<tr>
<td>-d</td>
<td>在解析时显示调试信息</td>
</tr>
<tr>
<td>-O</td>
<td>生成优化代码 ( .pyo 文件 )</td>
</tr>
<tr>
<td>-S</td>
<td>启动时不引入查找Python路径的位置</td>
</tr>
<tr>
<td>-V</td>
<td>输出Python版本号</td>
</tr>
<tr>
<td>-X</td>
<td>从 1.6版本之后基于内建的异常（仅仅用于字符串）已过时。</td>
</tr>
<tr>
<td>-c cmd</td>
<td>执行 Python 脚本，并将运行结果作为 cmd 字符串。</td>
</tr>
<tr>
<td>file</td>
<td>在给定的python文件执行python脚本。</td>
</tr>
</tbody>
</table>
<h3>2、命令行脚本</h3>
在你的应用程序中通过引入解释器可以在命令行中执行Python脚本，如下所示：
<div class="code">
<div>$ python script.py # Unix/Linux

或者

C:&gt;python script.py # Windows/DOS</div>
</div>
<strong>注意：</strong>在执行脚本时，请检查脚本是否有可执行权限。
<h3>3、集成开发环境（IDE：Integrated Development Environment）: PyCharm</h3>
PyCharm 是由 JetBrains 打造的一款 Python IDE，支持 macOS、 Windows、 Linux 系统。

PyCharm 功能 : 调试、语法高亮、Project管理、代码跳转、智能提示、自动完成、单元测试、版本控制……

PyCharm 下载地址 : <a href="https://www.jetbrains.com/pycharm/download/" target="_blank" rel="noopener">https://www.jetbrains.com/pycharm/download/</a>

PyCharm 安装地址：<a href="http://www.runoob.com/w3cnote/pycharm-windows-install.html" target="_blank" rel="noopener">http://www.runoob.com/w3cnote/pycharm-windows-install.html</a>

<img src="http://www.runoob.com/wp-content/uploads/2014/06/pycharm_ui_darcula.png" />

继续下一章之前，请确保您的环境已搭建成功。如果你不能够建立正确的环境，那么你就可以从您的系统管理员的帮助。

在以后的章节中给出的例子已在 Python2.7.6 版本测试通过。

<hr />

<h2 id="cs-python">在 Cloud Studio 中运行 Python 程序</h2>
<blockquote>因为 Python 是跨平台的，它可以运行在 Windows、Mac 和各种 Linux/Unix 系统上。在 Windows 上写 Python 程序，放到 Linux 上也是能够运行的。

要开始学习 Python 编程，首先就得把 Python 安装到你的电脑里。安装后，你会得到 Python 解释器（就是负责运行 Python 程序的），一个命令行交互环境，还有一个简单的集成开发环境。</blockquote>
或者推荐你使用 <a href="https://studio.dev.tencent.com/" target="_blank" rel="noopener">Coding Cloud Studio</a> 这款在线云端开发工具。它能提供原生的在线 Linux 命令交互终端环境，Python 运行解释器，在线开发文本编辑器，你可以直接在工作站中创建 Python 文件并在 Cloud Studio 中运行你写的 Python 程序。然后你可以略过本节余下的安装 Python 运行环境以及集成开发环境等部分。

<img src="http://static.runoob.com/images/ad/c2e566c7-7ebe-4266-b803-9d14c244c3b5.png" alt="图片" />

有任何疑问，可以查阅<a href="https://dev.tencent.com/help/doc/cloud-studio" target="_blank" rel="noopener">帮助文档</a>