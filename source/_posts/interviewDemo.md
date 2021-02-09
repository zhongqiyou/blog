---
title: 面试遇题1
categories:
- 面试
tags:
- 面试题
---


#### Demo来源：
上一年的二轮面试 ，面试考题

## 分享:
本文与考题大致相同，为了页面的美观，页面采用boostrap（UI）搭建
## 本文涉及技术：
html、css、javascript、bootstrap4、vue、jquery
## 效果视屏链接：
https://v.youku.com/v_show/id_XNDYwMzI2Nzk3Ng==.html

### 接下来是源代码：
html布局
```
<body>
    <div id="app">

        <div class="container">
            <!-- 添加商品start (添加按钮)-->
            <div class="input-margin">
                <button type="button" class="btn btn-primary" data-toggle="modal" data-target="#exampleModal">
                    添加商品
                </button>
            </div>
            <!-- Modal -->
            <div class="modal fade" id="exampleModal" tabindex="-1" role="dialog" aria-labelledby="exampleModalLabel"
                aria-hidden="true">
                <div class="modal-dialog" role="document">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h5 class="modal-title" id="exampleModalLabel">请输入商品名称</h5>
                            <button type="button" class="close" data-dismiss="modal" aria-label="Close">
                                <span aria-hidden="true">&times;</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <div class="input-group mb-3">
                                <div class="input-group-prepend">
                                    <span class="input-group-text">商品名称：</span>
                                </div>
                                <input type="text" ref="control" class="form-control" placeholder="tradeName">
                            </div>
                            <p class="red" v-if="textSwi">商品名称不能为空 , 请填写</p>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-dismiss="modal">取消</button>
                            <button type="button" class="btn btn-primary" :data-dismiss="modal"
                                @click="onhidden">确定</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 不同商品状态查看start (单选按钮)-->
            <div class="custom-control custom-radio inline input-margin" v-for="(value , index) in choice" :key="index">
                <input type="radio" :id="'customRadio'+index" :value='value.choiceName' v-model="radioState"
                    class="custom-control-input">
                <label class="custom-control-label" :for="'customRadio'+index">{{value.title}}</label>
            </div>

            <!-- 不同商品数目展示start (不同复选框勾选数目)-->
            <div>
                <button type="button" class="btn btn-primary btn-margin">
                    全部商品 <span class="badge badge-light">{{commodity.length}}</span>
                </button>
                <button type="button" class="btn btn-success btn-margin">
                    已选商品 <span class="badge badge-light">{{yesArr.length}}</span>
                </button>
                <button type="button" class="btn btn-danger btn-margin">
                    未选商品 <span class="badge badge-light">{{noArr.length}}</span>
                </button>
            </div>

            <!-- 选择商品start (商品勾选)-->
            <ul class="list-group">
                <li class="list-group-item d-flex justify-content-between align-items-center"
                    v-for="(value, index) in allArrSwi" :key="index" @dblclick="getEdit(index)">
                    <div class="custom-control custom-checkbox">
                        <input type="checkbox" class="custom-control-input" :id="'customCheck1'+index"
                            v-model="value.checked">
                        <label class="custom-control-label" :for="'customCheck1'+index">
                            {{value.title}}
                            <div class="input-group mb-3" v-show="value.edit">
                                <input type="text" @keyup.enter="complete(index)" class="form-control" ref="title"
                                    :value="value.copy" placeholder="Username">
                            </div>
                        </label>
                    </div>
                    <span class="badge badge-primary badge-pill cursor" @click="onDel(value.title)">X</span>
                </li>
            </ul>
        </div>

    </div>

</body>
```
js脚本
```
<!-- bootstrap + vue -->
<script src="https://cdn.jsdelivr.net/npm/jquery@3.4.1/dist/jquery.slim.min.js"
    integrity="sha384-J6qa4849blE2+poT4WnyKhv5vZF5SrPo0iEjwBvKU7imGFAV0wwj1yYfoRSJoZ+n"
    crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/popper.js@1.16.0/dist/umd/popper.min.js"
    integrity="sha384-Q6E9RHvbIyZFJoft+2mJbHaEWldlvI9IOYy5n3zV9zzTtmI3UksdQRVvoxMfooAo"
    crossorigin="anonymous"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@4.4.1/dist/js/bootstrap.min.js"
    integrity="sha384-wfSDF2E50Y2D1uUdj0O3uMBJnjuUD4Ih7YwaYd1iqfktj0Uod8GCExl3Og8ifwB6"
    crossorigin="anonymous"></script>
<script src="https://cdn.staticfile.org/vue/2.4.2/vue.min.js"></script>
<script>

    var vue = new Vue({
        el: "#app",
        data() {
            return {
                choice: [
                    { title: "全部", choiceName: "all" },
                    { title: "已选", choiceName: "yes" },
                    { title: "未选", choiceName: "no" }
                ],
                commodity: [
                    // 定义copy为了编辑商品input中value展示(展示旧值) ，edit为了 显示/隐藏 编辑(input) ，checked为了更好捕捉复选框勾选状态
                    { title: "小米", copy: "小米", checked: false, edit: false },
                    { title: "aphone", copy: "aphone", checked: false, edit: false },
                    { title: "魅族", copy: "魅族", checked: false, edit: false },
                    { title: "三星", copy: "三星", checked: false, edit: false },
                    { title: "诺基亚", copy: "诺基亚", checked: false, edit: false }
                ],
                modal: "nomodal",//bootstrap中的模态框 显示/隐藏
                textSwi: false,//在模态框添加中 ， 如有输入错误，则显示
                radioState: "all",//单选状态
            }
        },
        methods: {
            //点击事件：添加商品判断 ， 如输入错误，则文字警告，否则有效添加 
            onhidden() {
                if (this.$refs.control.value == "") {
                    this.textSwi = true;
                    this.modal = "nomodal";
                } else {
                    this.commodity.push({ title: this.$refs.control.value, copy: this.$refs.control.value, checked: false, edit: false })
                    this.modal = "modal";
                    this.textSwi = false;
                    this.$refs.control.value = "";
                }
            },
            //双击事件： 编辑商品 ，展示输入框（input）
            getEdit(index) {
                this.allArrSwi[index].edit = true;
                this.allArrSwi[index].title = "";
            },
            // 回车事件：修改商品名称
            complete(index) {
                this.allArrSwi[index].edit = false;
                this.allArrSwi[index].copy = this.$refs.title[index].value;
                this.allArrSwi[index].title = this.$refs.title[index].value;
            },
            // 点击事件：删除指定商品
            onDel(title) {
                let i = -1;
                this.commodity.forEach((value, index) => {
                    if (value.title == title) {
                        i = index;
                    }
                });
                this.commodity.splice(i, 1);
            }
        },
        computed: {
            // 勾选单选不同状态 ，展示商品
            allArrSwi() {
                if (this.radioState == "all") {
                    return this.commodity;
                } else if (this.radioState == "yes") {
                    return this.yesArr;
                } else if (this.radioState == "no") {
                    return this.noArr;
                }
            },
            // 已选商品
            yesArr() {
                return this.commodity.filter((value, index) => {
                    return value.checked == true;
                });
            },
            // 未选商品
            noArr() {
                return this.commodity.filter((value, index) => {
                    return value.checked == false;
                });
            }
        }
    });

</script>

```

css样式
```
 <!-- bootstrap样式 -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.4.1/dist/css/bootstrap.min.css"
        integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
  .inline {
        display: inline-block;
    }

    .input-margin {
        margin: 10px;
    }

    .btn-margin {
        margin: 0px 10px 10px 5px;
    }

    .red {
        color: red;
    } 
    .cursor{
      cursor: pointer;
    }
```


以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~
