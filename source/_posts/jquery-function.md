---
title: 进度条类封装(jQuery基本结构编写)
categories:
- 面向对象
tags:
- jquery
---

## 效果：https://v.youku.com/v_show/id_XNDY2ODI3NDg2OA==.html

## 一、为什么用jQuery基本结构来编写。
```
(function( window ) {
   var jQuery = function() {
		return new jQuery.prototype.init();
	}
    jQuery.prototype = {
        constructor:jQuery , 
        init:function(){
        }
    }
    jQuery.prototype.init.prototype = jQuery.prototype;
    window.jQuery = window.$ = jQuery;
    })( window );
```
好处：闭包可以让变量和方法 与 外界“互不干扰”。方便编写类 , 提高代码维护。
结构介绍，请到这里了解：https://zhongqiyou.github.io/2020/05/08/jquery-structure/
 
## 二、接下来就直接上源代码:
progress.js文件：
```
(function(window) {
    function ProgressBar(external, inside, dot) {
        return new ProgressBar.prototype.init(external, inside, dot);
    }
    ProgressBar.prototype = {
        constructor: ProgressBar,
        init: function(external, inside, dot) {
            this.external = external;
            this.inside = inside;
            this.dot = dot;
        },
        // 拖拽
        drag: function() {
            let _this = this;
            let realLeft, offLeft, result;
            let totalWidth = _this.external.offsetWidth;
            // 改变小圆点位置
            this.external.onmousedown = function(event) {
                    realLeft = event.clientX; //获取触发对象中鼠标的X位置
                    offLeft = _this.external.offsetLeft;//获取DOM元素，到自己的offsetParent（祖先）的距离
                    result = realLeft - offLeft;
                    _this.dot.style.left = result + "px";
                    _this.inside.style.width = result + "px";
                }
                // 移动小圆点位置
            this.dot.onmousedown = function() {
                document.onmousemove = function(event) {
                    realLeft = event.clientX;
                    offLeft = _this.external.offsetLeft;
                    result = realLeft - offLeft;
                    if (result < 0) {
                        result = 0;
                    } else if (result > totalWidth) {
                        result = totalWidth;
                    }
                    _this.dot.style.left = result + "px";
                    _this.inside.style.width = result + "px";
                }
            }
            document.onmouseup = function() {
                document.onmousemove = null;
            }
        }
    }
    ProgressBar.prototype.init.prototype = ProgressBar.prototype;
    window.ProgressBar = ProgressBar
})(window)
```
关于client、offset位置 ,如果有疑问 , 请到这里了解：https://zhongqiyou.github.io/2020/05/06/position/

样式：
```
 * {
        margin: 0;
        padding: 0;
    }
    
    body {
        background: rgba(0, 0, 0, 0.3);
    }
    
    .container {
        margin: 100px;
    }
    
    .bar,
    .bar2 {
        width: 800px;
        height: 5px;
        background: rgba(255, 255, 255, 0.5);
        margin-left: 60px;
    }
    
    .bar>div,
    .bar2>div {
        width: 0;
        height: 100%;
        position: relative;
        background: white;
    }
    
    .bar>div>i,
    .bar2>div>i {
        display: block;
        width: 16px;
        height: 16px;
        border-radius: 8px;
        background: white;
        position: absolute;
        left: 0px;
        top: -6.5px;
    }
    
    .back-img {
        width: 50px;
        height: 50px;
        position: relative;
        top: 25px;
        background: url("https://s1.ax1x.com/2020/05/11/YGRmxP.png");
        background-size: cover;
    }
    
    .icon {
        background: url("https://s1.ax1x.com/2020/05/11/YGR1aQ.png");
        background-size: cover;
    }
```
HTML:
```
 <!-- 播放 -->
    <div class="container">
        <div class="back-img"></div>
        <div class="bar">
            <div>
                <i></i>
            </div>
        </div>
    </div>

    <!-- 声音 -->
    <div class="container">
        <div class="back-img icon"></div>
        <div class="bar2" style="width:100px;">
            <div>
                <i></i>
            </div>
        </div>
    </div>

//使用
<script src="progress.js"></script>
<script>
    //播放
    var external = document.querySelector(".bar");
    var inside = document.querySelector(".bar>div");
    var dot = document.querySelector(".bar>div>i");

    //声音
    var external2 = document.querySelector(".bar2");
    var inside2 = document.querySelector(".bar2>div");
    var dot2 = document.querySelector(".bar2>div>i");

    //可以用new的方式
    // var bar = new ProgressBar(external, inside, dot);
    // var bar2 = new ProgressBar(external2, inside2, dot2);
    // bar.drag()
    // bar2.drag()
   
    //或者
    ProgressBar(external, inside, dot).drag();
    ProgressBar(external2, inside2, dot2).drag();
</script>
```

以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~


