---
title: jQuery 自定义（美化）滚动条 customcontentscroller 插件使用
categories:
- jquery
tags:
- jquery插件
---

## 官方地址：
http://manos.malihu.gr/jquery-custom-content-scroller/

## customcontentscroller简介：
依赖于jquery ， 能轻松追加很好看（美化）的滚动条


## 使用步骤:
1.下载并引入jQuery类库极其相关的插件js和css库
```
<link rel="stylesheet" href="malihu-custom-scrollbar-plugin-master/jquery.mCustomScrollbar.css" />
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js"></script>
<script src="malihu-custom-scrollbar-plugin-master/jquery.mCustomScrollbar.concat.min.js"></script>
```
注意：如果你在官方复制的jquery1.11.1版本，它会抛出以下错误:

![Image text](https://s1.ax1x.com/2020/04/19/JuWef0.png)

2.给元素追加自定义滚动条方法:
  2-1. 设置滚动条模板方式一：通过标签中属性节点方法设置
```
<div class="container" data-mcs-theme="dark">
  <!-- your content -->
</div>

 $(function() {
        $(".content").mCustomScrollbar();
    })
```
  2-2. 设置滚动模板条方式二：通过js方法设置
```
<div class="container">
  <!-- your content -->
</div>

$(function() {
        $(".content").mCustomScrollbar({
            theme: "rounded-dots"
        });
})

```

3.如果需要横向滑动，只需要在原来代码添加axis：
```
$(function() {
        $(".content").mCustomScrollbar({
            theme: "rounded-dots" , 
             axis: "x"
        });
```


以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~


