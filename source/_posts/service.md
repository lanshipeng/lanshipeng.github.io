---
title: 服务发现对比
categories: 
- 组件
tags: 
- 服务发现
comments: true
---
### cap原理  

<!-- more -->

Consistency(一致性)： 数据一致更新，所有数据变动都是同步的
Availability(可用性)： 好的响应性能
Partition tolerance(分区耐受性)： 可靠性


分区耐受性  
保证数据可持久存储，在各种情况下都不会出现数据丢失的问题。为了实现数据的持久性，不但需要在写入的时候保证数据能够持久存储，还需要能够将数据备份一个或多个副本，存放在不同的物理设备上，防止某个存储设备发生故障时，数据不会丢失。

数据一致性  
在数据有多份副本的情况下，如果网络、服务器、软件出现了故障，会导致部分副本写入失败。这就造成了多个副本之间的数据不一致，数据内容冲突。

数据可用性  
多个副本分别存储于不同的物理设备的情况下，如果某个设备损坏，就需要从另一个数据存储设备上访问数据。如果这个过程不能很快完成，或者在完成的过程中需要停止终端用户访问数据，那么在切换存储设备的这段时间内，数据就是不可访问的。

### 常用服务发现特性对比  

|feature|consul|zookeeper|etcd|eureka|
|-------|------|---------|----|------|
|服务健康检查|服务状态、内存,硬盘等|(弱)长链接,keepalive|连接心跳|可配支持|
|多数据中心|支持|-|-|-|
|kv存储服务|支持|支持|支持|-|
|一致性|raft|paxos|raft|-|
|cap原理|CA|CP|CP|AP|
|使用接口|http和dns|客户端|http/grpc|http|
|watch支持|全量/支持long polling|支持|支持long polling|支持long polling/大部分增量|
|自身监控|metrics|-|metrics|metrics|
|安全|acl/https|acl|http支持(弱)|-|