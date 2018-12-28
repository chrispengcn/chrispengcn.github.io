---
ID: 743
post_title: >
  javascript中getElementBy系列函数用法
author: chrispengcn
post_excerpt: ""
layout: post
permalink: 'http://hss5.com/2017/01/16/javascript%e4%b8%adgetelementby%e7%b3%bb%e5%88%97%e5%87%bd%e6%95%b0%e7%94%a8%e6%b3%95/'
published: true
post_date: 2017-01-16 18:23:40
---
WEB标准下可以通过getElementById(), getElementsByName(), and getElementsByTagName()访问DOCUMENT中的任一个标签：
1、getElementById()
getElementById()在我们做web时估计是用的最多的了.getElementById()可以访问DOCUMENT中的某一特定元素，顾名思义，就是通过ID来取得元素，所以只能访问设置了ID的元素。
比如说有一个DIV的ID为docid：
<div id="docid"></div>
那么就可以用getElementById("docid")来获得这个元素。
此函数一般多用在取得input表单的值,在用javascript检查表单时非常有用.
还有一点就是在ie这种非标准浏览器中如果你的input中没有id,只有name,在ie下用getElementbyid("name")是可以取得值的,但在firefox这种标准浏览器里是取不到值的
以下为引用的内容：
<input type="text" name="a" id="b" value="str">
<script>
var input_value=document.getElementById("a").value;
alert(input_value);
</script>
你可以用下面这段代码在ie和ff下看看效果
以下为引用的内容：
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
"http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>ById</title>
<style type="text/css">
<!--
#docid{
height:400px;
width:400px;
background-color:#999;}
-->
</style>
<script language="JavaScript" type="text/JavaScript">
<!--
function bgcolor(){ php程序员站
document.getElementById("docid").style.backgroundColor="#000"
}
-->
</script>
</head>
<body><div id="docid" name="docname" onClick="bgcolor()"></div>
</body>
</html>
2 . getElementsByName()
这个是通过NAME来获得元素，大家注意这个是GET　ELEMENTS，复数ELEMENTS代表获得的不是一个元素，为什么呢？
因为DOCUMENT中每一个元素的ID是唯一的，但NAME却可以重复。打个比喻就像人的身份证号是唯一的（理论上，虽然现实中有重复），但名字 php程序员之家
重复的却很多。如果一个文档中有两个以上的标签NAME相同，那么getElementsByName()就可以取得这些元素组成一个数组。
比如有两个DIV：
<div name="docname" id="docid1"></div>
<div name="docname" id="docid2"></div>
那么可以用getElementsByName("docname")获得这两个DIV，用getElementsByName("docname")[0]访问第一个DIV，用getElementsByName
php程序员之家
3、getElementsByTagName()
这个呢就是通过TAGNAME（标签名称）来获得元素，一个DOCUMENT中当然会有相同的标签，所以这个方法也是取得一个数组。
下面这个例子有两个DIV，可以用getElementsByTagName("div")来访问它们，用getElementsByTagName("div")[0]访问第一个DIV，用 phperz.com
getElementsByTagName("div")[1]访问第二个DIV。 php程序员站
以下为引用的内容：
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=gb2312">
<title>Byname,tag</title>
<style type="text/css">
<!--
#docid1,#docid2{
margin:10px;
height:400px;
width:400px;
background-color:#999;} www~phperz~com
-->
</style>
</head>
<body>
<div name="docname" id="docid1" onClick="bgcolor()"></div>
<div name="docname" id="docid2" onClick="bgcolor()"></div>
</body>
</html>
<script language="JavaScript" type="text/JavaScript">
<!--
function bgcolor(){
var docnObj=document.getElementsByTagName("div");
docnObj[0].style.backgroundColor = "black";
docnObj[1].style.backgroundColor = "black";
}
-->
</script>
总结一下标准DOM，访问某一特定元素尽量用标准的getElementById()，访问标签用标准的getElementByTagName(),但IE不支持getElementsByName() (ie7支持)，所以就要避免使用getElementsByName()，但getElementsByName()和不符合标准的document.all[]也不是全无是处，它们有自己的方便之处，用不用那就看网站的用户使用什么浏览器，由你自己决定了。
附加：在IE6中替代getElementsByName() 的方法：
<script language="JavaScript">
//Select or unselect all.
    function select_deselectAll(me) {
        var checkboxes = document.getElementsByTagName("input");
        for(var i=0 ; i<checkboxes.length ; i++){
            if(checkboxes[i].name=="checkbox_selectEmployee"){
                if(me.checked==true){
                    checkboxes[i].checked=true;
                }else{
                    checkboxes[i].checked=false;
                }
            }
        }
    }
</script>
<input type="checkbox" onclick="select_deselectAll(this)" />
<input type="checkbox" name="checkbox_selectEmployee" />
<input type="checkbox" name="checkbox_selectEmployee" />
<input type="checkbox" name="checkbox_selectEmployee" />