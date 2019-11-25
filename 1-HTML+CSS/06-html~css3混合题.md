#### 1.1 新特性、移除了那些元素

HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。

**新特性(包括标签):**

- 绘画 canvas;
- 用于媒介回放的 video 和 audio 元素;
- 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失;
  sessionStorage 的数据在浏览器关闭后自动删除;
- 语意化更好的内容元素，比如 article、footer、header、nav、section;
- 表单控件，calendar、date、time、email、url、search;
- 新的技术webworker, websocket, Geolocation;

**移除的元素：**

- 纯表现的元素：basefont，big，center，font, s，strike，tt，u;
- 对可用性产生负面影响的元素：frame，frameset，noframes；

#### 1.2 处理HTML5新标签的浏览器兼容问题

**支持HTML5新标签：**
IE8/IE7/IE6支持通过document.createElement方法产生的标签，
可以利用这一特性让这些浏览器支持HTML5新标签，
浏览器支持新标签后，还需要添加标签默认的样式。

当然也可以直接使用成熟的框架、比如html5shim;

```
<!--[if lt IE 9]>
 <script>src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script>
<![endif]-->
```

#### 1.3 区分 HTML 和 HTML5

DOCTYPE声明\新增的结构元素\功能元素

#### 1.4 HTML语义化的理解

用正确的标签做正确的事情。
html语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析;
及时在没有样式CCS情况下也以一种文档格式显示，并且是容易阅读的;
搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO;
使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。

#### 1.5 HTML5离线储存

在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件。

**原理：**HTML5的离线存储是基于一个新建的.appcache文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像cookie一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。

**如何使用：**

1. 页面头部像下面一样加入一个manifest的属性；
2. 在cache.manifest文件的编写离线存储的资源；
   CACHE MANIFEST
   \#v0.11
   CACHE:
   js/app.js
   css/style.css
   NETWORK:
   resourse/logo.png
   FALLBACK:
   / /offline.html
3. 在离线状态时，操作window.applicationCache进行需求实现。

**浏览器是怎么对HTML5的离线储存资源进行管理和加载的呢？**
在线的情况下，浏览器发现html头部有manifest属性，它会请求manifest文件，如果是第一次访问app，那么浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储。如果已经访问过app并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的manifest文件与旧的manifest文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。
离线的情况下，浏览器就直接使用离线存储的资源。

## 2. HTML标签相关

#### 2.1 iframe有那些缺点？

- iframe会阻塞主页面的Onload事件；
- 搜索引擎的检索程序无法解读这种页面，不利于SEO;
- iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。

```
使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好是通过javascript 动态给iframe添加src属性值，这样可以绕开以上两个问题。
```

#### 2.2 Label的作用是什么

label标签来定义表单控制间的关系,当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。

```
<label for="Name">Number:</label>
<input type=“text“name="Name" id="Name"/>

<label>Date:<input type="text" name="B"/></label>
```

#### 2.3 如何实现浏览器内多个标签页之间的通信?

WebSocket、SharedWorker； 也可以调用localstorge、cookies等本地存储方式；

localstorge另一个浏览上下文里被添加、修改或删除时，它都会触发一个事件，
我们通过监听事件，控制它的值来进行页面信息通信；
注意quirks：Safari 在无痕模式下设置localstorge值时会抛出 QuotaExceededError 的异常；

#### 2.4 webSocket如何兼容低浏览器？

Adobe Flash Socket 、
ActiveX HTMLFile (IE) 、
基于 multipart 编码发送 XHR 、
基于长轮询的 XHR

#### 2.5 字体标签的区别

title属性没有明确意义只表示是个标题，H1则表示层次明确的标题，对页面信息的抓取也有很大的影响；

strong是标明重点内容，有语气加强的含义，使用阅读设备阅读网络时：``会重读，而``是展示强调内容。

i内容展示为斜体，em表示强调的文本；

Physical Style Elements -- 自然样式标签
b, i, u, s, pre
Semantic Style Elements -- 语义样式标签
strong, em, ins, del, code
应该准确使用语义样式标签, 但不能滥用, 如果不能确定时首选使用自然样式标签。

## 3. css部分(1)

#### 3.1 盒子模型

介绍一下标准的CSS的盒子模型？低版本IE的盒子模型有什么不同的？
（1）有两种， IE 盒子模型、W3C 盒子模型；
（2）盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border)；
（3）区 别： IE的content部分把 border 和 pading计算了进去;

#### 3.2 css选择符与属性继承

**选择符**:
1.id选择器（ # myid）
2.类选择器（.myclassname）
3.标签选择器（div, h1, p）
4.相邻选择器（h1 + p）
5.子选择器（ul > li）
6.后代选择器（li a）
7.通配符选择器（ * ）
8.属性选择器（a[rel = "external"]）
9.伪类选择器（a:hover, li:nth-child）

**可继承的样式:**
font-size font-family color ul li dl dd dt

**不可继承的样式:**
border padding margin width height

#### 3.3 优先级算法

- 优先级就近原则，同权重情况下样式定义最近者为准;
- 载入样式以最后载入的定位为准;

优先级为:
!important > id > class > tag
important 比 内联优先级高

#### 3.4 css3新增伪类

```
 举例：
    p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。
    p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。
    p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。
    p:only-child        选择属于其父元素的唯一子元素的每个 <p> 元素。
    p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。

    :after          在元素之前添加内容,也可以用来做清除浮动。
    :before         在元素之后添加内容
    :enabled        
    :disabled       控制表单控件的禁用状态。
    :checked        单选框或复选框被选中。
```

#### 3.5 css居中

给div设置一个宽度，然后添加margin:0 auto属性

```
div{
    width:200px;
    margin:0 auto;
 }
```

**让绝对定位的div居中**

```
  position: absolute;
  width: 1200px;
  background: none;
  margin: auto;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
```

**水平垂直居中一**

```
  确定容器的宽高 宽500 高 300 的层
  设置层的外边距

 div {
      position: relative;     /* 相对定位或绝对定位均可 */
    width:500px;
    height:300px;
    top: 50%;
    left: 50%;
    margin: -150px 0 0 -250px;      /* 外边距为自身宽高的一半 */
    background-color: pink;     /* 方便看效果 */
 }
```

**水平垂直居中二**

```
未知容器的宽高，利用 `transform` 属性

div {
    position: absolute;     /* 相对定位或绝对定位均可 */
    width:500px;
    height:300px;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background-color: pink;     /* 方便看效果 */

}
```

**水平垂直居中三**

```
利用 flex 布局
实际使用时应考虑兼容性

.container {
    display: flex;
    align-items: center;        /* 垂直居中 */
    justify-content: center;    /* 水平居中 */

}
.container div {
    width: 100px;
    height: 100px;
    background-color: pink;     /* 方便看效果 */
}  
```

#### 3.6 display值及作用

```
block         象块类型元素一样显示。
none          缺省值。象行内元素类型一样显示。
inline-block  象行内元素一样显示，但其内容象块类型元素一样显示。
list-item     象块类型元素一样显示，并添加样式列表标记。
table         此元素会作为块级表格来显示
inherit       规定应该从父元素继承 display 属性的值
```

#### 3.7 position定位

```
absolute
    生成绝对定位的元素，相对于值不为 static的第一个父元素进行定位。
fixed （老IE不支持）
    生成绝对定位的元素，相对于浏览器窗口进行定位。
relative
    生成相对定位的元素，相对于其正常位置进行定位。
static
    默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right z-index 声明）。
inherit
    规定从父元素继承 position 属性的值。
```

#### 3.8 css3新特性

```
  新增各种CSS选择器  （: not(.input)：所有 class 不是“input”的节点）
  圆角           （border-radius:8px）
  多列布局        （multi-column layout）
  阴影和反射        （Shadow\Reflect）
  文字特效      （text-shadow、）
  文字渲染      （Text-decoration）
  线性渐变      （gradient）
  旋转          （transform）
  增加了旋转,缩放,定位,倾斜,动画，多背景
  transform:\scale(0.85,0.90)\ translate(0px,-30px)\ skew(-9deg,0deg)\Animation:
```

#### 3.9 创建一个三角形

```
把上、左、右三条边隐藏掉（颜色设为 transparent）
#demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```

#### 3.10 浏览器兼容

```
* png24位的图片在iE6浏览器上出现背景，解决方案是做成PNG8.

* 浏览器默认的margin和padding不同。解决方案是加一个全局的*{margin:0;padding:0;}来统一。

* IE6双边距bug:块属性标签float后，又有横行的margin情况下，在ie6显示margin比设置的大。

  浮动ie产生的双倍距离 #box{ float:left; width:10px; margin:0 0 0 100px;}

  这种情况之下IE会产生20px的距离，解决方案是在float的标签样式控制中加入 ——_display:inline;将其转化为行内属性。(_这个符号只有ie6会识别)

  渐进识别的方式，从总体中逐渐排除局部。

  首先，巧妙的使用“\9”这一标记，将IE游览器从所有情况中分离出来。
  接着，再次使用“+”将IE8和IE7、IE6分离开来，这样IE8已经独立识别。

  css
      .bb{
          background-color:#f1ee18;/*所有识别*/
          .background-color:#00deff\9; /*IE6、7、8识别*/
          +background-color:#a200ff;/*IE6、7识别*/
          _background-color:#1e0bd1;/*IE6识别*/
      }

*  IE下,可以使用获取常规属性的方法来获取自定义属性,
   也可以使用getAttribute()获取自定义属性;
   Firefox下,只能使用getAttribute()获取自定义属性。
   解决方法:统一通过getAttribute()获取自定义属性。

*  IE下,even对象有x,y属性,但是没有pageX,pageY属性;
   Firefox下,event对象有pageX,pageY属性,但是没有x,y属性。

*  解决方法：（条件注释）缺点是在IE浏览器下可能会增加额外的HTTP请求数。

*  Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示,
   可通过加入 CSS 属性 -webkit-text-size-adjust: none; 解决。

超链接访问过后hover样式就不出现了 被点击访问过的超链接样式不在具有hover和active了解决方法是改变CSS属性的排列顺序:
L-V-H-A :  a:link {} a:visited {} a:hover {} a:active {}
```

#### 3.11 li之前空白间隔

行框的排列会受到中间空白（回车\空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了。

#### 3.12 为什么要初始化css样式

```
- 因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。

- 当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。

最简单的初始化方法： * {padding: 0; margin: 0;} （强烈不建议）

淘宝的样式初始化代码：
body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
h1, h2, h3, h4, h5, h6{ font-size:100%; }
address, cite, dfn, em, var { font-style:normal; }
code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
small{ font-size:12px; }
ul, ol { list-style:none; }
a { text-decoration:none; }
a:hover { text-decoration:underline; }
sup { vertical-align:text-top; }
sub{ vertical-align:text-bottom; }
legend { color:#000; }
fieldset, img { border:0; }
button, input, select, textarea { font-size:100%; }
table { border-collapse:collapse; border-spacing:0; }
```

## 4. css部分(2)

#### 4.1 BFC规范

（W3C CSS 2.1 规范中的一个概念,它是一个独立容器，决定了元素如何对其内容进行定位,以及与其他元素的关系和相互作用。）
一个页面是由很多个 Box 组成的,元素的类型和 display 属性,决定了这个 Box 的类型。
不同类型的 Box,会参与不同的 Formatting Context（决定如何渲染文档的容器）,因此Box内的元素会以不同的方式渲染,也就是说BFC内部的元素和外部的元素不会互相影响。
**更多详细内容请看[CSS中的BFC](https://www.geekjc.com/)**

#### 4.2 css定义权重

```
以下是权重的规则：标签的权重为1，class的权重为10，id的权重为100，以下例子是演示各种定义的权重值：

/*权重为1*/
div{
}
/*权重为10*/
.class1{
}
/*权重为100*/
#id1{
}
/*权重为100+1=101*/
#id1 div{
}
/*权重为10+1=11*/
.class1 div{
}
/*权重为10+10+1=21*/
.class1 .class2 div{
}

如果权重相同，则最后定义的样式会起作用，但是应该避免这种情况出现
```

#### 4.3 css浮动

请解释一下为什么会出现浮动和什么时候需要清除浮动？清除浮动的方式：

**浮动元素脱离文档流，不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。**
**浮动元素引起的问题和解决办法？**

浮动元素引起的问题： 父元素的高度无法被撑开，影响与父元素同级的元素与浮动元素同级的非浮动元素（内联元素）会跟随其后若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构解决方法： 使用CSS中的clear:both;属性来清除元素的浮动可解决2、3问题，对于问题1，添加如下样式，给父元素添加clearfix样式：

```
.clearfix:after{content: ".";display: block;height: 0;clear: both;visibility: hidden;}
.clearfix{display: inline-block;} /* for IE/Mac */
```

清除浮动的几种方法：

- 额外标签法

```
<div style="clear:both;"></div>
```

（缺点：不过这个办法会增加额外的标签使HTML结构看起来不够简洁。）

- 使用after伪类

```
#parent:after{
content:".";
height:0;
visibility:hidden;
display:block;
clear:both;
}
注意这里是给父元素加
```

- 设置`overflow`为`hidden`或者auto

```
给包含浮动元素的父标签添加css属性 overflow:auto; zoom:1; //zoom:1用于兼容IE6。
```

#### 4.4 cookie隔离

```
如果静态文件都放在主域名下，那静态文件请求的时候都带有的cookie的数据提交给server的，非常浪费流量，
所以不如隔离开。

因为cookie有域的限制，因此不能跨域提交请求，故使用非主要域名的时候，请求头中就不会带有cookie数据，
这样可以降低请求头的大小，降低请求时间，从而达到降低整体请求延时的目的。

同时这种方式不会将cookie传入Web Server，也减少了Web Server对cookie的处理分析环节，
提高了webserver的http请求的解析速度。
```

#### 4.5 link和@import

（1）link属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;

（2）页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;

（3）import是CSS2.1 提出的，只在IE5以上才能被识别，而link是XHTML标签，无兼容问题;

#### 4.6 style标签写在body后与body前有什么区别？

提示对于js阻塞，网上已经有很多文章，但是css阻塞的描述较少，于是对css阻塞进行深入研究。 写了一个测试demo，代码如下

```
late-css.php
<?php
header("Content-type: text/css; charset=utf-8");
sleep(intval(@$_GET[time]));
$str = 'div{background:blue}';
echo $str;
?>
 
late-js.php
<?php
header("Content-type: text/js; charset=utf-8");
sleep(intval(@$_GET[time]));
$str = 'document.write("I'm the lazy note")';
echo $str;
?>
index3.html
<html>
<head>
    <style>
         div{background:red;width:200px;height:200px;}
    </style>
    <link rel="stylesheet" href="/lab/late/late-css.php?time=10&blue">
</head>
<body>
    <div></div>
</body>
</html>
```

运行demo呈现的是页面需要等待10秒才能打开，而且直接打开的是蓝色方块。（证明css在页面中存在阻塞,但不确定是在解析DOM时阻塞还是渲染时阻塞）

```
index4.html
<html>
<head>
    <style>
         div{background:red;width:200px;height:200px;}
    </style>
</head>
<body>
    <div></div>
</body>
    <link rel="stylesheet" href="/lab/late/late-css.php?time=10&blue">
</html>
```

运行demo呈现的是页面需要等待10秒才能打开，而且直接打开的是蓝色方块。（证明css在页面中存在的是阻塞渲染） 那么在我们设计页面的时候css，尽量将首屏显示的css直接放在html上面是最合适的，但是实际情况很难做到，比如一些导航的样式，未来在css文件中实现CDN，不得不将css放在外链中。

```
index.html
<html>
    <head>
        <style>
            div{background:red;width:200px;height:200px;}
        </style>
        <link rel="stylesheet" href="/lab/late/late-css.php?time=5&blue">
    </head>
    <body>
        <div><script src="/lab/late/late-js.php?time=10"></script></div>
    </body>
    <style>
        div{background:yellow;}
    </style>
</html>
```

（证明js，阻塞DOM解析，CSS作用于DOM解析到哪里，就渲染到哪里）
猜猜这是会显示什么？
先白色5秒，再蓝色5秒，再黄色

```
index2.html
<html>
    <head>
        <style>
            div{background:red;width:200px;height:200px;}
        </style>
    </head>
    <body>
    <div><script src="/lab/late/late-js.php?time=10"></script></div>
    </body>
    <link rel="stylesheet" href="/lab/late/late-css.php?time=5&blue">
    <style>
        div{background:yellow;}
    </style>
</html>
```

（证明css在下载时，不阻塞DOM解析，但是下载时阻塞渲染） 先红色，再黄色

**总结：**CSS和JS的下载都不会被解析阻塞，CSS下载时会阻塞渲染, 外部样式会阻塞后续脚本执行，直到外部样式加载并解析完毕。

1. 没遇到JS阻塞的情况，CSS只会在DOM解析完后渲染。
2. 遇到JS阻塞的情况，当DOM一旦遇到JS阻塞时，就会触发CSS渲染

那么现在有一个问题，css写在body前和body后有什么区别， 由上述可见，CSS无论是用STYLE标签还是link外链，

- 如果没有遇到jS阻塞，放哪里都一样，
- 如果遇到js阻塞，把CSS写在BODY上面，不会出现二次渲染，把CSS写在BODY下面，JS阻塞时，会出现一次渲染，等阻塞完毕会出现二次渲染（除非外链的CSS下载时间比阻塞时间长）。