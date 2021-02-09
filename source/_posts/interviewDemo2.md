---
title: 面试遇题2
categories:
- 面试
tags:
- 面试题
---


#### Demo来源：
二轮面试 ，面试考题。

## 谈谈demo中的功能实现：
对于"模糊查询"与"排序"，可能你会想到，数据库（mysql）：
排序命令：select * from 表名 order by 字段名 asc/desc;
模糊查询：select * from 表名 where 字段名 like '%关键字%';
当然你还要会一门后端语言来连接数据库并且操作数据库：如PHP、Node

如果你在写微信小程序，你会发现云服务数据库中，文档根本没有“模糊查询”这个语句，你只能用正则匹配;

所以本文排序功能的实现方法：使用原生"冒泡排序"。（没有涉及到上面所提到知识方面）
此方法的好处：可以减少与服务器的对话，从而提高性能
如果考虑到没有接触过上面所提到的知识，推荐考虑此方法...

## 分享:
本文与考题大致相同 ，vue-cli工具工程化，为了页面美观，用vant（UI）搭建页面
## 本文涉及技术：
html、css、javascript、vue、vue-cli、vant
## 效果视屏链接：
https://v.youku.com/v_show/id_XNDYwMzIzMzM2OA==.html

### 接下来是源代码：
公共组件代码（components文件夹）:
```
<template>
  <div>
    <!-- 筛选、排序 -->
    <div class="position">
      <van-dropdown-menu>
        <van-dropdown-item v-model="value1" @change="onSwiIndustry" :options="option1" />
        <van-dropdown-item v-model="value2" @change="onSwiFlow" :options="option2" />
      </van-dropdown-menu>
    </div>

    <!-- 数据加载 -->
    <van-pull-refresh v-model="refreshing" @refresh="onRefresh">
      <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="onLoad">
        <ul style="margin-top:50px">
          <!-- 商品卡片列表 -->
          <li v-for="(item, index) in list" :key="index">
            <van-card
              :desc="item.explain + '(' + item.or + ')'"
              :title="item.title"
              :thumb="item.src"
            >
              <div slot="bottom">
                <div class="list_info_i">
                  <dl class="list_info_views">
                    <!-- 点击浏览次数增加 -->
                    <dt @click="item.preview++"></dt>
                    <dd>{{item.preview}}</dd>
                    <div class="clear"></div>
                  </dl>
                  <dl class="list_info_comment">
                    <!-- 点击评论次数增加 -->
                    <dt @click="item.comment++"></dt>
                    <dd>{{item.comment}}</dd>
                    <div class="clear"></div>
                  </dl>
                  <dl class="list_info_like">
                    <!-- 点击喜欢次数增加 -->
                    <dt @click="item.like++"></dt>
                    <dd>{{item.like}}</dd>
                    <div class="clear"></div>
                  </dl>
                  <div class="clear"></div>
                </div>
              </div>
            </van-card>
          </li>
        </ul>
      </van-list>
    </van-pull-refresh>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      value1: 0,
      value2: 0,
      option1: [
        { text: "全部行业", value: 0 },
        { text: "企业集团", value: 1 },
        { text: "门户电商", value: 2 },
        { text: "游戏网页", value: 3 },
        { text: "摄影后期", value: 4 },
        { text: "图形设计", value: 5 },
        { text: "图标Icon", value: 6 },
        { text: "其他", value: 7 }
      ],
      option2: [
        { text: "最新推荐", value: 0 },
        { text: "最多喜欢", value: 1 },
        { text: "最多浏览", value: 2 },
        { text: "最多评论", value: 3 }
      ],
      list_arr: [
        {
          id: 0,
          src: "./static/images/1421841277109.jpg",
          title: "UEMO用户图片排序问题",
          explain: "经验分享",
          or: "其他",
          preview: 6907,
          comment: 5,
          like: 8
        },
        {
          id: 1,
          src: "./static/images/1421838333851.jpg",
          title: "动画写实场景",
          explain: "木白",
          or: "其他",
          preview: 186,
          comment: 0,
          like: 1
        },
        {
          id: 2,
          src: "./static/images/1421805669559.jpg",
          title: "太白案例更新+ui页面",
          explain: "u21",
          or: "企业集团",
          preview: 423,
          comment: 0,
          like: 1
        },
        {
          id: 3,
          src: "./static/images/1421805477901.jpg",
          title: "白 .",
          explain: "郗鉴",
          or: "图标Icon",
          preview: 876,
          comment: 2,
          like: 10
        },
        {
          id: 4,
          src: "./static/images/1421763729558.jpg",
          title: "MEIOS(决赛补齐界面)",
          explain: "yu0910",
          or: "图标Icon",
          preview: 1228,
          comment: 1,
          like: 8
        },
        {
          id: 5,
          src: "./static/images/1421742102374.jpg",
          title: "5年前的游戏icon及地图绘制",
          explain: "又见章鱼哥",
          or: "图标Icon",
          preview: 1228,
          comment: 0,
          like: 1
        },
        {
          id: 6,
          src: "./static/images/142174136543.jpg",
          title: "<hello logo>特别篇——动物畅想曲",
          explain: "君小阳",
          or: "图形设计",
          preview: 1264,
          comment: 4,
          like: 3
        },
        {
          id: 7,
          src: "./static/images/1421734678611.jpg",
          title: "离职后做的一个外包",
          explain: "烽烟",
          or: "企业集团",
          preview: 1441,
          comment: 1,
          like: 7
        },
        {
          id: 8,
          src: "./static/images/1421726012956.jpg",
          title: "Bodhi",
          explain: "酷站推荐",
          or: "企业集团",
          preview: 1987,
          comment: 1,
          like: 3
        },
        {
          id: 9,
          src: "./static/images/1420788431778.jpg",
          title: "复古小清新rosevovo",
          explain: "妞范儿",
          or: "门户电商",
          preview: 3186,
          comment: 8,
          like: 38
        }
      ],
      list: [],
      loading: false,
      finished: false,
      refreshing: false,
      len: 0
    };
  },
  methods: {
    onLoad() {
      setTimeout(() => {
        // 拉下刷新（全部数据初始化）
        if (this.refreshing) {
          this.value1 = 0;
          this.value2 = 0;
          this.list = [];
          this.len = 0;
          this.refreshing = false;
        }
        // 一次加载5条数据
        for (let i = 0; i < 5; i++) {
          this.list.push(this.list_arr[i + this.len]);
        }
        this.loading = false;

        if (this.list.length >= 10) {
          this.finished = true;
        }
      }, 1000);
      this.len = this.list.length;
    },
    onRefresh() {
      // 清空列表数据
      this.finished = false;

      // 重新加载数据
      // 将 loading 设置为 true，表示处于加载状态
      this.loading = true;
      this.onLoad();
    },
    // 切换行业（筛选）
    onSwiIndustry(e) {
      if (e == 0) {
        this.list = this.list_arr;
      } else {
        this.list = this.list_arr.filter((value, index) => {
          if (value.or == this.option1[e].text) {
            return index;
          }
        });
      }
    },
    // 切换各种（留言、喜欢、浏览）排序
    onSwiFlow(e) {
      if (e == 1) {
        this.onSort("like", ">");
      } else if (e == 2) {
        this.onSort("preview", ">");
      } else if (e == 3) {
        this.onSort("comment", ">");
      }
    },
    // 排序封装 (str：升序请使用“<” , 降序请使用“>”)
    onSort(name, str) {
      if (str == ">") {
        for (let i = 0; i < this.list.length; i++) {
          for (let j = 0; j < this.list.length - i - 1; j++) {
            if (this.list[j + 1][name] > this.list[j][name]) {
              let tem = this.list[j];
              this.list[j] = this.list[j + 1];
              this.list[j + 1] = tem;
            }
          }
        }
      } else if (str == "<") {
        for (let i = 0; i < this.list.length; i++) {
          for (let j = 0; j < this.list.length - i - 1; j++) {
            if (this.list[j + 1][name] < this.list[j][name]) {
              let tem = this.list[j];
              this.list[j] = this.list[j + 1];
              this.list[j + 1] = tem;
            }
          }
        }
      }
    } 
  }
};
</script>

<!-- 局部样式 --> 
<style scoped>
.list_info_i dl {
  display: block;
  height: 20px;
  float: left;
  margin-right: 10px;
}
.list_info_i dt {
  display: block;
  width: 20px;
  height: 20px;
  float: left;
  margin-right: 2px;
}
.list_info_i .list_info_views dt {
  background-position: 0 -21px;
}
.list_info_i .list_info_comment dt {
  background-position: -25px -20px;
}
.list_info_i .list_info_like dt {
  background-position: -50px -21px;
}
.list_info_i dt {
  background: url("../../static/images/icons.png");
  background-size: 500px 500px;
}
.list_info_i dd {
  display: block;
  height: 20px;
  float: left;
  line-height: 20px;
  font-size: 10px;
  color: #a7a7a7;
  margin: 0;
}
.position {
  width: 100%;
  height: 50px;
  border-bottom: 3px solid #cccccc46;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 666;
}
</style>

<!-- 全局样式（框架组件） -->
<style>
.van-card__title {
  font-size: 14px;
}
</style>

```
入口代码（main.js）:
```
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'
import { DropdownMenu, DropdownItem , Button ,Card ,List ,PullRefresh,Cell} from 'vant'


Vue.use(DropdownMenu)
  .use(DropdownItem)
  .use(Button)
  .use(Card)
  .use(List)
  .use(PullRefresh)
  .use(Cell);

Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

```



注意：如果需要打包代码，执行build命令时，你会发现，css中的背景图片出不来，此时你需要修改配置文件：
找到build/util.js文件中ExtractTextPlugin的css路径，手动添加publicPath参数，进行路径更改

以上就是本文的全部内容 ， 本文仅供参考~

如有代码错误 或者 你也有自己更好的想法。可以在下方留言。我洗耳恭听，共同进步...

我会及时做出更正 , 谢谢~
