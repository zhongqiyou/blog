---
title: 1-3微信小程序云开发（云存储入门使用）(完结)
categories:
- 微信小程序
tags:
- 云开发
---


## 一、云存储（官方简介）
云开发提供了一块存储空间，提供了上传文件到云端、带权限管理的云端下载能力，开发者可以在小程序端和云函数端通过 API 使用云存储功能。
在小程序端可以分别调用 wx.cloud.uploadFile 和 wx.cloud.downloadFile 完成上传和下载云文件操作。

官方地址：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/basis/capabilities.html#%E5%AD%98%E5%82%A8


## 来谈谈云存储的作用
虽然提供了数据库，但是数据库只能存一些文字（路径，评论，标题），但是我们有时候想上传一些媒体或者文件呢？
云存储就是解决这些问题。我们常上传就是 图片，视屏，word，excel了。上传完成后它会生成一个线上地址返回给我们。


## 二、使用
1、为了更简单的入门 ，这里不使用官方提供的云开发模板 

![nocloud](https://s1.ax1x.com/2020/05/18/YfyX7V.png)

1.1、我这里把不需要用到的文件进行了删除，最终文件结构展示
[查看大图](https://s1.ax1x.com/2020/05/25/tpHojJ.png)
![structure](https://s1.ax1x.com/2020/05/25/tpHojJ.png)



2、老规矩，要想使用云开发，第一步在app.js文件初始化我们的云环境。
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



4.1、操作前 , 我把改动过的文件都展示出来:
pages/index/index.wxml文件:
```
<view>
<button type="primary" bindtap="getImageSrc">图片上传</button>
<text>展示图片：</text>
<image src="{{imageSrc}}"></image>
</view>

<view>
<button type="primary" bindtap="getVideoSrc">视屏上传</button>
<text>展示视屏：</text>
<video src="{{videoSrc}}"></video>
</view>


<view>
<button type="primary" bindtap="getFileSrc">文件（excel）上传</button>
<button type="primary" bindtap="getDownloadOpen">下载该上传文件并打开</button>
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
 data:{
   imageSrc:"" , 
   videoSrc:"" ,
   fileSrc:""
 } , 
  // 上传图片并展示
  getImageSrc(){
    var _this = this;
    // 本地选择图片
    wx.chooseImage({
      count: 1,
      sizeType: ['original', 'compressed'],
      sourceType: ['album', 'camera'],
      success(res) {
        // 文件后缀名处理
        var src = res.tempFilePaths[0];
        var index = src.lastIndexOf(".");
        var suffix = src.slice(index , src.length);
        // 图片上传
        wx.cloud.uploadFile({
          cloudPath: Math.random() + suffix, // 上传至云端的路径
          filePath: res.tempFilePaths[0], // 小程序临时文件路径
          success: res => {
            // 获取图片路径并赋值
            _this.setData({
              imageSrc: res.fileID
            })
          },
          fail: console.error
        })
      }
    })
  } , 
 // 上传视屏并展示
  getVideoSrc(){
    var _this = this;
     // 本地选择s视屏
    wx.chooseVideo({
      sourceType: ['album', 'camera'],
      maxDuration: 60,
      camera: 'back',
      success(res) {
         // 文件后缀名处理
        var src = res.tempFilePath;
        var index = src.lastIndexOf(".");
        var suffix = src.slice(index , src.length);
        // 视屏上传
        wx.cloud.uploadFile({
          cloudPath: Math.random() + suffix, // 上传至云端的路径
          filePath: src, // 小程序临时文件路径
          success: res => {
              // 获取视屏路径并赋值
            _this.setData({
              videoSrc: res.fileID
            })
          },
          fail: console.error
        })
      }
    })
  } , 

 // 上传文件word或excel
  getFileSrc(){
    var _this = this;
    // 选择文件
    wx.chooseMessageFile({
      count: 1,
      type: 'all',
      success(res) {
         // 文件后缀名处理
        var src = res.tempFiles[0].path;
        var index = src.lastIndexOf(".");
        var suffix = res.tempFiles[0].path.slice(index , src.length);
        // 上传文件
        wx.cloud.uploadFile({
          cloudPath: Math.random() + suffix, // 上传至云端的路径
          filePath: src, // 小程序临时文件路径
          success: res => {
            // 获取视屏路径并赋值
           _this.setData({
             fileSrc: res.fileID
           })
          },
          fail: console.error
        })
      }
    })
  } ,

// 下载文件并打开
getDownloadOpen(){
  var _this = this;
  // 下载文件
  wx.cloud.downloadFile({
    fileID: _this.data.fileSrc, // 文件 ID
    success: res => {
      // 返回临时文件路径
      console.log(res.tempFilePath)
      // 打开文件
      wx.openDocument({
        filePath: res.tempFilePath,
        success: function (result) {
          console.log('打开文档成功' , result)
        }
      })
    },
    fail: console.error
  })
}
})
```
官方地址源代码：
选择本地图片文件：https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.chooseImage.html
选择本地视屏文件：https://developers.weixin.qq.com/miniprogram/dev/api/media/video/wx.chooseVideo.html
选择本地文件：https://developers.weixin.qq.com/miniprogram/dev/api/media/image/wx.chooseMessageFile.html
文件（上传，下载，删除）：https://developers.weixin.qq.com/miniprogram/dev/wxcloud/guide/storage/api.html
打开文件：https://developers.weixin.qq.com/miniprogram/dev/api/file/wx.openDocument.html

你从每一个上传函数里，都会看到以下代码：
```
  var src = res.tempFiles[0].path;
  var index = src.lastIndexOf(".");
  var suffix = res.tempFiles[0].path.slice(index , src.length);
```
解析：这是为了智能的获取后缀名。因为有些文件后缀名个数不一，后缀名不一。
比如图片,：.JPEG ， .JPG , .GIF , .PNG , .PDF , .SVG , 还有谷歌独特格式 .webp

你会发现我每次上传用随数作为路径 ， 如下：
```
cloudPath: Math.random() + suffix, // 上传至云端的路径
```
解释：还记得IE的缓存问题吗，如果通过ajax发送GET请求，那么IE浏览器认为同一个URL只用一个结果。
这里类似，所以解决方法是：每次请求不同的地址


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

源代码：https://github.com/zhongqiyou/wechat-storage







以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~