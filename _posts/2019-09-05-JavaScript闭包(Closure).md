---
layout:     post                    # 使用的布局（不需要改）
subtitle:   Hello World, Hello JS闭包 #副标题
date:       2019-09-05              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-swift.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - JS
---
## JavaScript闭包(Closure)

### 1. 什么是闭包

简单地说，<font color='red'>闭包就是指有权访问另一个函数作用域中的变量的函数</font>

由两部分组成：

	1. 函数
 	2. 创建该函数的环境

==闭包的关键在于：外部函数调用之后其变量对象本应该被销毁，但是闭包的存在使我们仍然可以访问外部函数的变量对象==

```js
//看一个闭包的例子
function outer() {
	var a = 1;//定义一个内部变量
    return function() {
        return a;//返回a变量值
    };
}
var b  = outer();
console.log(b());//打印1
```

### 2. 产生一个闭包

<font color='red'>创建闭包的最常见方式，就是在一个函数内部创建另一个函数。</font>

```
function func(){
	var a = 1,b = 2;
	return function closure(){//闭包
		return a + b;
	}
}
```

**闭包的作用域链包含着它自己的作用域，以及包含它的函数的作用域和全局作用域**

### 3. 闭包的注意事项

<font color='red'>通常，函数的作用域及其所有变量都会在函数执行结束后被销毁。但是在创建了闭包之后，这个函数的作用域就会一直保存到闭包不存在为止。</font>

```js
function add(x){
	return function(y){
		return  x + y;
	};
}
var add5 = add(5);
var add10 = add(10);

console.log(add5(2));//7
console.log(add10(2));//12

//释放对闭包的引用，变量被垃圾回收机制回收后，add函数的作用域也就不存在了
add5 = null;
add10 = null;
```

从上述代码可以看到add5 和 add10 都是闭包。它们共享相同的函数定义，但是保存了不同的环境。在 add5 的环境中，x 为 5。而在 add10 中，x 则为 10。最后通过 null 释放了 add5 和 add10 对闭包的引用。

**在javascript中，如果一个对象不再被引用，那么这个对象就会被垃圾回收机制回收**

### 4. 闭包只能取得包含函数中任何一个变量的最后一个值

```js
function  arrFunc() {
	var arr = [];
	for(var i = 0;i < 10;i++){
		arr[i] = function() {
			return i;
		};
	}
    return arr;
}
```

arr数组中包含了**10个匿名函数**，每个匿名函数都能访问外部函数的变量i。

当`arrFunc`执行完毕后，其作用域被销毁，但它的变量对象仍然保存在内存中，得以被匿名函数访问，这时i的值为10。

要想保存在循环过程中每一个i 的值，有两种==解决办法：==

	* 需要在匿名函数外部再套用一个立即执行匿名函数，在这个匿名函数中定义另一个变量并且立即执行来保存i 的值。
	* 把`var`换成`let`

### 5.闭包中的this对象

```js
var name = 'window';
var obj = {
	name: 'object',
	getName: function(){
		return function(){
			return this.name;
		}
	}
}
console.log(obj.getName()());//window
```

在上面这段代码中，<font color='red'>obj.getName()()实际上是在全局作用域中调用了匿名函数，this指向了window。</font>

这里要理解函数名与函数功能是分割开的，不要认为函数在哪里，其内部的this就指向哪里。

==window才是匿名函数功能执行的环境==

**如果想使this指向外部函数的执行环境，可以这样改写：**

```js
var name = 'window';
var obj = {
	name: 'object',
	getName: function(){
		var that = this;
		return function(){
			return that.name;
		}
	}
}
console.log(obj.getName()());//object
```

在闭包中，arguments与this也有相同的问题。下面的情况也要注意：

```js
var name = 'window';
var obj = {
	name: 'object',
	getName: function(){
		return this.name;
	}
}
console.log(obj.getName();//object
(obj.getName = obj.getName)();//window
```

* obj.getName();这时getName()是在对象obj的环境中执行的，所以this指向obj。

* (obj.getName = obj.getName)赋值语句返回的是等号右边的值，在全局作用域中返回，所以(obj.getName = obj.getName)();的this指向全局。**要把函数名和函数功能分割开来。****

### 6. 闭包的应用

 应用闭包的主要场合是：设计私有的方法和变量

### 7. 内存泄漏

闭包会引用包含函数的整个变量对象，如果闭包的作用域链中保存着一个HTML元素，那么就意味着该元素无法被销毁。所以我们有必要在对这个元素操作完之后主动销毁。

```js
function fun(){
	var element = $('#some');
	var id = element.id;
	element.onclick = function() {
		alert(id);
	}
	element = null;
}
```

### 8. 运用闭包的关键

* 闭包引用外部函数变量的值
* 在外部函数的外部调用闭包

### 9.闭包的缺陷

* 闭包的缺点就是常驻内存会增大内存使用量，并且使用不当很容易造成内存泄露。
* 如果不是因为某些特殊任务而需要闭包，在没有必要的情况下，在其它函数中创建函数是不明智的，因为闭包对脚本性能具有负面影响，包括处理速度和内存消耗。
