---
title: http相关特性
categories: 
- 协议
tags: 
comments: true
---

### http通信的过程

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


