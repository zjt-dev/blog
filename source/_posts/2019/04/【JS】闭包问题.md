---
title: 闭包
date: 2019-04-18 10:29:25
tags:
 - JS
categories:
 - JS
---
```js
function fun(n,o) {
 console.log(o) return {
  fun:function(m){ return fun(m,n);
  }
 };
} var a = fun(0); a.fun(1); a.fun(2); a.fun(3);//undefined,?,?,?
var b = fun(0).fun(1).fun(2).fun(3);//undefined,?,?,?
var c = fun(0).fun(1); c.fun(2); c.fun(3);//undefined,?,?,? //问:三行a,b,c的输出分别是什么？
```

这是一道非常典型的JS闭包问题。其中嵌套了三层fun函数，搞清楚每层fun的函数是那个fun函数尤为重要。

可以先在纸上或其他地方写下你认为的结果，然后展开看看正确答案是什么？

**答案**

//a: undefined,0,0,0 //b: undefined,0,1,2 //c: undefined,0,1,1

都答对了么？如果都答对了恭喜你在js闭包问题当中几乎没什么可以难住你了；如果没有答案，继续往下分析。

**JS中有几种函数**

首先，在此之前需要了解的是，在JS中函数可以分为两种，具名函数（命名函数）和匿名函数。

区分这两种函数的方法非常简单，可以通过输出 fn.name 来判断，有name的就是具名函数，没有name的就是匿名函数

**注意：**在低版本IE上无法获取具名函数的name，会返回undefined，建议在火狐或是谷歌浏览器上测试

或是采用兼容IE的获取函数name方法来获取函数名称：

/\*\*
  \* 获取指定函数的函数名称（用于兼容IE）
  \* @param {Function} fun 任意函数 \*/
function getFunctionName(fun) { if (fun.name !== undefined) return fun.name; var ret = fun.toString();
  ret \= ret.substr('function '.length);
  ret \= ret.substr(0, ret.indexOf('(')); return ret;
}

遂用上述函数测试是否为匿名函数：

![](https://files.jb51.net/file_images/article/201511/20151125161154422.png?2015102516123)

可以得知变量fn1是具名函数，fn2是匿名函数

**创建函数的几种方式**

说完函数的类型，还需要了解JS中创建函数都有几种创建方法。

**1、声明函数**

最普通最标准的声明函数方法，包括函数名及函数体。

**function fn1(){}**

**2、创建匿名函数表达式**

创建一个变量，这个变量的内容为一个函数

**var fn1=function (){}**  
注意采用这种方法创建的函数为匿名函数，即没有函数name

var fn1=function (){};
getFunctionName(fn1).length;//0

**3、创建具名函数表达式**

创建一个变量，内容为一个带有名称的函数

**var fn1=function xxcanghai(){};**  
**注意：**具名函数表达式的函数名只能在创建函数内部使用

即采用此种方法创建的函数在函数外层只能使用fn1不能使用xxcanghai的函数名。xxcanghai的命名只能在创建的函数内部使用

测试：

var fn1=function xxcanghai(){
  console.log("in:fn1<",typeof fn1,">xxcanghai:<",typeof xxcanghai,">");
};
console.log("out:fn1<",typeof fn1,">xxcanghai:<",typeof xxcanghai,">");
fn1(); //out:fn1< function >xxcanghai:< undefined > //in:fn1< function >xxcanghai:< function >

可以看到在函数外部（out）无法使用xxcanghai的函数名，为undefined。

**注意：**在对象内定义函数如var o={ fn : function (){…} }，也属于函数表达式

**4、Function构造函数**

可以给 Function 构造函数传一个函数字符串，返回包含这个字符串命令的函数，此种方法创建的是匿名函数。

![](https://files.jb51.net/file_images/article/201511/20151125161355276.png?2015102516143)

**5、自执行函数**

(function(){alert(1);})();
(function fn1(){alert(1);})();

自执行函数属于上述的“函数表达式”，规则相同

 **6、其他创建函数的方法**

当然还有其他创建函数或执行函数的方法，这里不再多说，比如采用 **eval ， setTimeout ， setInterval** 等非常用方法，这里不做过多介绍，属于非标准方法，这里不做过多展开

**三个fun函数的关系是什么？**

说完函数类型与创建函数的方法后，就可以回归主题，看这道面试题。

这段代码中出现了三个fun函数，所以第一步先搞清楚，这三个fun函数的关系，哪个函数与哪个函数时相同的。

function fun(n,o) {
 console.log(o) return {
  fun:function(m){ //...
 }
 };
}

先看第一个fun函数，属于标准具名函数声明，是新创建的函数，他的返回值是一个对象字面量表达式，属于一个新的object。

这个新的对象内部包含一个也叫fun的属性，通过上述介绍可得知，属于匿名函数表达式，即fun这个属性中存放的是一个新创建匿名函数表达式。

**注意：**所有声明的匿名函数都是一个新函数。

所以第一个fun函数与第二个fun函数不相同，均为新创建的函数。

**函数作用域链的问题**

再说第三个fun函数之前需要先说下，在函数表达式内部能不能访问存放当前函数的变量。

**测试1：对象内部的函数表达式：**

var o={
 fn:function (){
  console.log(fn);
 }
};
o.fn();//ERROR报错

![](https://files.jb51.net/file_images/article/201511/20151125161534228.png?20151025161556)

**测试2：非对象内部的函数表达式：**

var fn=function (){
 console.log(fn);
};
fn();//function (){console.log(fn);};正确

![](https://files.jb51.net/file_images/article/201511/20151125161644557.png?20151025161652)

**结论：**使用var或是非对象内部的函数表达式内，可以访问到存放当前函数的变量；在对象内部的不能访问到。

原因也非常简单，因为函数作用域链的问题，采用var的是在外部创建了一个fn变量，函数内部当然可以在内部寻找不到fn后向上册作用域查找fn，而在创建对象内部时，因为没有在函数作用域内创建fn，所以无法访问。

所以综上所述，可以得知，最内层的return出去的fun函数不是第二层fun函数，是最外层的fun函数。

所以，三个fun函数的关系也理清楚了，第一个等于第三个，他们都不等于第二个。

**到底在调用哪个函数？**

再看下原题，现在知道了程序中有两个fun函数(第一个和第三个相同)，遂接下来的问题是搞清楚，运行时他执行的是哪个fun函数？

function fun(n,o) {
 console.log(o) return {
  fun:function(m){ return fun(m,n);
  }
 };
} var a = fun(0); a.fun(1); a.fun(2); a.fun(3);//undefined,?,?,?
var b = fun(0).fun(1).fun(2).fun(3);//undefined,?,?,?
var c = fun(0).fun(1); c.fun(2); c.fun(3);//undefined,?,?,? //问:三行a,b,c的输出分别是什么？

**1、第一行a**

**var a = fun(0);  a.fun(1);  a.fun(2);  a.fun(3);  
**可以得知，第一个fun(0)是在调用第一层fun函数。第二个fun(1)是在调用前一个fun的返回值的fun函数，所以：

第后面几个fun(1),fun(2),fun(3),函数都是在调用第二层fun函数。

遂：

在第一次调用fun(0)时，o为undefined；

第二次调用fun(1)时m为1，此时fun闭包了外层函数的n，也就是第一次调用的n=0，即m=1，n=0，并在内部调用第一层fun函数fun(1,0);所以o为0；

第三次调用fun(2)时m为2，但依然是调用a.fun，所以还是闭包了第一次调用时的n，所以内部调用第一层的fun(2,0);所以o为0

第四次同理；

即：最终答案为undefined,0,0,0

**2、第二行b**

**var b = fun(0).fun(1).fun(2).fun(3);//undefined,?,?,?  
**先从fun(0)开始看，肯定是调用的第一层fun函数；而他的返回值是一个对象，所以第二个fun(1)调用的是第二层fun函数，后面几个也是调用的第二层fun函数。

遂：

在第一次调用第一层fun(0)时，o为undefined；

第二次调用 .fun(1)时m为1，此时fun闭包了外层函数的n，也就是第一次调用的n=0，即m=1，n=0，并在内部调用第一层fun函数fun(1,0);所以o为0；

第三次调用 .fun(2)时m为2，此时当前的fun函数不是第一次执行的返回对象，而是第二次执行的返回对象。而在第二次执行第一层fun函数时时(1,0)所以n=1,o=0,返回时闭包了第二次的n，遂在第三次调用第三层fun函数时m=2,n=1，即调用第一层fun函数fun(2,1)，所以o为1；

第四次调用 .fun(3)时m为3，闭包了第三次调用的n，同理，最终调用第一层fun函数为fun(3,2)；所以o为2；

即最终答案：undefined,0,1,2

 **3、第三行c**

**var c = fun(0).fun(1);  c.fun(2);  c.fun(3);//undefined,?,?,?  
**根据前面两个例子，可以得知：

fun(0)为执行第一层fun函数，.fun(1)执行的是fun(0)返回的第二层fun函数，这里语句结束，遂c存放的是fun(1)的返回值，而不是fun(0)的返回值，所以c中闭包的也是fun(1)第二次执行的n的值。c.fun(2)执行的是fun(1)返回的第二层fun函数，c.fun(3)执行的也是fun(1)返回的第二层fun函数。

遂：

在第一次调用第一层fun(0)时，o为undefined；

第二次调用 .fun(1)时m为1，此时fun闭包了外层函数的n，也就是第一次调用的n=0，即m=1，n=0，并在内部调用第一层fun函数fun(1,0);所以o为0；

第三次调用 .fun(2)时m为2，此时fun闭包的是第二次调用的n=1，即m=2，n=1，并在内部调用第一层fun函数fun(2,1);所以o为1；

第四次.fun(3)时同理，但依然是调用的第二次的返回值，遂最终调用第一层fun函数fun(3,1)，所以o还为1

即最终答案：undefined,0,1,1

后话

这段代码原本是在做一个将异步回调改写为同步调用的组件时的代码，发现了这个坑，对JS的闭包有了更深入的了解。

关于什么是闭包，网上的文章数不胜数，但理解什么是闭包还是要在代码中自己去发现与领悟。

如果要我说什么是闭包，我认为，广义上的闭包就是指一个变量在他自身作用域的被使用了，就叫发生了闭包。

转      <https://www.jb51.net/article/75450.htm>
