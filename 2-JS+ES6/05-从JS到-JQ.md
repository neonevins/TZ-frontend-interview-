# 05-从JS到-JQ



### 1. 闭包

> **1. 涉及面试题：什么是闭包？**

闭包的定义其实很简单：函数 A 内部有一个函数 B，函数 B 可以访问到函数 A 中的变量，那么函数 B 就是闭包。







```
function A(){                 
    let a=1;                 
    window.B=function(){                     
        console.log(a)                 
    }            
}            
A();            
B();//1复制代码
```



很多人对于闭包的解释可能是函数嵌套了函数，然后返回一个函数。其实这个解释是不完整的，就比如我上面这个例子就可以反驳这个观点。

在 JS 中，闭包存在的意义就是让我们可以间接访问函数内部的变量。

> **2. 经典面试题：循环中使用闭包解决`var `定义函数的问题；**

需求：每隔一秒输出一个数字，从 0 - 5；







```
for(var i=0;i<=5;i++){            
    setTimeout(function timer(){                
        console.log(i)            
    },i*1000)        
}        
console.log(i)复制代码
```



输出结果：立即输出一个6，然后每隔一秒输出一个6；

首先因为 `setTimeout` 是个异步函数，所以会先把循环全部执行完毕，这时候 `i` 就是 6 了，所以会输出一堆 6。

解决办法有3种，

**第一种是利用闭包的方式：**







```
for(var i=0;i<=5;i++){            
    (function(j){                
        setTimeout(function timer(){                    
            console.log(j)                
        },j*1000)            
    })(i)        
}复制代码
```



在上述代码中，我们首先使用了立即执行函数将 `i` 传入函数内部，这个时候值就被固定在了参数 `j`上面不会改变，当下次执行 `timer` 这个闭包的时候，就可以使用外部函数的变量 `j`，从而达到目的。

**第二种就是使用** `**setTimeout**` **的第三个参数，这个参数会被当成** `**timer**` **函数的参数传入。**







```
for(var i=0;i<=5;i++){           
    setTimeout((j) => {                
        console.log(j);            
    },i*1000,i)        
}复制代码
```



`setTimeout`还允许更多的参数。它们将依次传入推迟执行的函数（回调函数）。







```
setTimeout((a,b,c) => {            
    console.log(a,b,c)        
}, 2000, "my", "name", "is starsion");
//my name is starsion复制代码
```



**更多关于setTimeOut的用法，请参考阮一峰老师的 [异步操作 之 定时器](https://wangdoc.com/javascript/async/timer.html#settimeout)**

**第三种就是使用** `**let**` **定义** `**i**` **了来解决问题了，这个也是最为推荐的方式**







```
for(let i=0;i<=5;i++){                
    setTimeout(() => {                    
        console.log(i)                
    },i*1000)            
}复制代码
```



**更多关于let和var的区别，请**[**参考阮一峰老师的 let命令**](http://es6.ruanyifeng.com/#docs/let)

### 2. JS作用域

ES6 之前 JS 没有块级作⽤域。例如 







```
if (true) {
    var name = 'zhangsan'
}
console.log(name)复制代码
```



从上⾯的例⼦可以体会到作⽤域的概念，**作⽤域就是⼀个独⽴的地盘，让变量不会外泄、暴露出去。** 上⾯的 name 就被暴露出去了，因此，JS 没有块级作⽤域，只有全局作⽤域和函数作⽤域。

但是 ES6 中开始加⼊了块级作⽤域，使⽤ let 定义变量即可







```
if (true) {
 let name1 = 'zhangsan'
}
console.log(name1) // 报错，因为let定义的name是在if这个块级作⽤域复制代码
```



**1.       js作用域（全局作用域 和 函数作用域）内部可以访问外部，但外部的不能访问内部的**







```
var a=10;        
function aaa(){             
    alert(a);        
};        
aaa();//10复制代码
```









```
function aaa(){            
    var a=10;          
};        
aaa();        
console.log(a)//Uncaught ReferenceError: a is not defined复制代码
```









```
var a=10;         
function aaa(){             
    console.log(a);//10        
};                    
function bbb(){            
    var a=20;            
    aaa();        
}        
bbb();//10复制代码
```









```
function aaa(){            
    a=10;         
}        
aaa();        
function aaa(){            
    var a=b=10;         
}      
aaa();      
console.log(b)//10    
console.log(a)//Uncaught ReferenceError: a is not defined复制代码
```

**2. 不用var 定义变量时，会默认为是全局变量（不规范，不推荐）**







```
function aaa(){            
    a=10;         
}        
aaa();        
function aaa(){            
    var a=b=10;         
}        
aaa();        
console.log(b)        
console.log(a)复制代码
```



**3.** **变量的查找是就近原则去寻找，定义的****var****变量；**

变量的声明被提前到作用域顶部，赋值保留在原地，如下 domo ;







```
function aaa(){            
console.log(a);//undefined            
var a=20;        
}        
aaa(); 复制代码
```









```
var a=10;        
function aaa(){            
    console.log(a);//undefined            
    var a=20;        
}        
aaa();复制代码
```









```
var a=10;        
function aaa(a){             
    console.log(a);//10            
    var a=20;  //因为 a 是形参，优先级高于 var a; 所以 局部变量a的声明其实被忽略了。        
}         
aaa(a);复制代码
```



### 3. 作用域链

⾸先认识⼀下什么叫做 **⾃由变量** 。如下代码中，` console.log(a)` 要得到 `a` 变量，但是在当前的作 ⽤域中没有定义 `a` （可对⽐⼀下 `b` ）。当前作⽤域没有定义的变量，这成为 **⾃由变量** 。⾃由变量如 何得到 —— 向⽗级作⽤域寻找。







```
var a = 100;
function fn() {
    var b = 200;
    console.log(a);
    console.log(b);
};
fn();复制代码
```



如果⽗级也没呢？再⼀层⼀层向上寻找，直到找到全局作⽤域还是没找到，就宣布放弃。这种⼀层⼀ 层的关系，就是 **作⽤域链** 。







```
var a = 100;
function F1() {
    var b = 200;
    function F2() {
        var c = 300;
        console.log(a) // ⾃由变量，顺作⽤域链向⽗作⽤域找
        console.log(b) // ⾃由变量，顺作⽤域链向⽗作⽤域找
        console.log(c) // 本作⽤域的变量
    };
    F2();
};
F1();复制代码
```



### 作用域附加题：

**1. 判断输出结果，并且解释原因？**







```
var a = 1;        
(function a () {            
    a = 2;            
    console.log(a);        
})();
// 输出结果
ƒ a () {
           a = 2;
           console.log(a);
       }复制代码
```



这道题，猛地一看，很多人都会觉得，console.log(a) 的值为 2，其实不然，

立即调用的函数表达式（IIFE） 有一个 自己独立的 作用域，如果函数名称与内部变量名称冲突，就会永远执行函数本身；所以上面的结果输出是函数本身；

如果将函数名字改一下，比如改为 x

```
var a = 1;        
(function x () {            
    a = 2;            
    console.log(a);        
})();
// 2复制代码
```



函数表达式中的函数名称，只有自己独立的作用域可以拿到，不会影响全局

```
var a = 1;        
(function a () {            
    a = 2;            
    console.log(window.a);        
})();
// 1复制代码
var a = 1;        
(function x () {            
    a = 2;            
    console.log(a);        
})();
// 2复制代码
```

## JS 基础面试题

### 1.介绍下 js 中关于arguments 。 

【考点：函数arguments】



在函数代码中，使用特殊对象 arguments，开发者无需明确指出参数名，就能访问它们。



例如，在函数 sayHi() 中，第一个参数是 message。用 arguments[0] 也可以访问这个值，即第一个参数的值（第一个参数位于位置 0，第二个参数位于位置 1，依此类推）。

因此，无需明确命名参数，就可以重写函数：







```
function sayHi() {  
    if (arguments[0] == "bye") {    
        return;  
    }  
    alert(arguments[0]);
}复制代码
```



**[点击查看关于 arguments 对象](https://wangdoc.com/javascript/types/function.html#arguments-对象)**

### 2.看以下JavaScript程序问：执行以上程序后，num的值为（    D   ）

【考点：数据类型】





```
var num;              
num=5+true;                     
A、true  B、false  C、5  D、6复制代码
```

**解析：true 的值为1，false 的值为0；如下代码**







```
console.log(true==1)//true
console.log(true===1)//false
console.log(false==0)//true
console.log(false===0)//false复制代码
```



### 3、看以下JavaScript程序【考点：switch语句,break】











```
var   x=prompt("请输入1-5的数字","");                     
switch (x) {                            
    case “1”:alert(“one”);                            
    case “2”:alert(“two”);                            
    case “3”:alert(“three”);                            
    case “4”:alert(“four”);                            
    case “5”:alert(“five”);                            
    default:alert(“none”);
}复制代码
```

运行以上程序，在提示对话框中输入“4”，依次弹出的对话框将输出: （    B      ）









```
A、four,none   B、four,five,none     C、five    D、five,none复制代码
```

**解析：因为执行完之后，没有加break，会一直往下执行代码。**

### 4、分析下面的JavaScript代码段

输出结果是(B).(选择一项) 【考点：for循环】











```
a=new Array(2,3,4,5,6);
sum=0;
for(i=1;i<a.length;i++ )   
sum +=a[i];
document.write(sum);          
A.  20  B.  18  C.  14  D. 12复制代码
```

**注意：i 是从 1 开始循环的，不要掉到坑里。**

### 5、在HTML中，Location对象的()属性用于设置或检索URL的端口号。（B）  



​      【考点：location对象】









```
A.  hostname   B.  Port    C. pathname  D.  href复制代码
```

**解析：**`**Location**`**对象提供以下属性。**

- `Location.href`：整个 URL。
- `Location.protocol`：当前 URL 的协议，包括冒号（`:`）。
- `Location.host`：主机，包括冒号（`:`）和端口（默认的80端口和443端口会省略）。
- `Location.hostname`：主机名，不包括端口。
- `Location.port`：端口号。
- `Location.pathname`：URL 的路径部分，从根路径`/`开始。
- `Location.search`：查询字符串部分，从问号`?`开始。
- `Location.hash`：片段字符串部分，从`#`开始。
- `Location.username`：域名前面的用户名。
- `Location.password`：域名前面的密码。
- `Location.origin`：URL 的协议、主机名和端口。

**更多关于**[ **location对象的知识，请点击查看。**](https://wangdoc.com/javascript/bom/location.html)

### 6、分析下面的javascript代码： 【考点：+ 的用法，加法和连接符】











```
x=11;
y="number";
m= x+y ;
m的值为（A）   
A. 11number  B.  number C.  11  D.  程序报错复制代码
```

### 7、setInterval("alert('welcome');",1000);【考点：定时器的用法】











```
这段代码的意思是(D)     
A.  等待1000秒后，再弹出一个对话框     
B.  等待1秒钟后弹出一个对话框     
C.  语句报错,语法有问题  
D. 每隔一秒钟弹出一个对话框复制代码
```

### 8、分析下面的JavaScript代码段：【考点：Math对象的方法使用，round 四舍五入】











```
var a=15.49;
document.write(Math.round(a));
输出的结果是(A)        
A.  15  B. 16  C.  15.5 D.  15.4复制代码
```

解析：`Math.ceil()` 执行向上舍入

`Math.floor() `执行向下舍入

`Math.round() `执行标准舍入（四舍五入）

**更多****[关于math对象的知识点击查看 Math对象](https://juejin.im/post/5c2c331bf265da615a41ebec)** 

**[阮一峰的 标准库 math对象](https://wangdoc.com/javascript/stdlib/math.html#mathround)**

### 9、分析如下的JavaScript代码片段, b的值为(C ) 

【考点：parseInt取整】











```
var a = 1.5,b; 
b=parseInt(a);     
A.  2  B. 0.5  C.  1   D. 1.5复制代码
```

**解析：parseInt()函数将字符串转化为整数，他从字符串的开头开始解析，在第一个非整数位停止解析，并且返回前面读到的所有整数，如果字符串不以整数开头，将返回NaN(Not a Number：非数字值)。**

**点击查看 [parseInt() 用法](https://wangdoc.com/javascript/types/number.html#parseint)**

### 10、在表单(form1)中有一个文本框元素(fname)，用于输入电话号码，格式如：010-82668155，要求前3位是010，紧接一个“-”，后面是8位数字。要求在提交表单时，根据上述条件验证该文本框中输入内容的有效性，

【考点：substr（） 截取字符串的使用，isNaN 判断是否为数字】









```
var str= form1.fname.value;        
if(str.substr(0,4)!="010-" ||str.substr(4).length!=8 || isNaN(parseFloat(str.substr(4))))
alert("无效的电话号码！");复制代码
```

**解析：**

**substr(m,n) 截取一段字符，两个参数m，n，表示从m位开始（不包括m），向后边截取n位；如果只写一个参数m，会从这个参数后边全部截取；**

**isNaN判断是否NaN（不是数字），如果该字符里不是全数字，则返回true；如果是全数字，则返回false。**

### 11、以下哪个单词不属于javascript保留字：（b）

【考点：javascript保留字，防止命名出现使用保留字而产生错误】**
**









```
A.with      B.parent     C.class     D.void复制代码
```









```
关键字26
breakcase  catch  continue  debugger  default  delete  doelsefinally  for  function  
if  in  instance  new  return  switch  this   throw  try  typeofvar  voidwhile  with

保留字
abstract  boolean  bytechar class  const  double  enum   export   extends  final float
goto  implements   import  int  interface  long  native  package  private  protected 
public  short   static  super  synchronized  throws  transient  volatile复制代码
```

**在JavaScript引擎中使用关键字做标识符会导致"Identifier Expected"错误**

### 12、请选择结果为真的表达式：（  C   ） 



【考点：对null，undefined，NaN的理解】











```
A.null instanceof Object //false
B.null === undefined  //false
C.null == undefined //true
D.NaN == NaN //false复制代码
```

**解析：点击查看 [NaN的运算规则](https://wangdoc.com/javascript/types/number.html#特殊数值) [null和undefined](https://wangdoc.com/javascript/types/null-undefined-boolean.html)**

### 13、以下哪个运算符不属于逻辑运算符？（ C  ）

【考点：逻辑运算符和位运算符】











```
1.A、&&        B、||          C、^         D、!复制代码
```

**解析：布尔运算符用于将表达式转为布尔值，一共包含四个运算符。前三个称为逻辑运算符**

- **取反运算符：**`**!**`
- **且运算符：**`**&&**`
- **或运算符：**`**||**`
- **三元运算符：**`**?:**`

**点击查看更多 [关于运算符的知识](https://wangdoc.com/javascript/operators/bit.html)**

### 14、下面定义变量中错误的是（  D  ）。

【考点：变量定义规则】











```
A、 eee      B、 _abc      C、box_1    D、 2point复制代码
```

**解析：**

- 变量由字母，数字，下划线或者美元符号$组成；

- 第一个字符可以是字母，下划线或美元符号 $ ；

- 

  后续的字符可以使字母、数字、下划线或美元符号；

  

- 

  变量名称区分大小写

  

- 

  保留字、关键字、JavaScript预定义了很多全局变量和函数不能用作自定义变量名和函数名

  

- 约定俗称：标识符要见名知意；

  驼峰式命名法：又叫小驼峰式命名法。一般用来命名变量，用来解释描述变量

  例如：`var planeBulletSpeed; //飞机子弹的速度`

### 15、下面语句var x= -10, y; x=2*x; y=x+15;计算后y的结果是（  C  ）。



【考点：四则运算】









```
A.-15      B、10        C、-5      D、5复制代码
```

### 16、下列表达式运算结果为真的是（ B D   ）。

【考点：逻辑运算】点击查看 **[逻辑运算符](https://wangdoc.com/javascript/operators/boolean.html)**











```
A、1<2 && "5" !=5   //false
B、2>2*1 || "5" ==5  //true
C、2>2*1 && 5 ==5   //false
D、1<2 && "5" ==5  //true复制代码
```

### 17、以下程序段，执行的结果是（  C  ）。

【考点：while循环，避免出现死循环，除非有需要】











```
var x=-1; 
do{   
    x=x*x;  
}
while(!x);
A、是死循环          B、循环执行二次      C、循环执行一次       D、有语法错误复制代码
```

**解析：!1 的值为false，就是说满足循环体满足 为false 的是时候的执行代码块**

### 18.执行语句for(i=1;i++<10; );后变量i的值是（  C  ）。



【考点：for循环，什么时候跳出循环】









```
A、9            B、10             C、11              D、不定复制代码
```

**解析：**







```
for(var i=1;i++<10;){    
console.log(i)}
console.log(i)//11
// 2 3 4 5 6 7 8 9 10 11复制代码
```



### 19、键盘事件中不包括的是（ B   ）。

【考点：键盘事件】











```
A、keydown         B、keyover         C、keypress          D、keyup复制代码
```

**解析：点击查看 [鼠标事件](https://wangdoc.com/javascript/events/mouse.html) [键盘事件](https://wangdoc.com/javascript/events/keyboard.html)**

**键盘事件由用户击打键盘触发，主要有`keydown`、`keypress`、`keyup`三个事件，它们都继承了`KeyboardEvent`接口。
**

### 20、执行以下脚本语句在页面输出（  true  ）。

【考点：四则运算和逻辑运算综合应用】











```
var a=3;
a+=8*2;
alert(a>10&&a<=20);复制代码
```

### 21、预测以下代码片段的输出结果 var str ;（      B    ）

 ***\*alert(typeof str);\**** 【考点：变量的定义和typeof 操作符的使用】











```
A;string    B:undefined     C:Object      D:String复制代码
```

### 22.以下哪项不属于Javascript的特征？ 

【考点：JavaScript语言特性】C









```
A.Javascript是一种脚本语言 
B.Javascript是事件驱动的 
C.Javascript代码需要编译以后才能执行
D.Javascript是独立于平台的复制代码
```

**解析：JavaScript是一门脚本语言。弱数据类型，基于对象，基于事件驱动的语言。**

### 23.阅读下面的JavaScript代码： 

【考点：函数调用】B











```
function f(y) {
    var x=y*y;
    return x;
}
for(x=0;x<5;x++) {
    y=f(x);
    document.writeln(y);
}
A.0 1 2 3 4     B.0 1 4 9 16      C.0 1 4 9 16 25       D.以上答案都不对复制代码
```

 

### 24、关于Javascript中数组的说法中，不正确的是：（A） 

【考点：数组的理解，包括创建，长度，元素的类型等概念】









```
A.数组的长度必须在创建时给定，之后便不能改变 
B.由于数组是对象，因此创建数组需要使用new运算符 
C.数组内元素的类型可以不同
D.数组可以在声明的同时进行初始化复制代码
```

### 25、考察以下程序片段: 

 【考点：toFixed 保留小数位数的使用】









```
var n = new Number(3456);
alert(n.toFixed(2));
以下选项正确的是：(C)
A.输出34  B.输出 56       C.输出3456.00        D.输出345600复制代码
```

### 26、察以下程序片段以下选项正确的是 （ C   ）   :

【考点：注意：如果 end 未被规定，那么 slice() 方法会选取从 start 到数组结尾的所有元素】









```
var str = “32px”;
var str1 = str.slice(-2);
alert(str);
alert(str1);
A依次输出”px” “px”       
B依次输出”32” “32” 
C依次输出”32px” “px”     
D依次输出”32px” “32px”复制代码
```

解析：`slice`方法用于提取目标数组的一部分，返回一个新数组，原数组不变。

```
arr.slice(start, end);复制代码
```

`-2`表示倒数计算的第二个位置，`-1`表示倒数计算的第一个位置。

**点击查看更多关于 [数组slice方法](https://wangdoc.com/javascript/stdlib/array.html#slice) 的知识。**

### 27、考察以下程序片段以下选项正确的是( A )

【考点：字符串的方法indexof的使用，之一返回的是下标】**:
**









```
var str = “12px”;
var s = str.indexof(“2”);
alert(s);
A.输出1     B.输出 2     C.输出 p      D.输出 12复制代码
```

### 28、在JavaScript中，下列哪段代码能够在1秒之后执行表达式expression( D )



【考点：setTimeout 的使用，注意时间的表达，以毫秒计，1秒应写为1000毫秒】









```
A.window.setTimeout(1000，expression)； 
B.window.setTimeout(expression，1)；
C.window.setTimeout(1，expression)； 
D.window.setTimeout(expression，1000)；复制代码
```

**点击查看更多关于[ setTimeOut 的用法](https://wangdoc.com/javascript/async/timer.html#settimeout)**

### 29、在JavaScript中，如果不指明对象直接调用某个方法，则该方法默认属于哪个对象： （B）



【考点：方法的调用，一般如果没有明确对用者，则是window对象】









```
A.document     B.Window   C.form    D.Location复制代码
```

### 30、history从属于window,下列能访问前一页面方法是：(   D    )  

【考点：history对象使用】









```
A.back(-1)    B.back(1)     C.forward(1)     D.go(-1)复制代码
```

**解析：**`**window.history**`**属性指向 History 对象，它表示当前窗口的浏览历史。**



```
// 后退到前一个网址
history.back()
// 等同于
history.go(-1)复制代码
```

**点击查看更多[关于 history 的知识。](https://wangdoc.com/javascript/bom/history.html)**

### 31、有语句“var x=0;while(____) x+=2;”，要使while循环体执行10次，空白处的循环判定式应写为：( C )









```
A．x<10         B.x<=10         C.x<20         D.x<=20复制代码
```

【考点：while循环】

### 32、以下( )表达式产生一个0~7之间(含0,7)的随机整数.( C )









```
A.Math.floor(Math.random()*6)   
B.Math.floor(Math.random()*7)    
C.Math.floor(Math.random()*8)  
D.Math.ceil(Math.random()*8)复制代码
```

【考点：随机数产生，注意**左包右不包**，随机数日后会经常用到，需熟练掌握】

### 33、在HTML页面中包含如下所示代码，则编写Javascript函数判断是否按下键盘上的回车键正确的编码是（C）

【考点：键盘事件，获取按键编码 **event.keyCode**】











```
<input name=”password” type=”text” onkeydown="myKeyDown()">
A.   function myKeyDown(){
        if (window.keyCode==13)        
        alert(“你按下了回车键”);
B.   function myKeyDown(){
        if (document.keyCode==13)        
        alert(“你按下了回车键”);
C.   function myKeyDown(){
        if (event.keyCode==13)        
        alert(“你按下了回车键”);
D.  function myKeyDown(){
        if (keyCode==13)
        alert("你按下了回车键")
复制代码
```

### 34、在HTML页面上，当按下键盘上的任意一个键时都会触发Javascript的（D）事件









```
A.onFocus  B.onBlur C.onSubmit   D.onKeyDown复制代码
```

### 35、看以下JavaScript程序 【考点： ++ 运算符的使用，什么时候先加后赋值，什么时候先赋值后++】











```
var x,y;              
x=10；              
y=x++；                           
运行以上程序后，变量y的值为   10     。复制代码
```

**解析：自增和自减运算符有一个需要注意的地方，就是放在变量之后，会先返回变量操作前的值，再进行自增/自减操作；放在变量之前，会先进行自增/自减操作，再返回变量操作后的值。**

### 36、看以下JavaScript程序【考点： && 运算符】











```
var i,j;              
i=0；              
j=i&&(++i)；                          
运行以上程序后，变量i的值为0     。复制代码
```

解析：







```
console.log(0&&1)//0复制代码
```



### 37、看以下JavaScript程序 【考点： do while 语句，先执行一次，在判断条件】











```
var i;              
i=8；              
do{                     
    i++;              
}
while(i>100);                          
运行以上程序后，变量i的值为9。复制代码
```

### 38、看以下JavaScript程序 【考点： continue 和break的使用】











```
for(var i=1;i<=10;i++){                     
    if(i==5) 
    continue;                     
    if(i==8) 
    break;                     
    alert(i);              
}                           
运行以上程序后，弹出的对话框依次输出的值为 1,2,3,4,6,7           。复制代码
```

**解析：continue 是接着执行，但是不执行下边的语句了。**

### 39、编写程序实现去除数组重复元素

【考点：程序逻辑，简单的算法实现功能，一题多解】









```
function unique1(array){             
    var n=[]; //一个新的临时数组             
    //遍历当前数组             
    for(var i=0;i<array.length;i++){              
    //如果当前数组的第i已经保存进了临时数组，那么跳过， 否则把当前项push到临时数组里面                     
        if(n.indexOf(array[i])==-1) 
        n.push(array[i]);             
    }             
    return n;            
}复制代码
```

 

### 40、 









```
var k;       
for(var i=0;i<5;i++){              
    for (var j=0;j<10 ;j++){                     
        k=i+j;              
    }       
}       
alert(k) //13
【考点：for循环，注意何时跳出循环】复制代码
```

 

**解析：i=5 时跳出循环，此时k=4+9=13。**

### 41、 









```
var a=0;       
test();       
function test(){              
    var a = 6              
    b()                
}       
function b(){              
    alert(a)       
}       
结果为： 0 【考点：变量的作用域，此处容易出错】复制代码
```

 

**解析：在 test() 调用函数 b 的时候，b内的 alert(a),此时a的作用域为全局作用域，a=0。**

### 42  、









```
function foo(){
    alert("aaaa");
    a = setTimeout(foo,100);
}
foo();  
无限循环调用复制代码
```

【考点：setTimeout，以及递归调用】

 

### 43、window对象的方法中，（ A ）方法是用于弹出确认对话框，可让选择“确定”或“取消” 【考点：confirm】









```
A、confirm() B、alert() C、prompt() D、open()复制代码
```

**解析：**`confirm(“提示信息”)`:出现一个确认框（提示信息、确认按钮、取消按钮）;

`prompt(“提示信息”)`：出现一个输入框，提示用户输入信息的，开发时不用；

`window.alert(“提示信息”)`：使用窗口的提示框功能输出提示信息。



注： a)` alert`阻塞之后的代码执行。   b) `window`可以省略。c) `alert`中提示信息的换行

### 44、用JavaScript实现打开一个新窗口，地址为abc.html,正确的方法是（A）

【考点：BOM 中open()方法】









```
A、window.open(“abc.html”，“”，“”)；
B、window.open(“”，“abc.html”，“”)；
C、window.open(“”，“”，“abc.html”)；
D、window.open(“”，“”，“”)；复制代码
```

解析：`window.open`方法用于新建另一个浏览器窗口，类似于浏览器菜单的新建窗口选项。它会返回新窗口的引用，如果无法新建窗口，则返回`null`。`open`方法一共可以接受三个参数。

```
window.open(url, windowName, [windowFeatures])复制代码
```



- `url`：字符串，表示新窗口的网址。如果省略，默认网址就是`about:blank`。
- `windowName`：字符串，表示新窗口的名字。如果该名字的窗口已经存在，则占用该窗口，不再新建窗口。如果省略，就默认使用`_blank`，表示新建一个没有名字的窗口。另外还有几个预设值，`_self`表示当前窗口，`_top`表示顶层窗口，`_parent`表示上一层窗口。
- `windowFeatures`：字符串，内容为逗号分隔的键值对（详见下文），表示新窗口的参数，比如有没有提示栏、工具条等等。如果省略，则默认打开一个完整 UI 的新窗口。如果新建的是一个已经存在的窗口，则该参数不起作用，浏览器沿用以前窗口的参数。

**更多关于 [window.open 的用法请点击查看](https://wangdoc.com/javascript/bom/window.html#windowopen-windowclose，windowstop)；**

### 45、在JavaScript程序中加入注释，方法有（AB）【js注释】









```
A、//注释内容      B、/*注释内容*/    
C、/注释内容     D、/*注释内容复制代码
```

### 46、以下JavaScript变量名不合法的有（ABC） 【变量名，标识符规则】









```
A、4Myvariable   B、My@variable   C、function   D、Myvariable4复制代码
```

解析：

- 变量由字母，数字，下划线或者美元符号$组成；

- 第一个字符可以是字母，下划线或美元符号 $ ；

- 

  后续的字符可以使字母、数字、下划线或美元符号；

  

- 

  变量名称区分大小写

  

- 

  保留字、关键字、JavaScript预定义了很多全局变量和函数不能用作自定义变量名和函数名

  

- 约定俗称：标识符要见名知意；

- 驼峰式命名法：又叫小驼峰式命名法。一般用来命名变量，用来解释描述变量

- 例如：`var planeBulletSpeed; //飞机子弹的速度`

### 47、看下列代码，将会输出什么?(变量声明提升)











```
var foo = 1;
function(){    
    console.log(foo);    
    var foo = 2;    
    console.log(foo);
}
答案：输出 undefined 和 2。复制代码
```

 

### 48、求y和z的值是多少？两个undefined    









```
var x = 1;
var y = 0;
var z = 0;
function add(n){
    n=n+1;
}
y = add(x);
function add(n){
    n=n+3;
}
z = add(x);复制代码
```

**解析：首先，有两个同名函数，后边的函数会覆盖掉前面的函数，其次，一个函数没有返回值的情况下，执行完输出的结果都是 undefined**

###  49、写出函数DateDemo的返回结果，系统时间假定为今天 









```
function DateDemo(){  
    var d, s="今天日期是：";  
    d = new Date();  
    s += d.getMonth() +1 +"/";  
    s += d.getDate() + "/";  
    s += d.getYear();  
    return s;
}
console.log( DateDemo() )
结果：今天日期是：当前系统日期复制代码
```

### 50、写出程序运行的结果？









```
for(i=0, j=0; i<10, j<6; i++, j++){  
    k = i + j;
}复制代码
```

​                                                                                                                                                                                                                                                                           

结果：10（小心陷阱） 

### 51、编写一个程序将数组扁平化去并除其中重复部分数据，最终得到一个升序且不重复的数组

**var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10];**

**第一种方法：**









```
arr.toString().split(",").sort((a,b) => { return a - b}).map(Number)复制代码
```

 

**第二种方法：**

 









```
Array.prototype.flat= function() {
    return [].concat(...this.map(item => (Array.isArray(item) ? item.flat() :
    [item])));
}
Array.prototype.unique = function() {
    return [...new Set(this)]
}
const sort = (a, b) => a - b;
console.log(arr.flat().unique().sort(sort));复制代码
```

 

**第三种方法：**









```
function spreadArr(arr=[]){
    if(arr.some(ele=>Array.isArray(ele))){
        let newArr = [];
        arr.forEach((ele) => {
            if(Array.isArray(ele)){
                newArr = newArr.concat(...ele)
            }else{
                if(!newArr.includes(ele))
                newArr.push(ele)
            }
        })
        return spreadArr(newArr);
    }
    return arr.sort((a,b)=> a-b);
}
spreadArr([ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10]);复制代码
```

 

 

**第四种方法：**







```
var arr = [2, -5, 6, [6, -5, [2, 5], [8, 10, [6, 8], -3], 2], 5];        
function f (arr) {            
    var newarr = [];            
    function fn (arr) {                
        for (var i = 0; i < arr.length; i++) {                    
            if (arr[i].length) {
                if (Array.isArray(arr[i])) {  //加这步判定的目的是，判断这个元素是否为数组
                    fn(arr[i]);
                } else {
                    newarr.push(arr[i]);             
            }else{                        
                newarr.push(arr[i]);                    
            }                
        }            
    }            
    fn(arr);            
    return newarr;        
}        
var x = f(arr);        
var newarr = [];        
for(var n = 0;n < x.length; n++) {           
    if (newarr.indexOf(x[n]) == -1) {               
        newarr.push(x[n]);           
    }        
}        
newarr.sort((a, b) => a - b)        
console.log(newarr)复制代码
```



### 52、说几条写JavaScript的基本规范？

\1. 不要在同一行声明多个变量。

\2. 请使用 ===/!==来比较true/false或者数值

\3. 使用对象字面量替代new Array这种形式

4.不要使用全局函数。

\5. Switch语句必须带有default分支

\6. 函数不应该有时候有返回值，有时候没有返回值。

\7. For循环必须使用大括号

\8. If语句必须使用大括号

\9. for-in循环中的变量 应该使用var关键字明确限定作用域，从而避免作用域污染。

### 53、JS基本类型和引用类型的区别？

基本类型： undefined，boolean，number，string，null，symbol(ES6)

引用类型：object,arrary,date,RegExp(正则)，Function

 

基本数据类型是简单的数据段。

引用类型是由多个值构成的对象，其实都是Object的实例。

 

基本类型可以直接访问，而引用类型的访问是按照对象在内存中的地址，再按照地址去获取对象的值，叫做引用访问。

 

当从一个变量向另一个变量赋值引用类型的值时，同样也会将存储在变量中的对象的值复制一份放到为新变量分配的空间中。前面讲引用类型的时候提到，

保存在变量中的是对象在堆内存中的地址，所以，与简单赋值不同，这个值的副本实际上是一个指针，而这个指针指向存储在堆内存的一个对象。那么赋值操作后，

两个变量都保存了同一个对象地址，则这两个变量指向了同一个对象。因此，改变其中任何一个变量，都会相互影响，从而引发了对象的深拷贝和浅拷贝的问题。

**可以延伸问一下如何深拷贝。**

### 54、Var 、Let 和 const 的区别。

 

var 声明的变量会挂载在 window 上，而 let 和 const 声明的变量不会：

var 声明变量存在变量提升，let 和 const 不存在变量提升

let 和 const 声明形成块作用域

同一作用域下 let 和 const 不能声明同名变量，而 var 可以

​       

 



Const 　1、一旦声明必须赋值,不能使用 null 占位。2、声明后不能再修改 3、如果声明的是复合类型数据，可以修改其属性

### 55、有哪些变量类型？原始类型（值类型）有哪⼏种？null 是对象嘛？

根据 JavaScript 中的 **变量类型** 传递⽅式，分为 **值类型（原始类型） 和 引⽤类型** ，值类型变量包括 Boolean、 Null、Undefined、Number、String，引⽤类型包括了 Object 类的所有，如 Date、Array、 Function 等。在参数传递⽅式上，值类型是按值传递，引⽤类型是按共享传递。

在 JS 中，存在着 6 种原始值，分别是：

- boolean
- null
- undefined
- number
- string
- symbol

⾸先原始类型存储的都是值，是没有函数可以调⽤的，⽐如 undefined.toString()

此时你肯定会有疑问，这不对呀，明明 '1'.toString() 是可以使 ⽤的。其实在这种情况下，'1' 已经不是原始类型了，⽽是被强制 转换成了 String 类型也就是对象类型，所以可以调⽤ toString 函数。

除了会在必要的情况下强转类型以外，原始类型还有⼀些坑

其中 JS 的 number 类型是浮点类型的，在使⽤中会遇到某些 Bug， ⽐如 0.1 + 0.2 !== 0.3，但是这⼀块的内容会在进阶部分讲 到。string 类型是不可变的，⽆论你在 string 类型上调⽤何种 ⽅法，都不会对值有改变。

另外对于 null 来说，很多⼈会认为他是个对象类型，其实这是错误 的。虽然 typeof null 会输出 object，但是这只是 JS 存在的⼀ 个悠久 Bug。在 JS 的最初版本中使⽤的是 32 位系统，为了性能考 虑使⽤低位存储变量的类型信息，000 开头代表是对象，然⽽ null 表示为全零，所以将它错误的判断为 object 。虽然现在的内部类 型判断代码已经改变了，但是对于这个 Bug 却是⼀直流传下来。

### 56、对象类型和原始类型的不同之处？函数参数是对象会 发⽣什么问题？

在 JS 中，除了原始类型那么其他的都是对象类型了。对象类型和原 始类型不同的是，原始类型存储的是值，对象类型存储的是地址（指 针）。当你创建了⼀个对象类型的时候，计算机会在内存中帮我们开 辟⼀个空间来存放值，但是我们需要找到这个空间，这个空间会拥有 ⼀个地址（指针）。 







```
const a = [];复制代码
```



对于常量 a 来说，假设内存地址（指针）为 #001，那么在地址 #001 的位置存放了值 []，常量 a 存放了地址（指针） #001，再 看以下代码







```
const a = [];
const b = a;
b.push(1);复制代码
```



当我们将变量赋值给另外⼀个变量时，复制的是原本变量的地址（指 针），也就是说当前变量 b 存放的地址（指针）也是 #001，当我们 进⾏数据修改的时候，就会修改存放在地址（指针） #001 上的值， 也就导致了两个变量的值都发⽣了改变。

接下来我们来看函数参数是对象的情况







```
function test(person) {
    person.age = 26
    person = {
        name: 'yyy',
        age: 30
    }
    return person
};
const p1 = {
    name: 'yck',
    age: 25
};
const p2 = test(p1);
console.log(p1) // -> ?
console.log(p2) // -> ?复制代码
```



对于以上代码，你是否能正确的写出结果呢？接下来让我为你解析⼀ 番：

- ⾸先，函数传参是传递对象指针的副本 
- 到函数内部修改参数的属性这步，我相信⼤家都知道，当前 p1 的值也被修改了 
- 但是当我们重新为 person 分配了⼀个对象时就出现了分歧，

所以最后 person 拥有了⼀个新的地址（指针），也就和 p1 没有任何关系了，导致了最终两个变量的值是不相同的。

### 57、typeof 是否能正确判断类型？instanceof 能正确 判断对象的原理是什么？

typeof 对于原始类型来说，除了 null 都可以显示正确的类型







```
typeof 1 // 'number'
typeof '1' // 'string'
typeof undefined // 'undefined'
typeof true // 'boolean'
typeof Symbol() // 'symbol'
复制代码
```



typeof 对于对象来说，除了函数都会显示 object，所以说 typeof 并不能准确判断变量到底是什么类型







```
typeof [] // 'object'
typeof {} // 'object'
typeof console.log // 'function'
复制代码
```



如果我们想判断⼀个对象的正确类型，这时候可以考虑使⽤ instanceof，因为内部机制是通过原型链来判断的，在后⾯的章节 中我们也会⾃⼰去实现⼀个 instanceof。







```
const Person = function() {}
const p1 = new Person()
p1 instanceof Person // true
var str = 'hello world'
str instanceof String // false
var str1 = new String('hello world')
str1 instanceof String // true复制代码
```



对于原始类型来说，你想直接通过 instanceof 来判断类型是不⾏ 的，当然我们还是有办法让 instanceof 判断原始类型的







```
class PrimitiveString {
static [Symbol.hasInstance](x) {
return typeof x === 'string'
}
}
console.log('hello world' instanceof
PrimitiveString) // true复制代码
```



你可能不知道 Symbol.hasInstance 是什么东⻄，其实就是⼀个 能让我们⾃定义 instanceof ⾏为的东⻄，以上代码等同于 typeof 'hello world' === 'string'，所以结果⾃然是 true 了。这其实也侧⾯反映了⼀个问题， instanceof 也不是百 分之百可信的。 

### 58、如何正确判断 this？箭头函数的 this 是什么？







```
function a() {
    return () => {
        return () => {
            console.log(this)
        }
    }
}
console.log(a()()())复制代码
```



⾸先箭头函数其实是没有 this 的，箭头函数中的 this 只取决包裹 箭头函数的第⼀个普通函数的 this。在这个例⼦中，因为包裹箭头 函数的第⼀个普通函数是 a，所以此时的 this 是 window。另外对 箭头函数使⽤ bind 这类函数是⽆效的。

最后种情况也就是 bind 这些改变上下⽂的 API 了，对于这些函数 来说，this 取决于第⼀个参数，如果第⼀个参数为空，那么就是 window。

那么说到 bind，不知道⼤家是否考虑过，如果对⼀个函数进⾏多次 bind，那么上下⽂会是什么呢？ 







```
let a = {}
let fn = function () { console.log(this) }
fn.bind().bind(a)() // => ?复制代码
```



如果你认为输出结果是 a，那么你就错了，其实我们可以把上述代码 转换成另⼀种形式







```
// fn.bind().bind(a) 等于
let fn2 = function fn1() {
    return function() {
        return fn.apply()
    }.apply(a)
}
fn2()
复制代码
```



可以从上述代码中发现，不管我们给函数 bind ⼏次，fn 中的 this 永远由第⼀次 bind 决定，所以结果永远是 window。







```
let a = { name: 'yck' }
function foo() {
    console.log(this.name)
}
foo.bind(a)() // => 'yck'复制代码
```



以上就是 this 的规则了，但是可能会发⽣多个规则同时出现的情 况，这时候不同的规则之间会根据优先级最⾼的来决定 this 最终指 向哪⾥。 

⾸先，new 的⽅式优先级最⾼，接下来是 bind 这些函数，然后是 obj.foo() 这种调⽤⽅式，最后是 foo 这种调⽤⽅式，同时，箭 头函数的 this ⼀旦被绑定，就不会再被任何⽅式所改变。



## JS 高级 面试题

### 1、谈谈你对Ajax的理解？(概念、特点、作用)

AJAX全称为“Asynchronous JavaScript And XML”（异步JavaScript和XML） 是指一种创建交互式网页应用的开发技术、改善用户体验，实现无刷新效果。



**优点**

a、不需要插件支持
    b、优秀的用户体验
    c、提高Web程序的性能
    d、减轻服务器和带宽的负担

**缺点**

a、浏览器对XMLHttpRequest对象的支持度不足，几乎所有浏览器现在都支持
    b、破坏浏览器“前进”、“后退”按钮的正常功能，可以通过简单的插件弥补
    c、对搜索引擎的支持不足



### 2. 什么是跨域，如何实现跨域访问? 

​    跨域是指不同域名之间相互访问。    JavaScript同源策略的限制，A域名下的JavaScript无法操作B或是C域名下的对象

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="674" height="279"></svg>)

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="655" height="355"></svg>)

 

**实现：****
**    (1) JSONP跨域：利用script脚本允许引用不同域下的js实现的，将回调方法带入服务器，返回结果时回调。
    (2) 跨域资源共享（CORS）
 跨域资源共享（CORS）是一种网络浏览器的技术规范，它为Web服务器定义了一种方式，允许网页从不同的域访问其资源。
 **CORS****与JSONP相比:****
**   a、 JSONP只能实现GET请求，而CORS支持所有类型的HTTP请求。
    b、 使用CORS，开发者可以使用普通的XMLHttpRequest发起请求和获得数据，比起JSONP有更好的错误处理。
    c、 JSONP主要被老的浏览器支持，它们往往不支持CORS，而绝大多数现代浏览器都已经支持了CORS。

 

### 3. get和post的区别,何时使用post?

(1)、get 是从服务器上获取数据，post 是向服务器传送数据。 get 请求返回 request - URI 所指出的任意信息。
 Post 请求用来发送电子邮件、新闻或发送能由交互用户填写的表格。这是唯一需要在请求中发送body的请求。使用Post请求时需要在报文首部 Content - Length 字段中指出body的长度。
    (2)、get 是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。post是通过HTTP post机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址，用户看不到这个过程。
    (3)、对于 get 方式，服务器端用Request.QueryString获取变量的值，对于 post 方式，服务器端用Request.Form获取提交的数据。
    (4)、get 传送的数据量较小，不能大于2KB。post 传送的数据量较大，一般被默认为不受限制。但理论上，IIS4中最大量为80KB，IIS5中为100KB。 用IIS过滤器的只接受get参数，所以一般大型搜索引擎都是用get方式。
    (5)get安全性非常低，post 安全性相对较高。如果这些数据是中文数据而且是非敏感数据，那么使用get；如果用户输入的数据不是中文字符而且包含敏感数据，那么还是使用 post 为好。

 

### 4. 对比 Flash 与 ajax 哪个好,在使用中如何取舍？

**Ajax的优势：**
       (1)、可搜索性
           普通的文本网页会更有利于SEO。文本内容是搜索引擎容易检索的，而繁琐的swf字节码却是搜索引擎不愿触及的。虽然Google等一些大型的搜索引擎可以检索SWF内部的内容，但是仍然有很多麻烦存在。
       (2)、开放性
           Flash常年以来被Macromedia看的很死。包括Flex、FMS等辅佐技术一直都需要昂贵的安装、维护费用。而JS则没有这样的麻烦。没有人愿意承担法律和版权的风险。费用Flash开发是很昂贵的，因为FlashIDE等环境都是要收费的．而Ajax则不同．虽然有一些便宜的生成swf的工具，但是他们的工能实在无法满足复杂需求。
       (3)、易用性
           Ajax程序有更好的易用性。由于中间有一层Flashplayer代理层，因此许多辅助功能无法被Flash灵活利用。而且Flash在一些方面有着不好的口碑。比如弹出广告、比如恶意代码。
       (4)、易于开发
       人们开发复杂的Ajax和Flash应用程序时，都会借助一些高级的开发工具。普遍来说，Ajax的开发包比Flash简便、容易。
 **Flash的优势：**
    (1)、多媒体处理
    Flash在音频、视频等多媒体领域相比HTML有绝对的优势。现在几乎所有的网站都包含有Flash内容。
    (2)、兼容性
    兼容性好：由于通过了唯一的FlashPlayer“代理”。人们不必像调试JS那样，在不同的浏览器中调试程序。
    (3)、矢量图型
    这是Flash最大的优势，同样处在这一领域的SVG、Canvas element以及Direct完全不能与Flash相比。
    (4)、客户端资源调度
    Flash能够更容易的调用浏览器以外的外部资源。比如摄像头、麦克风等。然而这是普通的HTML无法完成的。但是这也许是一个缺点（为什么呢？）

**Ajax的劣势：**
    (1)、它可能破坏浏览器的后退功能 
    (2)、使用动态页面更新使得用户难于将某个特定的状态保存到收藏夹中 ，不过这些都有相关方法解决。
 **Flash的劣势：
**    (1)二进制格式 
    (2)格式私有
    (3)flash 文件经常会很大，用户第一次使用的时候需要忍耐较长的等待时间 
    (4)性能问题

 

### 5.同步和异步的区别？

​      举个例子：普通B/S模式（同步）AJAX技术（异步） 

**同步：**提交请求->等待服务器处理->处理完毕返回 这个期间客户端浏览器不能干任何事 

**异步:** 请求通过事件触发->服务器处理（这是浏览器仍然可以作其他事情）->处理完毕 

ajax.open方法中，第3个参数是设同步或者异步。

prototype等js类库一般都默认为异步，即设为true。先说下同步的情况下，js会等待请求返回，获取status。不需要onreadystatechange事件处理函数。而异步则需要 onreadystatechange事件处理，且值为4再正确处理下面的内容。 









```
//同步传输模式 

function RequestByGet(nProducttemp,nCountrytemp) { 
    var xmlhttp 
    if (window.XMLHttpRequest) { 
         //isIE = false; 
         xmlhttp = new XMLHttpRequest(); 
    }else if (window.ActiveXObject) { 
         //isIE = true; 
        xmlhttp = new  
        ActiveXObject("Microsoft.XMLHTTP"); 
    }             
    //Web page location.      
    var URL="http://www.baidu.com/; 
    xmlhttp.open("GET",URL, false); 
    //xmlhttp.SetRequestHeader("Content-Type","text/html;charset="utf-8") 
    xmlhttp.send(null); 
    var result = xmlhttp.status; 
    //OK 
    if(result==200){ 
       document.getElementById("div_RightBarBody").innerHTML=xmlhttp.responseText; 
    } 
    xmlhttp = null; 
}复制代码
```

 

 









```
//异步传输模式 
var xmlhttp 
function RequestByGet(nProducttemp,nCountrytemp) { 
    if (window.XMLHttpRequest) { 
         //isIE = false; 
         xmlhttp = new XMLHttpRequest(); 
    } else if (window.ActiveXObject) { 
         //isIE   =  true; 
         xmlhttp  =   new ActiveXObject("Microsoft.XMLHTTP"); 
    } 
                
    //Web page location. 
    var URL="http://www.baidu.com/"; 
    xmlhttp.open("GET",URL, true); 
    xmlhttp.onreadystatechange = handleResponse; 
    //xmlhttp.SetRequestHeader("Content-Type","text/html;charset=UTF-8") 
    xmlhttp.send(null); 
} 

function handleResponse() { 
    if(xmlhttp.readyState == 4 && xmlhttp.status==200) { 
       document.getElementById("div_RightBarBody").innerHTML=xmlhttp.responseText; 
       xmlhttp = null; 
    } 
}  复制代码
```

###         6.根据你的理解,请简述JavaScript脚本的执行原理?

   JavaScript是一种动态、弱类型、基于原型的语言，通过浏览器可以直接执行，当浏览器遇到<script> 标记的时候，浏览器会执行之间的javascript代码。嵌入的js代码是顺序执行的，每个脚本定义的全局变量和函数，都可以被后面执行的脚本所调用。 变量的调用，必须是前面已经声明，否则获取的变量值是undefined。

### 7. 说说你对json的理解?

**回答一：**

JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。
 它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小.
 json简单说就是javascript中的对象和数组，所以这两种结构就是对象和数组。**
            a****、**JSON对象：以“{”开始，以“}”结束，里面则是一系列的键（key）值（value）对，键和值用“:”分开，每对键值对之间用“,”分开。参考以下语法结构：{key1:value1,key2:value2,key3:value3…}其中键（key）是字符串，而值（value）可以是字符串，数值，true,false,null,对象或数组，也就是说某个键（key）的值（value）可以是一个数组，数组里面又是一些JSON对象，这种表示稍微复杂一些，但是参照这些理解可以很容易分辨出来。
    **b****、**JSON数组：以”[”开始，”]”结束，如同程序语言一样，例如C#，Button[] btnArray,则BtnArray是一个Button类型的数组，里面就存放Button类型的对象，那么JSON数组也一样，里面存放的也是JSON对象.

**回答二：****
      a****、**JSON 指的是 JavaScript 对象表示法（JavaScript Object Notation）
    **b****、**JSON 是轻量级的文本数据交换格式，并不是编程语言
    **c****、**JSON 独立于语言存在
    **d****、**JSON 具有自我描述性，更易理解
    **e****、**JSON 可以将 JavaScript 对象中表示的一组数据转换为字符串，然后就可以在函数之间轻松地传递这个字符串，或者在异步应用程序中将字符串从 Web 客户机传递给服务器端程序。这个字符串看起来有点儿古怪，但是JavaScript很容易解释它，而且 JSON 可以表示比"名称 / 值对"更复杂的结构。例如，可以表示数组和复杂的对象，而不仅仅是键和值的简单列表

 



### 8.谈谈你对闭包的理解?   

(1)、使用闭包主要是为了设计私有的方法和变量。闭包的优点是可以避免全局变量的污染，缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。
   **(2)、闭包有三个特性：**    a、函数嵌套函数    b、函数内部可以引用外部的参数和变量    c、参数和变量不会被垃圾回收机制回收

 

### 9. 谈谈你This对象的理解?

**回答一：****
**      (1)、js的this指向是不确定的，也就是说是可以动态改变的。call/apply 就是用于改变this指向的函数，这样设计可以让代码更加灵活，复用性更高
    (2)、this 一般情况下，都是指向函数的拥有者。
    (3)、在函数自执行里，this 指向的是 window 对象。
 扩展：关于this，还有一个地方比较让人模糊的是在dom事件里，通常有如下3种情况：
    a、使用标签属性注册事件，此时this指向的是window对象。
    b、对与a，要让this指向input，可以将this作为参数传递。
    c、使用addEventListener等注册事件。此时this也是指向 input。

**回答二：**

**（1）、处于全局作用域下的this：在全局作用域下this默认指向window对象。**









```
this;/*window*/
var a = {name:this}/*window*/
var b =[this];/*window*/复制代码
```

**(2)****、处在函数中的this，又分为以下几种情况：**

a、一般定义的函数，然后一般的执行：this还是默认指向window。

 









```
var a = function(){
    console.log(this);
}
a();/*window*/复制代码
```

 

b、一般定义，用new调用执行：这时候让this指向新建的空对象，我们才可以给空对象初始化自有变量









```
var a = function(){
    console.log(this);
}
new a();/*新建的空对象*/复制代码
```

c、作为对象属性的函数，调用时：这时候this指向调用f函数的a对象。









```
var a = {
    f:function(){
        console.log(this)
    }
}
a.f();/*a对象*/复制代码
```

 

**(3)****、通过call()和apply()来改变this的默认引用：**









```
var b = {id: 'b'};
var a = {
    f:function(){
        console.log(this)　
    }
}
a.f.call(b);  //{id: "b"}复制代码
```

 

所有函数对象都有的call方法和apply方法，它们的用法大体相似，f.call(b)的意思是，执行f函数，并将f函数执行期活动对象里的this指向b对象，这样标示符解析时，this就会是b对象了。不过调用函数是要传参的。所以，f.call(b, x, y); f.apply(b, [x, y]);以上就是用call方法执行f函数，与用apply方法执行f函数时传参方式，它们之间的差异，大家一目了然：apply通过数组的方式传递参数，call通过一个个的形参传递参数。

 

**(4)****、一些函数特殊执行情况this的指向问题：**

a、setTimeout()和setInverval():









```
var a = function(){
    console.log(this);
}
setTimeout(a,0);/*window*/复制代码
```

setInterval()类似。

b、dom模型中触发事件的回调方法执行中活动对象里的this指向该dom对象。

### 10. JavaScript对象的几种创建方式?

  **(1)****通过new 方法**









```
var obj = new Object(); 复制代码
```

  **(2)** **工厂模式**









```
function Parent(){
    var Child = new Object();
    Child.name="欲泪成雪";
    Child.age="20";
    return Child;
};
var x = Parent()复制代码
```

引用该对象的时候，这里使用的是 var x = Parent()而不是 var x = new Parent();因为后者会可能出现很多问题（前者也成为工厂经典方式,后者称之为混合工厂方式），不推荐使用new的方式使用该对象

**(2)****构造函数方式**









```
function Parent(){　　
this.name="欲泪成雪";　　
this.age="20";
};
var x =new Parent();复制代码
```

**(3)** **原型模式**









```
function Parent(){};
Parent.prototype.name="欲泪成雪";
Parent.prototype.age="20";
var x =new Parent();复制代码
```

(4)混合的构造函数，原型方式（推荐）









```
function Parent(){　　
    this.name="欲泪成雪";　　
    this.age=22;
};
Parent.prototype.lev=function(){　　
    return
    this.name;
};
var x =new Parent();复制代码
```

**(5)****动态原型方式**









```
function Parent(){　　
    this.name="欲泪成雪";　　
    this.age=22;
    if(typeofParent._lev=="undefined"){
        Parent.prototype.lev=function(){　　
        return this.name;
    }       
    Parent._lev=true;    
}}
var x =new Parent();复制代码
```

 

### 11. 请写出js内存泄漏的问题?

**回答一：****
**   (1)、IE7/8 DOM对象或者ActiveX对象循环引用导致内存泄漏
       a、多个对象循环引用
       b、循环引用自己
    (2)、基础的DOM泄漏
       当原有的DOM被移除时，子结点引用没有被移除则无法回收。
    (3)、timer定时器泄漏
        这个时候你无法回收buggyObject,解决办法，先停止timer然后再回收
 **回答二：****
**   内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。
    垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为 0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。
    setTimeout 的第一个参数使用字符串而非函数的话，会引发内存泄漏。
 闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）也会引发内存泄漏问题。

 

### 12. 请你解释一下事件冒泡机制?

a、在一个对象上触发某类事件（比如单击onclick事件），如果此对象定义了此事件的处理程序，那么此事件就会调用这个处理程序，如果没有定义此事件处理程序或者事件返回true，那么这个事件会向这个对象的父级对象传播，从里到外，直至它被处理（父级对象所有同类事件都将被激活），或者它到达了对象层次的最顶层，即document对象（有些浏览器是window）。
    b、冒泡型事件：事件按照从最特定的事件目标到最不特定的事件目标(document对象)的顺序触发
    c、js冒泡机制是指如果某元素定义了事件A，如click事件，如果触发了事件之后，没有阻止冒泡事件，那么事件将向父级元素传播，触发父类的click函数。

**//****阻止冒泡时间方法，兼容ie(e.cancleBubble)和ff(e.stopProgation)**









```
function stopBubble(e){
    var evt = e||window.event;
    evt.stopPropagation?evt.stopPropagation():(evt.cancelBubble=true);//阻止冒泡
    evt.preventDefault
}复制代码
```

### 13. 说说你对Promise的理解?

ES6 原生提供了 Promise 对象。
    所谓 Promise，就是一个对象，用来传递异步操作的消息。它代表了某个未来才会知道结果的事件（通常是一个异步操作），并且这个事件提供统一的 API，可供进一步处理。
 **Promise 对象有以下两个特点。**
       (1)、对象的状态不受外界影响。Promise 对象代表一个异步操作，有三种状态：Pending（进行中）、Resolved（已完成，又称 Fulfilled）和 Rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是 Promise 这个名字的由来，它的英语意思就是「承诺」，表示其他手段无法改变。
    (2)、一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise 对象的状态改变，只有两种可能：从 Pending 变为 Resolved 和从 Pending 变为 Rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。就算改变已经发生了，你再对 Promise 对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

有了 Promise 对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，Promise 对象提供统一的接口，使得控制异步操作更加容易。

Promise 也有一些缺点。首先，无法取消 Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise 内部抛出的错误，不会反应到外部。第三，当处于 Pending 状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

 

### 14. 谈谈你对Javascript垃圾回收机制的理解？

**(1)****、标记清除（mark and sweep）****
**   这是`JavaScript`最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”。
 垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了

**(2)****、引用计数(reference counting)****
**   在低版本`IE`中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个 变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时 候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间。
 在IE中虽然`JavaScript`对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的，也就是说只要涉及BOM及DOM就会出现循环引用问题。

### 15. 说说你对原型（prototype）理解?

JavaScript是一种通过原型实现继承的语言与别的高级语言是有区别的，像java，C#是通过类型决定继承关系的，JavaScript是的动态的弱类型语言，总之可以认为JavaScript中所有都是对象，在JavaScript中，原型也是一个对象，通过原型可以实现对象的属性继承，JavaScript的对象中都包含了一个" prototype"内部属性，这个属性所对应的就是该对象的原型。

"prototype"作为对象的内部属性，是不能被直接访问的。所以为了方便查看一个对象的原型，Firefox和Chrome内核的JavaScript引擎中提供了"__proto__"这个非标准的访问器（ECMA新标准中引入了标准对象原型访问器"Object.getPrototype(object)"）。

原型的主要作用就是为了实现继承与扩展对象。

### 16. typeof与instanceof的区别是什么？

在 **JavaScript** **中，判断一个变量的类型可以用typeof****
**   (1)、数字类型， typeof 返回的值是 number。比如说：typeof(1)，返回值是number
    (2)、字符串类型， typeof 返回的值是 string。比如typeof("123")返回值是string。 
    (3)、布尔类型， typeof 返回的值是 boolean 。比如typeof(true)返回值是boolean。
    (4)、对象、数组、null 返回的值是 object 。比如typeof(window)，typeof(document)，typeof(null)返回的值都是object。
    (5)、函数类型，返回的值是 function。比如：typeof(eval)，typeof(Date)返回的值都是function。
    (6)、不存在的变量、函数或者undefined，将返回undefined。比如：typeof(abc)、typeof(undefined)都返回undefined。
    在**JavaScript** **中,instanceof用于判断某个对象是否被另一个函数构造。****
** 使用 typeof 运算符时采用引用类型存储值会出现一个问题，无论引用的是什么类型的对象，它都返回 "object"。ECMAScript 引入了另一个 Java 运算符 instanceof 来解决这个问题。instanceof 运算符与 typeof 运算符相似，用于识别正在处理的对象的类型。与 typeof 方法不同的是，instanceof 方法要求开发者明确地确认对象为某特定类型。

 

### 17. 请简要说说你对Javascript面向对象的理解?

为了说明 JavaScript 是一门彻底的面向对象的语言，首先有必要从面向对象的概念着手 , 探讨一下面向对象中的几个概念：
    a、一切事物皆对象
    b、对象具有封装和继承特性
    c、对象与对象之间使用消息通信，各自存在信息隐藏
 以这三点做为依据，C++ 是半面向对象半面向过程语言，因为，虽然他实现了类的封装、继承和多态，但存在非对象性质的全局函数和变量。Java、C# 是完全的面向对象语言，它们通过类的形式组织函数和变量，使之不能脱离对象存在。但这里函数本身是一个过程，只是依附在某个类上。
 然而，面向对象仅仅是一个概念或者编程思想而已，它不应该依赖于某个语言存在。比如 Java 采用面向对象思想构造其语言，它实现了类、继承、派生、多态、接口等机制。但是这些机制，只是实现面向对象编程的一种手段，而非必须。换言之，一门语言可以根据其自身特性选择合适的方式来实现面向对象。所以，由于大多数程序员首先学习或者使用的是类似 Java、C++ 等高级编译型语言（Java 虽然是半编译半解释，但一般做为编译型来讲解），因而先入为主地接受了“类”这个面向对象实现方式，从而在学习脚本语言的时候，习惯性地用类式面向对象语言中的概念来判断该语言是否是面向对象语言，或者是否具备面向对象特性。这也是阻碍程序员深入学习并掌握 JavaScript 的重要原因之一。
    

**JavaScript** **语言是通过一种叫做 原型（prototype）的方式来实现面向对象编程的。**下面就来讨论 基于类的（class-based）面向对象和 基于原型的 (prototype-based) 面向对象这两种方式在构造客观世界的方式上的差别。

**基于类的面向对象和基于原型的面向对象方式比较,****在基于类的面向对象方式中****，对象（object）依靠类（class）来产生。而在基于原型的面向对象方式中，对象（object）则是依靠构造器（constructor）利用原型（prototype）构造出来的**。举个客观世界的例子来说明二种方式认知的差异。例如工厂造一辆车，一方面，工人必须参照一张工程图纸，设计规定这辆车应该如何制造。这里的工程图纸就好比是语言中的 类 (class)，而车就是按照这个 类（class）制造出来的；另一方面，工人和机器 ( 相当于 constructor) 利用各种零部件如发动机，轮胎，方向盘 ( 相当于 prototype 的各个属性 ) 将汽车构造出来。
 事实上关于这两种方式谁更为彻底地表达了面向对象的思想，目前尚有争论。但认为原型式面向对象是一种更为彻底的面向对象方式，理由如下：
    (1)、首先，客观世界中的对象的产生都是其它实物对象构造的结果，而抽象的“图纸”是不能产生“汽车”的，也就是说，类是一个抽象概念而并非实体，而对象的产生是一个实体的产生；
    (2)、其次，按照一切事物皆对象这个最基本的面向对象的法则来看，类 (class) 本身并不是一个对象，然而原型方式中的构造器 (constructor) 和原型 (prototype) 本身也是其他对象通过原型方式构造出来的对象。
    (3)、再次，在类式面向对象语言中，对象的状态 (state) 由对象实例 (instance) 所持有，对象的行为方法 (method) 则由声明该对象的类所持有，并且只有对象的结构和方法能够被继承；而在原型式面向对象语言中，对象的行为、状态都属于对象本身，并且能够一起被继承（参考资源），这也更贴近客观实际。
    (4)、最后，类式面向对象语言比如 Java，为了弥补无法使用面向过程语言中全局函数和变量的不便，允许在类中声明静态 (static) 属性和静态方法。而实际上，客观世界不存在所谓静态概念，因为一切事物皆对象！而在原型式面向对象语言中，除内建对象 (build-in object) 外，不允许全局对象、方法或者属性的存在，也没有静态概念。
 所有语言元素 (primitive) 必须依赖对象存在。但由于函数式语言的特点，语言元素所依赖的对象是随着运行时 (runtime) 上下文 context变化而变化的，具体体现在this指针的变化。正是这种特点更贴近“万物皆有所属，宇宙乃万物生存之根本”的自然
 

 

### 18. 你对JavaScript函数的理解是?

![img](https://user-gold-cdn.xitu.io/2019/4/27/16a5e7eee273210d?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

javascript中的函数就是对象，对象就是“键/值”对的集合并拥有一个连接到原型对隐藏连接。

**(1)****、参数对象 （arguments）
**      第一个函数中有一个默认对象叫arguments，类似数组，但不是数组，该对象是传递给函数的参数。
 **(2)****、构造函数
**      在javascript中对象构造函数可以创建一个对象。
 **(3)****、函数调用
**      a、call：调用一个对象的一个方法，以另一个对象替换当前对象
      b、apply：应用某一对象的一个方法，用另一个对象替换当前对象，与call类似。
      c、caller：在一个函数调用另一个函数时，被调用函数会自动生成一个caller属性，指向调用它的函数对象。如果该函数当前未被调用，或并非被其他函数调用，则caller为null。
      d、Callee：当函数被调用时，它的arguments.callee对象就会指向自身，也就是一个
 对自己的引用。

 

### 19. 简要说说什么是IIFE？它有什么作用?

IIFE即Immediately-Invoked Function Expression，立即执行函数表达式， 立即执行的函数表达式的执行括号应该写在外包括号内。虽然写在内还是写在外都是有效的，但写在内使得整个表达式看起来更像一个整体，因此推荐这么做。









```
//最常用的两种写法
(function(){ /* code */ }()); // 老师推荐写法
(function(){ /* code */ })(); // 当然这种也可以 
// 括号和JS的一些操作符（如 =&& || ,等）可以在函数表达式和函数声明上消除歧义
// 如下代码中，解析器已经知道一个是表达式了，于是也会把另一个默认为表达式
// 但是两者交换则会报错
var i = function(){ 
    return 10; 
}();
true && function(){ /* code */
}();0, function(){ /* code */ }(); 
// 如果你不怕代码晦涩难读，也可以选择一元运算符!function(){ 
    /* code */ 
}();
~function(){
    /* code */ 
}();
-function(){ 
    /* code */ 
}();
+function(){ 
    /* code */ 
}(); 
// 你也可以这样
new function(){ /* code */ }
new function(){ /* code */ }() // 带参复制代码
```

**IIFE****的作用
**   **(1)****、提高性能：**减少作用域查找时间。使用IIFE的一个微小的性能优势是通过匿名函数的参数传递常用全局对象window、document、jQuery，在作用域内引用这些全局对象。JavaScript解释器首先在作用域内查找属性，然后一直沿着链向上查找，直到全局范围。将全局对象放在IIFE作用域内提升js解释器的查找速度和性能。
      **(2)****、压缩空间：**通过参数传递全局对象，压缩时可以将这些全局对象匿名为一个更加精简的变量名
      **(3)****、避免冲突：**匿名函数内部可以形成一个块级的私有作用域。
      **(4)****、依赖加载：**可以灵活的加载第三方插件，当然使用模块化加载更好（AMD,CMD）

### 20. 谈谈你对Function与Object的理解?

**Function**
      函数就是对象,代表函数的对象就是函数对象。所有的函数对象是被Function这个函数对象构造出来的。Function是最顶层的构造器。它构造了系统中所有的对象，包括用户自定义对象，系统内置对象，甚至包括它自已。这也表明Function具有自举性(自已构造自己的能力)。这也间接决定了Function的call和constructor逻辑相同。每个对象都有一个 constructor 属性,用于指向创建其的函数对象。
      a、函数与对象具有相同的语言地位
      b、没有类,只有对象
      c、函数也是一种对象,所谓的函数对象
      d、对象是按引用来传递的
 **Object**
 对于Object它是最顶层的对象，所有的对象都将继承Object的原型，但是你也要明确的知道Object也是一个函数对象，所以说Object是被Function构造出来的。

### 21、JavaScript 原型链

![img](data:image/svg+xml;utf8,<?xml version="1.0"?><svg xmlns="http://www.w3.org/2000/svg" version="1.1" width="500" height="637"></svg>)

### 22. 请指出 JavaScript中的本地对象、内置对象、宿主对象的区别？

**(1)****、本地对象**
      ECMA-262 把本地对象（native object）定义为“独立于宿主环境的 ECMAScript 实现提供的对象”。
 再来看一下，“本地对象”包含哪些内容：
 Object、Function、Array、String、Boolean、Number、Date、RegExp、Error、EvalError、RangeError、ReferenceError、SyntaxError、TypeError、URIError
 由此可以看出，简单来说，本地对象就是 ECMA-262 定义的类（引用类型）。
 **(2)****、内置对象****
**      ECMA-262 把内置对象（built-in object）定义为“由 ECMAScript 实现提供的、独立于宿主环境的所有对象，在 ECMAScript 程序开始执行时出现”。这意味着开发者不必明确实例化内置对象，它已被实例化了。
 同样是“独立于宿主环境”。根据定义我们似乎很难分清“内置对象”与“本地对象”的区别。而ECMA-262 只定义了两个内置对象，即 Global 和 Math （它们也是本地对象，根据定义，每个内置对象都是本地对象）。
 如此就可以理解了。内置对象是本地对象的一种。而其包含的两种对象中，Math对象我们经常用到，可这个Global对象是啥东西呢？
 Global对象是ECMAScript中最特别的对象，因为实际上它根本不存在，但大家要清楚，在ECMAScript中，不存在独立的函数，所有函数都必须是某个对象的方法。类似于isNaN()、parseInt()和parseFloat()方法等，看起来都是函数，而实际上，它们都是Global对象的方法。
 **(3)****、宿主对象****
**      由ECMAScript实现的宿主环境提供的对象，即我们网页的运行环境（操作系统和浏览器），所有的BOM和DOM都是宿主对象。 
 宿主环境提供的全局方法： 
 alert、confirm、prompt、write、writeln(后面的两种是document的方法)
 内置对象是本地对象的一种，本地对象时由官方定义的，而宿主对象相当于由自己定义的对象、DOM对象和BOM对象组成的

### 23. 请解释一下变量声明提升?

**a****、变量定义****
**            可以使用var定义变量，变量如果没有赋值，那变量的初始值为undefined。
      **b****、变量作用域**
            变量作用域指变量起作用的范围。变量分为全局变量和局部变量。全局变量在全局都拥有定义；而局部变量只能在函数内有效。 
 在函数体内，同名的局部变量或者参数的优先级会高于全局变量。也就是说，如果函数内存在和全局变量同名的局部变量或者参数，那么全局变量将会被局部变量覆盖。 所有不使用var定义的变量都视为全局变量。

### 24. 规避javascript多人开发函数重名问题

命名空间
      封闭空间
      js模块化mvc（数据层、表现层、控制层）
      seajs
      变量转换成对象的属性
      对象化

 

### 25. javascript面向对象中继承实现









```
function Person(name){
        this.name = name;
}

Person.prototype.showName = function(){
        alert(this.name);
}

function Worker(name, job){
       
Person.apply(this,arguments)
        this.job = job;
}
for(var i in Person.prototype){
        Worker.prototype =
Person.prototype;
}
new Worker('sl', 'coders').showName();复制代码
```

 

### 26. 写出3个使用this的典型应用

事件： 如onclick  this->发生事件的对象
      构造函数          this->new 出来的object
      call/apply       改变this

 

### 27. 你如何优化自己的代码？

代码重用
      避免全局变量（命名空间，封闭空间，模块化mvc..）
      拆分函数避免函数过于臃肿
      注释

 

### 28. 请尽可能详尽的解释AJAX的工作原理

1）.创建ajax对象（XMLHttpRequest/ActiveXObject(Microsoft.XMLHttp)）
      2）判断数据传输方式(GET/POST)
      3）打开链接 open()
      4）发送 send()
      5） 当ajax对象完成第四步（onreadystatechange）数据接收完成，判断http响应状态（status）200-300之间或者304（缓存）执行回调函数

 

### 29. 怎么理解js中的事件委托

具体来说，事件委托就是事件目标自身不处理事件，而是把处理任务委托给其父元素或者祖先元素，甚至根元素（document）。jQuery为绑定和委托事件提供了.bind()、.live()和.delegate()方法

1..bind()只能给调用它的时候已经存在的元素绑定事件，不能给未来新增的元素绑定事件在下列情况下，应该使用.live()或.delegate()，而不能使用.bind()：

2.为DOM中的很多元素绑定相同事件；

3.为DOM中尚不存在的元素绑定事件；

 

### 30. call 和 apply的含义和区别？

apply:方法能劫持另外一个对象的方法，继承另外一个对象的属性.

Function.apply(obj,args)方法能接收两个参数

obj：这个对象将代替Function类里this对象

args：这个是数组，它将作为参数传给Function（args-->arguments）

call:和apply的意思一样,只不过是参数列表不一样.

Function.call(obj,[param1[,param2[,…[,paramN]]]])

obj：这个对象将代替Function类里this对象

params：这个是一个参数列表

 

在给对象参数的情况下,如果参数的形式是数组的时候,比如apply示例里面传递了参数arguments,这个参数是数组类型,并且在调用Person的时候参数的列表是对应一致的(也就是Person和Student的参数列表前两位是一致的) 就可以采用 apply , 如果我的Person的参数列表是这样的(age,name),而Student的参数列表是(name,age,grade),这样就可以用call来实现了,也就是直接指定参数列表对应值的位置(Person.call(this,age,name,grade));

**apply的一些其他巧妙用法**

调用apply方法的时候,第一个参数是对象(this), 第二个参数是一个数组集合,在调用Person的时候,他需要的不是一个数组,但是为什么他给我一个数组我仍然可以将数组解析为一个一个的参数,

这个就是apply的一个巧妙的用处,可以将一个数组默认的转换为一个参数列表([param1,param2,param3] 转换为 param1,param2,param3) 这个如果让我们用程序来实现将数组的每一个项,来装换为参数的列表,可能都得费一会功夫,借助apply的这点特性,所以就有了以下高效率的方法:

### 31. a、b、c分别输出什么?









```
function fun(n,o) { 
    console.log(o) 
    return {    
        fun:function(m){      
            return fun(m,n);    
        }  
    };
}
var a = fun(0);  
a.fun(1);  
a.fun(2);  
a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c = fun(0).fun(1);  c.fun(2);  
c.fun(3);
//undefined, 0, 0, 0
//undefined, 0, 1, 2
//undefined, 0, 1, 1复制代码
```

 

### 32. 看看下面这个会输出什么呢？

 









```
<ul id="test">
  <li>这是第一条alert(0);</li>
  <li>这是第二条alert(1);</li>
  <li>这是第三条alert(2);</li>
</ul>
<script type="text/javascript">
  var elements = document.getElementById('test').querySelectorAll('li');
  for (var i = 0; i < elements.length; i++) {
    elements[i].onclick = function () {
    　　alert(i);
    }
　 }
</script>//点击每一条都alert：2复制代码
```

 

### 33.读程序给结果：









```
function A(){　　　　}　　　　
function B(a){　　　　　　
this.a = a;　　　　
}　　　　
function C(a){　　　　　　
if(a){　　　　　　　　
this.a = a;　　　　　　
}　　　　
}　　　　
A.prototype.a = 1;　　　　
B.prototype.a = 1;　　　　
C.prototype.a = 1; 　　　　
console.log(new A().a);　　　　
console.log(new B().a);　　　　
console.log(new C(2).a);
结果：
1、undefined、
2.　 复制代码
```

### 34. 题目是

```
var F = function(){};  
Object.prototype.a = function(){};  
Function.prototype.b = function(){};  
var f = new F();复制代码
```

问：调用f.a( )和f.b( ) 能成功么？

答案：能调用f.a( )但是不能调用f.b( );

 

### 35. 有一条可爱的小狗(名字叫“小狗狗”)，它的叫声很好听(wow)，每次看到主人的时候就会乖乖叫(yelp)。另一只小狗精力很旺盛（名字叫做“精力旺盛的狗狗”），每隔5秒叫唤(yelp)一声(wow)。









```
使用面向对象的混合模式实现小狗的创建
function Dog() {    
    this.name = '小狗狗';    
    this.wow = 'wow';
}
Dog.prototype.yelp = function() {    
    console.log(this.wow);
} 
function MadDog() {    
    Dog.call(this);   
    this.name = '精力旺盛的狗狗';
}
for (var i in Dog.prototype) {    
    MadDog.prototype[i] = Dog.prototype[i];
};
MadDog.prototype.yelp = function () {    
    var _this = this;    
    setInterval(function(){        
        console.log(_this.wow);    
    }, 5000);
}
var dog = new MadDog();
dog.yelp();复制代码
```

### 36. HTTP状态码知道哪些？

100 Continue继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息

200 OK 正常返回信息

201 Created请求成功并且服务器创建了新的资源

202 Accepted服务器已接受请求，但尚未处理

301 Moved Permanently请求的网页已永久移动到新位置。

302 Found 临时性重定向。

303 See Other临时性重定向，且总是使用 GET 请求新的 URI。

304 Not Modified自从上次请求后，请求的网页未修改过。

400 Bad Request服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。

401 Unauthorized 请求未授权。

403 Forbidden 禁止访问。

404 Not Found 找不到如何与URI 相匹配的资源。

500 Internal ServerError 最常见的服务器端错误。

503 Service Unavailable服务器端暂时无法处理请求（可能是过载或维护）。

### 37. 你有哪些前端性能优化的方法？

（1） **减少http请求次数**：CSSSprites, JS、CSS源码压缩、图片大小控制合适；网页Gzip，**CDN 托管**，data 缓存 ，图片服务器。

（2） **让ajax可缓存，** 前端模板JS+数据，减少由于HTML标签导致的带宽浪费，前端用变量保存 ajax 请求结果，每次操作本地变量，不用请求，减少请求次数

（3）用 innerHTML 代替 DOM 操作，**减少 DOM 操作次数**，优化 javascript 性能。

（4）当需要设置的样式很多时设置 className 而不是直接操作style。

（5）少用全局变量、缓存 DOM 节点查找的结果。减少 IO 读取操作。

（6） **避免使用 CSSExpression**（css表达式)又称Dynamic properties(动态属性)。

（7）图片预加载，将样式表放在顶部（CSS使用<link>标签放在顶部），**将脚本放在底部** 加上时间戳。

（8）合理使用 HTTP 缓存

（9） JS去除重复脚本

（10） 优化图片、尽量使用PNG

（11） 延迟加载组件

（12） 减少DOM元素的数量

 

### 38. 哪些常见操作会造成内存泄漏？

内存泄漏指任何对象在您不再拥有或需要它之后仍然存在。

垃圾回收器定期扫描对象，并计算引用了每个对象的其他对象的数量。如果一个对象的引用数量为0（没有其他对象引用过该对象），或对该对象的惟一引用是循环的，那么该对象的内存即可回收。

setTimeout的第一个参数使用字符串而非函数的话，会引发内存泄漏。

闭包、控制台日志、循环（在两个对象彼此引用且彼此保留时，就会产生一个循环）。

 

### 39. 读程序：









```
 (function() {            
var a = b = 5;        
})();        
console.log(b);        
答案：5复制代码
```



### 40. 执行这段代码，输出什么结果。









```
function test() {       
    console.log(a);     
    console.log(foo());       
    var a = 1;    
    function foo() {          
        return 2;      
    }    
}   
test();复制代码
```



**这段代码的结果是** **undefined** **和** **2****。**

### 41. 下面的代码会输出什么结果？给出你的答案。

```
var fullname = 'John Doe';   
var obj = {      
    fullname: 'Colin Ihrig',      
    prop: {         
        fullname: 'Aurelio De Rosa',        
        getFullname: function() {           
            return this.fullname;         
        }      
    }   
};    
console.log(obj.prop.getFullname());   
var test = obj.prop.getFullname;  
console.log(test());
答案是Aurelio De Rosa和John Doe。复制代码
```



 

### 42. 如何阻止事件冒泡和默认事件









```
e. stopPropagation();//标准浏览器阻止事件冒泡
event.canceBubble=true;//ie9之前阻止默认事件：为了不让a点击之后跳转，我们就要给他的点击事件进行阻止return
falsee.preventDefault();复制代码
```

### 43. new操作符具体干了什么呢?

  1、创建一个空对象，并且 this 变量引用该对象，同时还继承了该函数的原型。

 2、属性和方法被加入到 this 引用的对象中。

 3、新创建的对象由 this 所引用，并且最后隐式的返回 this 。

### 44. 经常看到页面里加载的js与css文件带有参数，这样做的目的是什么？（答案来自百度）





（1）    版本设置

（2）    主要是避免缓存，每次修改css文件后，重新引用更改后面的参数，这样就会重新请求文件，不会使用缓存。

（3）    实现控制某个外部js或者css文件的加载顺序

​     async 表示让立即加载脚本

​     defer表示脚本可以延迟加载，等整个页面加载完以后再加载js或者css。目的就是控制夹杂顺序

### 45. 简述浏览器加载和渲染原理？（答案来自百度）



（1） IE下载的顺序是从上到下，渲染的顺序也是从上到下，下载和渲染是同时进行的

（2）在渲染到页面的某一部分时，其上面的所有部分都已经下载完成（并不是说所有相关联的元素都已经下载完）； 

（3）在下载过程中，如果遇到某一标签是嵌入文件，并且文件是具有语义解释性的（例如：JS脚本，CSS样式），那么此时IE的下载过程会启用单独连接进行下载，并且在下载后进行解析，解析（ JS、CSS中如有重定义，后定义函数将覆盖前定义函数）过程中，停止页面所有往下元素的下载； 

 （4）样式表文件比较特殊，在其下载完成后，将和以前下载的所有样式表一起进行解析，解析完成后，将对此前所有元素（含以前已经渲染的）重新进行样式渲染。并以此方式一直渲染下去，直到整个页面渲染完成。 

### 46. 制作一个访问量很高的大型网站，你会如何来管理所有CSS文件,js 与图片？



答案：涉及到人手、分工、同步

（1） 先期团队必须确定好全局样式，编码模式等

（2） 编写习惯必须一致

（3） 标注样式编写人，各模块都及时标注（标注关键样式调用的地方）

（4） 页面进行标注

（5） Css与html分文件夹并行存放，命名都要统一

（6） Js分文件夹存放，命名以该JS功能为准英文翻译

（7） 图片采用整合的.png格式文件使用，尽量整合在一起，方便将来的管理



### 47、 什么是防抖和节流？有什么区别？如何实现？

**防抖：**触发高频事件后n秒内函数只会执行一次，如果n秒内高频事件再次被触发，则重新计算时间

**思路：**每次触发事件时都取消之前的延时调用方法









```
function debounce (fn) {
    let timeout = null; // 创建一个标记用来存放定时器的返回值
    return function () {
        clearTimeout(timeout); // 每当用户输入的时候把前一个 setTimeout clear 掉
        timeout = setTimeout(() => { 
        // 然后又创建一个新的 setTimeout, 这样就能保证输入字符后的 interval 间隔内
        // 如果还有字符输入的话，就不会执行 fn 函数
            fn.apply(this, arguments);
        }, 500);
    };
}
function sayHi() {
    console.log('防抖成功');
}
var inp = document.getElementById('inp');
inp.addEventListener('input', debounce(sayHi)); // 防抖复制代码
```



**节流：**高频事件触发，但在n秒内只会执行一次，所以节流会稀释函数的执行频率



**思路：**每次触发事件时都判断当前是否有等待执行的延时函数











```
function throttle(fn) {
    let canRun = true; // 通过闭包保存一个标记
    return function () {
        if (!canRun) return; // 在函数开头判断标记是否为true，不为true则return
        canRun = false; // 立即设置为false
        setTimeout(() => { // 将外部传入的函数的执行放在setTimeout中
            fn.apply(this, arguments);
            // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。
            //当定时器没有执行的时候标记永远是false，在开头被return掉
            canRun = true;
        }, 500);
    };
}
function sayHi(e) {
    console.log(e.target.innerWidth, e.target.innerHeight);
}
window.addEventListener('resize', throttle(sayHi));复制代码
```



### 48、 讲一下Http 缓存策略



Http 的缓存主要利用 header 里的两个字段来控制：

Cache-control主要包含以及几个字段：

private:则只有客户端可以缓存

public:客户端和代理服务器都可以缓存

max-age:缓存的过期时间

no-cache:需要使用对比缓存来验证缓存数据

no-store:所有内存都不会进行缓存

ETag:即用来进行对比缓存，Etag 是服务端资源的一个标识码

当客户端发送第一次请求时服务端会下发当前请求资源的标识码 Etag，下次再请求时，客户端则会通过 header 里的 If-None-Match 将这个标识码 Etag 带上，服务端将客户端传来的 Etag 与最新的资源 Etag 做对比，如果一样，则表示资源没有更新，返回 304。



通过 Cache-control 和 Etag 的配合来实现 Http 的缓存机制。



 

## jQuery:

 

### 1.说说你对延迟对象deferred的理解?

deferred对象是从jQuery 1.5.0版本开始引入的一个新功能。
 **a****、什么是****deferred****对象**
      开发网站的过程中，我们经常遇到某些耗时很长的javascript操作。其中，既有异步的操作（比如ajax读取服务器数据），也有同步的操作（比如遍历一个大型数组），它们都不是立即能得到结果的。
 通常的做法是，为它们指定回调函数（callback）。即事先规定，一旦它们运行结束，应该调用哪些函数。
 但是，在回调函数方面，jQuery的功能非常弱。为了改变这一点，jQuery开发团队就设计了deferred对象。
 简单说，deferred对象就是jQuery的回调函数解决方案。在英语中，defer的意思是"延迟"，所以deferred对象的含义就是"延迟"到未来某个点再执行。
 它解决了如何处理耗时操作的问题，对那些操作提供了更好的控制，以及统一的编程接口。
 **b****、它的主要功能，可以归结为四点：****
**      (1)、实现链式操作
      (2)、指定同一操作的多个回调函数
      (3)、为多个操作指定回调函数
      (4)、普通操作的回调函数接口

 

### 2. $.extend与$.fn.extend区别是什么?

**$.extend**
      在jQuery根命名空间下直接调用的方法可以认为是jQuery的静态方法或属性，常常使用方法名来调用，使用.方法名来调用，使用.extend这个静态方法可以完成两个功能:
      a、扩展属性或方法给jQuery
      b、扩展对象
 **$.fn.extend**
      .fn就是jQuery的原型，.fn等于jQuery.prototype，是jQuery的别名。.fn.extend方法的作用是用于扩展jQuery实例对象，也就是我们从页面中获得的jQuery对象。
      .fn扩展了jQuery的原型，让所有的jQuery实例对象都得到的扩展的方法，其它也可以直接修改jQuery.prototype来实现，.fn是jQuery.prototype的简写

 

### 3. 什么是链式编程?

几乎在所有基于“类型”的语言中如果调用一个方法后将对象作为方法参数返回则就会形成链式编程链式编程是将多个操作（多行代码）通过点号"."链接在一起成为一句代码。 链式代码通常要求操作有返回值， 但对于很多操作大都是void型，什么也不返回，这样就很难链起来了， 当然也有解决办法，可能不太优雅。 链式编程的新思想在jQuery中已流行使用

示例:









```
return $.each(this,function(index, obj) {   
    $("<span/>").html("+").css("cursor","pointer").click(function() {    
        $(obj).width($(obj).width()+ length);   
    }).insertAfter(obj);  
});复制代码
```

上面的示例中当$.each循环完成后返回this对象，返回的仍然是一个jQuery对象，所以可以继续jQuery编程。









```
$("button").SuperPlus(10).height(26).width(100).css("color","blue");复制代码
```

 

### 4.window.load 和 document ready 的区别。

​      window.load 是整个文档结构和资源都加载完成执行此事件

​      documen ready 是整个文档结构加载完成即可执行

 

### 5. Jquery的美元符号$有什么作用？



回答：其实美元符号$只是”jQuery”的别名，它是jQuery的选择器，如下代码：







```
   // aaa(a);        
$(document).ready(function(){
    /.../
});复制代码
```



当然你也可以用jQuery来代替$，如下代码：







```
jQuery(document).ready(function(){  
    /.../ 
});复制代码
```



jQuery中就是通过这个美元符号来实现各种灵活的DOM元素选择的，例如$(“#main”)即选中id为main的元素。

### 6. Jquery中有哪几种类型的选择器？

从我自己的角度来讲，可以有3种类型的选择器，如下：

1、基本选择器：直接根据id、css类名、元素名返回匹配的dom元素。

2、层次选择器：也叫做路径选择器，可以根据路径层次来选择相应的DOM元素。

3、过滤选择器：在前面的基础上过滤相关条件，得到匹配的dom元素。

4、请使用jQuery将页面上的所有元素边框设置为2px宽的虚线？

代码如下:







```
<script language="javascript" type="text/javascript">        
    $("*").css("border", "2px dotted red"); 
</script>
```


