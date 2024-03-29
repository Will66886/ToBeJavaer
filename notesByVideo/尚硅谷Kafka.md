# 尚硅谷Kafka

## 一、Kafaka概述

### 1.1、定义

Kafka是一个分布式的基于发布/订阅模式的消息队列，主要应用于大数据实时处理领域

### 1.2消息队列

#### 1.2.1传统消息队列的应用场景

![image-20200622111532086](尚硅谷Kafka.assets/image-20200622111532086.png)

#### 1.2.2、使用消息队列的好处

1. 解耦

   允许你独立的扩展或修改两边的处理过程，只要确保他们遵守同样的接口约束

2. 可恢复性

   系统的一部分组件失效时，不会影响到整个系统。消息队列降低了进程间的耦合度，所以即使一个处理消息的进程挂掉，加入队列中的消息仍然可以在系统恢复后被处理。

3. 缓冲

   有助于控制和优化数据流经过系统的速度，解决生产消息和消费消息的处理速度不一致的情况。

4. 灵活性&峰值处理能力(削峰)

   在访问量剧增的情况下，应用仍然需要继续发挥作用，但是这样的突发流量并不常见。如果为以能处理这类峰值访问为标准来投入资源随时待命无疑是巨大的浪费。使用消息队列能够使关键组件顶住突发的访问压力，而不会因为突发的超负荷的请求而完全崩溃。

5. 异步通信

   很多时候，用户不想也不需要立即处理消息。消息队列提供了异步处理机制，允许用户把一个消息放入列，但并不立即处理它。想向队列中放入多少消息就放多少，然后在需要的时候再去处理它们。

#### 1.2.3、消息队列的两种模式

> 点对点模式(一对一，消费者主动拉取数据，消息收到后消息清除)

​		消息生产者生产消息到Queue中，然后消息消费者从Queue中取出并拉去消息。消息被消费后，Queue中不在存储，所以所以消费者不可能消费已经消费的消息，Queue支持多个消费者，但是对一个消息而言，只会有一个消费者可以消费，如果想要复用消息，需再建一个Queue传递消息

![image-20200622145139035](尚硅谷Kafka.assets/image-20200622145139035.png)

> 发布/订阅模式(一对多，消费者消费消息后不会清除消息)

​		消息生产者(发布)将消息发布到topic上，同时有多个消费者(订阅)消费该消息。和点对点不同，发布到topic的消息会被所有订阅者消费

​		发布/订阅模式还划分为由Topic主动推送消息和由消费者主动拉取消息

- 由Topic主动推送消息：如果消费者消费消息速度不同，则会导致资源浪费
- 由消费者主动拉取消息，消费者不停的询问Topic是否有消息，导致资源浪费

![image-20200622154808168](尚硅谷Kafka.assets/image-20200622154808168.png)

### 1.3、Kafka基础架构

![image-20200622164140892](尚硅谷Kafka.assets/image-20200622164140892.png)

1. Producer ：消息生产者，就是向kafka broker 发消息的客户端；
2. Consumer ：消息消费者，向kafka broker 取消息的客户端；
3. Consumer Group （CG）：消费者组，由多个consumer 组成。消费者组内每个消费者负责消费不同分区的数据，一个分区只能由一个组内消费者消费；消费者组之间互不影响。所有的消费者都属于某个消费者组，即消费者组是逻辑上的一个订阅者。
4. Broker ：一台kafka 服务器就是一个broker。一个集群由多个broker 组成。一个broker可以容纳多个topic。
5. Topic ：可以理解为一个队列，生产者和消费者面向的都是一个topic；
6. Partition：为了实现扩展性，一个非常大的topic 可以分布到多个broker（即服务器）上，一个topic 可以分为多个partition，每个partition 是一个有序的队列；
7. Replication：副本，为保证集群中的某个节点发生故障时，该节点上的partition 数据不丢失，且kafka 仍然能够继续工作，kafka 提供了副本机制，一个topic 的每个分区都有若干个副本，一个leader 和若干个follower。
8. Leader：每个分区多个副本的“主”，生产者发送数据的对象，以及消费者消费数据的对象都是leader。
9. Follower：每个分区多个副本中的“从”，实时从leader 中同步数据，保持和leader 数据的同步。leader 发生故障时，某个follower 会成为新的follower。