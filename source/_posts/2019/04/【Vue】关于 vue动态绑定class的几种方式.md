---
title: 
date: 2019-04-20 10:29:25
tags: Vue
categories: Vue

---
###对象方法  
\-最简单的绑定（这里的active加不加单引号都可以，以下也一样都能渲染）

：class=“{‘active’:isTrue}”

判断是否绑定一个active

:class\="{'active':isActive==-1}" 或者
:class\="{'active':isActive==index}"

绑定并判断多个

第一种（用逗号隔开）
```js
:class="{ 'active': isActive, 'sort': isSort }"
```
第二种（放在data里面）  
//也可以把后面绑定的对象写在一个变量放在data里面，可以变成下面这样
```js
:class="classObject" data() { return {
        classObject:{ active: true, sort:false }
    }
}
```

第三种（使用computed属性）
```js
:class="classObject" data() { return {
        isActive: true,
        isSort: false }
},
computed: {
    classObject: function () { return {
          active: this.isActive,
          sort:this.isSort
       }
          
　　}
} 
```js
###数组方法

单纯数组
```js
:class="\[isActive,isSort\]" data() { return{
    isActive:'active',
    isSort:'sort' }
}
```
数组与三元运算符结合判断选择需要的class  
（注意：三元运算符后面的“：”两边的class需要加上单引号，否则不能正确渲染）
```js
:class="\[isActive?'active':''\]" 或者
:class\="\[isActive==1?'active':''\]" 或者
:class\="\[isActive==index?'active':''\]" 或者
:class\="\[isActive==index?'active':'otherActiveClass'\]"
```
数组对象结合动态判断  
//前面这个active在对象里面可以不加单引号，后面这个sort要加单引号

//前面这个active在对象里面可以不加单引号，后面这个sort要加单引号
```js
:class="\[{ active: isActive }, 'sort'\]" 或者
:class\="\[{ active: isActive==1 }, 'sort'\]" 或者
:class\="\[{ active: isActive==index }, 'sort'\]"  
```
原文链接：https://blog.csdn.net/qq\_43077894/article/details/83544399