---
layout:     post                    # 使用的布局（不需要改）
subtitle:   Hello World, Hello JS #副标题
date:       2019-09-05              # 时间
author:     keguniang                      # 作者
header-img: img/home-bg-art.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - JS
---
## position:sticky

举个例子：

==微博的导航栏！！==之前还在想这个效果是怎么实现的

### 1. 使用

```css
position:sticky;
position:-webkit-sticky;//safiri浏览器
```

### 2. 简介

中文意思是“粘性的”。表现也符合粘性的表现。基本上，该定位可以看成是position:relative和position:fixed的综合体-----

1. 当元素在屏幕内：表现为 relative

2. 当元素就要滚出显示器屏幕的时候，表现为 fixed

举个例子看一下:

<p><iframe width="414" height="480" src="https://www.zhangxinxu.com/study/201812/position-sticky.html" frameborder="0" style="max-width:100%;border:1px solid #ddd;"></iframe></p>
其中导航元素设置了如下CSS

```css
nav{
    position:sticky;
    position: -webkit-sticky;
    top:0;
}
```

于是，正如大家看到，随着页面的滚动，当导航距离top为0的时候，就黏在了上边缘，表现如同`position:fixed`

**使用场景：**

导航栏的跟随效果，例如微博导航栏。

### 3. 富有层次的滚动交互

￼滚动下方的滚动条，可以看到  **新闻标题依次推上去，网友评论也会在滚动在屏幕一般的时候从内容的背后钻出来**

<p><iframe width="414" height="480" src="https://www.zhangxinxu.com/study/201812/position-sticky-demo.php?iframe=true" frameborder="0" style="max-width:100%;border:1px solid #ddd;"></iframe></p>
### 4. 使用原理

1. 不能有任何祖先元素设置`overflow:hidden`，否则没有粘滞效果。
2. 当sticky 元素的父级元素已不再完整占据sticky 元素的固定区域时， sticky元素不再固定因此：
   1. 父级元素的height 必须超过 sticky元素的高度
   2. 同一个直接父容器中的sticky元素，如果定位置相等，则会重叠；如果属于不同直接父级元素，则会随着父元素不再完整占据sticky元素的固定区域以后，再由其他父级元素的sticky元素占据固定位置。

若每个新闻都在同一个直接父元素 section  中，则标题会出现重叠的效果

### 5. 关键点

1. 定位用的`bottom`，效果和`top`正好是对立的。设置`top`粘滞的元素随着往下滚动，是先滚动后固定；而设置`bottom`粘滞的元素则是先固定，后滚动；
2. `z-index:-1`让网友评论footer元素藏在了content的后面，于是才有了“犹抱琵琶半遮面”的效果。

### 参考链接

[杀了个回马枪，还是说说position:sticky吧](https://www.zhangxinxu.com/wordpress/2018/12/css-position-sticky/)
