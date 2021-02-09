---
title: 透明渐变图片轮播(按钮控制 + 自动切换)
categories:
- 轮播
tags:
- 原生
---


#### 透明过渡轮播：
图片来自“线上京东”网图。
#### 样式中包含css中flex布局。如有不懂，前往学习：
http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
#### 注意：
可能会有失效图片地址。你可以选择自身图片
#### 本文涉及技术：
html、css、javascript
#### 效果视屏链接：
https://v.youku.com/v_show/id_XNDU2MDI4MjgyMA==.html

### 接下来是源代码：
html布局
```
<body>
    <ul class="container">
        <li>
            <img src="http://img14.360buyimg.com/pop/s590x470_jfs/t1/85986/37/11799/95055/5e3ce64bE2306ef3f/f3314abc6248bbb7.jpg.webp" alt="" style="opacity: 1;">
        </li>
        <li>
            <img src="http://img13.360buyimg.com/pop/s590x470_jfs/t1/91015/40/10083/87653/5e15aa98Eea5f9113/e3165d6363298955.jpg.webp" alt="">
        </li>
        <li>
            <img src="http://img14.360buyimg.com/pop/s590x470_jfs/t1/106727/35/11964/77968/5e44d8afEbe03f26b/d74b87fcced1e74a.jpg.webp" alt="">
        </li>
        <li>
            <img src="http://img14.360buyimg.com/pop/s590x470_jfs/t1/107724/16/6091/178936/5e475e18E55aecc6f/597edf74bdb70a3a.jpg.webp" alt="">
        </li>
        <li>
            <img src="http://img20.360buyimg.com/babel/s590x470_jfs/t1/100441/5/12532/101378/5e4a3b4eE4c807442/bf61190512946daa.jpg.webp" alt="">
        </li>
        <ol>
            <div>
                <li>1</li>
                <li>2</li>
                <li>3</li>
                <li>4</li>
                <li>5</li>
            </div>
        </ol>
        <div>
            <a href="#javascript"><</a>
            <a href="#javascript">></a>
        </div>
    </ul>
</body>
```
js脚本
```
var btnLR = document.querySelectorAll(".container>div>a");
var btnOTTFF = document.querySelectorAll(".container>ol>div>li");
var img = document.querySelectorAll(".container>li>img");
var btnStyle = document.querySelectorAll(".container>ol>div>li");

// 初始化小按钮样式
btnStyle[0].classList.add("btnstyle");

var bool = true;
var index = 0;

// 左右按钮切换
btnLR.forEach((value, num) => {
    btnLR[num].onclick = function () {
       if(bool){
        if (num == 0) {
            index <= 0 ? index = 4 : index--
        } else {
            index >= 4 ? index = 0 : index++
        }
        time();
        bool = false;
       }
    }
});

// 小按钮切换
btnStyle.forEach((value, num) => {
    btnStyle[num].onclick = function () {
        if (bool) {
            index = num;
            time()
            bool = false;
        }
    }
});

// 自动切换
setInterval(() => {
    if(bool){
        index >= 4 ? index = 0 : index++;
        time();
        bool = false;
    }
    
}, 2000);

function time() {
    // 初始化全部照片
    img.forEach((value, i) => {
        img[i].style.opacity = 0;
    });

    // 透明过渡显示
    let tranNum = 0
    let time = setInterval(() => {
        tranNum = tranNum + 0.1;
        if (tranNum > 1) {
            clearInterval(time);
            bool = true;
        }
        img[index].style.opacity = tranNum;
    }, 30);

    // 按钮切换样式
    btnStyle.forEach((value, i) => {
        btnStyle[i].classList.remove("btnstyle");
    });
    btnStyle[index].classList.add("btnstyle");
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
    list-style: none;
    position: relative;
    margin: 0 auto;
}
.container>li{
    width: 100%;
    height: 100%;
    position: absolute;
}
.container>li>img{
    width: 100%;
    height: 100%;
    opacity: 0;
}
.container>ol{
    width: 100px;
    height: 20px;
    position:absolute;
    list-style: none;
    bottom: 0;
    left: 200px; 
    z-index: 99;
}
.container>ol>div{
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.container>ol>div>li{
    background: #ffffff;
    border-radius: 100%;
    width: 15%;
    height: 75%;
    font-size: 9px;
    line-height: 125%;
    text-align: center;
    cursor: pointer;
}
.container>div{
    width: 100%;
    height: 100%;
    position: absolute;
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.container>div>a{
    display: block;
    width: 30px;
    height: 30px;
    background:rgba(0, 0, 0, 0.5);
    color: #ffffff;
    font-size: 30px;
    line-height: 30px;
    text-decoration: none;
    text-align: center;
}
.btnstyle{
    font-weight: bold;
    background: #000000 !important;
    color: #ffffff;
}
```

以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~
