---
title: 1-0微信小程序云开发（入门简介）
categories:
- 微信小程序
tags:
- 云开发
---

## 一、云开发简介
1、云开发是微信小程序提供给我们的服务器。
2、包含：数据库、云函数、存储。（会在后面篇一一简介）

## 二、对比
1、如果要问它是用来干什么？有什么好处？那得谈一谈“传统服务器”。
2、服务器分为两种：线下服务器（本地），线上服务器（阿里云，腾讯云.）。
  2.1、本地服务器：通常用wampServer、phpstudy一些集成环境软件。集成环境包括：php，mysql，Apache。也就是说如果你想搭建自己的本地服务器，你需要懂得一门后端语言和一门数据库。
3.传统服务器需要域名，微信小程序提供的不用。注意：微信小程序只支持https
4.线上服务器需要付费使用，微信小程序提供的免费

好处：学习成本低，简单入门。

## 三、开通并初始化
如图步骤：
1.在微信的左上角调试器旁边有一个云开发，在这里我们点击云开发
[查看大图](https://s1.ax1x.com/2020/05/17/YRXsSA.png)
![one](https://s1.ax1x.com/2020/05/17/YRXsSA.png)

2.点击开通云开发
[查看大图](https://s1.ax1x.com/2020/05/17/YRjKXt.png)
![two](https://s1.ax1x.com/2020/05/17/YRjKXt.png)

3.官网建议我们建两个环境一个测试环境一个正式环境，在这里我就新建一个test方便后面的开发
[查看大图](https://s1.ax1x.com/2020/05/18/YRj4HK.png)
![three](https://s1.ax1x.com/2020/05/18/YRj4HK.png)

4.环境到这里就配置完成了
[查看大图](https://s1.ax1x.com/2020/05/18/YRvn5F.png)
![four](https://s1.ax1x.com/2020/05/18/YRvn5F.png)

5.初始化
5.1、初始化需要环境ID， 查找ID
[查看大图](https://s1.ax1x.com/2020/05/18/YRxlFS.png)
![five](https://s1.ax1x.com/2020/05/18/YRxlFS.png)

5.2、app.js文件 ，完成初始化
[查看大图](https://s1.ax1x.com/2020/05/18/YRxOTf.png)
![six](https://s1.ax1x.com/2020/05/18/YRxOTf.png)
初始化官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/init.html


提示：如果你想使用云函数，要在project.config.json文件进行配置。
```
{
   "cloudfunctionRoot": "./functions/"
}
```
官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/functions/getting-started.html


以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~


