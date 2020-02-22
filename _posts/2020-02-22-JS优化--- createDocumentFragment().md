---
layout:     post                    # 使用的布局（不需要改）
title:      JS优化--- createDocumentFragment()               # 标题 
date:       2020-02-22              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-hacker.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - js
---
# JS优化--- createDocumentFragment()

## 1. 常见的动态创建html节点的方法

| 方法                     | 说明                     |
| ------------------------ | ------------------------ |
| createComment(text)      | 创建带文本text的注释节点 |
| createDocumentFragment() | 创建文档碎片节点         |
| createElement(tagName)   | 创建标签                 |
| createTextNode(text)     | 创建文本节点             |

以上的操作，每次均会改变当前页面的呈现， **并重新刷新整个页面，从而耗费了大量的时间**

## 2.  createDocumentFragment()

**作用：**创建一个文档碎片，把所有的新节点都附加在上边，然后将文档碎片的内容一次性的添加到document中。

**注意点：**

1. `DocumentFragment`不属于DOM树。
2. **插入的不是 DocumentFragment 自身，而是它的所有子孙节点**

这使得 DocumentFragment 成了有用的占位符，**暂时存放那些一次插入文档的节点。**

## 3. 用法示例

使用appendChild逐个向DOM文档中添加1000个新节点：

```dart
 for (var i = 0; i < 1000; i++)
  {
    var el = document.createElement('p');
    el.innerHTML = i;
    document.body.appendChild(el); //直接用appendChild向文档中插入节点
  }
```

使用createDocumentFragment()一次性向DOM文档中添加1000个新节点：

```dart
var frag = document.createDocumentFragment();
for (var i = 0; i < 1000; i++)
{
  var el = document.createElement('p');
  el.innerHTML = i; 
  frag.appendChild(el); //首先将新节点先添加到DocumentFragment 节点
}
document.body.appendChild(frag);//然后用appendChild插入文档中
```

## 4. 总结

**我们可以理解为DocumentFragment （文档碎片节点）是一个插入结点时的过渡，我们把要插入的结点先放到这个文档碎片里面，然后再一次性插入文档中，这样就减少了页面渲染DOM元素的次数**。

经IE和FireFox下测试，在append1000个元素时，效率能提高10%-30%，FireFox下提升较为明显。
 不要小瞧这10%-30%，效率的提高是着眼于多个细节的，如果我们能在很多地方都能让程序运行速度提高10%-30%，那将是一个质的飞跃！

## 参考链接

https://www.jianshu.com/p/8ae83364c09c
