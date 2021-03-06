# 第6章 理解对象

- 理解对象属性
- 创建对象
- 对象继承

## 6.1 理解对象

### 6.1.1 属性类型：两类属性可以同时存在

  - 数据属性： 修改默认特性需要使用ES5的 Object.defineProperty(obj, 'prop', {...}) 方法。IE8 中该方法有限制。
    - [[Configurable]]： 表示是否可以通过 delete 删除属性，能否修改属性的特性，或者把属性修改为访问器属性。默认为 true；
    - [[Enumerable]]： 表示是否可以通过 for-in 循环返回属性。默认为 true；
    - [[Writable]]： 表示能否修改属性的值。默认为 true。
    - [[Value]]： 包含这个属性的数据值。读取写入属性值，都是操作它。默认值为 undefined；
  - 访问器属性：包含一对 getter 和 setter 函数（都不是必须的）。读取属性值时调用 getter 函数，写入属性值时调用 setter 函数。常用来设置一个属性值导致其他属性值发生变化。
    - [[Configurable]]： 同上；
    - [[Enumerable]]： 同上；
    - [[Get]]： 读取属性时调用的函数。默认值为 undefined；
    - [[Set]]： 写入属性时调用的函数。默认值为 undefined；

### 6.1.2 定义多个属性

使用 Object.defineProperties() 方法。

```
Object.defineProperties(book, {_year:{ ... }, editon:{ ... }, year:{ ... }})
```
 
兼容性 IE9+，其他

### 6.1.3 读取属性的特性

使用方法 Object.getOwnPropertyDescriptor(obj, 'prop') 方法。返回一个对象。

## 6.2 创建对象

### 6.2.1 工厂模式

```
function createPerson(name, age, job) {
	var o = new Object();
	o.name = name;
	o.age = age;
	o.job = job;
	o.sayName = function(){
		console.log(this.name);
	};
	return o;
}

var p1 = createPerson('zhang', 41, 'Doctor');
var p2 = createPerson('jin', 42, 'Teacher');
```

工厂模式解决了创建多个相似对象问题。但没有解决对象的识别问题（即怎么知道一个对象的类型）。

### 6.2.2 构造函数模式

```
function Person(name, age, job) {
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function(){
		console.log(this.name);
	};
}

var p1 = new Person('zhang', 41, 'Doctor');
var p2 = new Person('jin', 42, 'Teacher');
```
与工厂模式的区别：
- 没有显示创建对象
- 属性和方法直接赋值给 this 对象
- 没有 return 语句
- 函数名第一个字母大写（只是惯例）

p1 和 p2 是两个对象。Person 函数为构造函数。p1 和 p2 都有一个 constructor 属性，即构造函数属性，该属性指向 Person。
```
console.log(p1.constructor === Person); // true
console.log(p1.constructor === Person); // true
```
> 这正是构造函数模式优于工厂模式的地方，构造函数模式将它的实例标记为了一种特定的类型。

构造函数与其他函数的区别在于**调用方式不同**。任何函数，只要通过 new 操作符来调用，那么它就可以作为构造函数。而任何函数，如果不通过 new 操作符来抵用，那么它和普通函数也没什么两类。

构造函数的问题：它的不同实例的同名函数时不等的。如果改变定义方式可以相等，但却破坏了自定义类型的封装性。

### 6.2.3 原型模式

每个函数都有一个 property 属性，即原型属性，这个属性是一个指针，指向一个对象，成为原型对象。
原型对象让所有的对象实例共享它包含的属性和方法。也就是不必再构造函数中定义对象实例的信息，而是将这些共享的信息直接添加到原型对象中。

```
function Person(){

}

Person.prototype.name = 'default name';
Person.prototype.age = 1;
Person.prototype.job = '';
Person.prototype.sayName = function(){
	console.log(this.name);
}

var p1 = new Person();
p1.sayName(); // default name

var p2 = new Person();
p2.sayName(); // default name

console.log(p1.sayName === p2.sayName); // true
```

1、 理解原型对象、构造函数、实例之间的关系

- 构造函数的属性 prototype 是个指针，指向原型对象；

- 实例有个属性 [[prototype]]，同样是个指针，且指向原型对象。但这个属性没有标准的访问方式。Chrome、Firefox、Safari在每个实例对象上有个属性`__proto__`；

- 原型对象有个 constructor 属性，称为构造函数属性，该属性是个指针，指向构造函数。

关系示意图可以参照书中图 6-1

2、属性相关知识

代码中读取对象的某个属性时，首先从对象实例开始搜索，如果对象实例上有该属性，则返回属性值，如果对象实例上没有该属性，则会在原型对象上搜索，如果有该属性，则返回属性值。

因此，原型对象的 constructor 属性是可以通过对象实例进行访问的。

hasOwnProperty() 方法可以判断属性值来自实例对象还是原型对象的。属性存在在实例对象上，hasOwnProperty() 方法返回 true。

for...in 操作变量对象属性时，返回的是所有可以通过对象访问的、可枚举的属性。不仅包括实例对象上的属性，也包括原型对象上的属性。

Object.keys() 方法接受一个对象作为参数，返回一个包含所有可枚举属性的字符串数组。
Object.getOwnProperty() 方法接受一个对象，返回所有实例属性，无论它是否可以枚举。比如 constructor 属性是不可枚举，但能通过该方法返回。
支持这两个方法的浏览器有 IE9+、Firefox4+，Safari5+、Chrome等。

