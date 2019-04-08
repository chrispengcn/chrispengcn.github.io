---
ID: 2668
post_title: Wordpress 网站 插件设计初步
author: chrispengcn
post_excerpt: ""
layout: post
permalink: >
  http://hss5.com/2019/04/08/wordpress-plugin-design-basic/
published: true
post_date: 2019-04-08 17:09:37
---
如果你熟悉Wordpress，应该是很熟悉插件(plugins)， 插件扩展和添加wordpress 的功能。我们经常在Wordpress面板安装，激活，取消激活，删除插件，wordpress 有很多很多插件，我们可以搜索，但有时候我们需要的功能没有，或不够，这时需要我们自己设计插件。

插件简介：
WordPress本身由一组相对较小的功能组成，这些功能统称为平台的“核心”。插件是可下载的附加组件，包含扩展或改变该核心功能的文件和代码。 事实上，WordPress哲学指出，如果一个功能可能被不少于80％的用户使用，它应该包含在核心中。 否则，它应该是一个插件。

出于这个原因，插件提供的广泛可能性是巨大的。 您可以使用它们添加选择加入表单，滑块或弹出窗口。 它们可以非常简单，例如Hello Dolly，它在仪表板中显示名义歌曲的随机行。 或者它们可以是令人难以置信的扩展，例如Jetpack插件，它为您的网站添加了许多新的设置和功能。

这一事实意味着插件是WordPress最重要的功能之一。 它们可以轻松地为您的网站添加几乎任何功能，无需编码或先前的知识。 更重要的是，下载和安装插件就像点击几下一样简单。

最后一部分是要记住的重点。 由于WordPress和插件是开源的，因此他们的代码可供任何人使用和试验。 对于初露头角的开发人员来说，这是一种很好的方式，可以了解幕后的工作方式，并参与自己创建插件。

插件和主题之间的差异

在我们开始学习之前，我们需要快速介绍一些重要的基础工作。首先，我们来谈谈主题和插件之间的差异。从表面上看，这似乎是一个明显的区别。当然，一个主题只是改变了你网站的外观，而一个插件增加了功能？

然而，事实比这更加模糊。

事实上，主题也可以改变网站的功能，而插件可以改变它的外观。所有WordPress主题都包含一个functions.php文件，其中包含为您的网站添加功能的代码。这与插件的工作方式非常相似。实际上，您可以将相同的代码添加到插件或functions.php中，它将在您的站点上运行相同。

不同之处在于，当您向functions.php添加代码时，它会链接到您当前的主题。如果您想要更改主题的某些功能，或者想要快速添加功能而无需编写整个插件，这将非常有用。如果您要创建主题并想要插入自定义函数，也可以使用此方法。

但是，如果您决定更改主题，则您添加的代码将不再有效。另一方面，插件是独立的实体（通常）不依赖于任何特定主题，这意味着您可以切换主题而不会丢失插件的功能。使用插件而不是主题也可以使您想要创建的功能更易于维护和与他人共享。

插件的工作原理：挂钩，动作和过滤器简介

我们已经提到插件字面上“插入”到WordPress核心。 这里使用'hooks'完成的，它允许一段代码与另一段代码进行交互。 因此，钩子确定您的网站上实际使用插件的时间和位置。

让我们考虑一个例子： 想象一下，如果有人试图使用错误的密码登录站点，会看到一个更改错误消息的插件。 在这种情况下，错误消息是钩子。 插件可以连接到显示该消息的代码，并更改显示的文本。

WordPress可以理解两种类型的钩子。 他们是：

(actions)  操作：这些用于添加或更改WordPress功能。
(filters)  过滤器：这些过滤器用于更改操作的功能。

如何创建第一个WordPress插件（分4步）
现在使用最少量的编码创建第一个插件。 因此，我们会坚持一些基本的东西。 在以下步骤中，我们将创建一个插件，该插件将在脚标后显示一段文字。

第1步：设置测试环境
开发任何内容时，无论是创建插件还是进行其他可能影响网站的更改，都应该始终使用测试环境。这也称为“暂存站点”或“本地环境”，具体取决于站点是存储在外部服务器还是自己的计算机上。

无论位置如何，测试环境都应该是站点的私有副本。这使您可以添加和编辑站点的文件和功能，而不会对实际的实时网站造成损害。当您使用核心文件和插件时，这一点尤为重要，因为错误可能会对您的网站造成持久的伤害。

幸运的是，由于有各种各样的工具可以帮助您，因此设置测试站点非常简单。要设置本地环境，请查看 Wordpress 网站设计入门0 本地Web主机安装

第2步：创建一个新的插件文件
要开始整理新插件，您需要访问您网站的目录。 可以使用SFTP，也可以使用Wordpress 面板，需要安装文件管理插件，参见 Wordpress 网站设计 文件管理器插件。常用的sftp为FileZilla的客户端，因为它既免费且易于使用。如果是本地那就直接资源管理器。

导航到站点。 进入后，导航到包含插件的文件夹，该插件位于/ wp-content / plugins /。

添加新插件，需要在此目录中创建一个新文件夹。 这样做，并给它一名称。 这里为veryfirstplugin。

然后在这目录下建一个新文件，取名为veryfirstplugin.php

编辑该文件为如下内容：

&lt;?php
/**
* Plugin Name: Very First Plugin
* Plugin URI: https://www.yourwebsiteurl.com/
* Description: This is the very first plugin I ever created.
* Version: 1.0
* Author: Your Name Here
* Author URI: http://yourwebsiteurl.com/
**/
在保存文件之前，请随时更改此处的信息以符合您的实际细节。

完成后，您实际上可以在站点的管理仪表板中看到该插件。 立即登录并查看您的插件库。

你甚至可以继续现在激活你的插件。 当然，该插件实际上并没有做任何事情。 那是因为我们没有添加任何功能。

第3步：为插件添加代码
这是一个用于在每个页面的页脚之后显示文本的示例插件代码
这个插件挂钩了wp_footer（）动作钩子，它在每个页面的结束&lt;/ body&gt;标签之前调用。并添加了一个名为mfp_Add_Text（）的函数。 这个函数的内容就是简单显示：After the footer is loaded, my text is addded!

因为是挂钩wp_footer，所以就是脚标后显示这行内容。打开任何一个页面都是会显示脚标的。

由于这是插件的一部分而不是主题，因此如果激活完全不同的主题，它将继续有效。

// Hook the 'wp_footer' action hook, add the function named 'mfp_Add_Text' to it
add_action("wp_footer", "mfp_Add_Text");

// Define 'mfp_Add_Text'
function mfp_Add_Text()
{
echo "&lt;p style='color: black;'&gt;After the footer is loaded, my text is added!&lt;/p&gt;";
}
第4步：在实际站点上导出并安装插件
新插件现在可以在实际网站上使用了。 幸运的是，这一步通常是最简单的。 需要做的就是将first-first-plugin文件夹压缩为ZIP文件。 如果使用本地环境来创建插件，则只需右键单击该文件夹并选择“压缩”。

然后可以将此zip文件上传到您的实际站点。 打开WordPress管理仪表板，导航到插件，然后单击添加新。

在下一个屏幕上，可以选择上传插件，然后从计算机中选择插件文件。

当然我们这个代码是不适合上传的，只是一个示例。

我们看看运行的效果，红框中就是我们显示的文字：

现在再把整个文件内容，贴这里，给你做个简单参考。

&lt;?php
/**
* Plugin Name: Very First Plugin
* Plugin URI: https://www.yourwebsiteurl.com/
* Description: This is the very first plugin I ever created.
* Version: 1.0
* Author: Your Name Here
* Author URI: http://yourwebsiteurl.com/
**/
// Hook the 'wp_footer' action hook, add the function named 'mfp_Add_Text' to it
add_action("wp_footer", "mfp_Add_Text");

// Define 'mfp_Add_Text'
function mfp_Add_Text()
{
echo "&lt;p style='color: black;'&gt;After the footer is loaded, my text is added!&lt;/p&gt;";
}
?&gt;
参考 https://www.hostinger.com/tutorials/how-to-create-wordpress-plugin， 调试而成。

&nbsp;