---
ID: 3811
post_title: >
  Meta
  Box：一个被名字耽误的强大的WordPress自定义字段插件
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'https://vpseo.com/2020/12/07/meta-box%ef%bc%9a%e4%b8%80%e4%b8%aa%e8%a2%ab%e5%90%8d%e5%ad%97%e8%80%bd%e8%af%af%e7%9a%84%e5%bc%ba%e5%a4%a7%e7%9a%84wordpress%e8%87%aa%e5%ae%9a%e4%b9%89%e5%ad%97%e6%ae%b5%e6%8f%92%e4%bb%b6/'
published: true
post_date: 2020-12-07 20:28:04
---
<div class="col-2-article-header">
<div class="article-infos-wrap">
<div class="article-infos-extra">meta这个词根本身在英语中的意思就有很多，计算机科学中的翻译，meta通常翻译成元：</div>
</div>
</div>
<div class="com-markdown-collpase">
<div class="com-markdown-collpase-main">
<div class="rno-markdown J-articleContent">

meta-data：data about data, 元数据

meta model：model about model，元模型
<blockquote>meta-合成词代表的东西可以归结为某种额外之物，伴随着原始对象的更高需求而产生 典型的HTML meta标签是HTML文档的补充（加强） meta-programming（元编程）在做的事是提升编程语言的能力 统计学中的meta-analysis ，对数据事后的量化分析 <em>作者：幽灵代笔 来源：</em><a href="https://www.zhihu.com/question/47196881/answer/105343764" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">知乎</a></blockquote>
<h2 id="WordPress%E4%B8%AD%E7%9A%84Meta-Box%E6%98%AF%E4%BB%80%E4%B9%88%EF%BC%9F">WordPress中的Meta Box是什么？</h2>
<blockquote>When a user edits a post, the edit screen is composed of several default boxes: Editor, Publish, Categories, Tags, etc. These boxes are meta boxes. Plugins can add custom meta boxes to an edit screen of any post type.</blockquote>
根据官网的介绍，meta box是WordPress后台编辑界面上的一些功能框，比如编辑器、发布按钮、目录/Tag选择框等等都叫做meta box。总体来说，meta box就是WorPress管理后台界面的一部分，有自己的信息区域，可以接收用户输入。插件和主题可以通过使用add_meta_box()函数可以在 WordPress 后台的编辑区加入自定义meta box。

add_meta_box 是 WordPress 2.5版本加入的一个高级函数。能用到它，说明你现在正在折腾一个你自己的主题、插件，甚至是在折腾 WordPress 后台了，是一个高阶的WordPress玩家了。虽然说可以通过主题直接使用add_meta_box函数，但是更多的情况下还是在插件中来使用，比如注明的WordPress超级自定义字段插件：Advanced Custom Fields 。在之前的文章中也有介绍过这个插件的使用案例：<a href="https://bestscreenshot.com/create-related-posts-genesis-using-acf/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">在Genesis主题中手动添加WordPress相关文章</a>。类似的插件还有 Carbon Fields 等等。

本文的重点不是介绍add_meta_box函数，也不是Advanced Custom Fields。本文的主角是一个叫做Meta Box的WordPress插件
<h2 id="Meta-Box%E6%8F%92%E4%BB%B6%E4%BB%8B%E7%BB%8D">Meta Box插件介绍</h2>
Meta Box是一个用来创建meta box的插件，不得不说这个名字起得真是有点太随意了，太大了，是一个失败的产品名字。。。

Meta Box的官网介绍说从2010年开始就专注于该插件的开发 ，目标是帮助开发者更快更好的处理WordPres中的自定义meta box ，不仅仅是一个插件，甚至可以说是一个帮助WordPress开发者处理数据的框架。

出品公司是<a href="https://elightup.com/about/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">eLightUp</a>，一个越南的10人小团队，开发了不少的优质WordPress主题和插件。

下面看一下它和同类的竟品相比有什么特别之处
<h3 id="%E5%AE%89%E8%A3%85%E5%92%8C%E4%BD%BF%E7%94%A8">安装和使用</h3>
安装步骤和其他插件无异，可以通过wordpress.org下载安装，或者如果你是PHP开发者，还可以通过PHP的包管理工具<a href="https://docs.metabox.io/composer/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680"> composer </a>来进行引入 。

和别的插件不同的是，安装完成之后没有任何介绍说明或者配置页面。你需要手动在 php 文件中手动设置。下面是一个调用API的例子，将下面的实例代码加入主题的function.php文件中，这会设置四个自定义字段 name, gender, email, biography. :
<pre class="prism-token token language-javascript"><span class="token function">add_filter</span><span class="token punctuation">(</span> <span class="token string">'rwmb_meta_boxes'</span><span class="token punctuation">,</span> <span class="token string">'prefix_meta_boxes'</span> <span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token keyword">function</span> <span class="token function">prefix_meta_boxes</span><span class="token punctuation">(</span> $meta_boxes <span class="token punctuation">)</span> <span class="token punctuation">{</span>
    $meta_boxes<span class="token punctuation">[</span><span class="token punctuation">]</span> <span class="token operator">=</span> <span class="token function">array</span><span class="token punctuation">(</span>
        <span class="token string">'title'</span>  <span class="token operator">=&gt;</span> <span class="token string">'Test Meta Box'</span><span class="token punctuation">,</span>
        <span class="token string">'fields'</span> <span class="token operator">=&gt;</span> <span class="token function">array</span><span class="token punctuation">(</span>
            <span class="token function">array</span><span class="token punctuation">(</span>
                <span class="token string">'id'</span>   <span class="token operator">=&gt;</span> <span class="token string">'name'</span><span class="token punctuation">,</span>
                <span class="token string">'name'</span> <span class="token operator">=&gt;</span> <span class="token string">'Name'</span><span class="token punctuation">,</span>
                <span class="token string">'type'</span> <span class="token operator">=&gt;</span> <span class="token string">'text'</span><span class="token punctuation">,</span>
            <span class="token punctuation">)</span><span class="token punctuation">,</span>
            <span class="token function">array</span><span class="token punctuation">(</span>
                <span class="token string">'id'</span>      <span class="token operator">=&gt;</span> <span class="token string">'gender'</span><span class="token punctuation">,</span>
                <span class="token string">'name'</span>    <span class="token operator">=&gt;</span> <span class="token string">'Gender'</span><span class="token punctuation">,</span>
                <span class="token string">'type'</span>    <span class="token operator">=&gt;</span> <span class="token string">'radio'</span><span class="token punctuation">,</span>
                <span class="token string">'options'</span> <span class="token operator">=&gt;</span> <span class="token function">array</span><span class="token punctuation">(</span>
                    <span class="token string">'m'</span> <span class="token operator">=&gt;</span> <span class="token string">'Male'</span><span class="token punctuation">,</span>
                    <span class="token string">'f'</span> <span class="token operator">=&gt;</span> <span class="token string">'Female'</span><span class="token punctuation">,</span>
                <span class="token punctuation">)</span><span class="token punctuation">,</span>
            <span class="token punctuation">)</span><span class="token punctuation">,</span>
            <span class="token function">array</span><span class="token punctuation">(</span>
                <span class="token string">'id'</span>   <span class="token operator">=&gt;</span> <span class="token string">'email'</span><span class="token punctuation">,</span>
                <span class="token string">'name'</span> <span class="token operator">=&gt;</span> <span class="token string">'Email'</span><span class="token punctuation">,</span>
                <span class="token string">'type'</span> <span class="token operator">=&gt;</span> <span class="token string">'email'</span><span class="token punctuation">,</span>
            <span class="token punctuation">)</span><span class="token punctuation">,</span>
            <span class="token function">array</span><span class="token punctuation">(</span>
                <span class="token string">'id'</span>   <span class="token operator">=&gt;</span> <span class="token string">'bio'</span><span class="token punctuation">,</span>
                <span class="token string">'name'</span> <span class="token operator">=&gt;</span> <span class="token string">'Biography'</span><span class="token punctuation">,</span>
                <span class="token string">'type'</span> <span class="token operator">=&gt;</span> <span class="token string">'textarea'</span><span class="token punctuation">,</span>
            <span class="token punctuation">)</span><span class="token punctuation">,</span>
        <span class="token punctuation">)</span><span class="token punctuation">,</span>
    <span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">return</span> $meta_boxes<span class="token punctuation">;</span>
<span class="token punctuation">}</span></pre>
调用API很简单。 把你需要的字段作为数组放入一个函数中。对于用过 Carbon Fields 的人来说, 这一步有点类似。一开始看起来可能比较麻烦，但是掌握之后就会显得很简单。

如果需要的字段比较多，手写起来还是很费事的，所以Meta Box也提供了一个在在线工具可以帮你快速生成代码, <a href="https://metabox.io/online-generator/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">form generator</a> .

把上面的代码加入到你的 functions.php 文件后，新建一个文章或者页面，在编辑器下方就可以看到出现了一个新的meta box，在其中输入必要的信息如下：

test meta box
<h3 id="%E6%98%BE%E7%A4%BA%E6%95%B0%E6%8D%AE">显示数据</h3>
在上一步中已经创建了一个meta box，可以在其中输入和保存相关数据了，那么要使用这些数据要怎么做呢？有两种方式：
<h4 id="%E9%80%9A%E8%BF%87%E5%87%BD%E6%95%B0%E6%9D%A5%E8%8E%B7%E5%8F%96%E6%95%B0%E6%8D%AE">通过函数来获取数据</h4>
Meta Box 提供了一个辅助函数<code>rwb_meta()</code>用来获取指定field的值，本质上这个函数是对WordPress自身函数<code>get_post_meta</code>的一层封装。如果想要在主题中显示出设置的自定义字段，使用函数的用法如下：
<pre class="prism-token token language-javascript">$value <span class="token operator">=</span> <span class="token function">rwmb_meta</span><span class="token punctuation">(</span> $field_id <span class="token punctuation">)</span><span class="token punctuation">;</span>
echo $value<span class="token punctuation">;</span></pre>
<h4 id="%E9%80%9A%E8%BF%87%E7%9F%AD%E7%A0%81%E8%8E%B7%E5%8F%96">通过短码获取</h4>
除了使用函数的方式之外，Meta Box还提供了一个短码<code>rwmb_meta</code>可以方便的在日志中调用自定义字段。 用法如下：
<pre class="prism-token token language-javascript"><span class="token punctuation">[</span>rwmb_meta meta_key<span class="token operator">=</span><span class="token string">"field_id"</span> post_id<span class="token operator">=</span><span class="token string">"15"</span> <span class="token operator">...</span><span class="token punctuation">]</span></pre>
<h3 id="%E6%94%AF%E6%8C%81%E7%9A%84%E5%AD%97%E6%AE%B5%E7%B1%BB%E5%9E%8B%E5%92%8C%E6%89%A9%E5%B1%95%E6%8F%92%E4%BB%B6">支持的字段类型和扩展插件</h3>
Meta Box支持多达46中字段类型，应有尽有 ，基本可以满足所有场景的需求，完整列表如下：
<ul class="ul-level-0">
 	<li><a href="https://docs.metabox.io/fields/autocomplete/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Autocomplete</a></li>
 	<li><a href="https://docs.metabox.io/fields/background/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Background</a></li>
 	<li><a href="https://docs.metabox.io/fields/button/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Button</a></li>
 	<li><a href="https://docs.metabox.io/fields/button-group/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Button Group</a></li>
 	<li><a href="https://docs.metabox.io/fields/checkbox/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Checkbox</a></li>
 	<li><a href="https://docs.metabox.io/fields/checkbox-list/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Checkbox list</a></li>
 	<li><a href="https://docs.metabox.io/fields/color/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Color</a></li>
 	<li><a href="https://docs.metabox.io/fields/custom-html/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Custom HTML</a></li>
 	<li><a href="https://docs.metabox.io/fields/date/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Date</a></li>
 	<li><a href="https://docs.metabox.io/fields/datetime/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Datetime</a></li>
 	<li><a href="https://docs.metabox.io/fields/divider/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Divider</a></li>
 	<li><a href="https://docs.metabox.io/fields/fieldset-text/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Fieldset Text</a></li>
 	<li><a href="https://docs.metabox.io/fields/file/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">File</a></li>
 	<li><a href="https://docs.metabox.io/fields/file-advanced/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">File Advanced</a></li>
 	<li><a href="https://docs.metabox.io/fields/file-input/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">File Input</a></li>
 	<li><a href="https://docs.metabox.io/fields/file-upload/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">File Upload</a></li>
 	<li><a href="https://docs.metabox.io/fields/heading/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Heading</a></li>
 	<li><a href="https://docs.metabox.io/fields/hidden/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Hidden</a></li>
 	<li><a href="https://docs.metabox.io/fields/image/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Image</a></li>
 	<li><a href="https://docs.metabox.io/fields/image-advanced/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Image Advanced</a></li>
 	<li><a href="https://docs.metabox.io/fields/image-select/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Image Select</a></li>
 	<li><a href="https://docs.metabox.io/fields/image-upload/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Image Upload</a></li>
 	<li><a href="https://docs.metabox.io/fields/key-value/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Key Value</a></li>
 	<li><a href="https://docs.metabox.io/fields/map/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Map</a></li>
 	<li><a href="https://docs.metabox.io/fields/number/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Number</a></li>
 	<li><a href="https://docs.metabox.io/fields/oembed/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">oEmbed</a></li>
 	<li><a href="https://docs.metabox.io/fields/osm/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Open Street Map</a></li>
 	<li><a href="https://docs.metabox.io/fields/password/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Password</a></li>
 	<li><a href="https://docs.metabox.io/fields/post/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Post</a></li>
 	<li><a href="https://docs.metabox.io/fields/radio/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Radio</a></li>
 	<li><a href="https://docs.metabox.io/fields/range/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Range</a></li>
 	<li><a href="https://docs.metabox.io/fields/select/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Select</a></li>
 	<li><a href="https://docs.metabox.io/fields/select-advanced/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Select Advanced</a></li>
 	<li><a href="https://docs.metabox.io/fields/sidebar/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Sidebar</a></li>
 	<li><a href="https://docs.metabox.io/fields/single-image/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Single image</a></li>
 	<li><a href="https://docs.metabox.io/fields/slider/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Slider</a></li>
 	<li><a href="https://docs.metabox.io/fields/switch/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Switch</a></li>
 	<li><a href="https://docs.metabox.io/fields/taxonomy/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Taxonomy</a></li>
 	<li><a href="https://docs.metabox.io/fields/taxonomy-advanced/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Taxonomy Advanced</a></li>
 	<li><a href="https://docs.metabox.io/fields/text/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Text</a></li>
 	<li><a href="https://docs.metabox.io/fields/text-list/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Text List</a></li>
 	<li><a href="https://docs.metabox.io/fields/textarea/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Textarea</a></li>
 	<li><a href="https://docs.metabox.io/fields/time/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Time</a></li>
 	<li><a href="https://docs.metabox.io/fields/user/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">User</a></li>
 	<li><a href="https://docs.metabox.io/fields/video/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Video</a></li>
 	<li><a href="https://docs.metabox.io/fields/wysiwyg/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">WYSIWYG</a></li>
 	<li>自动完成</li>
 	<li>背景</li>
 	<li>按钮</li>
 	<li>按钮组合</li>
 	<li>复选框</li>
 	<li>复选框列表</li>
 	<li>颜色</li>
 	<li>自定义HTML</li>
 	<li>日期</li>
 	<li>日期时间</li>
 	<li>分割线</li>
 	<li>Fieldset文字</li>
 	<li>文件</li>
 	<li>高级文件</li>
 	<li>文件输入框</li>
 	<li>文件上传</li>
 	<li>标题</li>
 	<li>隐藏元素</li>
 	<li>图像</li>
 	<li>高级图像</li>
 	<li>图像选择器</li>
 	<li>图像上传</li>
 	<li>键值对</li>
 	<li>地图</li>
 	<li>数字</li>
 	<li>嵌入对象</li>
 	<li>Open Street Map地图</li>
 	<li>密码</li>
 	<li>文章</li>
 	<li>单选按钮</li>
 	<li>滑动条</li>
 	<li>单选或多选菜单</li>
 	<li>单选或多选菜单高级选项</li>
 	<li>侧边栏</li>
 	<li>单图片</li>
 	<li>滑块</li>
 	<li>开关</li>
 	<li>分类</li>
 	<li>高级分类</li>
 	<li>文本</li>
 	<li>文字列表</li>
 	<li>文字区</li>
 	<li>时间</li>
 	<li>用户</li>
 	<li>视频</li>
 	<li>所见即所得编辑器</li>
</ul>
其中对开发者来说，button和HTML字段会特别有用。将自定义动作绑定到button可以实现各种功能，比如一键发布到其他网站、拼写检查、字数统计等等。

HTML字段可以使用HTML代码，所以开发者可以用它来加入一些带有格式的引导介绍。或者在开发插件时可以引入 MetaBox 用来显示一些通知。

除此之外 , 通过Meta Box提供的API，你也可以<a href="https://docs.metabox.io/custom-field-type/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">创建自己需要的特殊字段</a>。

Meta Box还有很多丰富的高级扩展，结合起来使用，基本上可以实现各种需求。这也是Meta Box 想要秉持的开发哲学：核心代码轻量化，附加功能在需要时再安装。
<h3 id="API">API</h3>
上文提到Meta Box提供了一个简单的函数用来在前端获取数据 <code>rwmb_meta($name)</code>.

官网的文档中也详细描述了可用的过滤器和动作。通过这些可以在meta box创建之前或之后挂载一些操作，比如在存入数据库之前对数据做一些修改，或者对metabox做一些样式修改等等。扩展性非常强。
<h3 id="%E6%96%87%E6%A1%A3">文档</h3>
官网文档写的非常详细，组织清晰，以上所有字段都有详细解释和代码实例。 并且除了文档之外，还有很好的教程和使用指导，非常值得一看。
<h3 id="Rest-API">Rest API</h3>
Meta Box 还提供了一个<a href="https://metabox.io/plugins/mb-rest-api/" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">辅助插件 </a> 来扩展 WordPress自身的rest API. 不像同类其他的插件，你不需要做任何设置，安装之后就可以像原生的restAPI一样来获取自定义字段.
<h2 id="%E4%BB%B7%E6%A0%BC">价格</h2>
Meta Box 提供免费版和付费的高级功能包，基本上免费的已经很强大够用了。
<div class="table-wrapper">
<table>
<thead>
<tr>
<th>
<div class="table-header">

Core Bundle

</div></th>
<th>
<div class="table-header">

Developer Bundle

</div></th>
<th>
<div class="table-header">

Lifetime Bundle

</div></th>
</tr>
</thead>
<tbody>
<tr>
<td>
<div class="table-cell">

$79

</div></td>
<td>
<div class="table-cell">

$149

</div></td>
<td>
<div class="table-cell">

$349

</div></td>
</tr>
</tbody>
</table>
</div>
<h2 id="%E6%80%BB%E7%BB%93">总结</h2>
Meta Box 在自定义字段上的功能基本上涵盖了所有场景，能想到的几乎都能做到，某些方面甚至比同类插件做的更好，但是可能会需要引入一些它自身的其他插件来进行配合。

官网有完善的文档和详细的教程，开发者在 <a href="https://github.com/wpmetabox" target="_blank" rel="nofollow noopener noreferrer" data-from="10680">Github</a> 上也比较活跃，但是因为名字其的不好，所以比较难搜到更多资料。

值得推荐。

</div>
<div class="col-2-article-source">

本文参与<a class="com-link" href="https://cloud.tencent.com/developer/support-plan">腾讯云自媒体分享计划</a>，欢迎正在阅读的你也

</div>
</div>
</div>