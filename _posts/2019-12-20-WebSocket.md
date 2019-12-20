---
layout:     post                    # 使用的布局（不需要改）
subtitle:   Hello JSd, Hello WebSocket #副标题
date:       2019-12-20              # 时间
author:     keguniang                      # 作者
header-img: img/post-bg-e2e-ux.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - WebSocket
---
## 1. 什么是websocket?

> 是H5新增的，提供的一种在单个TCP连接上进行全双工通讯的应用层协议。

【全双工】：一边发送请求，一边接收请求，二者可以同步进行。

> 像平时打电话一样，说话的同时也能够听到别人的话。
>
> 【半双工】：一个时间段内只有一个动作发生。举个例子：一条很窄的马路，同时只能有一辆车通过。所以当有两辆车对开，只能先等一辆车过去之后另一辆车再过。

## 2. 作用

使得客户端和服务端之间的数据交换变得更加简单，<font style="color:red">允许服务端主动向客户端推送数据。</font> 浏览器和服务器只需要完成一次握手，两者之间就可以创建持久性的连接，并进行双向数据传输。

## 3. 应用场景

需要实时处理信息，如聊天室。

关于聊天室的一个第三方库：[Ueditor](http://fex.baidu.com/ueditor/) （富文本编辑器）

### 4. 为什么HTTP协议不能做到全双工

实际上HTTP协议是建立在TCP协议上的，TCP协议本身就实现了全双工通信，但是HTTP协议是一个  **请求-响应** 协议，请求必须先由浏览器发给服务器，服务器才能响应这个请求，再把数据响应给服务器，这种机制限制了全双工通信。换句话说，<font style="color:red">浏览器不主动发送请求，服务器是没法主动给浏览器发送数据的。</font>

## 5. 之前实现实时推送的方法

### 5.1 ajax轮询

1. 短轮询：浏览器通过JS启动一个定时器，定时向服务器发送请求询问服务器有没有新数据。

**缺点：**

1. 实时性不够
2. 频繁的请求会给服务器带来极大的压力

### 5.2 Comet

本质上也是轮询

1. 长轮询：浏览器向服务器发送一个请求，然后服务器一直保持连接打开，直到有数据发送。发送完数据后，浏览器关闭连接，随即又发起一个到服务器的新请求。（是短轮询的改进）
2. 二者区别：短轮询是无论数据是否有效立即发送响应。而长轮询是等待服务器发送响应。

**优点：**暂时的解决了实时性的问题。

**缺点：** 

1. 以多线程模式运行的服务器会让大部分线程大部分时间都处于挂起状态，极大地浪费服务器资源。
2. 链路上的任何一个网关都可能关闭这个连接，而网管我们是不可控的，这就要求Comet  连接必须定期发一些ping数据表示连接“正常工作”。

**以上两种机制都治标不治本。所以，H5推出了WebSocket标准，让浏览器和服务器之间可以建立无限制的全双工通信，任何一方都可以主动发消息给对方。**

## 6. 建立 websocket连接的过程

**websocket 并不是全新的协议，而是利用了HTTP协议来建立连接。**

为了建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP 请求【一开始的握手需要借助HTTP请求完成】，这个请求和通常的 HTTP 请求不同，包含了一些附加头信息，其中附加头信息**"Upgrade: WebSocket"表明这是一个申请协议升级的 HTTP 请求**，服务器端解析这些附加的头信息然后产生应答信息返回给客户端，客户端和服务器端的 WebSocket 连接就建立起来了【从HTTP协议升级为websocket协议】，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接。

websocket  使用了自定义的协议，所以URL模式也有所不同。未加密的连接不再是`http://`，而是`ws://`。加密的连接不再是`https://`，而是`wss://`。

## 7. 请求头与响应头

### 7.1 请求头

```powershell
GET ws://localhost:3000/ws/chat HTTP/1.1
Host: localhost
Upgrade: websocket
Connection: Upgrade
Origin: http://localhost:3000
Sec-WebSocket-Key: client-random-string
Sec-WebSocket-Version: 13
```

该请求和普通的HTTP请求有几点不同：

1. GET请求的地址不是类似`/path/`，而是以`ws://`开头的地址；
2. 请求头`Upgrade: websocket`和`Connection: Upgrade`表示这个连接将要被转换为WebSocket连接；
3. `Sec-WebSocket-Key`是用于标识这个连接，并非用于加密数据；
4. `Sec-WebSocket-Version`指定了WebSocket的协议版本。

### 7.2 响应头

随后服务器如果接受该请求，就会返回如下响应：

```powershell
HTTP/1.1 101 Switching Protocols
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Accept: server-random-string
```

该响应代码`101`表示本次连接的HTTP协议即将被更改，更改后的协议就是`Upgrade: websocket`指定的WebSocket协议。

现在，一个WebSocket连接就建立成功，浏览器和服务器就可以随时主动发送消息给对方。消息有两种，一种是文本，一种是二进制数据。通常，我们可以发送JSON格式的文本，这样，在浏览器处理起来就十分容易。

## 8 WebSocket属性

| 属性                  | 描述                                                         |
| :-------------------- | :----------------------------------------------------------- |
| Socket.readyState     | 只读属性 **readyState** 表示连接状态，可以是以下值：0 - 表示连接尚未建立。1 - 表示连接已建立，可以进行通信。2 - 表示连接正在进行关闭。3 - 表示连接已经关闭或者连接不能打开。 |
| Socket.bufferedAmount | 只读属性 **bufferedAmount** 已被 send() 放入正在队列中等待传输，但是还没有发出的 UTF-8 文本字节数。 |

```js
if (ws.bufferedAmount === 0) {
    console.log("发送完毕");
} else {
    console.log("还有", ws.bufferedAmount, "数据没有发送");
}
```



## WebSocket 事件

以下是 WebSocket 对象的相关事件。假定我们使用了以上代码创建了 Socket 对象：

| 事件    | 事件处理程序     | 描述                       |
| :------ | :--------------- | :------------------------- |
| open    | Socket.onopen    | 连接建立时触发             |
| message | Socket.onmessage | 客户端接收服务端数据时触发 |
| error   | Socket.onerror   | 通信发生错误时触发         |
| close   | Socket.onclose   | 连接关闭时触发             |

------

## WebSocket 方法

以下是 WebSocket 对象的相关方法。假定我们使用了以上代码创建了 Socket 对象：

| 方法           | 描述             |
| :------------- | :--------------- |
| Socket.send()  | 使用连接发送数据 |
| Socket.close() | 关闭连接         |

## 9 简单实例

利用`ws`库实现了一个简单的前后端打招呼的功能。

**服务端代码**

```js
var webSocket = require('ws'); //导入WebSocket模块

var wss = new webSocket.Server({ port: 8181 }); //引用server类

// on事件是在连接建立时触发
wss.on('connection', function(ws) {
    console.log('server收到连接');
    // 服务端接收数据时触发
    ws.on('message', function(message) {
        console.log('server收到消息', message);
    });
    ws.send('server:hi 客户端');
})
```

**客户端代码**

```js
<script>
    // 判断当前浏览器是否支持websocket
    if ("WebSocket" in window) {
        console.log("您的浏览器支持 WebSocket!");
        let ws = new WebSocket("ws://localhost:8181");
        ws.onopen = function() {
            console.log('client:打开连接');
            ws.send('client:hello,服务端');
        };
        ws.onmessage = function(e) {
            //通过参数.data获取到返回的数据
            console.log("client：接收到服务端的消息 " + e.data);
            setTimeout(() => {
                ws.close();
            }, 5000);
        };
        ws.onclose = function(params) {
            console.log("client：关闭连接");
        }


    } else {
        console.log("您的浏览器不支持 WebSocket!");
    }
</script>
```

<img src="https://ae01.alicdn.com/kf/U8fd6ec870dd34b0a8352d7499d969a3aW.png">

### 同源策略

从上面的测试可以看出，WebSocket协议本身不要求同源策略（Same-origin Policy），也就是某个地址为`http://a.com`的网页可以通过WebSocket连接到`ws://b.com`。但是，浏览器会发送`Origin`的HTTP头给服务器，服务器可以根据`Origin`拒绝这个WebSocket请求。所以，是否要求同源要看服务器端如何检查。

### 路由

还需要注意到服务器在响应`connection`事件时并未检查请求的路径，因此，在客户端打开`ws://localhost:3000/any/path`可以写任意的路径。

实际应用中还需要根据不同的路径实现不同的功能。

## 参考链接：

[HTML5 WebSocket](https://www.runoob.com/html/html5-websocket.html)

[WebSocket](https://www.jianshu.com/p/3b5fbc1abc9d)

[廖雪峰WebSocket](https://www.liaoxuefeng.com/wiki/1022910821149312/1103303693824096)
