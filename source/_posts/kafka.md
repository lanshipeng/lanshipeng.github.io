---
title: kafka消息中间件
categories: 
- 消息队列
tags: 
comments: true
---


## cli of kafka 

<!-- more -->

- start zookeeper
```
    zookeeper-server-start /usr/local/etc/kafka/zookeeper.properties 
```
- start kafka
```
    kafka-server-start /usr/local/etc/kafka/server.properties 
```
- create topic
```
    cd /usr/local/Cellar/kafka/2.1.0
    ./bin/kafka-topics --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test
```
- show topics
```
    cd /usr/local/Cellar/kafka/2.1.0
    ./bin/kafka-topics --list --zookeeper localhost:2181
```
- delete topic
```
    cd /usr/local/Cellar/kafka/2.1.0
    ./bin/kafka-run-class kafka.admin.DeleteTopicCommand --topic test --zookeeper127.0.0.1:2181
```
- start producer
```
    cd /usr/local/Cellar/kafka/2.1.0
    bin/kafka-console-producer --broker-list localhost:9092 --topic test
```
- start consumer
```
    cd /usr/local/Cellar/kafka/2.1.0
    ./bin/kafka-console-consumer --bootstrap-server localhost:9092 --group cousunmer --topic test --from-beginning  
```        
- stop kafka
```
    cd /usr/local/Cellar/kafka/2.1.0 
     ./bin/kafka-server-stop
```

- 查看消费组列表
```
    ./bin/kafka-consumer-groups --bootstrap-server localhost:9092 --list
```  
- 查看某个消费组信息
```
    /bin/kafka-consumer-groups --bootstrap-server localhost:9092  --describe --group consumer03
```
- 查看topic列表
```
    ./bin/kafka-topics.sh --zookeeper localhost:2181 --list
```
- 查看指定topic信息
```
    ./bin/kafka-topics.sh --zookeeper localhost:2181 --describe --topic Hello-Kafka
```
- 修改topic分区数
```
    ./bin/kafka-topics.sh --zookeeper localhost：2181 --alter --topic Hello-Kafka --partitions 3
```
    
## why need kafka?
- 解藕消息的生产和消费
- 缓冲
- 并行

- 设计目标
    - 消息持久化：以时间复杂度为O(1)的方式提供消息持久化能力，即使对TB级以上的数据也能保证常数时间复杂度的访问性能。
    - 高吞吐：在廉价的商用机器上也能支持单机每秒10万条以上的吞吐量。
    - 分布式：支持消息分区以及分布式消费，并保证分区内的消息顺序。
    - 跨平台：支持不同语言平台的客户端。
    - 实时性：支持实时数据处理和离线数据处理。
    - 伸缩性：支持水平扩展（producer可以将数据发给多个broker上的多个partition,consumer也可以从多个broker上的不同partition读取数据）

## 使用场景1:

- 解藕 各个系统之间通过消息系统这个统一的接口交换数据，无须关心彼此的存在
- 冗余 消息系统具有持久化的能力，规避消息处理前丢失的风险
- 拓展 消息系统是统一的数据接口 各系统可独立扩展
- 异步通信 在不需要立即处理请求的情况下 可以将请求放入消息系统 合适的时候再处理
- 峰值处理能力 消息系统可以顶住峰值流量 业务系统可根据处理能力从消息系统获取并处理对应量的请求
- 可恢复性 系统中部分组件失效不影响整个系统 恢复后仍可从消息系统获取并处理数据

## 常用消息系统对比
- RabbitMQ erlang编写 支持多协议AMQP，XMPP,SMTP,STOMP,支持负载均衡 数据持久化
    支持peer-2-peer和发布/订阅模式
- Redis 轻量级。就入队操作而言 redis对于短消息小于10kb的性能比rabbitmq好
- ZeroMQ 轻量级 不需要单独消息服务器或者中间件，应用程序扮演该角色，peer-2-peer本质是一个库
- ActiveMQ JMS实现 peer-to-peer 支持持久化 XA分布式事务
- kafka高性能跨语言的分布式发布/订阅消息系统 数据持久化 全分布式 同时支持在线和离线处理
- MetaQ/RocketMQ 纯java实现 发布/订阅消息系统 支持本地事务和分布式事务

## zookeeper & kafka

kafka使用zookeeper来实现动态的集群扩展，不需要更改客户端（producer和consumer）的配置。broker会在zookeeper注册并保持相关的元数据（topic，partition信息等）更新。而客户端会在zookeeper上注册相关的watcher。一旦zookeeper发生变化，客户端能及时感知并作出相应调整。

说明：Producer端使用zookeeper用来"发现"broker列表,以及和Topic下每个partition的leader建立socket连接并发送消息。也就是说每个Topic的partition是由Lead角色的Broker端使用zookeeper来注册broker信息,以及监测partition leader存活性.Consumer端使用zookeeper用来注册consumer信息,其中包括consumer消费的partition列表等,同时也用来发现broker列表,并和partition leader建立socket连接,并获取消息.


## 消息传递pull & push
- Producer和consumer采用的是push-and-pull模式  
    Producer只管向broker push消息，consumer只管从broker pull消息，两者对消息的生产和消费是异步的   

- question: customer应该从brokes拉取消息还是brokers将消息推送到consumer，也就是pull还push?
    - push   
        - 优势：  
            延时低  
        - 劣势：  
            push模式很难适应消费速率不同的消费者，因为消息发送速率是由broker决定的。当broker推送速率远高于消费者消费速率，consumer可能崩溃。push模式的目标是尽可能以最快速度传递消息，但是这样很容易造成consumer来不及处理消息，典型的表现就是拒绝服务以及网络拥塞。
    - pull(kafka选择的模式)  
        - 优势:  
            而pull模式则可以根据consumer的消费能力以适当的速率消费消息。
        - 劣势:  
            如果处理不好，实时性不足(kafka使用long polling)
            如果broker没有可供消费的消息，将导致consumer不断在循环中轮询，直到新消息到t达。为了避免这点，Kafka有个参数可以让consumer阻塞知道新消息到达(也可以阻塞知道消息的数量达到某个特定的量这样就可以批量发送）  
  
## Semantic  

- topic & partition
    一个队列只有一种topic,一种topic的消息可以根据key值分散到多条队列中。

    topic在逻辑上可以被认为是一个在的queue，每条消费都必须指定它的topic，可以简单理解为必须指明把这条消息放进哪个queue里。为了使得Kafka的吞吐率可以水平扩展，物理上把topic分成一个或多个partition（为了加快消费速度），每个partition在物理上对应一个文件夹，该文件夹下存储这个partition的所有消息和索引文件

- broker
    消息中间件处理结点，一个Kafka节点就是一个broker，多个broker可以组成一个Kafka集群。

- offset
    每个partition都由一系列有序的、不可变的消息组成，这些消息被连续的追加到partition中。partition中的每个消息都有一个连续的序列号叫做offset,用于partition唯一标识一条消息

- kafka 支持多个consumer group 消费一条消息。但是如果一个consumer group 下有多个consumer，那么只能有一个consumer消费该条消息。这种机制可实现消息的单播和广播（可通过启动多个consumer验证）。

## publish & subscribe

- 生产者(producer)生产消息(数据流), 将消息发送到到kafka指定的主题队列(topic)中，也可以发送到topic中的指定分区(partition)中，消费者(consumer)从kafka的指定队列中获取消息，然后来处理消息

- 相同的消费者组中不能有比分区更多的消费者，否则多出的消费者一直处于空等待，不会收到消息


## delivery guarantee：

- At most once 消息可能会丢，但绝不会重复传输 
  读完消息先commit再处理，如果消息在commit后还没来得及处理就crash了，下次重新开始工作后无法读到刚刚提交但未处理的消息。
- At least one 消息绝不会丢，但可能会重复传输
  先处理完再commit,如果消息在处理完但在commit之前crash了，下次重新开始工作后还会处理未commit的消息
- Exactly once 每条消息肯定会被传输一次且仅传输一次，很多时候这是用户所想要的  

代码地址：https://github.com/lanshipeng/kafka-example
