---
title: http相关特性
categories: 
- 协议
tags: 
comments: true
---

### http通信的过程

<!-- more -->

(1) 建立tcp链接  
(2) web浏览器向web服务器发送请求命令  
(3) web浏览器发送请求头消息  
(4) web服务器应答  
(5) web服务器发送应答头消息  
(6) web服务器向浏览器发送数据  
(7) web服务器关闭tcp链接  

### HTTP2.0和HTTP1.X相比的新特性

1.新的二进制格式（Binary Format），HTTP1.x的解析是基于文本。基于文本协议的格式解析存在天然缺陷，文本的表现形式有多样性，要做到健壮性考虑的场景必然很多，二进制则不同，只认0和1的组合。基于这种考虑HTTP2.0的协议解析决定采用二进制格式，实现方便且健壮。

2.多路复用（MultiPlexing），即连接共享，即每一个request都是是用作连接共享机制的。一个request对应一个id，这样一个连接上可以有多个request，每个连接的request可以随机的混杂在一起，接收方可以根据request的 id将request再归属到各自不同的服务端请求里面。

3.header压缩，如上文中所言，对前面提到过HTTP1.x的header带有大量信息，而且每次都要重复发送，HTTP2.0使用encoder来减少需要传输的header大小，通讯双方各自cache一份header fields表，既避免了重复header的传输，又减小了需要传输的大小。

4.服务端推送（server push），同SPDY一样，HTTP2.0也具有server push功能。

### http url 字符含义

* URL特殊符号及对应的十六进制值编码：  

    (1)'+'     URL中+号表示空格                  %2B  

    (2)空格     URL中的空格可以用+号或者编码        %20  

    (3)/       分隔目录和子目录                   %2F  

    (4)?       分隔实际的 URL 和参数              %3F  

    (5)%       指定特殊字符                      %25  

    (6)'#'       表示书签                       %23  

    (7)&       URL中指定的参数间的分隔符           %26  

    (8)=       URL中指定参数的值                 %3D   

### http和https区别  
  
- HTTPS：是以安全为目标的HTTP通道，简单讲是HTTP的安全版，即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL  
- HTTP：是互联网上应用最为广泛的一种网络协议，是一个客户端和服务器端请求和应答的标准（TCP），用于从WWW服务器传输超文本到本地浏览器的传输协议，它可以使浏览器更加高效，使网络传输减少    
- http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议  
- http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443  
- http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全  

### 无连接、无状态  

- 无连接的含义是限制每次连接只处理一个请求。服务器处理完客户的请求，并收到客户的应答后，即断开连接。采用这种方式可以节省传输时间。  

- 无状态是指协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。即我们给服务器发送 HTTP 请求之后，服务器根据请求，会给我们发送数据过来，但是，发送完，不会记录任何信息。

### get 和post区别

- GET后退按钮/刷新无害，POST数据会被重新提交（浏览器应该告知用户数据会被重新提交）。  
- GET书签可收藏，POST为书签不可收藏。GET能被缓存，POST不能缓存 。  
- GET编码类型application/x-www-form-url，POST编码类型encodedapplication/x-www-form-urlencoded 或 multipart/form-data。为二进制数据使用多重编码。  
- GET历史参数保留在浏览器历史中。POST参数不会保存在浏览器历史中。    
- GET对数据长度有限制，当发送数据时，GET 方法向 URL 添加数据；URL 的长度是受限制的（URL 的最大长度是 2048 个字符）。POST无限制。    
- GET只允许 ASCII 字符。POST没有限制。也允许二进制数据。     
- 与 POST 相比，GET 的安全性较差，因为所发送的数据是 URL 的一部分。在发送密码或其他敏感信息时绝不要使用 GET ！POST 比 GET 更安全，因为参数不会被保存在浏览器历史或 web 服务器日志中。GET的数据在 URL 中对所有人都是可见的。POST的数据不会显示在 URL 中。