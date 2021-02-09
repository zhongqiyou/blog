---
title: 1-1微信小程序云开发（数据库入门使用）
categories:
- 微信小程序
tags:
- 云开发
---

## 一、数据库（官方简介）
云开发提供了一个 JSON 数据库，顾名思义，数据库中的每条记录都是一个 JSON 格式的对象。一个数据库可以有多个集合（相当于关系型数据中的表），集合可看做一个 JSON 数组，数组中的每个对象就是一条记录，记录的格式是 JSON 对象。
官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/capabilities.html#%E6%95%B0%E6%8D%AE%E5%BA%93

## 二、使用
1、为了更简单的入门 ，这里不使用官方提供的云开发模板 

![nocloud](https://s1.ax1x.com/2020/05/18/YfyX7V.png)

1.1、我这里把不需要用到的文件进行了删除，最终文件结构展示
[查看大图](https://s1.ax1x.com/2020/05/18/Yf6TUK.png)
![structure](https://s1.ax1x.com/2020/05/18/Yf6TUK.png)

2、要想使用数据库，第一步初始化我们的云环境了。
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

3.创建一个集合
[查看大图](https://s1.ax1x.com/2020/05/18/YfBVDf.png)
![create](https://s1.ax1x.com/2020/05/18/YfBVDf.png)

3.1、打开云开发 ， 点击数据库 ， 填写集合名称。我这里创建的是list

4.集合的基本操作就是：增、删、改、查。与mysql中操作表的意思是一样的。
4.1、操作前 , 我把改动过的文件都展示出来:
pages/index/index.wxml文件:
```
<view>
<text>云数据库测试:</text>
<input type="text" bindinput="getId" placeholder="ID"></input>
<input type="text" bindinput="getName" placeholder="姓名"></input>
<input type="text" bindinput="getAge" placeholder="年龄"></input>
<button bindtap="insert" type="primary">增加数据</button>
<button bindtap="select" type="primary">查询数据</button>
<button bindtap="update" type="primary">修改数据</button>
<button bindtap="delete" type="primary">删除数据</button>
</view>
```
pages/index/index.wxss文件:
```
button{
  margin: 5px auto;
}
input[type=text]{
 width: 80%;
 margin: 5px auto;
 border: 1px solid #cccccc;
}
```

pages/index/index.js文件：
```
// 声明要操作的数据集合
const db = wx.cloud.database().collection('list');
Page({
  data:{
    id:"" , 
    name:"" , 
    age:""
  } , 

// 获取表单中的value值
  getId(event){
   this.setData({
     id:event.detail.value
   })
  } , 
  getName(event){
    this.setData({
      name: event.detail.value
    })
  } , 
  getAge(event){
    this.setData({
      age: event.detail.value
    })
  } ,

// 插入数据
  insert(){
    var _this = this;
    db.add({
      //参数
      data: {
        _id: _this.data.id, 
        name: _this.data.name, 
        age: _this.data.age  
      },
      success: function (res) {
        console.log(res);
      }
    })
  } , 

// 查询数据
  select() { 
    var _this = this;
    
    // 指定查询（一条）
    // db.doc(_this.data.id).get({
    //   success: function (res) {
    //     console.log(res)
    //   }
    // })

    // 条件查询（一条或多条）
    // db.where({
    //   name: _this.data.name ,
    //   age: _this.data.age
    // })
    // .get({
    //     success: function (res) {
    //       console.log(res)
    //     }
    //   })

    // 所有查询（全部）
    db.get({
      success: function (res) {
        console.log(res.data)
      }
    })
  } ,

// 更新数据
  update() { 
    var _this = this;
    //传入对应数据的id
    db.doc(_this.data.id).update({
      data: {
        name: _this.data.name , 
        age:_this.data.age
      },
      success: function (res) {
        console.log(res)
      }
    })
  } ,

  // 删除数据
  delete() {
    var _this = this;
    //传入对应数据的id
    db.doc(_this.data.id).remove({
      success: function (res) {
        console.log(res)
      }
    })
   }
})
```
注意：1.添加时 ，集合中如果不传入"_id"字段，那么会自动生成。反之传入，会把该值保留为_id字段中的值。
     2.获取时 ，上面提到三种：通过对应id获取一条。通过满足条件获取一条或多条，官方还提供一些额外命令（这里不做介绍）。最后是获取全部，但是最多只能获取20条。现要突破获取更多的，需要借助“云函数”来实现(后面篇介绍)

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
官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/database/init.html


源代码：https://github.com/zhongqiyou/wechat-db









以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~


