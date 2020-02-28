---
layout:     post                    # 使用的布局（不需要改）
title:   JS中的forEach,for in,for of和for的遍历优缺点及区别
date:       2019-10-30              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-ioses.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    -js
---
# JS中的forEach,for in,for of和for的遍历优缺点及区别

## 1. forEach

三个参数：

1. value
2. index
3. arr

**缺点：**

不能使用 break ，continue语句跳出循环，或者使用 return 语句返回外层函数。空数组不会执行回调函数。

**优点：**

遍历的时候更加简洁，效率和  for  循环是相同的，但是不关心下标的问题，减少了出错的效率。

**不可遍历对象，只能遍历数组**

## 2. for in

**优点：**

* **既能遍历数组，也能遍历对象，但大都用于遍历对象，因为遍历数组返回的是索引**
* 不仅遍历自身的属性，还包括原型上的属性和方法

**缺点：**

* 遍历顺序有肯能不是按照是实际数组的顺序

## 3. for of

**优点：**

* 不仅可以遍历数组，还可以遍历类数组（set,map,string）

**缺点：**

* 不能遍历对象。

* 只是遍历数组内的元素，不包括原型上的属性和方法
