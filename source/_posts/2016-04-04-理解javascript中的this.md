---
layout: post
title: 理解javascript中的this
date: 2016-04-04 10:12:38
comments: true
categories: JS
tags: [this, 面向对象]
keywords: JS, 原生, this, 面向对象
description: this指向描述
---
&emsp;&emsp;在js函数中，除了定义函数时的形参，还有两个附加参数，即this和arguments，本篇就讲一下this关键字的用法以及this在不同场景下值。
初学时受启发得知this的指向取决于函数的调用方式，总结下来有以下几点：
1、作为函数调用的模式，this指向window
```javascript
function func(){
    console.log(this);
}

func();             //window
```
2、作为对象的方法调用的时候,this指向方法所在的对象
```javascript
var obj = {};
obj.name = 'hugh dai';
obj.getNmae = function(){
    console.log(this.name);
}

obj.getName();      //hugh dai
```
3、构造函数模式，this指向构造函数的实例
```javascript
function Person(name){
    this.name = name;
    this.getName = function(){
        console.log(this.name);
    }
}

var p = new Person('hugh dai');
p.getName();        //hugh dai
```
4、bind/call/apply方式调用，this指向方法中的第一个参数
```javascript
var foo = {
    x: 3
}
var bar = function(){
    console.log(this.x);
}
bar(); // undefined
bar.bind(foo)();    //3
bar.call(foo);      //3
bar.apply(foo);     //3
```