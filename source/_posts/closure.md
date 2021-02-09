---
title: 什么是闭包？ 
categories:
- 闭包
tags:
- 闭包
---


## 一、闭包是什么？
1.要理解闭包，首先要理解javascript中的变量作用域：变量作用域分为两种 ，一种叫全局变量，另一种叫局部变量。
2.闭包是一个立即执行的函数，把全部变量与方法都都定义在一个function(局部作用域)里边。一般用这个函数内部定义的函数来访问局部变量。所以很多人称它为：定义在一个函数中的函

例子：
```
(function() {
        var num = "变量num1";

        function childOne(){
        console.log(num);
        }

        window.childOne = childOne;
    })();

(function() {
        var num = "变量num2";

        function childTwo(){
        console.log(num);
        }

        window.childTwo = childTwo;
    })();
```
解释：声明一个匿名函数，在内部声明一个变量num 和 函数childOne。在childOne函数中访问外部变量num构成了一个闭包。num作用域在匿名函数开始到结束。
在我声明第二个匿名函数，你会发现，也在内部声明了num变量。在javascript中，无论声明变量，还是声明函数，重复声明，都会发生后面声明的覆盖前面声明的。

好处：变量和方法都可以重复声明互不干扰，不用担心覆盖问题。

最后你会发现，我用window把childOne变成全局变量。至于为什么，接着看例子
```
function parent() {
        var i = 1;
        return function() {
            alert(i++);
        }
    }
    var child = parent();
    child() // i = 1
    child() // i = 2
    child() // i = 3
```
解释：声明一个名为parent函数 ， 在内部声明变量i。函数返回值为一个函数，里边弹一个从函数外部访问的变量i。
把parent函数的返回值 赋值 child变量。然后执行了三次。会发现i一直没有被回收，一直存在内存中。

好处：在内存中维持变量。
注意：这个好处是一把双刃刀，如果太多变量需要维持在内存中，那么网页会变慢，严重会出现网页奔溃

回到刚才第一个例子提出的问题 , 为什么要用window把childOne变成全局变量 ，为什么要举第一个例子：首先来看看第一个例子与第二例子的区别。
第一个：匿名函数立即执行 ， 在内部用window把childOne变成全局变量。
```
  //函数内部
  window.childTwo = childTwo;
```
第二个：声明parent函数，在外部用child变量来 接收 执行的parent的返回值。
```
  //函数外部
  var child = parent();
```
区别：第一个写完了，就可以直接使用了。而第二个还需要在外面手动执行来使用。一个自动，一个手动。


举第一个例子，原因有二：
1、立即执行闭包，优化，全自动。
2、jquery框架中做法 ， 还有它确实比第二个例子好。

闭包经典例子就是jquery框架。


以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~

