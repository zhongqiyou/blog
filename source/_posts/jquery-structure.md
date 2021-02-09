---
title: 解析jQuery基本结构 
categories:
- 框架
tags:
- jQuery
---


## 一、先上代码，再拆代码
```
(function( window, undefined ) {
   var jQuery = function() {
		return new jQuery.prototype.init();
	}
    jQuery.prototype = {
        constructor:jQuery , 
        init:function(){
           this.name = "zqy";
           this.age = 23;
        } , 
        log:function(){
            alert(this.name , this.age)
        }
    }
    jQuery.prototype.init.prototype = jQuery.prototype;
    window.jQuery = window.$ = jQuery;
    })( window );
```

1.从最外层开始 ，如下：
```
(function(window , undefined){

})(window)
```
解释：从上可以看出jQuery的最外层是一个闭包。还带两个参数window ， undefined。
为什么要传window：为了方便后期代码压缩
为什么要传undefined：也是为了后期代码压缩 ，在IE9以下的浏览器undefined可以被修改，为了保证内部使用的undefined不被修改，所以需要接受一个正确的undefined
如果不知道闭包是什么，可以移步去看我写的闭包篇:https://zhongqiyou.github.io/2020/05/07/closure/

2.在闭包中声明构造函数 ， 函数内部带有return
```
var jQuery = function (){
    return  new jQuery.prototype.init();
}
```
解释：声明一个构造函数，如果构造函数中添加return 或者 不添加return，那么new出来的实例会是什么？
两种情况：
第一种:构造函数内部没有添加return 或者 添加return但是没有添加值 再或者添加return但是后面跟着值为“特殊类型”或者“基本类型”数据：
```
function Test(){
     this.name = "zqy";
     return // ; 数字、字符串、num、undefined
}
var test = new Test();
console.log(test);
```
结果，如图所示：

![return1](https://s1.ax1x.com/2020/05/08/YMkeBQ.png)

第二种：构造函数内部添加return , 并rerun后面带的是引用类型数据：
```
function Test(){
     this.name = "zqy";
     return {}
}
var test = new Test();
console.log(test);
```
结果，如图所示：

![return2](https://s1.ax1x.com/2020/05/08/YMkaNR.png)

对比得出：如果构造函数没有添加return或者添加了return但是后面值为“特殊类型”与“基本类型”数据 那么new出来的实例依然指向自己 ， 反之指向return后面带的值；
这里就不多举例了，如果想详细了解：https://www.jianshu.com/p/0bcdf34ba14f

3.原型prototype重定义：
```
 jQuery.prototype = {
        constructor:jQuery , 
        init:function(){
           this.name = "zqy";
           this.age = 23;
        } , 
         log:function(){
            alert(this.name , this.age)
        } , 
        
 }
```
解析：把自己的原型给重新定义。还记得寄生组合继承就是用重定义prototype来实现的吗？
它里边用一个Object.create(obj)方法:创建新对象，里边提供新的\__proto__,而这个\__proto__指向参数obj;然后在和新创建的对象添加上constructor属性，然后就完成了继承。
那上面的代码也是一样，只不过不提供新的\__proto___ , 相当于创建了一个新对象 ，里面补上了constructor属性， 保证原型的完整性。

在没有重新定义原型之前 ，添加属性和方法：
```
jQuery.prototype.name = "zqy";
jQuery.prototype.getName = function(){
    return this.name;
}
```
和上面一比：是不是比之前，编写方便 , 代码直观，好看整齐。

4.改变原型
```
jQuery.prototype.init.prototype = jQuery.prototype;
```
解释:把jQuery重新定义的原型 ， 赋值给init中的原型。也相当于重新定义了init的原型

5.定义全局变量直接使用
```
 window.jQuery = window.$ = jQuery;
```
解析：为了使用闭包内部函数。如果不理解，为什么这样做:请回到1里面提到的地址;


6.为什么要在构造函数return和修改init的原型如下代码：
```
var jQuery = function() {
		return new jQuery.prototype.init();
}
jQuery.prototype.init.prototype = jQuery.prototype;
```

6.1:为了方便初始化数据
没有添加return与重新定义init原型前：
```
  var newJquery = new jQuery();
  newJquery.log(); //undefined undefined
```
//在调用之前newJquery.log()之前 , 必须调用newJquery.init() ,才能拿到结果;

添加return与修改原型后：
```
  var newJquery = new jQuery();
  newJquery.log(); //zqy 23
```
在上面的2讲到函数中的return，那么上面的new代表实例化init构造函数。
但是init原型中并没有log函数方法，所以走上面的4改变原型。

6.2:为了new实例化简写成$()
旧：
```
var newJquery = new jQuery();
```
新：
```
$() 或 jQeury()
```


7.像jQuery一样编码：
为了不打乱上面的代码在下面重新写一次源代码：
```
(function( window, undefined ) {
   var jQuery = function() {
		return new jQuery.prototype.init();
	}
    jQuery.prototype = {
        constructor:jQuery , 
        init:function(){
           this.name = "zqy";
           this.age = 23;
        } , 
        log:function(){
            alert(this.name , this.age)
        } , 
    }
    jQuery.each = function(arr , callBack){
              for (let i = 0; i < arr.length; i++) {
                    callBack(arr[i] , i)         
              }
        }
    jQuery.prototype.init.prototype = jQuery.prototype;
    window.jQuery = window.$ = jQuery;
    })( window );
    
    //使用each
    var arr = [1 , 3 , 5 , "zqy" , 23];
    $.each(arr , function(value , index){
          console.log(value , index)
    })
```
结果如图所示：
![use](https://s1.ax1x.com/2020/05/09/YMQzBq.png)

本文来自李南江老师教学视屏知识点讲解，另外加上我个人的一些理解，把视屏转写成了这章博客。
如果不懂，可以看李南江老师的视屏解析：https://www.bilibili.com/video/BV17W41137jn?p=94




以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~

