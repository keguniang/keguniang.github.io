---
layout:     post                    # 使用的布局（不需要改）
title:      cookie和session               # 标题 
subtitle:   Hello cookie, Hello session #副标题
date:       2017-02-06              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-hacker.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - cookie
    - session
---
# cookie和session

## 一、Cookie

### 1. cookie

​	饼干

​	其实是一份很小的文本文件，是纯文本，没有可执行代码，是<font color='red'>服务器给客户端</font>，并且<font color='red'>存储在客户端</font>上的一份键值对形式小数据。

==**原理：**==

​	客户端请求服务器时，如果服务器需要记录该用户状态，就使用response向客户端浏览器颁发一个Cookie。之后，客户端浏览器会把Cookie保存起来。当浏览器再次请求服务器时，浏览器就把请求的网址连同该Cookie一同交给服务器。服务器通过检查Cookie来获取用户状态。

### 2. 应用场景

​	自动登录(下次访问同一网站时，用户会发现不必输入用户名和密码就已经登录了)、浏览记录、用户访问次数、购物车。

### 3. 为什么有cookie

	> http 的请求是无状态的。客户端与服务器在通讯的时候，是无状态的，其实就是客户端在第二次来访的时候，服务器根本就不知道这个客户端以前有没有来访问过。（例如：今天，你跟你妈借了500块钱，第二天再打电话，你妈问：你是谁啊？）

        > 一旦客户端和服务器的数据和服务器的数据交换完毕，就会断开连接，再次请求，会重新连接，这说明服务器但从网络连接上是没有办法知道用户身份的。

<img src="//images2017.cnblogs.com/blog/1203274/201712/1203274-20171209110335937-33858862.png" alt="img" style="zoom:50%;" />

如图所示,用户首次访问服务器，服务器会返回一个独一无二的识别码；id=23451，这样服务器可以用这个码跟踪记录用户的信息，==（购物历史，地址信息等）==。

### 4.cookie的类型

按照过期时间分为两类：

	1. 会话cookie。是一种临时cookie，用户退出浏览器，会话cookie就被删除
 	2. 持久cookie。持久cookie会储存在硬盘里，保留时间长，关闭浏览器，重启电脑，它依然存在。

### 5. cookie的属性

 1. cookie的域

    产生Cookie的服务器可以向set-Cookie响应首部添加一个Domain属性来控制哪些站点可以看到那个cookie，例如下面：

    ```
    Set-Cookie: name="wang"; domain="m.zhuanzhuan.58.com"
    ```

    如果用户访问的是m.zhuanzhuan.58.com那就会发送cookie: name="wang", 如果用户访问www.aaa.com（非zhuanzhuan.58.com）就不会发送这个Cookie

 2. cookie的路径 path

    Path属性可以为服务器特定文档指定Cookie，这个属性设置的url且带有这个前缀的url路径都是有效的。

    例如：m.zhuanzhuan.58.com 和 m.zhaunzhuan.58.com/user/这两个url。 m.zhuanzhuan.58.com 设置cookie

    ```
    Set-cookie: id="123432";domain="m.zhuanzhuan.58.com";
    ```

    m.zhaunzhuan.58.com/user/ 设置cookie：

    ```
    Set-cookie：user="wang", domain="m.zhuanzhuan.58.com"; path=/user/
    ```

    但是访问其他路径m.zhuanzhuan.58.com/other/就会获得

    ```
    cookie: id="123432"
    ```

    如果访问m.zhuanzhuan.58.com/user/就会获得

    ```
      cookie: id="123432"
      cookie: user="wang"
    ```

3. secure

   设置了属性secure，cookie只有在https协议加密情况下才会发送给服务端。但是这并不是最安全的，由于其固有的不安全性，敏感信息也是不应该通过cookie传输的.

   ```
   Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure;
   ```

4. httponly

   表示此cookie必须用于http或https传输。这意味着，浏览器脚本，比如javascript中，是不允许访问操作此cookie的。

5. Expire time/Max-age

   表示了cookie的有效期。expire的值，是一个时间，过了这个时间，该cookie就失效了。或者是用max-age指定当前cookie是在多长时间之后而失效。如果服务器返回的一个cookie，没有指定其expire time，那么表明此cookie有效期只是当前的session，即是session cookie，当前session会话结束后，就过期了。对应的，当关闭（浏览器中）该页面的时候，此cookie就应该被浏览器所删除了。

### 6. cookie 满足同源策略

虽然网站images.google.com与网站www.google.com同属于Google，但是域名不一样，二者同样不能互相操作彼此的Cookie。

    问题来了 举个例子：
    
    访问玩zhidao.baidu.com 再访问wenku.baidu.com还需要重新登陆百度账号吗？
    
    解决办法: 设置document.domain = ‘baidu.com’;    

### 7. 服务器发送cookie给客户端

从服务器端，发送cookie给客户端，是对应的Set-Cookie。包括了对应的cookie的名称，值，以及各个属性。

```
Set-Cookie: lu=Rg3vHJZnehYLjVg7qi3bZjzg; Expires=Tue, 15 Jan 2013 21:47:38 GMT; Path=/; Domain=.169it.com; HttpOnly

Set-Cookie: made_write_conn=1295214458; Path=/; Domain=.169it.com

Set-Cookie: reg_fb_gate=deleted; Expires=Thu, 01 Jan 1970 00:00:01 GMT; Path=/; Domain=.169it.com; HttpOnly
```

### 8. 从客户端把cookie发送到服务器

从客户端发送cookie给服务器的时候，是不发送cookie的各个属性的，而只是发送对应的名称和值。形式是<font color='red'>键值对</font>的形式。

```
Cookie: name=value; name2=value2
```

### 9. 修改、设置cookie

 * 服务器发送给浏览器时，通过 `set-Cookie`，创建或更新对应的cookie;

 * 浏览器发送给服务器，通过浏览器内置的一些脚本，比如javascript，去设置对应的cookie，对应实现是操作js中的document.cookie。可以查看该链接：https://blog.csdn.net/qq_29132907/article/details/80390792

   ```js
   <script language=javascript> 
       //添加cookie
       function setCookie(name,value,time){ 
           var date= new Date(); 
           date.setDate(date.getDate()+time); 
           document.cookie = name+"="+value+";expires="+date; 
       } 
   
       //获得cookie
       function getCookie(name){ 
           var arr = document.cookie.split(";"); 
           for(var i=0; i<arr.length; i++){ 
           var arr2 = arr[i].split("="); 
               if(arr2[0] == name){ 
                   return arr2[1]; 
               } 
           } 
           return null; 
       } 
   
       //删除cookie
       function removeCookie(name){ 
           setCookie(name,"",0) 
       } 
   </script>
   ```

   `jquery.cookie.js`封装了这些方法。

   [jquery.cookie.js 下载和使用方法](https://blog.csdn.net/qq_29207823/article/details/81745757)

### 10. cookie的缺陷

	* cookie 会被附加在每个HTTP请求中，所以无形中增加了流量。
	* 由于在HTTP请求中的cookie是明文传递的，所以安全性成问题。（除非使用HTTPS）
	* cookie的大小限制在4KB左右。

### 参考链接：

[这一次带你彻底了解Cookie](https://www.cnblogs.com/zhuanzhuanfe/p/8010854.html)

[Http协议中Cookie详细介绍](https://www.cnblogs.com/bq-med/p/8603664.html)

[Cookie用法大全](https://blog.csdn.net/qq_29132907/article/details/80390792)

### 附加：

Q：为什么再次打开QQ后，发现密码位数不同？

A：如果将密码加密后存进cookie后，当我们下次访问登录页时，密码框里的密码将不是用户真正的密码。

详情：https://blog.csdn.net/qq_29132907/article/details/80390792

### Cookie小实例

https://www.runoob.com/try/try.php?filename=tryjs_cookie_username

## 二、Session

<font color='red'>Cookie **通过在客户端记录信息确定用户身份**，Session**通过在服务器端记录信息确定用户身份**</font>

### 1.Cookie 机制

​	在程序中，会话跟踪是很重要的事情。理论上，<font color='red'>**一个用户的所有请求操作都应该属于同一个会话**</font>，而另一个用户的所有请求操作则应该属于另一个会话，二者不能混淆。例如，用户A在超市购买的任何商品都应该放在A的购物车内，不论是用户A什么时间购买的，这都是属于同一个会话的，不能放入用户B或用户C的购物车内，这不属于同一个会话。

​	而Web应用程序是使用HTTP协议传输数据的。<font color='red' style="font-weight:bold">HTTP协议是无状态的协议。一旦数据交换完毕，客户端与服务器端的连接就会关闭，再次交换数据需要建立新的连接。这就意味着服务器无法从连接上跟踪会话。</font>即用户A购买了一件商品放入购物车内，当再次购买商品时服务器已经无法判断该购买行为是属于用户A的会话还是用户B的会话了。要跟踪该会话，必须引入一种机制。

​	Cookie就是这样的一种机制。它可以弥补HTTP协议无状态的不足。在Session出现之前，基本上所有的网站都采用Cookie来跟踪会话。

### 2.Session机制

<font color='red' style="font-weight:bold">	Session是另一种记录客户状态的机制，不同的是Cookie保存在客户端浏览器中，而Session保存在服务器上。客户端浏览器访问服务器的时候，服务器把客户端信息以某种形式记录在服务器上。这就是Session。</font>客户端浏览器再次访问时只需要从该Session中查找该客户的状态就可以了。

​	如果说Cookie机制是通过检查客户身上的<font color='red' >“通行证”</font>来确定客户身份的话，那么Session机制就是通过检查服务器上的<font color='red' >“客户明细表”</font>来确认客户身份。Session相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。

### 参考链接

[Cookie/Session机制详解](https://blog.csdn.net/fangaoxin/article/details/6952954)
