## 一、双向绑定 最简单的使用
```
<!DOCTYPE html>
<html ng-app>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>JS Bin</title>
  <script src="http://cdn.tig.qq.com/js/lib/angular-1.3.15.min.js"></script>
</head>
<body>
    <p>jsbin对应地址：<a href="http://jsbin.com/yavicuyife/edit?html,js,output">http://jsbin.com/yavicuyife/edit?html,js,output</a></p>
<input type="text" ng-model="msg">
  {{msg}}
</body>
</html>
```

## 二、MVC 模式

M

V

- 数据模型变化驱动视图视图
- 声明式 UI 结构

不要去操作 Dom 元素

C

- 作用域 $scope
- 作用域层级与继承
- 作用域与事件系统



## 三、模块与依赖注入

- 为什么要使用模块？
- 我们需要使用其他的协作模块怎么办？

```
var myMod = angular.module('myMod', []);
```
如果不使用模块，那只能使用全局的控制器方式。污染全局变量，难以管理、维护、组织、复用代码。
模块化可以解决这些问题，它带来了命名空间和组织代码，复用代码等。

依赖注入解决模块之间的协作问题。

### 模块上注册定义服务

- 值 ```myMod.value(...)```
- 服务
  - ```myMod.service(...)```
  - ```myMod.factory(...)```
  - ```myMod.provider(...)```
  - ```myMode.constant(...)```
  
### 模块的生命周期
- 配置阶段：收集对象创建的解决方案，并进行配置。 ```myMod.config(...)```
- 运行阶段：执行所有初始化后的逻辑。```myMod.run(...)```

不同的阶段与不同的注册方法。

- 配置阶段可以注入：常量值、Provider
- 运行阶段可以注入：常量值、变量值、Service、Factory

### 模块依赖

服务的跨模块可见性

兄弟模块之间可以相互使用服务，也可以覆盖兄弟的服务。


## 指令