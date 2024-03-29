# 图解TCP|IP

## 第一章、网络基础知识

### 1.1、OSI参考模型

![preview](https://pic1.zhimg.com/v2-f0d1c53bce5f241223560b04c6077fc0_r.jpg)

1. 应用层

   主要用于打包发送消息，发送内容+收件人地址+发件人等信息

2. 表示层

   主要用于协商数据交换格式，将数据转换为网络通用的标准数据格式再进行传输，如：UTF-8、GBK、ISO-8859

3. 会话层

   采用某种方法传输数据

传输层以上并不具有实际传输数据的功能，真正负责在网络上传输具体数据的是会话层以下的这些服务层

4. 传输层

   确立连接与断开连接、重发，用于建立会话(三次握手)、保持会话(设定会话没有通信则自动断开时间)，断开连接(四次挥手)

5. 网络层

   从主机A到主机B的数据通信处理，路由选择，流量控制，IP地址，形成IP包

6. 数据链路层

   通信传输实际上是通过芜劣传输介质实现的，数据链路层的作用就是在这些通过传输介质互连的设备之间进行数据处理，成帧，保证帧的无误传输，MAC地址，形成EHTHERNET帧

7. 物理层

   物理接口规范，传输比特流

形象比喻

1. 应用层：A公司经理把他想要告诉B公司经理的事说出来
2. 表示层：秘书把A公司经理说的事情翻译成英文写到纸上
3. 会话层：行政职员把秘书写的这封信装到了信封封号，写上了信封的信息
4. 传输层：A邮局的职工把这封信取走
5. 网络层：A邮局分派的职工，把这封信分派到指定的送信区域
6. 数据链路层：A邮局的装箱职工，把送往同一区域的信封装到了木箱里
7. A邮局的物流职工把木箱运到了铁路

### 1.2、传输方式的分类

#### 1.2.1、面向有连接型与面向无连接型

- 面向有连接型：在发送数据之前，需要在收发主机之间连接一条通信线路
- 面向无连接型：面向无连接型不要求建立和断开连接。发送端可于任何时候自由发送数据，但接收端也永远不知道自己会在何时从哪里收到数据，因此，在面向无连接的情况下，接收端需要时长确认是否收到了数据

#### 1.2.2、电路交换与分组交换

- 电路交换：交换机主要负责数据的中转处理。计算机首先被连接到交换机上，而交换机与交换机之间则由众多通信线路在继续连接。因此计算机之间在发送数据时，需要通过交换机与目标主机建立通信线路再继续连接。建立好连接以后，用户就可以一直使用这条电路，直到该连接呗断开为止
- 分组交换：让连接到通信电路的计算机所要发送的数据分成多个数据包，按照一定的顺序排列之后分别发送。
  有了分组交换，数据被细分后，所有即使在分组的过程中，已经在每组的首部写入了发送端和接收端地址，所以即使同一条线路同时为多个用户提供服务，也可以明确区分每组数据发往的目的地，以及它是与哪台计算机进行的通信

![image-20210729181011340](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210729181011340.png)

电路交换与分组交换区别：

- 电路交换最多只能有两个用户同时通信，但是连接后可以随时通讯，并且连接稳定。
- 分组交换，通讯线路的速度可能会有所不同，根据网络拥堵的情况，数据达到目标地址的时间有长有短。另外，路由器的缓存饱和或溢出时，甚至可能会发生分组数据丢失、无法发送到对端的情况

#### 1.2.3、根据接收端数量分类

- 单播(Unicast)：一对一通信，最早的固定电话就是单播

- 广播(Broadcast)：将消息从一台主机发送给与之相连的所有其他主机，比如广播电台与收音机

  每个广播电台都有自己的频段，只有在相应频段的可接收范围内才能收到电视信号。与之类似的进行广播通信的计算机也有广播范围，这个范围叫做广播域

- 多播(Multicast)：多播与广播类似，不同之处在于多播要限定某一组主机作为接收端，比如视频会议

- 任播(Anycast)：特定的多台主机中选出一台作为接收端的通信方式，与多播有很多相似之处，但是传播行为上是不同的，任播通信从目标主机群中选择一台最符合网络条件的主机作为目标主机发送消息

### 1.3、地址

#### 1.3.1、地址的唯一性

如果想让地址在通信当中发挥作用，首先需要确定通信的主体。一个地址必须明确地表示一个主体对象。在同一个通信网络中不允许有两个相同地址的通信主体存在

单播很好理解，一个地址对应一个主机

多播就需要对应一组主机地址，举个例子，老师说：“一年一班的同学们起立”其中，“一年一班”实际上就是多播的目标地址，具有唯一性

再比如任播,老师说：“一年一班的哪一个同学来一下”，“一年一班任意一位同学”就成了此次目标地址

#### 1.3.2、地址的层次性

正如送信地址要分为国名-省名-市名和区名，网络地址也要有这些层次才能更快地定位某一个地址

MAC地址和IP地址在标识一个通信主体时虽然都具有唯一性，但是它们当中只有IP地址具有层次性。

MAC地址是由设备的制造厂商针对每块网卡进行分别指定的。

IP地址由网络号和主机号两部分组成。即使通信主体的IP地址不同，若主机号不同，网络号相同，说明他们处于同一网段。

网络传输中，每个节点会根据分组数据的地址信息，来判断该报文应该由哪个网卡发出去。为此，各个地址会参考一个发出接口列表。在这一点上MAC寻址与IP寻址是一样的。只不过MAC寻址中所参考的这张表叫做地址转发表，而IP寻址中所参考的叫做路由控制表。MAC地址转发表中所记录的是实际的MAC地址本身，而路由表中记录的IP地址则是集中了之后的网络号。

![image-20210730175839160](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210730175839160.png)

### 1.4、网络的构成要素

![image-20210731085956566](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210731085956566.png)

1. 网卡(NIC)：使计算机连网的设备(Network Interface)
2. 中继器(Repeater)：从物理层上延长网络的设备
3. 网桥(Bridge)/2层交换机：从数据链路层上延长网络的设备
4. 路由器(Router)/3层交换机：通过网络层转发分组数据的设备
5. 4-7层交换机：数据传输层以上各层网络传输的设备
6. 网关(Gateway)：转换协议的设备

#### 1.4.1、通信媒介与数据链路

![image-20210731094239719](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210731094239719.png)

传输速率与吞吐量

在数据传输的过程中，两个设备之间数据流动的物理速度称为传输速率。单位为bps(Bits Per Second，每秒比特数)，传输速率又称作带宽(Bandwidth)

主机之间实际的传输速率被称作吞吐量，单位bps。吞吐量不仅衡量带宽，同时也衡量主机的CPU处理能力、网络的拥堵程度、报文中数据字段的占有份额(不含报文首部，只计算数据字段本身)等信息。

#### 1.4.2、网卡

任何一台计算机连接网络时，必须要使用网卡(网络接口卡)，也称网络适配器、网卡、LAN卡。

#### 1.4.3、中继器

中继器(Repeater)是在OSI模型的第一层 -- 物理层面上延长网络的设备。由电缆传过来的电信号或光信号经由中继器的波形调整和放大再传给另一个电缆。

![image-20210802092652427](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802092652427.png)

中继器也可以完成不同媒介之间的转换工作。

通过中继器而进行的网络延长，其距离并非可以无限扩大。如：10Mps的以太网最多可以用4个中继器分段连接，而一个100Mps的以太网则最多只能连两个中继器。

有些中继器也可以提供多个端口服务。这种中继器被称为中继集线器或集线器

![image-20210802093901696](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802093901696.png)

#### 1.4.4、网桥/2层交换机

![image-20210802094147120](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802094147120.png)

网桥是在OSI模型的第二层 -- 数据链路层面上连接两个网络的设备。它能够识别数据链路层中的数据帧，并将这些数据帧临时存储于内存，再重新生成信号作为一个全新的帧转发给相连的另一个网段。由于能够存储这些数据帧，网桥能够连接10BASE-T与100BASE-TX等传输速率完全不同的数据链路，并且不限制连续网段的个数。

数据链路的数据帧中有一个数据位叫做FCS，用以校验数据是否正确送达目的地。网桥通过检查这个城中的值，将那些损坏的数据丢弃，从而避免发送给其他的网段，此外，网桥还能通过地址自学机制和过滤功能控制网络流量。

![image-20210802134834269](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802134834269.png)

以太网等网络中经常使用的交换集线器(Hub)，现在基本也属于网桥的一种

#### 1.4.5、路由器/3层交换机

![image-20210802140017875](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802140017875.png)

路由器是在OSI模型的第3层 -- 网络层面上链接两个网络、并对分组报文进行转发的设备。网桥是根据物理地址进行处理，而路由器/3层交换机则是根据IP地址进行处理的。由此，TCP/IP中网络层的地址就成为了IP地址。

路由器可以连接不同的数据链路。例如：连接两个以太网，或者连接一个以太网与一个FDDI。

路由器还分担网络负荷的作用，甚至有些路由器具备一定的网络安全功能。

#### 1.4.6、4~7层交换机

![image-20210802160945156](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802160945156.png)

4~7层交换机负责处理OSI模型中从传输层至应用层的数据。如果用TCP/IP分层模型来表述，4~7层交换机就是以TCP等协议的传输层及其上面的应用层为基础，分析收发数据，并对其进行特定的处理

负载均衡器是4~7层交换机的一种。

4~7层交换机功能：

- 负载均衡
- 带宽控制
- 广域网加速器
- 特殊应用访问加速
- 防火墙

#### 1.4.7、网关

![image-20210802165620933](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802165620933.png)

网关是OSI参考模型中负责将从传输层到应用层的数据进行转换和转发的设备。它与4~7层交换机一样都是处理传输层以上的数据，但是网关不仅转发数据还负责对数据进行转换，它通常会使用一个表示层或应用层网关，在两个不能进行直接通信的协议之间进行翻译。

![image-20210802170757918](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802170757918.png)

代理服务器(Proxy Server)：又称应用网关，用于流量控制和信息监控

防火墙也是一种网关

![image-20210802171756320](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802171756320.png)

![image-20210802171802800](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210802171802800.png)

## 第二章、TCP/IP基础

![image-20210803092628676](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210803092628676.png)

### 2.1、TCP/IP出现的背景及其历史

#### 2.1.1、从军用技术的应用谈起

20世纪60年代，一家以美国国防部(DoD,The Department of Defense)为中心的组织，想到即使遭到敌方攻击，也可以经过迂回线路实现最终通信

随后分组交换技术提出，它可以使多个用户同一时间共享一条通信线路进行通信，从而提高线路的使用效率，也降低了搭建线路的成本

![image-20210803110627724](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210803110627724.png)

![image-20210803110718486](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210803110718486.png)

#### 2.1.2、ARPANET的诞生

1969年，为验证分组交换技术的实用性，研究人员搭建了一套网络。在美国西海岸的大学和研究所等4个节点(UCLA（加州大学洛杉矶分校）、UCSB（加州大学圣巴巴拉分校）、SRI（斯坦福研究所）和犹他州大学)

这个网络被称为ARPANET(Advanced Research Projects Agency Network,阿帕网)，也是全球互联网的鼻祖。

#### 2.1.3、TCP/IP的诞生

20世纪70年代前半叶，ARPANET中的一个研究机构研发出了TCP/IP。直到1982年，TCP/IP的具体规范才被最终定下来，并与1983年成为ARPANET网络唯一指定的协议

![image-20210803113233988](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210803113233988.png)

#### 2.1.4、UNIX系统的普及与互联网的扩张

1980年左右，ARPANET中的很多大学与研究机构开始使用一种叫做BSD UNIX(由美国加州大学伯克利分校开发的免费UNIX系统)的操作系统，由于BSD UNIX实现了TCP/IP协议，很快在1983年，TCP/IP便被ARPANET正式采用。同年，前SUN公司也开始向一般用户提供实现了TCP/IP的产品。

20世纪80年代，ARPANET连接到NSFnet网络，基于TCP/IP而形成的世界性范围的网络 -- 互联网(The Internet)便诞生了

#### 2.1.5、商用互联网服务的启蒙

1990年逐渐被引入公司企业及一般家庭。也出现了ISP(Internet Service Provider)互联网服务提供商

### 2.2、TCP/IP的标准化

20世纪90年代，ISO开展了OSI这一国际标准化进程。然而，OSI协议并没有得到普及，真正被广泛使用的是TCP/IP协议

#### 2.2.1、TCP/IP的具体含义

TCP/IP是利用IP进行通信时所必须用到的协议群的统称。IP或ICMP、TCP或UDP、TELNET或FTP、以及HTTP等都属于TCP/IP的协议，有时TCP/IP也称为网络协议群

![image-20210803161621218](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210803161621218.png)

#### 2.2.2、TCP/IP标准化精髓

TCP/IP标准化过程的两大特点：

- 开放性

  允许任何人加入讨论

- 实用性

  TCP/IP在制定某一协议规范的过程中会考虑到这个协议实现的可行性，不停地实验，一旦发现有问题，可以继续在IEFT中讨论，及时修改程序、协议或相应的文档

相比TCP/IP，OSI未达到普及的原因在于未能尽早地制定可行性较强的协议、未能提出应对技术快速革新的协议以及没有能及时进行后期改良的方案这几点

#### 2.2.3、TCP/IP规范 —— RFC

TCP/IP的协议由IETF讨论制定。那些需要标准化的协议，被人们列入RFC(Requst For Comment)(RFC从字面意义上看就是指征求意见表，属于一种征求协议相关意见的文档)文档并在互联网上公布，RFC还包含协议实现和运用相关意见的文档(FYI(For Your Infomation)，以及实验方面的信息(Experimental)

RFC文档通过编号组织每个协议的标准化请求，如IP协议由RFC279制定，TCP协议由RFC793制定

由于RFC太麻烦，人们采用STD(Standard，例如STD5表示包含ICMP的IP协议标准。因此，STD5由RFC791、RFC919、RFC922、RFC792、RFC950以及RFC1112，6个RFC组成)方式管理编号。

而后FYI也开始标注编号组织。

![image-20210804110315064](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210804110315064.png)

![image-20210804110326819](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210804110326819.png)

#### 2.2.4、TCP/IP的标准化流程

标准化流程大致分为以下几个阶段：

- 提议阶段

  提议人或组织撰写文档作为草案发布，IETF通过邮件讨论，也会进行相应的设备实现、模拟以及应用实验

- 互联网草案阶段

  有效期通常为6个月。6个月内如果没有讨论结果会自动消除

- 如果认为可以进行标准化，就记入RFC进入提议标准阶段

  得到IESC(IETF Engineering Steering Group，由IETF的主要成员组成)的批准，就能被编入RFC文档。这个文档叫做提议标准(Proposed Standard)

- 草案标准阶段

  如果能得到IESG的认可，就可以成为草案标准(Draft Standard)

- 标准阶段

  如果所有参与的人都觉得它“实用性强，没什么问题”，并得到IESG的最终批准，那么这个草案标准就可成为正式标准

![image-20210804134314747](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210804134314747.png)

#### 2.2.5、RFC的获取方法

- RFC获取网址：

  http://www.rfc-editor.org/rfc/

  ftp://ftp.rfc-editor.org/in-notes/

- STD获取网址 

  http://www.rfc-editor.org/in-notes/std/ 

- FYI获取网址 

  http://www.rfc-editor.org/in-notes/fyi/ 

- ID获取网址 

  http://www.rfc-editor.org/internet-drafts/ 

JPNIC的ftp服务器中的目录： 

- STD获取网址 

  ftp://ftp.nic.ad.jp/rfc/std/ 

- FYI获取网址 

  ftp://ftp.nic.ad.jp/rfc/fyi/ 

- ID获取网址 

  ftp://ftp.nic.ad.jp/internet-drafts/ 

### 2.3、互联网基础知识

#### 2.3.1、互联网定义

互联网(Internet)，字面上理解是将多个网络连接使其构成一个更大的网络，所以Internet本意为网际网。然而现在，当专门指代网络之间的连接时，可以使用网际网。

“互联网”是指由ARPANET发展而来、互连全世界的计算机网络。

Internet：网际网

The Internet：互联网

Intranet：封闭网络

#### 2.3.2、互联网与TCP/IP的关系

互联网进行通信时，需要相应的网络协议支持，这个网络协议就是TCP/IP协议

#### 2.3.3、互联网的结构

互联网中的每个网络都是由骨干网(BackBone)和末端网(Stub)组成的。每个网络之间通过NOC(Network Operation Center,网络操作中心)相连。如果网络的运营商不同，它的网络连接方式和使用方法也会不同。连接这种异构网络需要有IX(Internet Exchange,网络交换中心)的支持。总之，互联网就是众多异构网络通过IX互连的一个巨型网络

![image-20210805140159950](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210805140159950.png)

#### 2.3.4、ISP和区域网

连接互联网需要向ISP或区域网提出申请，申请入网只要与ISP签约即可。不同的ISP所提供的的互联网接入服务的项目也不同。

区域网指的是在特定区域内由团体或志愿者所运营的网络

### 2.4、TCP/IP协议分层模型

#### 2.4.1、TCP/IP与OSI参考模型

![image-20210805145748957](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210805145748957.png)

#### 2.4.2、硬件(物理层)

TCP/IP的最底层是负责数据传输的硬件

#### 2.4.3、网络接口层(数据链路层)

网络接口层利用以太网中的数据链路层进行通信。可以把它当做NIC起作用的"驱动程序"

#### 2.4.4、互联网层(网络层)

互联网层使用IP协议，它相当于OSI模型中的第3层网络层。IP协议基于IP地址转发分包数据

![image-20210805152451974](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210805152451974.png)

TCP/IP分层中的互联网层与传输层的功能通常由操作系统提供。尤其是路由器，它必须得实现通过互联网层转发分组数据包的功能

连接互联网的所有主机和路由器必须都实现IP的功能。其他连接互联网的网络设备(如网桥、中继器或集线器)就没必要一定实现IP或TCP的功能(有时为了监控和管理网桥、中继器、集线器等设备，也需要让它们具备IP、TCP的功能)

##### IP

IP是跨越网络传送数据包，是整个互联网都能收到数据的协议。IP协议使数据能够发送到地球的另一端，这期间他使用IP协议作为主机的标识(连接IP网络的所有设备必须有自己唯一的识别号，以便识别具体设备。分组数据在IP地址的基础上被发送到对端。)

IP还隐含着数据链路层的功能。通过IP，相互通信的主机之间不论经过怎样的底层数据链路都能实现通信

虽然IP也是分组交换的一种协议，但是它不具有重发机制。即使分组数据包未能达到对端主机也不会重发。因此，属于非可靠性传输协议

##### ICMP

IP数据包在发送途中一旦发生异常导致无法达到对端目标地址时，需要给发送端发送一个发生异常的通知。ICMP就是为了这一功能而制定的。它有时也被用来诊断网络的健康状况。

##### ARP

从分组数据包的IP地址中解析出物理地址(MAC地址)的一种协议

#### 2.4.5、传输层

TCP/IP的传输层与OSI参考模型中的传输层类似，主要功能就是让应用之间实现通信

![image-20210814101058607](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210814101058607.png)

##### TCP

TCP是一种面向有连接的传输层协议，可保证两端通信主机之间的通信可达。TCP能够正确处理在传输过程中丢包、传输顺序乱掉等情况。TCP能有效利用带宽。

TCP建立连接会导致流量浪费，此外TCP协议定义了很多复杂的规范，因此不利于视频会议等场合

##### UDP

UDP是一种面向无连接的传输层协议。UDP不会关注对端是否真的收到传送过去的数据。

UDP常用于分组数据较少或多播、广播通信以及视频通信等多媒体领域

#### 2.4.6、应用层(会话层以上的分层)

TCP/IP将OSI参考模型中的会话层、表示层和应用层的功能都集中到了应用程序中实现

![image-20210814162448442](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210814162448442.png)

TCP/IP应用的架构绝大多数属于客户端/服务端模型。这种模式中，提供服务的程序会预先被部署到主机上，等待接收任何时刻客户可能发送的请求。

客户端可以随时发送请求发送服务端。有时服务端可能会有处理异常、超负载等情况，这时客户端可以等待片刻后重新发一次请求

##### www

浏览器与服务端之间通信所用的协议是HTTP(HyperText Transfer Protocol)。所传输数据的主要格式是HTML(HyperText Markup Language)。www中的HTTP属于OSI应用层的协议，而HTML属于表示层的协议

##### 电子邮件(E-Mail)

电子邮件用到的协议叫做SMTP(Simple Mail TranferProtocol

最初，人们只能发送文本格式的电子邮件。然而现在，电子邮件的格式由MIME协议扩展以后，可以发送声音、图像等信息，还可以修改邮件文字的大小、颜色。

MIME属于OSI参考模型的第6层 —— 表示层

##### 文件传输(FTP)

FTP(File Transfer Prototol)，传输过程中可以选择用二进制方式还是文本方式

在FTP中进行文件传输时会建立两个TCP连接，分别是发出传输请求时所用到的控制连接与实际传输数据时所用到的数据连接

##### 远程登录(TELNET与SSH)

TCP/IP网络中远程登录常用TELNET(TELetypewriter NETwork。有时也称作默认协议)和SSH(Secure SHell)两种协议。还有BSD UNIX系中rlogin的命令协议以及X Window System中的X协议也可以实现远程登录

##### 网络管理(SNMP)

TCP/IP网络管理采用SNMP(Simple Network Management Protocol)x协议。使用SNMP管理的主机、网桥、路由器等称作SNMP代理(Agent)，而进行管理的那一段叫做管理器(Manager)

![image-20210816104139415](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210816104139415.png)

在SNMP的代理端，保存着网络接口的信息、通信数据量、异常数据量以及设备温度等信息。这些信息可以通过MIB(Management Infomation Base)(MIB也被称为是一种可透过网络的结构变量)访问。SNMP属于应用层协议，MIB属于表示层协议

### 2.5、TCP/IP分层模型与通信示例

#### 2.5.1、数据包首部

![image-20210816114114431](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210816114114431.png)

每个分层，都会对所发送的数据附近一个首部，在这个首部中包含了该层必要的信息，如发送的目标地址以及协议相关信息。通常，为协议提供的信息为包首部，所要发送的内容为数据

表述数据的单位

- 包：全能性术语
- 帧：数据链路层中包的单位
- 数据包：IP和UDP等网络层以上的分层中包的单位
- 段：TCP数据流中的信息
- 消息：指应用协议中数据的单位

网络中传输的数据包由两部分组成：一部分是协议要用到的首部，另一部分是上层传过来的数据。受不得结构由协议的具体规范详细定义。例如，识别上一层协议的域应该从包的哪一位开始取多少个比特、如何计算校验和并插入包的哪一位等。相互通信的两端计算机如果在识别协议的序号以及校验和的计算方式上不一样，就根本无法实现通信

#### 2.5.2、发送数据包

计算机A向计算机B发送了电子邮箱，内容为“早上好”

1. 应用程序处理

   点击发送，应用程序首先进行编码处理，如使用UTF-8进行编码。这些编码相当于OSI的表示层

   编码转换后，实际邮件不一定会马上被发送出去，因为有些邮件软件可以同时发送多个邮件，也可能会有用户点击"收信"按钮以后才一并接收。

   应用在发送邮件的那一刻建立TCP连接，从而利用这个TCP连接发送数据。它的过程首先是将应用的数据发送给下一层的TCP，再做实际的转发处理

2. TCP模块的处理

   TCP根据应用的指示，负责建立连接、发送数据以及断开连接。TCP提供将应用层发来的数据顺利发送至对端的可靠传输(相当于OSI会话层)

   为了实现TCP的这个功能，需要在应用层数据的前端附加一个TCP首部。TCP首部中包括源端口号和目标端口号(用以识别发送主机跟接收主机上的应用)、序号(用以发送的包中哪部分是数据)以及校验和Check Sum(用来检验数据的读取是否正常进行的方法，判断数据是否被损坏)。随后将附加了TCP首部的包再发送给IP

3. IP模块的处理

   IP将TCP传过来的TCP首部和TCP数据合起来当做自己的数据，并在TCP首部的前端再加上自己的IP首部。

   IP包生成后，参考路由控制表决定接收此IP包的路由或主机。然后发送。

   如果尚不知道接收端的MAC地址，可以利用ARP(Address Resolution Protocol)查找。只要知道了对端的MAC地址，就可以将MAC地址和IP地址交给以太网的驱动程序，实现数据传输

4. 网络接口(以太网驱动)的处理

   从IP传过来的IP包，给这数据附加上以太网首部并进行发送处理。以太网首部中包含接收端MAC地址、发送端MAC地址以及标志以太网类型的以太网数据的协议。根据上述信息产生的以太网数据包将通过物理层传输给接收端。发送处理中的FCS(Frame Check Sequence)由硬件计算，添加到包的最后。设置FCS的目的是为了判断数据包是否由于噪声而被破坏

![image-20210817085201608](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210817085201608.png)

#### 2.5.3、经过数据链路的包

分组数据包经过以太网的数据链路时的大致流程如下图

![image-20210817085324020](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210817085324020.png)

包流动时，从前往后依次被附加了以太网包首部、TCP包首部(或UDP包首部)以及应用自己的包首部和数据。而包的最后则追加了以太网包尾(包首部附加于包的前端，而包尾则只则只追加到包的后端的部分)

每个包首部中至少都会包含两个信息，一个是发送端和接收端地址，另一个是上一层的协议类型。

以太网会用MAC地址，IP会用IP地址，而TCP/UDP则会用端口号作为识别两端主机的地址。即使是在应用程序中，像电子邮件地址这样的信息也是一种地址标识。

每个分层的包首部中还包含一个识别位，它是用来表示上一层协议的种类信息。例如以太网的包首部中的以太网类型，IP中的协议类型以及TCP/UDP中两个端口的端口号等都起着标识协议类型的作用。就是在应用的首部信息中，有时也会包含一个用来识别其他数据类型的标签

#### 2.5.4、数据包接收处理

包的接收流程是发送流程的逆序过程

5. 网络接口(以太网驱动)的处理
   主机收到以太网包以后，首先从以太网的包首部找到MAC地址判断是否为发给自己的包。如果不是，则丢弃(很多NIC产品可以设置为即使不是发给自己的包也不丢弃数据。这可以用于监控网络流量)

   如果接收到了，就查找以太网包首部中的类型域从而确定以太网协议所传送过来的数据类型。在这个例子中数据类型显然是IP包，因此再将数据川蜀阁处理IP的子程序，如果这时不是IP而是其他诸如ARP的协议，就把数据传给ARP处理。总之，如果以太网包首部的类型域包含了一个无法识别的协议类型，则丢弃数据

6. IP模块的处理

   IP模块收到数据后，如果判断IP地址与自己的IP地址匹配，则可接收数据并从中查找上一层协议。如果上一层是TCP就把后面的部分给TCP处理，如果是UDP则给UDP处理。对于有路由器的情况下，接收端地址往往不是自己的地址，此时，需要借助路由控制表，在调查应该送达的主机或路由器以后再转发数据

7. TCP模块的处理

   TCP模块中，首先会计算一下校验和，判断数据是否被破坏。然后检查是否在按照序号接收数据，最后检查端口号，确定具体的应用程序

8. 应用程序的处理

   接收端应用程序会直接发送端发送的数据。通过解析数据可以获知邮件的收件人地址是乙的地址。如果vujiB没有乙的邮箱，那么会报“无此收件地址”

   如果一切正常那么会返回“处理正常”，如果未能保存那么会返回“处理异常”

## 第三章、数据链路

### 3.1、数据链路的作用

数据链路。指OSI参考模型中的数据链路层，有时也指以太网、无线局域网等通信手段。

TCP/IP中对于OSI参考模型的数据链路层及以下部分(物理层)未作定义。因为TCP/IP以这两层的功能是透明的为前提。

数据链路层的协议定义了通过通信媒介互连的设备之间传输的规范。

实际的通信媒介之间处理的是电压的高低、光的闪灭以及点播的强弱等信号。把这些信号与二进制的0、1进行转换正是物理层的责任。数据链路层处理的数据也不是单纯的0、1序列，该层把它们集合为一个叫做“帧”的块，然后在进行传输。

数据链路也可以被视为网络传输中的最小单位

在以太网与FDDI(Fiber Distributed Data Interface，光纤分布式数据接口)的规范中，不仅包含OSI参考模型的第二层数据链路层，也规定了第一层物理层的规格。而在ATM(Asynchronous Transfer Mode，异步传输方式)的规范中，还包含了第三层网络层的一部分功能。

数据链路的段是指一个被分割的网络。

- 从网络层的概念看，它是一个网络(逻辑上)→即，从网络层的立场出发，这两条网线组成一个段。
- 从物理层的概念看，两条网线分别是两个物体（物理上）→即，从物理层的观点出发，一条网线是一个段

![image-20210823183125839](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210823183125839.png)

网络拓扑：网络的连接和构成的形态称为网络拓扑(Topology)。网络拓扑包括总线型、环型、星型、网状型等。拓扑依此不仅用于直观可见的配线方式上，也用于逻辑上网络的组成结构。两者有时可能会不一致

![image-20210824085446775](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824085446775.png)

### 3.2、数据链路相关技术

#### 3.2.1、MAC地址

MAC地址用于识别数据链路中互连的节点。以太网或FDDI中，根据IEEE802.3(IEEE：美国电气和电子工程师协会，也叫"I triple E"。IEEE802是制定局域网标准化相关的组织。其中IEEE802.3是关于以太网的国际规范)的规范使用MAC地址。其他诸如无线LAN、蓝牙等设备中也是用相同规格的MAC地址

![image-20210824090701476](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824090701476.png)

MAC地址长48比特，在使用网卡(NIC)的情况下，MAC地址一般会被烧入到ROM中。因此，任何一个网卡的MAC地址都是唯一的(虚拟机和微机板可以设置自己的MAC地址)

![image-20210824092038964](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824092038964.png)

MAC地址不论那种数据链路的网络(以太网、FDDI、ATM、无线LAN、蓝牙等)，都不会相同

厂商识别码(OUI：Organizationally Unique Ideifier)：网络分析器可以分析出局域网中的包是由哪个厂商的网卡发出的。它通过读取数据帧当中发送MAC地址里的厂商识别码进行识别。

OUI信息一般都会公开在以下网站上（由于最近网络设备厂商的收购与合并，OUI的数据库和实际厂商名字也出现了不一致的情况。）： 

http://standards.ieee.org/develop/regauth/oui/public.html 

此外，MAC地址的分配，通过以下站点申请（收费）： 

http://standards.ieee.org/develop/regauth/oui/index.html 

#### 3.2.2、共享介质型网络

从通信介质的使用方法上看，网络可分为共享介质型和非共享介质型。

共享介质型网络指由多个设备共享一个通信介质的一种网络。最早的以太网和FDDI就是介质共享型网络。在这种方式下，设备之间使用同一种载波信道进行发送和接收。为此，基本上采用半双工通信方式，并有必要对介质进行访问控制

争用方式(Contention)是指争夺获取数据传输的权利，也叫CSMA(载波监听多路访问)。这种方式通常令网络中的各个站(数据链路中很多情况下称节点为“站”)采用先到先得的方式占用信道发送数据，如果多个站同时发送帧，则会产生冲突现象。也因此会导致网络拥堵与性能下降

![image-20210824114034391](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824114034391.png)

在一部分以太网当中，采用了改良CSMA的另一种方式——CSMA/CD(Carrier Sense Multiple Access with Collision Detection)方式。CSMA/CD要求每个站提前检查冲突，一旦发生冲突，则尽早释放信道。原理如下：

- 如果载波信道上没有数据流动，则任何站都可以发送数据
- 检查是否发生冲突。一旦发生冲突时，放弃发送数据(会发送一个32位特别的信号，在阻塞报文以后再停止发送。接收端通过发生冲突时帧的FCS，判断出改帧不正确从而丢弃帧)同时立即释放载波信道
- 放弃发送以后，随机延时一段时间，再重新争用介质，重新发送帧

![image-20210824120037169](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824120037169.png)

令牌传递方式是沿着令牌发送一种叫做"令牌"的特殊报文，是控制传输的一种方式。只有获得令牌的站才能发送数据。这种方式有两个特点，一是不会有冲突，二是每个站都有通过平等循环获得令牌的机会。因此，即使网络拥堵也不会导致性能下降。

不过如果网络在不太拥堵的情况下数据链路的利用率达不到100%。因此衍生出多种令牌传递的技术。例如，早期令牌释放、令牌追加(不等待接收方的数据到达确认就将令牌发送给下一个站)等方式以及多个令牌同时循环等方式

![image-20210824140933682](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824140933682.png)

#### 3.2.3、非共享介质网络

非共享介质网络是指不共享介质，是对介质采取专用的益中所传输控制方式。在这种方式下，网络中的每个站直连交换机，由交换机负责转发数据帧。此方法下，发送端与接收端并不共享通信介质，因此大多采用全双工通信

通过以太网交换机构建网络，从而使计算机与交换机端口之间形成一对一的连接，即可实现全双工通信，在这种一对一连接全双工通信的方式下不会发生冲突，因此不需要CSMA/CD的机制就可以实现更高效的通信。

该方式还可以根据交换机的高级特性构建虚拟局域网(VLAN，Virtual LAN)、进行流量控制等。

缺点就是，一旦交换机发生故障，与之相连的所有计算机之间都将无法通信。

![image-20210824143020168](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824143020168.png)

半双工与全双工

- 半双工：只发送或只接收的通信方式，类似无线电收发器
- 全双工：可同时发送接收数据，类似电话

采用CSMA/CD方式的以太网，首先要判断是否可以通信，如果可以就独占通信介质发送数据，是半双工通信

![image-20210824143537665](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824143537665.png)

使用交换机与双绞线电缆(亦或光缆)时，既可以通过交换机的端口与计算机之间进行一对一的连接，也可以通过相连电缆内部的收发线路分别进行接收和发送数据。因此，可以实现全双工通信

![image-20210824143920802](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210824143920802.png)

#### 3.2.4、根据MAC地址转发

在使用同轴电缆的以太网(10BASE5、10BASE2)等介质共享网络中，同一时间只能有一台主机发送数据。当联网的主机数量增加时，通信性能会明显下降。若将集线器或集中器等设备以星型连接，就出现了交换集线器，这是一种将非介质共享型网络中所使用的交换机用在以太网中的技术，交换集线器也叫做以太网交换机。

以太网交换机就是持有多个端口(计算机设备的外部接口都称作端口，注意TCP或UDP等传输)的网桥。它们根据数据链路层中每个帧的目标MAC地址，决定从那个网络接口发送数据。这时所参考的，用以记录发送接口的表就叫做转发表(ForwardingTable)

转发表的内容可以自动生成。数据链路层的的每个通过点在接到包时，会从中将源MAC地址以及曾经接收该地址发送的数据包的接口作为对应关系记录到转发表中。以某个MAC地址作为源地址的包由某一接口接收，实质上可以理解为该MAC地址就是该接口的目标。也可以说，以该MAC地址作为目标地址的包，经由该接口送出即可。这一过程也叫自学过程。

![image-20210827113135051](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210827113135051.png)

由于MAC地址没有层次性，转发表中的入口个数与整个数据链路中所有网络设备的数量有关，当设备数量增加时，转发表也会随之变大，检索转发表所用的时间也就越来越长，当连接多个终端时，有必要将网络分成多个数据链路，采用类似于网路层的IP地址一样对地址进行分层管理。

交换机转发方式有两种：存储转发和直通转发

- 存储转发：

  存储转发方式检查以太网数据帧末尾的FCS位后再进行转发。因此，可以避免发送由于冲突而被破坏的帧或噪声导致的错误帧

- 直通转发：

  直通转发方式中不需要将整个帧全部接收下来以后再进行转发。只需要得知目标地址即可开始转发。因此，它具有延迟较短的优势。但同时也不可避免地有发送错误帧的可能性。

#### 3.2.5、环路检测技术

网络环路会带来以下问题：

- 广播风暴

  广播帧不断在环路中被转发，大量消耗网络资源，使得网络无法正常转发其他数据帧

- 主机收到重复的广播帧

- 交换机的帧交换表震荡(漂移)

![image-20210830091812860](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210830091812860.png)

解决方式有两种，生成树与源路，如果使用具有这些功能的网桥，那么即使构建了一个带有环路的网路，也不会造成那么严重的问题。只要搭建合适的环路，就能分散网路流量，在发生某一处路由故障时选择绕行，可以提高容灾能力。

![image-20210828141215937](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210828141215937.png)

- 生成树(Spanning Tree Protocol，STP)的方式：

  该方法由IEEE802.1D定义。每个网桥必须在每1~10秒内相互交换BPDU(Bridge Protocol Data Unit)包，从而判断哪些端口使用，以便消除环路。一旦发生故障，则自动切换通信线路，利用那些没有被使用的端口继续进行传输。

  生成树法与计算机和路由器的功能没有关系，但是只要有生成树的功能就足以消除环路

  ![image-20210828142006909](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210828142006909.png)

  生成树方法有个弊端，就是在发生故障切换网络时需要几十秒的时间。IEEE802.1W中定义了一个叫RSTP(Rapid Spanning Tree Protocol)的方法，解决了这个问题

- 源路由法

  源路由法最早由IBM提出，已解决令牌环网络问题。该方式可以判断发送数据的源地址是通过哪个网桥实现传输的，并将帧写入RIF(Routing Infomation Field)网桥则根据这个RIF信息发送帧给目标地址。因此，即便网桥中出现了环路，数据帧也不会被反复转发，可成功地发送到目标地址。在这种机制中发送端本身必须具备源路由的功能

#### 3.2.6、VLAN

网络管理员在管理网络时，有时不得不修改网络的拓扑结构，也必须进行硬件线路的改造。如果采用带有VLAN技术的网桥，就不用实际修改网络布线，只需修改网络的结构即可。VLAN技术附加到网桥/2c层交换机上，就可以切断所有VLAN之间的所有VLAN之间的所有通信。因此，相比一般的网桥/2层交换机，VLAN可以过滤多余的包，提高网络的承载效率。

交换机按照其端口区分了多个网段，从而区分了广播数据的传播的范围、减少了网络负载并提高了网络的安全性。然而异构的两个网段之间，就需要利用具有路由功能的交换机(如3层交换机)，或在各段中间通过路由器的连接才能实现通信。

![image-20210902163412061](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210902163412061.png)

对这种VLAN进行了扩展，又定义了IEEE802.1Q的标准(也叫TAG VLAN)，该标准允许包含跨越异构交换机的网段。TAG VLAN中的每个网段都用一个VLAN ID的标签进行唯一标识。在交换机中传输帧时，在以太网首部加入这个VID标签，根据这个值决定将数据帧发送给哪个网段。

![image-20210903095326773](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210903095326773.png)

### 3.3、以太网

以太网最早是由美国的Xerox公司与前DEC公司设计的一种通信方式，当时命名为Ethernet。之后由IEEE802.3委员会将其规范化。但是这两者之间对以太网网帧的格式定义还是有所不同的。因此，IEEE802.3所规范的以太网有时又被称为802.3以太网(也有DIX以太网。DIX由DEC、Intel和Xerox等公司名称的首字母组成)

#### 3.3.1、以太网连接形式

最初，一般采用多台终端使用同一根同轴电缆的共享介质型连接方式

现在，一般采用终端与交换机之间独占电缆的方式实现以太网通信

#### 3.3.2、以太网的分类

10BASE中的"10"、100BASE中的"100"指的是10M、100M的传输速度。而追加于后面的"5"、"2"、"T"、"F"等字符表示的是传输介质。在传输速度相同而传输所用电缆不同的情况下，可以连接那些允许更换传输介质的中继器或集线器。而在传输速度不同的情况下，则必须采用那些允许变更速度的设备如网桥、交换集线器或路由器。

![image-20210903164030471](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210903164030471.png)

计算机内部采用二进制：

- 1K = 1024B
- 1M = 1024K
- 1G  = 1024M

以太网以时钟频率决定传输速度

- 1K = 1000B
- 1M = 1000K
- 1G = 1000M

#### 3.3.3、以太网的历史

1. 半双工通信为前提的CSMA/CD方式，结果网速滞留在10M。
2. 因ATM交换技术的进步和CAT5 UTP电缆的普及，采用共享介质网络同样的直接与交换机连接的方式，不用再检查冲突，网络变得更加高速10G

#### 3.3.4、以太网帧格式

以太网帧前端有一个叫做前导码(Preamble)的部分，它由0、1数字交替组合而成，表示一个以太网帧的开始，也是对端网卡能够确保与其同步的标志。

如下图，前导码末尾是一个叫做SFD(Start Frame Delimiter)的域，它的值是"11"。在这个域之后就是以太网帧的本体。前导码与SFD合起来占8个字节

![image-20210906104154069](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210906104154069.png)

以太网帧本体的前端是以太网的首部，它总共占14个字节。分别是目标MAC地址、源MAC地址以及上层协议类型。

帧头后面的是数据。一个数据帧所能容纳的最大数据范围超是46~1500个字节。帧尾是一个叫做FCS(Frame Check Sequence，帧检验序列)的4个字节

在目标MAC地址中存放了目标工作站的物理地址。源MAC地址中则存放构造以太网帧的发送端工作站的物理地址。

![image-20210906104609995](C:\Users\black\AppData\Roaming\Typora\typora-user-images\image-20210906104609995.png)

类型通常跟数据一起传送，它包含用以标识协议类型的编号

























