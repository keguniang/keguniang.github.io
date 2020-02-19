---
layout:     post                    # 使用的布局（不需要改）
title:      whistle实现代理转发               # 标题 
subtitle:   Hello JS, Hello whistle #副标题
date:       2019-09-12              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-hacker.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - whistle
    - js
---
# display:inline-block元素之间空隙的产生原因和解决办法

### 1. 产生现象

在CSS布局中，如果我们想要将一些元素在同一行显示，其中的一种方法就是把要同行显示的元素设置display属性为inline-block。但是你会发现这些同行显示的inline-block元素之间经常会出现一定的空隙，这就是**“换行符/空格间隙问题”**。

### 2. 产生原因

**标签与标签之间使用了空格或换行符。因为空白字符也是字符，也会引用CSS样式。**

<img src='https://img-blog.csdn.net/20180520204150440?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xpemhlbmd4dg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70'>

### 3. 解决办法

#### 3.1 去除元素之间的换行

```html
<!-- 将所有子元素写在同一行 -->
<ul>
    <li>111</li><li>222</li><li>333</li><li>444</li><li>555</li>
</ul>
```

**缺点：**代码可读性变差

#### 3.2 为父元素中设置font-size: 0，在子元素上重置正确的font-size

```css
ul{
    list-style: none;
    font-size: 0;
}
li{
    display: inline-block;
    width: 100px;
    height: 100px;
    background: red;
    font-size: 16px;    
}
```

#### 3.3 为inline-block元素添加样式float:left

缺点：父元素高度塌陷

#### 3.4 设置子元素margin-left为负数（不推荐）

缺点：**元素之间间距的大小与上下文字体大小相关；并且同一大小的字体，元素之间的间距在不同浏览器下是不一样的**

#### 3.5 设置父元素，display:table和word-spacing（最优解）

```css
.parent{
  display: table;
  word-spacing:-1em; /*兼容其他浏览器，题主还未验证*/
}
```

参考链接：

https://blog.csdn.net/qq_32614411/article/details/82223624



