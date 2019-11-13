---
title: 论-JavaScript、jQuery、AJAX、JSON及四者关系
date: 
tags: js
categories: web
---
  在学习前端的过程中，经常被各种各样的语法名词搞混，这里简单归纳下容易混淆的四种名词含义，JavaScript、jQuery、AJAX、JSON 以及这四者之间的关系。

## JavaScript

JavaScript（简称js）是一种主要运行于浏览器中的**弱类型的动态脚本语言**，可以用来实现网页上的一些高级功能，如数据验证处理、页面动态效果、定时任务、与用户交互、发送/接收服务器端数据等等。

弱类型指的是js中的变量在参与运算的时候可以根据实际需要动态转换类型。与之相对应的是强类型语言——变量一般不允许自动转换类型（某些强类型语言的字符串连接操作除外），如果参与运算、调用时不符合要求的类型，则会在编译阶段报错，比如说java各种复杂的变量声明。
<!--more-->
而因为JavaScript 是脚本语言。浏览器会在读取代码时，逐行地执行脚本代码。对于传统编程来说，则是会在执行前对所有代码进行编译。
又因为他弱语言的特性，经常见到var出一切变量！

简单的js示例：

```js
<html>
<head></head>
<body>
<script type="text/javascript">{
	document.write("<h1>这是标题</h1>");
	document.write("<p>Hello word！</p>");
	document.write("<p>这是另一个段落。</p>");
	}
</script>
</body>
</html>
```


## jQuery

**jQuery是js的一个工具库**，由John Resig在2006年发布。
意思就是基于js的一种实用工具。在jQuery出现之前，在js程序中获取元素节点比较麻烦，例如：

```js
document.getElementById('id')
```

之后John Resig根据css选择器编写了jQuery选择器，并对选择器的规则进行了扩充，从而让元素查找变得非常方便。

```js
$('#id')
```

除此之外还有链式操作

```js
$('div.con')
    .height(100)
    .show();
```

这种连续调用可以让代码书写更加简洁，也印证了jQuery自己的口号：**write less, do more**

此外，jQuery还提供了浏览器兼容、样式读写、事件绑定与执行、动画等特性，后来又加入了ajax、promise等，再加上方便的插件编写机制，对整个js的生态圈产生了重大的影响，可以说是js历史上影响力最大的一个库。

## AJAX

ajax全称Asynchronous JavaScript and XML（异步的JavaScript与XML），是网页无需刷新页面、使用js**与服务器进行交互的一种技术**。

有时候会有这样一种需求：只希望更改页面上的一个区域。然而在从前的技术框架内只能刷新整个页面，带来的后果是：①需要重新传输整个页面，服务器端与客户端的流量消耗都会比较大；②如果是动态页，服务器端需要重新生成整个页面，即使是那些客户原本不想要刷新的区域，增大了服务器的负担。

ajax的基本流程可以概括为：页面上js脚本实例化一个XMLHttpRequest对象，设置好服务器端的url、必要的查询参数、回调函数之后，向服务器发出请求，服务器在处理请求之后将处理结果返回给页面，触发事先绑定的回调函数。这样，页面脚本如果想要改变一个区域的内容，只需要通过ajax向服务器获取与该区域有关的少量数据，在回调函数中将该区域的内容替换掉即可，不需要刷新整个页面。

XMLHttpRequest在发送请求的时候，有两种方式：同步与异步。这里就不细谈了。

简单的示例：

```js
$.ajax({
  url:'/user/ajaxshow',       //规定发送请求的 URL。默认是当前页面。
  method:'GET',               //与type一模一样，只是type对于目前jQuery的版本全部兼容，method在jQuery1.9以后的版本
  contentType:'application/json',     //发送数据到服务器时所使用的内容类型。
  dataType:'json',                //预期的服务器响应的数据类型。
  success:function (data) {
    /**在此进行数据处理*/
    alert(data);                    
      }
  }
```

## JSON

JSON全称JavaScript Object Notation（js对象标记法），由Douglas Crockford在2002年发现并制定了标准。JSON是基于JavaScript的，是JavaScript的一个**子集**。JSON是用JavaScript语法来表示数据的一种轻量级语言。

从ajax的命名中我们就可以看到，数据交换是通过XML格式进行的。在ajax刚出现的时候，绝大多数应用都是采用XML格式，也有少数使用纯文本的。但是XML格式有一个缺点，就是文档构造复杂，需要传输比较多的字节数。在这种情况下，JSON的轻便性逐渐得到重视，后来替代XML成为ajax最主要的数据传输格式。可以举个简单的例子比较一下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <article>
    <title>Article Title1</title>
    <content>content1</content>
  </article>
  <article>
    <title>Article Title2</title>
    <content>content2</content>
  </article>
</root>
```

```json
{
  "article" : [
    {
      "title": "Article Title1",
      "content": "content1"
    },
    {
      "title": "Article Title2",
      "content": "content2"
    }
  ]
}
```

说到js自身，对于对象构造有两种方法：基于对象的完整写法，字面量表示法。前者如：

```js
var obj = new Object();
obj.title = "title1";
obj.content = "content1";
```

而与之对应的字面量表示法则写为：

```js
var obj = {
    title: "title1",
    content: "content1"
};
```

可以明显看出字面量表示法要简洁得多。而JSON基本就是字面量表示法的一个子集，除了强制要求键与字符串类型的值必须用双引号包起之外，它剔除了undefined、function等类型，也不包括浏览器内置对象类型（如Date、RegExp等），是基于文本的、比较纯粹的数据表示方法。

## 总结

总结一下就是，js老大哥，jQuery是他手头最好用的工具（之一），AJAX是基于js的一门页面部分刷新技术，JSON是js部分语法的升级优化版。