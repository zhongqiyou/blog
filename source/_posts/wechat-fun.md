---
title: 1-2微信小程序云开发（云函数入门使用）
categories:
- 微信小程序
tags:
- 云开发
---


## 一、云函数（官方简介）
云函数是一段运行在云端的代码，无需管理服务器，在开发工具内编写、一键上传部署即可运行后端代码。

小程序内提供了专门用于云函数调用的 API。开发者可以在云函数内使用 wx-server-sdk 提供的 getWXContext 方法获取到每次调用的上下文（appid、openid 等），无需维护复杂的鉴权机制，即可获取天然可信任的用户登录态（openid）。
官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/capabilities.html#%E4%BA%91%E5%87%BD%E6%95%B0


## 来谈谈云函数的作用
1.云函数可以直接方便的拿到我们openid（解答疑问：“openid”相当于我们现实生活中的“身份证”）。
要知道我们以前是怎么拿的openid，这里我就不多介绍了，看这里：https://jingyan.baidu.com/article/fdffd1f898a588f3e98ca1a7.html
扯一扯：因为在页面操控，你的appid , 秘钥 ，code一些敏感信息会暴露,还有微信小程序上线时，也因这个审核不过。你需要自己搭建的后台。

2.操作数据库
2.1、不受数据库权限影响
[查看大图](https://s1.ax1x.com/2020/05/24/YzzLHH.png)
![jurisdiction](https://s1.ax1x.com/2020/05/24/YzzLHH.png)
意思就是：别人发布的东西你也可以操作。不受权限限制

2.2、获取数据库中的数据，突破20条。
原来的数据库操作，只能最多获取20条。现在使用云函数，可以获取更多

还有一些微信支付：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/wechatpay.html

## 二、使用
1、为了更简单的入门 ，这里不使用官方提供的云开发模板 

![nocloud](https://s1.ax1x.com/2020/05/18/YfyX7V.png)

1.1、我这里把不需要用到的文件进行了删除，最终文件结构展示
[查看大图](https://s1.ax1x.com/2020/05/24/YzNeN4.png)
![structure](https://s1.ax1x.com/2020/05/24/YzNeN4.png)

在文件结构你会发现，多了一个functions | test 文件夹。先不要疑问，接着往下看...

2、要想使用云开发，第一步在app.js文件初始化我们的云环境 与 配置project.config.json文件。
app.js文件
```
App({
// 初始化云服务环境
  onLaunch:function(){
    wx.cloud.init({
      env: 'test-fph51'
    })
  }
})
```

2.1、打开云开发 ，点击设置查看云环境ID。上面env键 对应的值为 云环境ID
如有不懂，这里了解：https://zhongqiyou.github.io/2020/05/17/wechat-cloud/

2.2、初始化云函数文件夹
project.config.json文件
```
	"cloudfunctionRoot": "functions/" //文件夹名称
```
//因为只是添加一行代码，所以就不全部编写了。看图：
[查看大图](https://s1.ax1x.com/2020/05/24/Yza3kD.png)
![configure](https://s1.ax1x.com/2020/05/24/Yza3kD.png)

看到这里就可以解释functions | test 文件夹：新建的文件夹（初始化一个装云函数的总文件夹）
functions:就是在project.config.json文件配置的cloudfunctionRoot属性 , 后面的值就是代表文件夹名称
test：后面跟着的就是，我们初始化的云开发环境名称
注意：文件夹的icon必须带着云朵 ， funcions 后面也必须有 云环境显示（如果显示的是未定义。那就是没初始化成功）

3、在新创建的文件夹中创建云函数
3.1新建Node.js 云函数 , 如图：
[查看大图](https://s1.ax1x.com/2020/05/24/YzjYX4.png)
![create](https://s1.ax1x.com/2020/05/24/YzjYX4.png)

我这里新建了两个：1.getBreach , 2.getOpenId。每一个云函数文件夹都自动生成两个文件分别为：index.js , package.json


4.下面展示两个云函数
4.1.获取openid操作
getOpenId文件夹中index.js
```
// 云函数入口文件
const cloud = require('wx-server-sdk')

cloud.init("test-fph51")

// 云函数入口函数
exports.main = async (event, context) => {
  const wxContext = cloud.getWXContext();

  return {
   event , 
   context , 
   wxContext
  }
}
```

4.2.突破20条数据操作
getBreach文件夹中index.js
```
const cloud = require('wx-server-sdk')
cloud.init("test-fph51")
const db = cloud.database()
const MAX_LIMIT = 100
exports.main = async (event, context) => {
  // 先取出集合记录总数
  const countResult = await db.collection('list').count()
  const total = countResult.total
  // 计算需分几次取
  const batchTimes = Math.ceil(total / 100)
  // 承载所有读操作的 promise 的数组
  const tasks = []
  for (let i = 0; i < batchTimes; i++) {
    const promise = db.collection('list').skip(i * MAX_LIMIT).limit(MAX_LIMIT).get()
    tasks.push(promise)
  }
  // 等待所有
  return (await Promise.all(tasks)).reduce((acc, cur) => {
    return {
      data: acc.data.concat(cur.data),
      errMsg: acc.errMsg
    }
  })
}
```
突破20条官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/database/read.html

云函数使用官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/functions/userinfo.html

注意：当你看完我的与官方的源码后，你会发现 , 我在我的源码再次初始化。如下(我只把我与官方的代码不同处展示出来，其他的不动)：
```
//官方
cloud.init()

//我的
cloud.init("test-fph51")
```
我这样做的原因是，当我们有两个云开发环境的时候系统不知道我们想要初始化那一个环境，所以需要我们要再次初始化一次（此情况偶尔出现）

每次改动都要重新上传部署，如图：
[查看大图](https://s1.ax1x.com/2020/05/24/tSiE9A.png)
![upload](https://s1.ax1x.com/2020/05/24/tSiE9A.png)


4.1、操作前 , 我把改动过的文件都展示出来:
pages/index/index.wxml文件:
```
<view>
<text>云函数测试：</text>
<button type="primary" bindtap="getOpenId">获取openId</button>
<button type="primary" bindtap="getBreach">突破获取20条数据</button>
</view>
```
pages/index/index.wxss文件:
```
button{
  margin: 5px auto;
}
```

pages/index/index.js文件：
```
Page({
  // 获取openId
   getOpenId(){
     wx.cloud.callFunction({
       // 云函数名称
       name: 'getOpenId',
       // 传给云函数的参数
       data: {
         title: "测试数据"
       },
       success: function (res) {
         console.log(res)
         console.log("openId:" + res.result.event.userInfo.openId)
       },
       fail: console.error
     })
   } ,

  //  突破20条数据
  getBreach(){
    wx.cloud.callFunction({
      // 云函数名称
      name: 'getBreach',
      success: function (res) {
        console.log(res)
      },
      fail: console.error
    })
  }
})

```

app.json文件：
```
{
  "pages":[
    "pages/index/index"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "WeChat",
    "navigationBarTextStyle":"black"
  },
  "style": "v2",
  "sitemapLocation": "sitemap.json"
}
```
因为把原来的pages/log文件夹删除。所以这里也要把配置过的log路径移除


源代码：https://github.com/zhongqiyou/wechar-fun







以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~