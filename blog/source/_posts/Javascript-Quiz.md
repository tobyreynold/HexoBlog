---
title: Javascript Quiz
date: 2016-06-07 17:14:30
tags: JavaScript
---

## 简单的几道JavaScript Quiz

本人收集了几道不错JavaScript小题，能cover住一部分JavaScript比较重要的基础部分，汇总分享出来学习一下，答案在文章最后。

## 第一题

``` bash
(function(){ 
  return typeof arguments; 
})();
``` 

考察arguments相关知识，[参考](https://developer.mozilla.org/zh-CN/docs/JavaScript/Reference/Operators/typeof)

## 第二题

``` bash
var f = function g(){ return 23; }; 
typeof g();
``` 

考察函数表达式相关知识，[参考](http://kangax.github.io/nfe/)

## 第三题

``` bash
(function(x){ 
  delete x; 
  return x; 
})(1);
``` 

考察参数是否可以被删除，[参考](http://perfectionkills.com/understanding-delete/)

## 第四题

``` bash
var y = 1, x = y = typeof x; 
x;
``` 

考察typeof相关知识

## 第五题

``` bash
(function f(f){ 
  return typeof f(); 
})(function(){ return 1; });
``` 

考察自执行函数和参数名称冲突的相关问题

## 第六题

``` bash
var foo = {  
  bar: function() { return this.baz; },  
  baz: 1 
}; 

(function(){  
  return typeof arguments[0](); 
})(foo.bar);
``` 

考察this指向以及参数问题，[参考](https://developer.mozilla.org/en/Core_JavaScript_1.5_Reference/Operators/Special_Operators/this_Operator#Description)

## 第七题

``` bash
var foo = { 
  bar: function(){ return this.baz; }, 
  baz: 1 
} 
typeof (f = foo.bar)();
``` 

还是this指向问题。


## 第八题

``` bash
var f = (function f(){ return "1"; }, function g(){ return 2; })(); 
typeof f;
``` 

考察逗号和括号表达式问题，[参考](http://www.2ality.com/2012/09/expressions-vs-statements.html)

## 第九题

``` bash
var x = 1; 
if (function f(){}) { 
  x += typeof f; 
} 
x;
```

考察if语句判断中不是statement的情况，以及typeof statement的情况。

## 第十题

``` bash
(function f(){ 
  function f(){ return 1; } 
  return f(); 
  function f(){ return 2; } 
})();
```

考察函数生命解析顺序问题

## 第十一题

``` bash
function f(){ return f; } 
new f() instanceof f;
```

考察new运算符相关知识，[参考](http://www.cnblogs.com/aaronjs/archive/2012/07/04/2575570.html)

## 第十二题

``` bash
var x = [typeof x, typeof y][1];
typeof typeof x;
```

考察typeof和[][1]的相关问题。

## 第十三题

``` bash
function(foo){ 
  return typeof foo.bar; 
})({ foo: { bar: 1 } });
```

纯文字游戏，考查传进去的参数问问题。

## 第十四题

``` bash
with (function(x, undefined){}) length;
``` 

考察with，和函数length问题，其实就是参数的个数，[参考](http://developer.mozilla.org/en/docs/Core_JavaScript_1.5_Reference:Statements:with)


## 答案汇总
1. "object"; 2. 报错; 3. 1; 4."undefined"; 5."number"; 6."undefined"; 7."undefined"; 8."number"; 9."1undefined"; 10.2; 11.false; 12."string"; 13."undefined"; 14.2 。








