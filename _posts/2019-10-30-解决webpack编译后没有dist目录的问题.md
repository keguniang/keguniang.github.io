---
layout:     post                    # 使用的布局（不需要改）
subtitle:   Hello Webpack #副标题
date:       2019-10-30              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-ioses.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Webpack
---
### 适用现象：
1. 把dist文件夹删掉之后又重新进行打包编译，但是dist目录没有重新生成。
2. 项目从一开始就没有生成 dist 目录。
### 解决办法：
1. 先运行 `webpack`命令。
2. 再运行`webpack-dev-server` 命令，即可。

为什么不会生成dist目录?
    运行`webpack`才会进行build，生成dist目录，如果内容更改，dist目录下的内容也会更改。但是`webpack-dev-server`则是从内存中读取，所以看不到dist目录，也看不到dist目录中文件的更改。
