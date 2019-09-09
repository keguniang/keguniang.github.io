---
layout:     post                    # 使用的布局（不需要改）
title:      【ajax】readyState=4并且status=200时，还进error方法               # 标题 
subtitle:   Hello JS, Hello Ajax #副标题
date:       2019-09-09              # 时间
author:     keguniang                      # 作者
header-img: img/home-bg-o.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - JS
    - Ajax
---
# 【ajax】readyState=4并且status=200时，还进error方法

```js
$.ajax({
            type: "GET",//提交方式
            url : 'http://10.12.137.105:8080/feeims2019090401/tbimmodelAction.do?model&id=e1efb77fc57211e995c7107b4428025c',
            dataType: 'json',
            success: function(res){
                console.log(res);
                $('h3').text(res);
            },
            error: function (XMLHttpResponse, textStatus, errorThrown) {
                console.log("1 异步调用返回失败,XMLHttpResponse.readyState:"+XMLHttpResponse.readyState);
                console.log("2 异步调用返回失败,XMLHttpResponse.status:"+XMLHttpResponse.status);
                console.log("3 异步调用返回失败,textStatus:"+textStatus);
                console.log("4 异步调用返回失败,errorThrown:"+errorThrown);
            }
        });
```

最终控制台打出来的错误是parsererror,那就是json解析出错了。

#### 1.第一个参数XMLHttpRequest.readyState:状态码

	* 0  （未初始化） 还没有调用send()方法
	* 1   （载入）已调用send()方法，正在发送请求
	* 2   （载入完成）send()方法执行完成，已经接收到全部响应内容
	* 3  （交互）正在解析响应内容
	* 4  （完成）响应内容解析完成，可以在客户端调用了

#### 2.XMLHttpRequest.status:状态码

	* 0XX: 状态未初始化
	* 1XX : 信息提示
	* 2XX: 成功
	* 3XX：重定向
	* 4XX： 客户端错误
	* 5XX：服务端错误

#### 3. 第二个参数textStatus 字符串类型的返回状态

“timeout”（超时）, “error”（错误）, “abort”(中止), “parsererror”（解析错误），还有可能返回空值。

#### 4.第三个参数errorThrown返回的错误信息

表示服务器抛出返回的错误信息，如果产生的是HTTP错误，那么返回的信息就是HTTP状态码对应的错误信息，比如404的Not Found,500错误的Internal Server Error。





==总结：==

​	 一般readyState为4，且status为200，说明请求成功了，但是却进入失败的方法，可能有三点的原因：

	1. `dataType`与api  返回的结果不匹配
 	2. 返回的 `json`格式不规范
 	3. api  有权限限定，要在`header`中设置 `cookie`

==解决办法：==

​	1.可以先将`dataType`设置为`text`，可以进入success方法查看返回结果

​	2.在`header`中设置 `cookie`暂未解决。

#### 参考链接：

[ajax请求返回状态为200但还是进入error事件](https://blog.csdn.net/mafan121/article/details/50832873)

[[前台]---ajax返回200成功,却进入error函数的解决方法](https://blog.csdn.net/java_zhangshuai/article/details/80274510)

[【ajax】readyState=4并且status=200时，还进error方法](https://blog.csdn.net/u013412066/article/details/50624966)

