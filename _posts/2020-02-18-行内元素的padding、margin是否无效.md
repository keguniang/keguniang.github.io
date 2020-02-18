---
layout:     post                    # 使用的布局（不需要改）
title:      行内元素的padding、margin是否无效               # 标题 
date:       2020-02-18              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-universe.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - css
---
**行内元素是否具有盒子模型？**

  行内元素同样具有盒子模型。

**行内元素的padding、margin是否无效？**

* 行内元素的padding-top、padding-bottom、margin-top、margin-bottom属性设置是无效的
* 行内元素的padding-left、padding-right、margin-left、margin-right属性设置是有效的
* 行内元素的padding-top、padding-bottom从显示的效果上是增加的，但其实设置的是无效的。并不会对他周围的元素产生任何影响。

转自：https://blog.csdn.net/sleepwalker_1992/article/details/80659623
