---
title: AJAX终极封装（兼容IE） 
categories:
- ajax
tags:
- ajax
---


## ajax简介：
AJAX 是与服务器交换数据并更新部分网页的艺术，在不重新加载整个页面的情况下。

## 学习地址：
https://www.w3school.com.cn/ajax/index.asp

js代码：
```
//格式化数据（“{name:'zqy',age:21} => name=zqy&age=23”）
function format(data) {
    var arr = [];
    // 兼容ie缓存问题:每次访问同一个url默认同一个结果
    // 所以解决方案 , 每一次请求不同的url 
    data.ran = Math.ceil(Math.random() * 10); //添加随机数
    for (var key in data) {
        // 在url中是不能出现中文的 ， 如果出现了中文需要转码 , 使用encodeURLComponent
        arr.push(encodeURIComponent(key) + "=" + encodeURIComponent(data[key]));
    };
    return arr.join("&");
};

function ajax(options) {
    // type(请求方式), url(请求地址), data(请求参数), time(请求超时时间), success(成功回调), error(失败回调)
    var xml;
    // 兼容ie对象不同问题
    if (window.XMLHttpRequest) {
        xml = new XMLHttpRequest();
    }
    else {
        xml = new ActiveXObject("Microsoft.XMLHTTP");
    };
    
    if (options.type.toUpperCase() == "GET") { //不区分大小写
        xml.open(options.type, options.url + "?" + format(options.data), true);
        xml.send();
    } else {
        xml.open(options.type, options.url, true);
        xml.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
        xml.send(format(options.data));
    }

    xml.onreadystatechange = function () {
        if (xml.readyState === 4) {
            if (xml.status === 200 || xml.status === 304) {
                options.success(xml);
            } else {
                options.error(xml);
            }
        }
    }

    // 超时终止请求
    if (options.time) {
        setTimeout(function () {
            xml.abort();
        }, options.time);
    }
}


//测试封装
 ajax({
        url: "test.php",
        type: "post",
        data: {
            "name": "zqy",
            "age": "23"
        },
        time: 3000,
        success: function (res) {
            console.log("请求成功" + res.responseText);
        },
        error: function (res) {
            console.log("请求失败" + res)
        }
    })

// 用对象作为参数 ，就不用顾虑参数的顺序问题。
// 参数想怎么传就怎么传。
```

以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~

