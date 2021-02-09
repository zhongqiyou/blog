---
title: 通知轮播(无脚本)
categories:
- 轮播
tags:
- 原生
---

#### 来源：
根据苏宁官网效果来编写,类似移动端文字轮播,可根据自己的想法进行图片扩展 
#### 样式中包含css中flex布局。如有不懂，前往学习：
http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html
#### 本文涉及技术：
html、css
#### 效果视屏链接：
https://v.youku.com/v_show/id_XNDU2MDIzODAxMg==.html

### 接下来是源代码：


HTML源码
```
<body>
    <div class="container">
        <ul>
            <li>
                <a href="#javasrcipt">热门</a>
                <span> 我是第一个广告文本</span>                
            </li>
            <li>
                <a href="#javasrcipt">最新</a>
                <span>我是第二个广告文本</span>
            </li>
            <li>
                <a href="#javasrcipt">新品</a>
                <span>我是第三个广告文本</span>
            </li>
            <li>
                <a href="#javasrcipt">折扣</a>
               <span>我是第四个广告文本</span>
            </li>
            <li>
                <a href="#javasrcipt">通知</a>
               <span>我是第五个广告文本</span>
            </li>
            <li>
                <a href="#javasrcipt">热门</a>
                <span> 我是第一个广告文本</span>                
            </li>
        </ul>
    </div>
</body>
```
css样式

```
 * {
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
    .container {
        width: 250px;
        height: 30px;
        border-radius: 2px;
        border: 1px solid #000000;
        overflow: hidden;
        line-height: 30px;
    }

    .container>ul {
        width: 250px;
        height: 150px;
        animation: autoFunText 8s infinite;
        font-size: 12px;
    }
    
    .container>ul>li>a{
       background: yellow;
       border-radius:2px;
       text-decoration: none;
       margin: 0 5px;
       padding:0 5px;
   }

    @keyframes autoFunText {

        0% {
            margin-top: 0;
        }

        20% {
            margin-top: -30px;
        }

        40% {
            margin-top: -60px;
        }

        60% {
            margin-top: -90px;
        }

        80% {
            margin-top: -120px;
        }

        100% {
            margin-top: -150px;
        }
    }

    .container li {
        line-height: 30px;
    }

```
以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~





