WEB前端【CSS】考核题目

### 1. 介绍一下标准的CSS的盒子模型？与低版本IE的盒子模型有什么不同的？

盒子模型就是 元素在网页中的实际占位，**有两种：****标准盒子模型和****IE盒子模型**

**标准(W3C)盒子模型：**内容content+填充padding+边框border+边界margin

宽高指的是 content 的宽高

**低版本IE盒子模型：**内容（content+padding+border）+ 边界margin，

宽高指的是 content+padding+border 部分的宽高

> **问题的拓展：**



 **CSS 如何设置这两种模型？

```
box-sizing : content-box  
box-sizing : border-box
```

 **JS 如何设置获取盒模型对应的宽和高？**

```
dom.style.width/height;//设置获取的是内联样式
dom.currentStyle.width/height;//只有IE支持
window.getComputedStyle(dom).width/height;//兼容性好
dom.getBoundingClientRect().width/height;//适用场所：计算一个元素的绝对位置
```

 **实例题（根据盒模型解释边距重叠）？**

**该例子是父子边距重叠，还有兄弟元素的边距重叠**

```
<style>        
    html *{            
        padding: 0;            
        margin: 0;        
    }        
    #sec{            
        background: #f00;            
        overflow: hidden; //创建了一个BFC，块级格式化上下文   
    }        
    .child{            
        height: 100px;            
        margin-top: 10px;            
        background: yellow;        
    }    
</style>
<section id="sec">        
    <article class="child"></article>    
</section>
```



 **BFC（边距重叠解决方案）？**

### 2. box-sizing属性？

用来控制元素的盒子模型的解析模式，默认为content-box
**context-box：W3C的标准盒子模型**，设置元素的 height/width 属性指的是 content 部分的高/宽
**border-box：IE 传统盒子模型。**设置元素的 height/width 属性指的是 content + border + padding 部分的高/宽

### 3. 对 BFC 规范(块级格式化上下文：block formatting context)的理解？

（W3C CSS 2.1 规范中的一个概念,它是一个独立容器，决定了元素如何对其内容进行定位,以及与其他元素的关系和相互作用。）

一个页面是由很多个 Box 组成的,元素的类型和 display 属性,决定了这个 Box 的类型。



不同类型的 Box，会参与不同的 Formatting Context（决定如何渲染文档的容器），因此Box内的元素会以不同的方式渲染，也就是说BFC内部的元素和外部的元素不会互相影响。

**BFC 规定了内部的 Block Box 如何布局。**

定位方案：

1. 内部的 Box 会在垂直方向上一个接一个放置。
2. Box 垂直方向的距离由 margin 决定，属于同一个 BFC 的两个相邻 Box 的 margin 会发生重叠。
3. 每个元素的 margin box 的左边，与包含块 border box 的左边相接触。
4. BFC 的区域不会与 float box 重叠。
5. BFC 是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。
6. 计算 BFC 的高度时，浮动元素也会参与计算。

**满足下列条件之一就可触发 BFC**

1. 根元素，即 html
2. float 的值不为none（默认）
3. overflow 的值不为 visible（默认）
4. display 的值为 inline-block、table-cell、table-caption
5. position 的值为 absolute 或 fixed

**更多关于 BFC 的介绍，请看我的文章 [BFC 是什么？有什么用？](https://juejin.im/post/5cc023c56fb9a0323070d410)**

### 4. CSS选择器有哪些？哪些属性可以继承？CSS优先级算法如何计算？

**CSS选择符：**

\1. id选择器（ # myid）

\2. 类选择器（.myclassname）

\3. 标签(元素)选择器（div, h1, p）

\4. 相邻选择器（h1 + p）

\5. 子选择器（ul > li）

\6. 后代选择器（li a）

\7. 通配符选择器（ * ）

\8. 属性选择器（a[rel = "external"]）



\9. 伪类选择器（a:hover, li:nth-child）

伪元素选择器、分组选择器。

**继承性：**

可继承的样式：font-size, font-family, color，ul，li，dl，dt，dd；

不可继承的样式：border, padding, margin, width, height

优先级（就近原则）：!important > [ id > class > tag ]
!important 比内联优先级高

**优先级算法计算**

优先级就近原则，同权重情况下样式定义最近者为准

!important>id >class>tag



important比内联优先级高

元素选择符的权值：元素标签（派生选择器）：1，class选择符：10，id选择符：100，内联样式权值最大，为1000

1. !important声明的样式优先级最高，如果冲突再进行计算。
2. 如果优先级相同，则选择最后出现的样式。
3. 继承得到的样式的优先级最低。

### 5. CSS3新增伪类有那些?

p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。

p:last-of-type 选择属于其父元素的最后 <p> 元素的每个 <p> 元素。

p:only-of-type 选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。

p:only-child 选择属于其父元素的唯一子元素的每个 <p> 元素。

p:nth-child(2) 选择属于其父元素的第二个子元素的每个 <p> 元素。

:enabled

:disabled 控制表单控件的禁用状态。

:checked，单选框或复选框被选中。

:before在元素之前添加内容，也可以用来做清除浮动

:after在元素之后添加内容

### 6. 如何居中div？如何居中一个浮动元素？如何让绝对定位的div居中？如何居中一个img（position定位）

**水平居中div：**

```
border: 1px solid red;
margin: 0 auto; 
height: 50px;
width: 80px;
```

**水平垂直居中一个浮动元素（position定位）**

第一种：未知元素宽高







```
<div class="outer">
    <span>我想居中显示</span>
</div>
<style>
    .outer{
        width:300px;
        height:300px;
        position:relative;
        background-color:#ccc;
    }
    span{
        float:left;
        position:absolute;
        backgroond-color:red;
        top:50%;
        left:50%;
        transform:translate(-50%,-50%);
    }
</style>
```



第二种：已知元素宽高的

```
<div class="outer">
    <span>我想居中显示</span>
</div>
<style>
    .outer{
        width:300px;
        height:300px;
        position:relative;
        background-color:#ccc;
    }
    span{
        float:left;
        position:absolute;
        backgroond-color:red;
        width:150px;
        height:50px;
        top:50%;
        left:50%;
        margin:-25px 0px 0px -75px;
    }
</style>
```

如何垂直居中一个img（display : table-cell 或者 position定位）

```
<div class="outer">        
    <img src="nz.jpg" alt="">    
</div>
<style>        
    .outer{            
        width: 300px;           
        height: 300px;            
        border: 1px solid #cccccc;            
        display: table-cell;            
        text-align: center;            
        vertical-align: middle;        
    }        
    img{            
        width: 150px;            
        height: 150px;        
    }    
</style>
```

**绝对定位的div水平垂直居中：**

```
border: 1px solid black;
position: absolute;
width: 200px;
height: 100px;
margin: auto;
left: 0;
right: 0; 
top:0;
bottom:0;
```

**还有更加优雅的居中方式就是用****[ ](https://juejin.im/post/5cb2ee5ef265da03474df33f)flex布局，点击查看我的文章 [掌握flex布局，这篇文章就够了](https://juejin.im/post/5cc657ff5188252dce2ae0f4)**

**更多的居中问题，点击查看我的文章 [div居中的几种方法](https://juejin.im/post/5cc657ff5188252dce2ae0f4)**

### 7. display 有哪些值？说明他们的作用?

| 值           | 描述                                                        |
| ------------ | ----------------------------------------------------------- |
| inline       | 默认。此元素会被显示为内联元素，元素前后没有换行符。        |
| block        | 此元素将显示为块级元素，此元素前后会带有换行符。            |
| none         | 此元素不会被显示（隐藏）。                                  |
| inline-block | 行内块元素。（CSS2.1 新增的值）                             |
| list-item    | 此元素会作为列表显示。                                      |
| table        | 此元素会作为块级表格来显示（类似table），表格前后带有换行符 |

### 8. position 的值？

| 值       | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| absolute | 生成绝对定位的元素，相对于 static 定位以外的第一个父元素进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| fixed    | 生成固定定位的元素，相对于浏览器窗口进行定位。（老IE不支持）元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。 |
| relative | 生成相对定位的元素，相对于其正常位置进行定位，不脱离文档流。因此，"left:20" 会向元素的 LEFT 位置添加 20 像素。 |
| static   | 默认值。没有定位，元素出现在正常的文档流中（忽略 top, bottom, left, right 或者 z-index 声明）。 |
| inherit  | 规定应该从父元素继承 position 属性的值。                     |

css 定位还有一个新增属性，**粘性定位 sticky**，它主要用在对 scroll 事件的监听上；

粘性定位可以被认为是相对定位和固定定位的混合。元素在跨越特定阈值前为相对定位，之后为固定定位。例如：

```
#one { position: sticky; top: 10px; }
```

在 viewport 视口滚动到元素 top 距离小于 10px 之前，元素为相对定位。之后，元素将固定在与顶部距离 10px 的位置，直到 viewport 视口回滚到阈值以下。

**更多关于 sticky 的介绍，请点击查看 [粘性定位介绍](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)**

### 9. CSS3有哪些新特性？

1. 新增各种 CSS 选择器 ` :not(p) ` 选择每个非p的元素； `p:empty `选择每个没有任何子级的p元素（包括文本节点）

2. 边框（Borders）：

   

   

   

   ```
   div{ 
       border:2px solid; 
       border-radius:25px; //用于设置圆角
       box-shadow: 10px 10px 5px #888888; //水平阴影 垂直阴影 模糊距离 阴影颜色
       border-image:url(border.png) 30 30 round;// 设置所有边框图像的速记属性。
   }
   ```

   

3. 背景 background-clip（规定背景图的绘制区域），background-origin，background-size

   

   

   

   ```
   div{ 
       background:url(img_flwr.gif); 
       background-repeat:no-repeat; 
       background-size:100% 100%; //规定背景图的尺寸
       background-origin:content-box;//规定背景图的定位区域
   } 
   多背景 
   body{ 
       background-image:url(img_flwr.gif),url(img_tree.gif); 
   }
   ```

   

4. 线性渐变 （Linear Gradients） 向下/向上/向左/向右/对角方向

   

   

   

   ```
   background: linear-gradient(direction, color-stop1, color-stop2, ...);
   ```

   

5. 文本效果 阴影text-shadow，textwrap，word-break，word-wrap；

6. **2D 转换** transform:scale(0.85,0.90) | translate(0px,-30px) | skew(-9deg,0deg) |rotate() **3D转换** perspective()；transform是向元素应用 2D 或者 3D 转换；

7. **过渡** transition

8. **动画**

9. **多列布局** （multi-column layout）

10. **盒模型**

11. **flex 布局**

12. **多媒体查询** 定义两套css，当浏览器的尺寸变化时会采用不同的属性

**更多 CSS3 和 HTML5的新特性，请点击**[ **CSS3 和 HTML5 新特性一览**](https://juejin.im/post/5cbffe386fb9a0322564c0e5)

### 10. 请解释一下 CSS3 的 flexbox（弹性盒布局模型）,以及适用场景？

该布局模型的目的是提供一种更加高效的方式来对容器中的条目进行布局、对齐和分配空间。在传统的布局方式中，block 布局是把块在垂直方向从上到下依次排列的；而 inline 布局则是在水平方向来排列。弹性盒布局并没有这样内在的方向限制，可以由开发人员自由操作。
试用场景：弹性布局适合于移动前端开发，在Android和ios上也完美支持。

**更多关于** **flex 布局，****请点击查看****我的文章 [你真的了解 flex 布局吗？](https://juejin.im/post/5cc6d6f2f265da03b36ef325)**

### 11. 用纯CSS创建一个三角形的原理是什么？

![img](https://user-gold-cdn.xitu.io/2019/4/27/16a5dcb4c320fc4f?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

首先，需要把元素的宽度、高度设为0。然后设置边框样式。

```
width: 0;
height: 0;
border-top: 40px solid transparent;
border-left: 40px solid transparent;
border-right: 40px solid transparent;
border-bottom: 40px solid #ff0000;
```

![img](https://user-gold-cdn.xitu.io/2019/5/4/16a824751ea4c4c0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 12. 一个满屏 品 字布局如何设计?

第一种真正的品字：

1. 三块高宽是确定的；
2. 上面那块用margin: 0 auto;居中；
3. 下面两块用float或者inline-block不换行；
4. 用margin调整位置使他们居中。

第二种全屏的品字布局:
上面的div设置成100%，下面的div分别宽50%，然后使用float或者inline使其不换行。

### 13. 常见的兼容性问题？

1. 不同浏览器的标签默认的margin和padding不一样。解决办法是加一个全局的

   `*{margin:0;padding:0;} `来统一；

2. IE6双边距bug：块属性标签float后，又有横行的margin情况下，在IE6显示margin比设置的大。hack：

   ```
   display:inline; 
   ```

   将其转化为行内属性。渐进识别的方式，从总体中逐渐排除局部。首先，巧妙的使用“9”这一标记，将IE浏览器从所有情况中分离出来。接着，再次使用“+”将IE8和IE7、IE6分离开来，这样IE8已经独立识别。 渐进识别的方式，从总体中逐渐排除局部。首先，巧妙的使用“9”这一标记，将IE浏览器从所有情况中分离出来。接着，再次使用“+”将IE8和IE7、IE6分离开来，这样IE8已经独立识别。

   ```
   {
   background-color:#f1ee18;/*所有识别*/
   .background-color:#00deff\9; /*IE6、7、8识别*/
   +background-color:#a200ff;/*IE6、7识别*/
   _background-color:#1e0bd1;/*IE6识别*/
   }
   ```

3. 设置较小高度标签（一般小于10px），在IE6，IE7中高度超出自己设置高度。hack：给超出高度的标签设置overflow:hidden;或者设置行高line-height 小于你设置的高度。

4. IE下，可以使用获取常规属性的方法来获取自定义属性,也可以使用getAttribute()获取自定义属性；Firefox下，只能使用getAttribute()获取自定义属性。解决方法:统一通过getAttribute()获取自定义属性。

5. Chrome 中文界面下默认会将小于 12px 的文本强制按照 12px 显示,可通过加入 CSS 属性 -webkit-text-size-adjust: none; 解决。

6. 超链接访问过后hover样式就不出现了，因为被点击访问过的超链接样式不再具有hover和active了

   。

   解决方法是改变CSS属性的排列顺序:L-V-H-A :

   

   

   

   

   ```
   a:link {} 
   a:visited {} 
   a:hover {} 
   a:active {}
   ```

7. IE下,even对象有x,y属性,但是没有pageX,pageY属性;

   Firefox下,event对象有pageX,pageY属性,但是没有x,y属性。

   

   **解决方法**：（条件注释）缺点是在IE浏览器下可能会增加额外的HTTP请求数。

8. png24位的图片在iE6浏览器上出现背景，解决方案是做成PNG8.

### 14. 写出几种IE6 BUG的解决方法



\1. 双边距BUG float引起的 使用display

\2. 3像素问题 使用float引起的 使用dislpay:inline -3px

\3. 超链接hover 点击后失效 使用正确的书写顺序 link visited hover active

\4. Ie z-index问题 给父级添加position:relative

\5. Png 透明 使用js代码 改

\6. Min-height 最小高度 !Important 解决’

\7. select 在ie6下遮盖 使用iframe嵌套

\8. 为什么没有办法定义1px左右的宽度容器(IE6默认的行高造成的，使用over:hidden,zoom:0.08 line-height:1px)

\9. ie 6 不支持!important

### 15. 为什么要初始化 CSS 样式

因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。

![img](https://user-gold-cdn.xitu.io/2019/4/27/16a5de34892605f5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 16. absolute 的 containing block(容器块)计算方式跟正常流有什么不同？

无论属于哪种，都要先找到其祖先元素中最近的 position 值不为 static 的元素，然后再判断：

1. 若此元素为 inline 元素，则 containing block 为能够包含这个元素生成的第一个和最后一个 inline box 的 padding box (除 margin, border 外的区域) 的最小矩形；
2. 否则,则由这个祖先元素的 padding box 构成。

如果都找不到，则为 initial containing block。



补充：

\1. static(默认的)/relative：简单说就是它的父元素的内容框（即去掉padding的部分）

\2. absolute: 向上找最近的定位为absolute/relative的元素



\3. fixed: 它的containing block一律为根元素(html/body)，根元素也是initial containing block

### 17. CSS里的 visibility 属性有个 collapse 属性值是干吗用的？在不同浏览器下以后什么区别？

当一个元素的 `**visibility**` 属性被设置成 `**collapse**` 值后，对于一般的元素，它的表现跟 `**hidden**` 是一样的。但例外的是，如果这个元素是table相关的元素，例如table行，table group，table列，table column group，它的表现却跟 `**display: none**` 一样，也就是说，它们占用的空间也会释放。

在谷歌浏览器里，使用 `**collapse**` 值和使用 `**hidden**` 值没有什么区别。

在火狐浏览器、Opera和IE11里，使用 `**collapse**` 值的效果就如它的字面意思：table的行会消失，它的下面一行会补充它的位置。

### 18. display:none 与 visibility:hidden 的区别是什么？

**display : none** 隐藏对应的元素，在文档布局中不再分配空间（回流+重绘）

**visibility:hideen** 隐藏对应的元素，在文档布局中仍保留原来的空间（重绘）

使用 CSS display:none 属性后，HTML 元素（对象）的宽度、高度等各种属性值都将“丢失”;而使用 visibility:hidden 属性后，HTML元素（对象）仅仅是在视觉上看不见（完全透明），而它所占据的空间位置仍然存在。

![img](https://user-gold-cdn.xitu.io/2019/4/27/16a5d7b4b9ffefa2?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 19. position 跟 display、overflow、float 这些特性相互叠加后会怎么样？

display 属性规定元素应该生成的框的类型；position属性规定元素的定位类型；float属性是一种布局方式，定义元素在哪个方向浮动。
类似于优先级机制：position：absolute/fixed优先级最高，有他们在时，float不起作用，display值需要调整。float 或者absolute定位的元素，只能是块元素或表格。

### 20. 为什么会出现浮动? 什么时候需要清除浮动？清除浮动有哪些方式？优缺点是什么？你认为最好的是哪一种？为什么？

**出现浮动的原因：**

浮动元素碰到包含它的边框或者浮动元素的边框停留。在CSS规范中，浮动定位不属于正常的页面流，而是独立定位的，所以文档流的块框表现得就像浮动框不存在一样。浮动元素会漂浮在文档流的块框上。



**关于css的定位机制**：普通流，浮动，绝对定位（position：fixed是position：absolute的一个子类）。浮动的框可以左右移动，直到它的外边缘遇到包含框或者另一个浮动框的边缘，所以才说浮动定位不属于正常的页面流。文档中的普通流就会表现得和浮动框不存在一样，**当浮动框高度超出包含框的时候，就会出现包含框不会自动伸缩高度类笔盒浮动元**素。所以，只含有浮动元素的父容器在显示时不需要考虑子元素的位置，就造成显示父容器像空容器一样。

**浮动带来的问题：**

1. 父元素的高度无法被撑开，影响与父元素同级的元素
2. 与浮动元素同级的非浮动元素（内联元素）会跟随其后
3. 若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构。

**清除浮动的方式**：

1. 父级div定义height
2. 最后一个浮动元素后加空 div 标签 并添加样式 clear:both。（理论上能清除任何标签，增加无意义的标签）
3. 包含浮动元素的父标签添加样式 overflow 为 hidden 或 auto。
4. 父级 div 定义 zoom（空标签元素清除浮动而不得不增加无意义代码的弊端，使用zoom:1用于兼容IE）
5. 用after伪元素清除浮动（用于非IE浏览器）

**1****、父级div定义height**

原理：父级div手动定义height，就解决了父级div无法自动获取到高度的问题

优点：简单，代码少，容易掌握

缺点：只适合高度固定的布局，要给出精确的高度，如果高度和父级div不一样时，会产生问题

建议：不推荐使用，只建议高度固定的布局时使用

**2****、结尾处加空 div 标签 clear:both**

原理：添加一个空div，利用css提高的clear:both清除浮动，让父级div能自动获取到高度

优点：简单，代码少，浏览器支持好，不容易出现怪问题

缺点：不少初学者不理解原理；如果页面浮动布局多，就要增加很多空div，让人感觉很不爽

建议：不推荐使用，但此方法是以前主要使用的一种清除浮动方法

**3****、父级 div 定义 overflow:hidden**

原理：必须定义width或zoom:1，同时不能定义height，使用overflow:hidden时，浏览器会自动检查浮动区域的高度

优点：简单，代码少，浏览器支持好

缺点：不能和position配合使用，因为超出的尺寸的会被隐藏

建议：只推荐没有使用position或对overflow:hidden理解比较深的朋友使用

**4****、父级div定义伪类 :after 和 zoom**

原理：IE8以上和非IE浏览器才支持:after，原理和方法2有点类似，zoom(IE转有属性)可解决ie6,ie7浮动问题

优点：浏览器支持好，不容易出现怪问题（目前：大型网站都有使用，如：腾迅，网易，新浪等等）

缺点：代码多，不少初学者不理解原理，要两句代码结合使用，才能让主流浏览器都支持

建议：推荐使用，建议定义公共类，以减少CSS代码



(1) 在子元素后添加一个空div div{clear:both;}

(2) 在父元素中{overflow:hidden|auto;zoom:1}

(3) :after伪选择符，在父容器的尾部自动创建一个子元素









```
.clearfix:after {
    content: "\0020";display: block;height: 0;clear: both;
}
.clearfix {
    zoom: 1;
}
```







`"clearfix"`是父容器的class名称，`"content:"020";"`是在父容器的结尾处放一个空白符，

`"height: 0;"`是让这个这个空白字符不显示出来，`"display: block; clear: both;"`是确保这个空白字符是非浮动的独立区块。`:after`选择符IE 6不支持,添加一条IE 6的独有命令`"zoom:1;"`就行了.

### 21. 上下 margin 重合的问题

在重合元素外包裹一层容器，并触发该容器生成一个BFC。例子：

```
<div class="aside"></div>
<div class="text">
    <div class="main"></div>
</div>
<!--下面是css代码-->
 .aside {
            margin-bottom: 100px;  
            width: 100px;
            height: 150px;
            background: #f66;
        }
        .main {
            margin-top: 100px;
            height: 200px;
            background: #fcc;
        }
         .text{
/*盒子main的外面包一个div，通过改变此div的属性使两个盒子分属于两个不同的BFC，以此来阻止margin重叠*/
            overflow: hidden;  //此时已经触发了BFC属性。
        }

```

### 22. 设置元素浮动后，该元素的 display 值是多少？

自动变成display:block

### 23. 移动端的布局用过媒体查询吗？

通过媒体查询可以为不同大小和尺寸的媒体定义不同的css，适应相应的设备的显示。

1. <head>里边

   

   

   

   

   ```
   <link rel="stylesheet" type="text/css" href="xxx.css" media="only screen and (max-device-width:480px)">
   ```

2. CSS :

3. 

   

   

   

   ```
   @media only screen and (max-device-width:480px) {
       /css样式/}
   ```

### 24. 什么是CSS 预处理器 / 后处理器？大家为什么要使用他们？

**预处理器**例如：LESS、Sass、Stylus，用来预编译Sass或less，增强了css代码的复用性，还有层级、mixin、变量、循环、函数等，具有很方便的UI组件模块化开发能力，极大的提高工作效率。



**后处理器**例如：PostCSS，通常被视为在完成的样式表中根据CSS规范处理CSS，让其更有效；目前最常做的是给CSS属性添加浏览器私有前缀，实现跨浏览器兼容性的问题。

CSS 预处理器为 CSS 增加一些编程的特性，无需考虑浏览器的兼容性问题”，例如你可以在 CSS 中使用变量、简单的逻辑程序、函数（如右侧代码编辑器中就使用了变量$color）等等在编程语言中的一些基本特性，可以让你的 CSS 更加简洁、适应性更强、可读性更佳，更易于代码的维护等诸多好处。

其它 CSS 预处理器语言：

- Sass（SCSS）
- LESS
- Stylus
- Turbine
- Swithch CSS
- CSS Cacheer
- DT CSS

**为什么要使用它们？**

结构清晰，便于扩展。

可以方便地屏蔽浏览器私有语法差异。这个不用多说，封装对浏览器语法差异的重复处理，减少无意义的机械劳动。

可以轻松实现多重继承。

完全兼容 CSS 代码，可以方便地应用到老项目中。LESS 只是在 CSS 语法上做了扩展，所以老的 CSS 代码也可以与 LESS 代码一同编译。



### 25. CSS优化、提高性能的方法有哪些？

1. 避免过度约束

2. 避免后代选择符

3. 避免链式选择符

4. 使用紧凑的语法

5. 避免不必要的命名空间

6. 避免不必要的重复

7. 最好使用表示语义的名字。一个好的类名应该是描述他是什么而不是像什么

8. 避免！important，可以选择其他选择器

9. 尽可能的精简规则，你可以合并不同类里的重复规则

10. 修复解析错误

11. 避免使用多类选择符

12. 移除空的css规则

13. 正确使用display的属性：由于display的作用，某些样式组合会无效，徒增样式体积的同时也影响解析性能。

    display:inline后不应该再使用width、height、margin、padding以及float。

    display:inline-block后不应该再使用float。

    display:block后不应该再使用vertical-align。

    display:table-*后不应该再使用margin或者float。

14. 不滥用浮动：虽然浮动不可避免，但不可否认很多css bug是由于浮动而引起。

15. 不滥用web字体

    对于中文网站来说Web Fonts可能很陌生，国外却很流行。web fonts通常体积庞大，而且一些浏览器在下载web fonts时会阻塞页面渲染损伤性能。

16. 不声明过多的font-size：这是设计层面的问题，设计精良的页面不会有过多的font-size声明。

17. 不在选择符中使用ID标识符，主要考虑到样式重用性以及与页面的耦合性。

18. 不给h1~h6元素定义过多的样式

19. 全站统一定义一遍heading元素即可，若需额外定制样式，可使用其他选择符作为代替。

20. 不重复定义h1~h6元素

21. 值为0时不需要任何单位

22. 标准化各种浏览器前缀：通常将浏览器前缀置于前面，将标准样式属性置于最后，类似：.foo{

    ```
        -moz-border-radius: 5px;
        border-radius: 5px; 
    }
    ```

23. 使用CSS渐变等高级特性，需指定所有浏览器的前缀

24. 避免让选择符看起来像正则表达式

25. CSS3添加了一些类似~=等复杂属性，也不是所有浏览器都支持，需谨慎使用。

26. 遵守盒模型规则（Beware of broken box models）

### 26. 浏览器是怎样解析CSS选择器的？

**CSS选择器的解析是从右向左解析的，为了避免对所有元素进行遍历**。若从左向右的匹配，发现不符合规则，需要进行回溯，会损失很多性能。若从右向左匹配，先找到所有的最右节点，对于每一个节点，向上寻找其父节点直到找到根元素或满足条件的匹配规则，则结束这个分支的遍历。两种匹配规则的性能差别很大，是因为从右向左的匹配在第一步就筛选掉了大量的不符合条件的最右节点（叶子节点），而从左向右的匹配规则的性能都浪费在了失败的查找上面。
而在 CSS 解析完毕后，需要将解析的结果与 DOM Tree 的内容一起进行分析建立一棵 Render Tree，最终用来进行绘图。在建立 Render Tree 时（WebKit 中的「Attachment」过程），浏览器就要为每个 DOM Tree 中的元素根据 CSS 的解析结果（Style Rules）来确定生成怎样的 Render Tree。

### 27. 在网页中的应该使用奇数还是偶数的字体？为什么呢？

**使用偶数字体**。偶数字号相对更容易和 web 设计的其他部分构成比例关系。Windows 自带的点阵宋体（中易宋体）从 Vista 开始只提供 12、14、16 px 这三个大小的点阵，而 13、15、17 px时用的是小一号的点。（即每个字占的空间大了 1 px，但点阵没变），于是略显稀疏。

### 28. margin 和 padding 分别适合什么场景使用？

**◆何时应当使用margin**

需要在border外侧添加空白时。

空白处不需要背景（色）时。

上下相连的两个盒子之间的空白，需要相互抵消时。如15px+20px的margin，将得到20px的空白。

**◆何时应当时用padding**

需要在border内测添加空白时。

空白处需要背景（色）时。

上下相连的两个盒子之间的空白，希望等于两者之和时。如15px+20px的padding，将得到35px的空白。

**◆浏览器兼容性问题**

在IE5.x、IE6中，为float的盒子指定margin时，左侧margin可能会变成两倍的宽度。通过改用padding或指定盒子为display:inline可以解决。

### 29. 元素竖向的百分比设定是相对于容器的高度吗？

当按百分比设定一个元素的宽度时，它是相对于父容器的宽度计算的，但是，对于一些表示竖向距离的属性，例如 padding-top , padding-bottom , margin-top , margin-bottom 等，当按百分比设定它们时，依据的也是父容器的宽度，而不是高度。

### 30. 全屏滚动的原理是什么？用到了CSS的哪些属性？

1. 原理：有点类似于轮播，整体的元素一直排列下去，假设有5个需要展示的全屏页面，那么高度是500%，只是展示100%，剩下的可以通过transform进行y轴定位，也可以通过margin-top实现
2. overflow：hidden；transition：all 1000ms ease；

### 31. 什么是响应式设计？响应式设计的基本原理是什么？如何兼容低版本的IE？

响应式网站设计(Responsive Web design)是一个网站能够兼容多个终端，而不是为每一个终端做一个特定的版本。

基本原理是通过媒体查询检测不同的设备屏幕尺寸做处理。

页面头部必须有meta声明的viewport。

```
<meta name=’viewport’ content=”width=device-width, initial-scale=1. maximum-scale=1,user-scalable=no”>

```

### 32. 视差滚动效果？

视差滚动（Parallax Scrolling）通过在网页向下滚动的时候，控制背景的移动速度比前景的移动速度慢来创建出令人惊叹的3D效果。

1. CSS3实现
   优点：开发时间短、性能和开发效率比较好，缺点是不能兼容到低版本的浏览器
2. jQuery实现
   通过控制不同层滚动速度，计算每一层的时间，控制滚动效果。
   优点：能兼容到各个版本的，效果可控性好
   缺点：开发起来对制作者要求高
3. 插件实现方式
   例如：parallax-scrolling，兼容性十分好

### 33. ::before 和 :after中双冒号和单冒号有什么区别？解释一下这2个伪元素的作用

1. 单冒号(:)用于CSS3伪类，双冒号(::)用于CSS3伪元素。
2. ::before就是以一个子元素的存在，定义在元素主体内容之前的一个伪元素。并不存在于dom之中，只存在在页面之中。

:before 和 :after 这两个伪元素，是在CSS2.1里新出现的。起初，伪元素的前缀使用的是单冒号语法，但随着Web的进化，在CSS3的规范里，伪元素的语法被修改成使用双冒号，成为::before ::after



注意:对于IE6/7/8仅支持单冒号表示法，而现代浏览器同时支持这两种表示法。另外，在CSS3中单冒号和双冒号的区域主要是用来区分伪类和伪元素的。

### 34. 你对line-height是如何理解的？

行高是指一行文字的高度，具体说是两行文字间基线的距离。CSS中起高度作用的是height和line-height，没有定义height属性，最终其表现作用一定是line-height。
单行文本垂直居中：把line-height值设置为height一样大小的值可以实现单行文字的垂直居中，其实也可以把height删除。
多行文本垂直居中：需要设置display属性为inline-block。

### 35. 怎么让Chrome支持小于12px 的文字？

```
p{
    font-size:10px;
    -webkit-transform:scale(0.8);//0.8是缩放比例
} 

```

### 36. 让页面里的字体变清晰，变细用CSS怎么做？

-webkit-font-smoothing 在 window 系统下没有起作用，但是在 IOS 设备上起作用 -webkit-font-smoothing：antialiased 是最佳的，灰度平滑。

### 37. position:fixed; 在 android 下无效怎么处理 ？

```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no"/>

```



### 38. 如果需要手动写动画，你认为最小时间间隔是多久，为什么？

多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60＊1000ms ＝ 16.7ms。



### 39. li 与 li 之间有看不见的空白间隔是什么原因引起的？有什么解决办法？

行框的排列会受到中间空白（回车空格）等的影响，因为空格也属于字符,这些空白也会被应用样式，占据空间，所以会有间隔，把字符大小设为0，就没有空格了。
解决方法：

1. 可以将<li>代码全部写在一排
2. 浮动li中float：left
3. 在ul中用font-size：0（谷歌不支持）；
4. 可以将 ul{letter-spacing: -4px;};li{letter-spacing: normal;}

### 40. display:inline-block 什么时候会显示间隙？

1. 有空格时候会有间隙 解决：s除空格
2. margin正值的时候 解决：margin使用负值
3. 使用font-size时候 解决：font-size:0、letter-spacing、word-spacing

### 41. 有一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度

外层div使用position：relative；高度要求自适应的div使用position: absolute; top: 100px; bottom: 0; left: 0

### 42. png、jpg、gif 这些图片格式解释一下，分别什么时候用。有没有了解过webp？

1. png是便携式网络图片（Portable Network Graphics）是一种无损数据压缩位图文件格式.优点是：压缩比高，色彩好。 大多数地方都可以用。
2. jpg是一种针对相片使用的一种失真压缩方法，是一种破坏性的压缩，在色调及颜色平滑变化做的不错。在www上，被用来储存和传输照片的格式。
3. gif是一种位图文件格式，以8位色重现真色彩的图像。可以实现动画效果.
4. webp格式是谷歌在2010年推出的图片格式，压缩率只有jpg的2/3，大小比png小了45%。缺点是压缩的时间更久了，兼容性不好，目前谷歌和opera支持。

### 43. style 标签写在 body 后与 body前有什么区别？

页面加载自上而下 当然是先加载样式。
写在 body 标签后由于浏览器以逐行方式对HTML文档进行解析，当解析到写在尾部的样式表（外联或写在 style 标签）会导致浏览器停止之前的渲染，等待加载且解析样式表完成之后重新渲染，在windows的IE下可能会出现 FOUC 现象（即样式失效导致的页面闪烁问题）

### 44. CSS属性overflow属性定义溢出元素内容区的内容会如何处理?

参数是 scroll 时候，必会出现滚动条。
参数是 auto 时候，子元素内容大于父元素时出现滚动条。
参数是 visible 时候，溢出的内容出现在父元素之外。
参数是 hidden 时候，溢出隐藏。

### 45. CSS Sprites是什么？它的优势和劣势？

CSS Sprites小图片背景共享技术。它把一堆小的图片整合到一张大的图片上。然后利用CSS的 background-image，background- repeat，background-position 的组合进行背景定位。利用CSS Sprites能很好地减少网页的http请求，从而大大的提高页面的性能；CSS Sprites能减少图片的字节。

**优势：**

1.很好的减少网页的请求，大大提高页面的性能；

2.减少图片的字节；

3.解决了网页设计师在图片命名上的困扰；

4.更换风格方便，维护方便。

**劣势：**

1.图片合并时需预留好足够空间，宽屏、高分辨率的屏幕下易出现背景断裂；

2.开发较麻烦，测量繁琐；（可使用样式生成器）



3.维护麻烦，背景少许改动有可能影响整张图片，使得字节增加还要改动css。

### 46. CSS 伪类和伪元素的区别？

**第一种回答：**

伪类： :focus、:hover、:active

伪元素：:before、:after

伪类本质上是为了弥补常规CSS选择器的不足，以便获取到更多信息；

伪元素本质上是创建了一个有内容的虚拟容器；

CSS3中伪类和伪元素的语法不同；



可以同时使用多个伪类，而只能同时使用一个伪元素；

**第二种回答：**



**伪类**：伪类选择元素基于的是当前元素处于的状态，或者说元素当前所具有的特性，而不是元素的id、class、属性等静态的标志。由于状态是动态变化的，所以一个元素达到一个特定状态时，它可能得到一个伪类的样式；当状态改变时，它又会失去这个样式。由此可以看出，它的功能和class有些类似，但它是基于文档之外的抽象，所以叫伪类。

```
:first-child  :link:  :visitive  :hover  :active  :focus  :lang
```

![img](https://user-gold-cdn.xitu.io/2019/4/28/16a641f059343d90?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**伪元素**：与伪类针对特殊状态的元素不同的是，伪元素是对元素中的特定内容进行操作，它所操作的层次比伪类更深了一层，也因此它的动态性比伪类要低得多。实际上，设计伪元素的目的就是去选取诸如元素内容第一个字（母）、第一行，选取某些内容前面或后面这种普通的选择器无法完成的工作。它控制的内容实际上和元素是相同的，但是它本身只是基于元素的抽象，并不存在于文档中，所以叫伪元素。









```
:first-line 
:first-letter 
:before 
:after
```

### 47.有哪项方式可以对一个 DOM 设置它的CSS样式？　　

外部样式表，引入一个外部css文件

内部样式表，将css代码放在 <head> 标签内部

内联样式，将css样式直接定义在 HTML 元素内部

### 48. CSS 中可以通过哪些属性定义，使得一个 DOM 元素不显示在浏览器可视范围内？　　

**最基本的：**设置 display 属性为 none，或者设置 visibility 属性为 hidden

**技巧性：**设置宽高为 0，设置透明度为 0，设置 z-index 位置在 -1000

### 49. 什么是 Css Hack？ie6,7,8 的 hack 分别是什么？

答案：解决各浏览器对 CSS 解释不同所采取的，区别不同浏览器制作不同CSS样式的设置就叫作 CSS Hack。

![img]()![img](https://user-gold-cdn.xitu.io/2019/4/26/16a59a128cdc47ec?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 50. 行内元素和块级元素的具体区别是什么？行内元素的 padding 和 margin 可设置吗？

**块级元素( block )特性：**

总是独占一行，表现为另起一行开始，而且其后的元素也必须另起一行显示；

宽度(width)、高度(height)、内边距(padding)和外边距(margin)都可控制；

**内联元素(inline)特性：**

和相邻的内联元素在同一行;

宽度(width)、高度(height)、内边距的top/bottom(padding-top/padding-bottom)和外边距的top/bottom(margin-top/margin-bottom)都不可改变（也就是padding和margin的left和right是可以设置的）。

　　**那么问题来了，浏览器还有默认的天生inline-block元素（拥有内在尺寸，可设置高宽，但不会自动换行），有哪些？**

　　答案：<input> 、<img> 、<button> 、<textarea>。

**相关文章 [为何img、input等内联元素可以设置宽高](https://blog.csdn.net/jlds123/article/details/8647448)**

### 51. 什么是外边距重叠？重叠的结果是什么？

　　外边距重叠就是 margin-collapse。

　　在CSS当中，相邻的两个盒子的外边距可以结合成一个单独的外边距。这种合并外边距的方式被称为折叠，并且因而所结合成的外边距称为折叠外边距。

　　折叠结果遵循下列计算规则：

1. 两个相邻的外边距都是正数时，折叠结果是它们两者之间较大的值。
2. 两个相邻的外边距都是负数时，折叠结果是两者绝对值的较大值。
3. 两个外边距一正一负时，折叠结果是两者的相加的和。　　

### 52. rgba() 和 opacity 的透明效果有什么不同？

rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，

　　而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）

### 53. css 中可以让文字在垂直和水平方向上重叠的两个属性是什么？

　　垂直方向：line-height

　　水平方向：letter-spacing (letter-spacing 属性增加或减少字符间的空白)

　　**那么问题来了，关于letter-spacing的妙用知道有哪些么？**

　　答案:可以用于消除inline-block元素间的换行符空格间隙问题。

### 54. px 和 em 的区别。

px和em都是长度单位，区别是：px的值是固定的，指定是多少就是多少，计算比较容易。em得值不是固定的，并且em会继承父级元素的字体大小。

　　浏览器的默认字体高都是16px。所以未经调整的浏览器都符合: 1em=16px。那么12px=0.75em, 10px=0.625em。

### 55. 描述一个"reset"的CSS文件并如何使用它。知道normalize.css吗？你了解他们的不同之处？　

不同的浏览器对一些元素有不同的默认样式，如果不处理，在不同的浏览器下会存在必要的风险。

　　你可能会用[Normalize](http://necolas.github.io/normalize.css/)来代替你的重置样式文件。它没有重置所有的样式风格，但仅提供了一套合理的默认样式值。既能让众多浏览器达到一致和合理，但又不扰乱其他的东西（如粗体的标题）。

### 56. 问:translate()方法能移动一个元素在z轴上的位置？

不能。它只能移动x,y轴的位置。translate3d可以。

### 57. CSS3有哪些新特性？

\1. CSS3实现圆角（border-radius），阴影（box-shadow），

\2. 对文字加特效（text-shadow、），线性渐变（gradient），旋转（transform）

\3. transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);// 旋转,缩放,定位,倾斜

\4. 增加了更多的CSS选择器 多背景 rgba

\5. 在CSS3中唯一引入的伪元素是 ::selection.

\6. 媒体查询，多栏布局

\7. border-image

### 58. xhtml 和 html 有什么区别

**功能上的差别**：HTML是一种基本的WEB网页设计语言；XHTML是一个基于XML的置标语言，XHTML可兼容各大浏览器、手机以及PDA，并且浏览器也能快速正确地编译网页。



**书写习惯上：**

(1) XHTML 所在标签都必须小写

(2) XHTML 标签必须成双成对

(3) XHTML 标签顺序必须正确

(4) XHTML 所有属性都必须使用双引号

(5) 不XHTML 允许使用target=”_blank”

(6) XHTML 文档必须拥有根元素。

(7) XHTML 元素必须被正确地嵌套。

### 59. 前端页面有哪三层构成，分别是什么?



结构层 Html 表示层 CSS 行为层 js

### 60. css的基本语句构成是?



选择器{属性1:值1;属性2:值2;……}

### 61. 与 HTML4 比较，HTML5 废弃了哪些元素？



废弃的元素包括：

· frame

· frameset

· noframe

· applet

· big

· center

· basefront

### 62. CSS引入的方式有哪些？使用Link和@import有什么区别？

答：内联，内嵌，外链，导入

（1）link 属于 XHTML 标签，除了加载 CSS 外，还能用于定义RSS，定义 rel 连接属性等作用，无兼容性，支持使用javascript改变样式；而@import是CSS提供的，只能用于加载CSS，不支持使用 javascript 改变样式；

（2）页面被加载的时，link 会被同时加载，而@import 引用的CSS会等到页面加载完再加载；

（3）import是CSS2.1 提出的，CSS2.1以下浏览器不支持，只在IE5以上才能被识别，而link是XHTML标签，无兼容问题。

### 63. 介绍一下你对浏览器内核的理解

主要分成两部分：渲染引擎（layout engineer或Rendering Engine）和 JS 引擎。

（1）渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户以及其他它所需要编辑、显示网络的应用程序都需要内核。

（2）JS引擎则：解析和执行JavaScript 来实现网页的动态效果；

最开始渲染引擎和 JS 并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只渲染引擎。

### 64. 常见的浏览器内核有哪计算些？

**Trident内核**：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]

**Gecko内核**：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等。

**Presto内核**：Opera7及以上。[Opera内核原为：Presto，现为：Blink;]

**Webkit内核**：Safari,Chrome等。[ Chrome的：Blink（WebKit的分支）]

**EdgeHTML内核**：Microsoft Edge。[此内核其实是从MSHTML fork而来，删掉了几乎所有的IE私有特性]

### 65. 对WEB标准以及W3C的理解与认识

标签闭合、标签小写、不乱嵌套、提高搜索机器人搜索几率、使用外链css和js脚本、结构行为表现的分离、文件下载与页面速度更快、内容能被更多的用户所访问、内容能被更广泛的设备所访问、更少的代码和组件，容易维护、改版方便，不需要变动页面内容、提供打印版本而不需要复制内容、提高网站易用性;

### 66. 为什么HTML5里面我们不需要DTD（Document Type Definition文档类型定义）？



HTML5没有使用SGML或者XHTML，他是一个全新的东西，因此你不需要参考DTD，对于HTML5，你仅需放置下面的文档类型代码告诉浏览器识别这是HTML5文档

### 67. 如果我不放入，HTML5还会工作么？



不会，浏览器将不能识别他是HTML文档，同时HTML5的标签将不能正常工作

### 68. HTML5 中的 datalist 是什么？



HTML5中的Datalist元素有助于提供文本框自动完成特性，如下图所示：

![img](https://user-gold-cdn.xitu.io/2019/4/27/16a5d9d22590a9ed?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



### 69. CSS中使用列布局是什么？

CSS列布局帮助你分割文本变为列，例如考虑下面的杂志新闻在一个大的文本中，但是我们需要在他们之间使用边界划分为3列，这里HTML5的列布局就有所帮助了



![img]()![img](https://user-gold-cdn.xitu.io/2019/4/27/16a5d9ddd53f7691?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2019/4/27/16a5d9eadc51d9bd?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 70. 如何水平并且垂直居中一张背景图

设置 background-position:center;

### 71. font-style属性可以让它赋值为“oblique”是什么意思

让一种字体标识为斜体(oblique)，如果没有这种格式，就使用italic字体

### 72. 如何理解 CSS 的继承和重用？

继承：在一个属性应用于一个特定标签之后，该标签的子标签也应该应用该属性。这一行为称为继承。但并不是所有的属性都会被继承。如文字样式一般都继承，盒图的宽高一般不 继承，总之该继承的都会继承，不该继承的都不继承。

重用：一个样式文件，可以多个页面使用，这对于一些公共样式的重构是很有用的。

### 73. 制作一个访问量很高的大型网站，你会如何来管理所有CSS文件,js 与图片？



答案：涉及到人手、分工、同步

（1） 先期团队必须确定好全局样式，编码模式等

（2） 编写习惯必须一致

（3） 标注样式编写人，各模块都及时标注（标注关键样式调用的地方）

（4） 页面进行标注

（5） Css与html分文件夹并行存放，命名都要统一

（6） Js分文件夹存放，命名以该JS功能为准英文翻译

（7） 图片采用整合的.png格式文件使用，尽量整合在一起，方便将来的管理。

### 74. 强制换行的css是什么？



Word-break:break-all;

![img](https://user-gold-cdn.xitu.io/2019/5/4/16a8247f39a7a1d3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



## (HTML和CSS)测试题：

### 1. 简述的作用。

<!DOCTYPE> 声明位于文档中的最前面的位置，处于 <html> 标签之前。

它不是一个 HTML 标签，它是用来告知（声明） Web 浏览器页面使用了哪种 HTML 版本

### 2. 行内元素和块级元素的区别是是什么。

**行内元素：**

（1）行内元素不换行

（2）行内元素不可以设置大小

（3）行内元素大小由内容决定

**块元素**：

（1）块元素独立成行

（2）块元素可以设置大小

（3）块元素如果不设置宽度，宽度会自适应其父级的宽度

### 3. 列举常用行内元素和块元素，并解释其作用

**行元素：**

span、img、a、lable、input、abbr（缩写）、em（强调）、big、cite（引用）、i（斜体）、q（短引用）、textarea、select（下拉列表）、small、sub、sup，strong、u（下划线）、button（默认display：inline-block）



从浏览器的显示结果可以看出，相邻的行内元素不换行，宽度即为内容的宽度、padding的4个方向都有效（从span标签可以看出，对于行内非替换元素，不会影响其行高，不会撑开父元素；而对于替换元素，则会撑开父元素）、margin只有水平方向有效（其中垂直方向的margin对行内替换元素（比如img元素）有效，对行内非替换元素无效）、不可以设置width和height属性。行内块元素表现其实和行内元素一样，只是其可以设置width和height属性。

**块元素：**

div、p、h1~h6、ul、ol、dl、li、dd、table、hr、blockquote、address、table、menu、pre，HTML5新增的header、section、aside、footer等



从浏览器的显示结果可以看出，块级元素新开启一行（即使是设置了width属性也是独占一行）、尽可能撑满父级元素的宽度，可以设置width和height属性；table元素浏览器默认的display属性为table。

### 4. 让行内元素水平居中的两种方法

(1) 找到对应其标签的父级，给其父级设置 text-align : center；

(2) 将元素转化成块元素，设置margin : 0 auto，（必须是块元素，而且有宽度）

### 5. 如何实现一个标题文字居中？text-align : center

### 6. 使块元素在一行显示。float : left

### 7. 文字加粗，以及文字字体倾斜

**加粗：**

**CSS：**font-weight : bold

**HTML :** <b></b>，<strong></strong>

**斜体：**

**CSS :** font-style : italic | oblique，font-style : normal（正常的字体）

**HTML :** <i></i> ，<em></em>

### ✳8. 解决 img 图片自带边距的问题

谷歌中这样是解释的：

图片底部的空隙实际上涉及行内元素的布局模型，图片默认的垂直对齐方式是基线，而基线的位置是与字体相关的。所以在某些时候，图片**底部**的空隙可能是 2px，而有时可能是 4px 或更多。不同的 font-size 应该也会影响到这个空隙的大小。

**解决办法：**

最优的解决办是定义vertical-align，注：定义vertical-align为middle时在IE6中大概还有一像素的顶边距，最好为top或bottom。当然还有种极端解决办法大家可以试试就是将 `父容器的字体大小为零，font-size:0`

（1）转化成（行级）块元素









```
 display : block
```



（2）浮动，浮动后的元素默认可以转化为块元素（可以随意设置宽高属性）









```
float ： left；
```



（3）给 img 定义 vertical-align（消除底部边距）









```
img{    
    border: 0;    
    vertical-align: bottom;
}
```



（4）将其父容器的font-size 设为 0；

（5）给父标签设置与图片相同的高度

### 9. 如果使用了 float 的元素，没有实现并排效果，可能出现的问题？

父级的宽度不够，导致元素无法并排显示；

### 10. 设置css方式有哪些？

行内样式、内部样式（写一个style）、外部样式

### 11. CSS 内边距和外边界用什么属性表示？









```
padding margin
```

### 12. 清除所有的默认边距。









```
padding : 0;
margin : 0;
```

### 13. 单词间距与字母间距属性，中文应用哪一种？

word-spacing : 单词

letter-spacing : 字母、中文

### 14. 文字如何加下划线，上划线，删除线

text-decoration : underline | overline | line-through

### 15. 清除列表符号，背景改成图片







```
list-style:none;
background-image:url()
```



### 16. 边框（boder）得基本属性都有哪些？

宽度（width）

颜色（color）

线形（solid，dashed，dotted，）实线，虚线，点画线

### 17. 行元素如何转化为块元素，或者 行级块元素，行级块元素有什么特点

diaplay：block | inline-block

### 18. 标签上 title 属性与 alt 属性的区别是什么？

title ：鼠标放入时提示的文字，

alt : 图片路径出错时，提示文字

### 19. 如何防止出现乱码？

<meta charset="UTF-8">