---
title: 图片无缝轮播(按钮控制+自动切换+鼠标移入/移出)
categories:
- 轮播
tags:
- 原生
---

#### 关于上一篇透明轮播：
没有鼠标移入轮播视图时，让图片停止自动切换 与 鼠标移出轮播视图时，启动自动切换的更换功能。
#### 无缝过渡轮播：
这篇把“鼠标 移入/移出”功能补上 ，图片选自“线上京东”网图。 注意：可能会有失效图片地址。你可以选择自身图片.
#### 样式中包含css中flex布局。如有不懂，前往学习：
http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
#### 本文涉及技术：
html、css、javascript
#### 效果视屏链接：
https://v.youku.com/v_show/id_XNDU2MDMxNDIyNA==.html

### 接下来是源代码：
html布局
```
<body>
    <div class="container">
    <!-- 轮播图片 -->
    <ul>
        <li>
            <img src="http://img20.360buyimg.com/babel/s590x470_jfs/t1/100441/5/12532/101378/5e4a3b4eE4c807442/bf61190512946daa.jpg.webp"
                alt="">
        </li>
        <li>
            <img src="http://img14.360buyimg.com/pop/s590x470_jfs/t1/85986/37/11799/95055/5e3ce64bE2306ef3f/f3314abc6248bbb7.jpg.webp"
                alt="">
        </li>
        <li>
            <img src="http://img13.360buyimg.com/pop/s590x470_jfs/t1/91015/40/10083/87653/5e15aa98Eea5f9113/e3165d6363298955.jpg.webp"
                alt="">
        </li>
        <li>
            <img src="http://img14.360buyimg.com/pop/s590x470_jfs/t1/106727/35/11964/77968/5e44d8afEbe03f26b/d74b87fcced1e74a.jpg.webp"
                alt="">
        </li>
        <li>
            <img src="http://img20.360buyimg.com/babel/s590x470_jfs/t1/100441/5/12532/101378/5e4a3b4eE4c807442/bf61190512946daa.jpg.webp"
                alt="">
        </li>
    </ul>
    <!-- 小按钮 -->
    <ol>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
    </ol>
    <!-- 左右按钮 -->
    <div>
        <a href="#javascript"><</a>
        <a href="#javascript">></a>
    </div>
</div>
</body>
```
js脚本
```
const totalImg = document.querySelector(".container>ul");
const btnLR = document.querySelectorAll(".container>div>a");
const btnSmall = document.querySelectorAll(".container>ol>li");
const view = document.querySelector(".container");

// 初始化第一个小按钮的样式
btnSmall[0].classList.add("btnstyle");

// 获取图片单个宽度
var imgWidth = document.querySelector(".container>ul>li>img").offsetWidth;
var index = 0;
var btnIndex = 0;
var time = null;

// 等待图片过渡完成开关
var bool = true;

// 左按钮
btnLR[0].onclick = function () {
    if (bool) {

        if (index <= 0) {
            index = 3
            totalImg.style.marginLeft = -4 * imgWidth + "px";
        } else { index--; }
        btnStyle(0, "-");
        bool = false;
    }

}

// 右按钮
btnLR[1].onclick = function () {
    if (bool) {

        if (index >= 4) {
            index = 1
            totalImg.style.marginLeft = 0 + "px";
        } else { index++; }
        btnStyle(1, "+");
        bool = false;
    }

}

// 单(小)按钮
btnSmall.forEach((value, i) => {
    btnSmall[i].onclick = function () {
        if (bool) {

            btnIndex = i + 1;
            if (i > index) {
                index = i;
                btnStyle("", "+");
            } else {
                index = i;
                btnStyle("", "-");
            }
            bool = false;
        }
    }
});

// 自动
autoImg();

// 鼠标移入轮播视图取消自动
view.onmouseover = function () {
    clearInterval(time);
}

// 鼠标移出轮播视图启动自动
view.onmouseout = function () {
    autoImg();
}


// 自动切换封装
function autoImg() {
    time = setInterval(() => {
        if (bool) {

            if (index >= 4) {
                index = 1
                totalImg.style.marginLeft = 0 + "px";
            } else { index++; }
            btnStyle(1, "+");
            bool = false;
        }
    }, 2000);
}


// 图片过渡与按钮封装
function btnStyle(arrIndex, str) {
    if (arrIndex == 0) {
        btnIndex <= 0 ? btnIndex = 3 : btnIndex--;
    } else if (arrIndex == 1) {
        btnIndex >= 3 ? btnIndex = 0 : btnIndex++;
    }

    // 小按钮样式
    btnSmall.forEach((value, i) => {
        btnSmall[i].classList.remove("btnstyle");
    });
    btnSmall[btnIndex].classList.add("btnstyle");

    // 图片过渡
    let time = setInterval(() => {

        var distance = -(totalImg.offsetLeft);
        let num = imgWidth * index;
        if (num == distance) {
            clearInterval(time);
            bool = true;
        } else {
            switch (str) {
                case "-":
                    distance = distance - 50;
                    break;
                case "+":
                    distance = distance + 50;
            }
            totalImg.style.marginLeft = -distance + "px";
        }
    }, 20);
}
```

css样式
```
*{
    margin: 0;
    padding: 0;
}
html , body{
    width: 100%;
    height: 100%;
}
body{
    display: flex;
    justify-content: center;
    align-items: center;
}
.container{
    width: 500px;
    height: 400px;
    margin: 0 auto;
    overflow: hidden;
    position: relative;
}
.container>ul{
    list-style: none;
    width:2500px;
    width: 100%;
    display: flex;
}
.container>ul>li>img{
    width: 500px;
    height: 400px;
}

.container>div{
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    position: absolute;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.container>div>a{
    font-size: 20px;
    text-decoration: none;
    color: #ffffff;
    background: rgba(0, 0,0, 0.5);
    padding: 10px;
}
.container>ol{
    position: absolute;
    width: 100px;
    height: 15px;
    left: 200px;
    bottom: 0;
    display: flex;
    list-style: none;
    justify-content: space-between;
    z-index: 99;
}
.container>ol>li{
    width: 15%;
    background: #ffffff;
    border-radius:50% ;
    line-height: 15px;
    text-align: center;
    font-size: 9px;
    cursor: pointer;
}
.btnstyle{
    font-weight: bold;
    color: #ffffff;
    background: #000000 !important;
}

```

无缝轮播也有css3实现方法 ， 不过想控制轮播切换 ， 也是要js脚本。
如果你要尝试css3写法时。提醒你一点：在写js控制轮播时,你必须要监听每一张图结束状态，来设定一个开关。
具体怎么监听：transition也给出了对应的结束函数：transitionend。在这里就不多做介绍了。

对于轮播：无论我们学习哪一个UI框架(例如：boostrap\vant\element)都配有轮播，各种各样！
         最后分享一个轮播库：https://www.swiper.com.cn/

以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~
