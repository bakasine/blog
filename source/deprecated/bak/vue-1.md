---
title: Vue 笔记一
date: 2020-03-29 14:25:21
tags:
    - vue
categories: 
    - 前端
---

* [Vue环境搭建](#build)
* [Vue基本使用](#start)
* [Vue生命周期](#cycle)
* [Vue模板语法](#template)

# <h2 id="build">Vue环境搭建</h2>

1.直接使用script标签引入或者使用CDN

[Vue下载](https://cn.vuejs.org/v2/guide/installation.html#%E7%9B%B4%E6%8E%A5%E7%94%A8-lt-script-gt-%E5%BC%95%E5%85%A5)

``` javascript
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

# <h2 id="start">Vue基本使用</h2>

``` javascript
<div id="app">
    {{ msg }}
</div>

var vue = new Vue({
    el : "#app",
    data : {
        msg : "Hello World"
    }
}
```

# <h2 id="cycle">Vue生命周期</h2>

``` js
var vue = new Vue({
    el : "#app",
    data : {
        msg : "Hello World"
    },
    // 在实例还没有别完全创建出来之前执行
    beforeCreate() {}, 
    // 已经初始化好了Vue对象，一般可以在这里做初始数据的获取
    created() {},
    // 已经初始化完成但还没绑定到el上，一般可以在这里做初始数据的获取
    beforeMount() {},
    // 已经绑定到el上
    mounted() {},
    // data被修改时,页面还未重新渲染
    beforeUpdate() {},
    // data被修改后，页面也已经渲染完毕
    updated() {},
    // 销毁前执行（$destroy方法被调用的时候就会执行）,一般用于：清除计时器、清除非指令绑定的事件等
    beforeDestroy() {},
    // 销毁后执行（$destroy方法被调用后执行）,所有的事件监听器被移除，所有的子实例也都被销毁。
    destroyed() {}
});
```

# <h2 id="template">Vue模板语法</h2>

``` html
<!-- v-text 更新元素的 text 和 {{}} 效果一样 -->
<p v-text="msg" v-once></p>
<p>{{ msg }}</p>

<!-- v-html 更新元素的页面。用 v-text 插入 html 语句会被 vue 当成字符串处理。 -->
<div v-html="html"></div>

<!-- v-show，为 true 显示，false 不显示。 -->
<p v-show="show"></p>

<!-- v-if，和 show 相似，区别在于需要满足 if 的条件才会渲染，而 show 是一定会渲染只是控制是否显示。 -->
<div v-if=""></div>
<div v-else-if=""></div>
<div v-else></div>

<!-- v-for -->
<div v-for="item in items"></div>
<div v-for="(item, index) in items"></div>
<div v-for="(val, key) in object"></div>
<div v-for="(val, name, index) in object"></div>

<!-- v-on 绑定事件监听器，可缩写成 @ -->
<button v-on:click="do"></button>
<button @click='do'></button>

<!-- v-bind 绑定属性或者特征，可缩写成 : -->
<img v-bind:src="imageSrc">
<img :src="imageSrc">

<!-- v-model 表单输入绑定。message 需要在 data 里提前定义 -->
<input v-model="message">
<textarea v-model="message"></textarea>
<!-- 如果单个复选框要设置成bool值默认为字符串，多个复选框应该初始化为 [] 数组 -->
<input type="checkbox" v-model="message" id="checkbox">
<input type="radio" value="" v-model="message">
<select v-model="selected">
    <option>A</option>
    <option>B</option>
    <option>C</option>
</select>
```

