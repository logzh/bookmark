#Vue.js 学习

## 一、Vue实例

在实例化 Vue 时，需要传入一个选项对象，它可以包含数据、模板、挂载元素、方法、生命周期钩子、计算属性等选项。全部的选项可以在 API 文档中查看。

## 二、数据绑定语法

- 插值 
  - 文本 {{mag}}  {{*msg}}
  - html {{{html}}}
  - html 属性 id="item-{{id}}"
- 绑定表达式
  - js表达式
  - 过滤器
- 指令 v-if、v-show、v-else、v-for（遍历数组或对象） ...

## 三、指令

v-if、v-show、v-else、v-for 等

v-if、v-for 可用于 `<template>` 标签上，分别表示模版可见、遍历模版

有些指令可以带*参数*、*修饰符*：

参数使用`:`隔开；
修饰符使用`.`隔开
```
v-on:click //click 为参数
v-on:keyup.13 //keyup 为参数，13为修饰符
v-on:keyup.enter 
v-on:click.stop 
```

其中注意到两个指令
- v-bind 指令用于响应地更新 HTML 属性
- v-on 指令，它用于监听 DOM 事件

出现频率很高 因此有缩写形式

```
<div v-bind:class="[classA, isB ? classB : '']">
<div class="static" v-bind:class="{ 'class-a': isA, 'class-b': isB }"></div>

<!-- 完整语法 -->
<a v-bind:href="url"></a>

<!-- 缩写 -->
<a :href="url"></a>

<!-- 完整语法 -->
<button v-bind:disabled="someDynamicCondition">Button</button>

<!-- 缩写 -->
<button :disabled="someDynamicCondition">Button</button>
```

```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>

<!-- 缩写 -->
<a @click="doSomething"></a>
```

## 四、组件

编写可复用的组件
组件的 API 来自三部分：props、事件、slot

- props 允许外部环境传递数据给组件（其中数据也可以是 action）
- 事件 允许在组件内触发外部环境的 action
- slot 允许外部环境插入内容到组件的视图结构内 （包括指定插入内容在组件中的位置）

```
<my-component
  :foo="baz"
  :bar="qux"
  hh="来自字面量语法"
  @event-a="doThis"
  @event-b="doThat">
  
  <!-- content -->
  <img slot="icon" src="...">
  <p slot="main-text">Hello!</p>
  
</my-component>
```

使用：
```
<component-a :msg="msg" hh="来自字面量语法" :click-handle="clickHandle"></component-a>
```
组件定义：
*ComponentA.vue*
```
<style>
    .red {
        color: #f00;
    }
</style>

<template>
    <h1 class="red" @click="clickHandle(11)">{{msg}}</h1>
    <p>hh属性：{{hh}}</p>
    <ul>
        <li v-for="item in shiwan" track-by="$index">
            {{ item.name }}
        </li>
    </ul>
</template>

<script type="text/ecmascript-6">
    export default {
        props:['msg', 'hh', 'clickHandle'],
        data() {
            return {
                shiwan: []
            }
        },
        ready() {
            console.log('ready');
            var self = this;
            self.$http.get('/static/json/shiwan.json').then(function(response) {
                // get status
                response.status;
                // get all headers
                response.headers();
                // get 'expires' header
                response.headers('expires');
                // set data on vm
                self.$set('shiwan', response.data);
            }, function(response) {
                // handle error
            });
        },
        methods: {
            click11 (mm){
                console.log(this)
            },
            click() {
                var self = this;
                self.$http.get('/static/json/shiwan.json').then(function(response) {
                    // get status
                    response.status;
                    // get all headers
                    response.headers();
                    // get 'expires' header
                    response.headers('expires');
                    // set data on vm
                    self.$set('shiwan', response.data);
                }, function(response) {
                    // handle error
                });
            }
        }
    }
</script>
```
