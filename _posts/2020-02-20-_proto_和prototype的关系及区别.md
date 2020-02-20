---
layout:     post                    # 使用的布局（不需要改）
title:      _proto_和prototype的关系及区别               # 标题 
date:       2019-11-18              # 时间
author:     keguniang                     # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - js
---
# _proto_和prototype的关系及区别

转自：https://www.jianshu.com/p/7d58f8f45557

原型是javascript面向对象编程中非常重要的概念，而且并不是那么容易懂。偶然看到一个题目：阐述proto和prototype的关系。看到这个问题的时候，我的脑海浮现出一些概念，但却说不出来。先来看一张图

<img src="https:////upload-images.jianshu.io/upload_images/6264932-2aab1c8c439923ec.png?imageMogr2/auto-orient/strip|imageView2/2/w/770/format/webp" alt="img" style="zoom:67%;" />
 如果能看懂图中的关系基本上就可以解释出`*proto*`和`prototype`的关系和区别了

所以接下来一一介绍图中的一些概念

### 构造函数

使用构造函数创建对象

![img](https:////upload-images.jianshu.io/upload_images/6264932-5c4b76c632eca7a7.png?imageMogr2/auto-orient/strip|imageView2/2/w/774/format/webp)

image.png

Person就是一个构造函数，通过new创建了person1对象实例

其实构造函数就和普通函数没有多大区别，首字母大写只是约定俗成，不大写照样可以。关键是调用它的方式——通过new，那么这里又会牵扯到另一个问题，使用new调用后会内部会执行哪些操作

### prototype

图中可以看到在Person构造函数下有一个prototype属性。

<img src="https:////upload-images.jianshu.io/upload_images/6264932-cbaf62ea04c5f9e9.png?imageMogr2/auto-orient/strip|imageView2/2/w/756/format/webp" alt="img" style="zoom:67%;" />

这个并不是构造函数专有，每个函数都会有一个prototype属性，这个属性是一个指针，指向一个对象，记住只有函数才有,并且通过bind()绑定的也没有。

<img src="https:////upload-images.jianshu.io/upload_images/6264932-41808f494ac092b6.png?imageMogr2/auto-orient/strip|imageView2/2/w/763/format/webp" alt="img" style="zoom:67%;" />
 前面所说prototype属性指向一个对象，那么这个对象是什么？

根据第一张图片可以清晰看到，prototype指向Person.prototype。没错Person.prototype就是原型对象，也就是实例person1和person2的原型。

原型对象的好处是可以让所有对象实例共享它所包含的属性和方法

所以构造函数和原型之间的关系为

<img src="https:////upload-images.jianshu.io/upload_images/6264932-138dd0a0af915bcf.png?imageMogr2/auto-orient/strip|imageView2/2/w/750/format/webp" alt="img" style="zoom:80%;" />
 第一张图中看到，在person1和person2实例对象下面有一个[[prototype]],其实没有标准的方式可以访问它，但是主流浏览器上在每个对象上(null除外)都支持一个属性,那就是proto，这个属性会指向该对象的原型

![img](https:////upload-images.jianshu.io/upload_images/6264932-b41826910941a88c.png?imageMogr2/auto-orient/strip|imageView2/2/w/782/format/webp)

<img src="https:////upload-images.jianshu.io/upload_images/6264932-b6b1dc81d2981684.png?imageMogr2/auto-orient/strip|imageView2/2/w/775/format/webp" alt="img" style="zoom:67%;" />
		 所以总结可得proto就是用来将对象与该对象的原型相连

在所有实现中都无法访问到[[prototype]],但是可以通过一些方法来确定对象之间时候存在这种关系

instanceof,这个操作符只能处理对象(person1)和函数(带.prototype引用的Person)之间的关系

```js
person1 instanceof Person // true

isPrototypeOf，如果[[prototype]]指向调用此方法的对象，那么这个方法就会返回true

Person.prototype.isPrototypeOf(person1) // true
 Person.prototype.isPrototypeOf(person2) // true

Object.getPrototypeOf这个方法返回[[Prototype]]的值,可以获取到一个对象的原型

Object.getPrototypeOf(person1) === Person.prototype // true
```

 我们来继续理解第一张图中的关系，现在理解原型对象(PErson.prototype)下constructor属性

这个属性其实就是将原型对象指向关联的构造函数

![img](https:////upload-images.jianshu.io/upload_images/6264932-821fea4ed7a93903.png?imageMogr2/auto-orient/strip|imageView2/2/w/768/format/webp)

再来看一些代码

![img](https:////upload-images.jianshu.io/upload_images/6264932-983111333ae95264.png?imageMogr2/auto-orient/strip|imageView2/2/w/772/format/webp)
 那岂不是实例person1也有.constructor属性，其实没有，通过原型链在原型Person.prtototype上面找到的

理顺了这层关系，第一张图就全部理解完了，是不是差不多就可以解释出proto和prototype的关系和区别呢

原型链
 我们既然探索完了他们的关系，那我们来继续探索一下原型和原型链的奥秘所在，其实原型链就是依托proto和prototype连接起来的

在探索之前我们先来理解一下实例属性和原型属性的关系，见代码

![img](https:////upload-images.jianshu.io/upload_images/6264932-44176ab5393faddf.png?imageMogr2/auto-orient/strip|imageView2/2/w/778/format/webp)

上面代码中在实例属性和原型属性都有一个名为name的属性，但是最后输出来的是实例属性上的值

当我们读取一个属性的时候，如果在实例属性上找到了，就读取它，不会管原型属性上是否还有相同的属性，这其实就是属性屏蔽。即当实例属性和原型属性拥有相同名字的时候，实例属性会屏蔽原型属性，记住只是屏蔽，不会修改，原型属性那个值还在

但是如果在实例属性上没有找到的话，就会在实例的原型上去找，如果原型上还没有，就继续到原型的原型上去找，直到尽头，这个尽头是啥？不急，等会说

![img](https:////upload-images.jianshu.io/upload_images/6264932-6166401dff98ed25.png?imageMogr2/auto-orient/strip|imageView2/2/w/774/format/webp)

上面代码中person1实例并没有name属性，但仍然可以输出值，就是在原型上找到的
 如何检测一个属性存在于实例中，还是原型中？
 使用方法hasOwnProperty,属性只有存在于实例中才会返回true

![img](https:////upload-images.jianshu.io/upload_images/6264932-811eae5c169d2dd9.png?imageMogr2/auto-orient/strip|imageView2/2/w/775/format/webp)

既然讲到了方法，就再补充一些方法

in操作符

前面提到hasOwnProperty方法可用于检测属性是否是实例属性，in则会遍历所有属性，不管是实例上的，还是原型上的

in操作符有两种使用方式，单独使用和在for-in循环中使用,先上基础代码

![img](https:////upload-images.jianshu.io/upload_images/6264932-5f70e6b586f738ef.png?imageMogr2/auto-orient/strip|imageView2/2/w/785/format/webp)

单独使用

![img](https:////upload-images.jianshu.io/upload_images/6264932-e901108cc2fd7c8b.png?imageMogr2/auto-orient/strip|imageView2/2/w/728/format/webp)


 Object.keys() 此方法可以获取对象的所有可枚举的属性的名字

 ```js
var keys = Object.keys(person1)

console.log(keys) // ["name"]

var keys = Object.keys(Person.prototype)
 console.log(keys) // ["age"]
 ```

好了方法讲完了，继续，假如在原型对象Person.prototype还是没有找到这个属性，会停止吗？前面已经说了，还会继续找，这并不是尽头。原型对象也是对象，所以它也有proto属性，连接它的原型，原型对象Person.prototype的原型就是Object.prototype这个大boss，所有原型对象都是Object构造函数生成的

正是因为所有的原型最终都会指向Object.prototype，所以对象的很多方法其实都是继承于此，比如toString()、valueOf()，前面用到的hasOwnProperty，甚至是.constructor、proto

Object.prototype有原型吗？

![img](https:////upload-images.jianshu.io/upload_images/6264932-913f8bdd4cfdf1db.png?imageMogr2/auto-orient/strip|imageView2/2/w/776/format/webp)

没有，为null，所以它就是前面所提到的尽头

总结成一张图

![img](https:////upload-images.jianshu.io/upload_images/6264932-e589472e4f3f6d32.png?imageMogr2/auto-orient/strip|imageView2/2/w/779/format/webp)

