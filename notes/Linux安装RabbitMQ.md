# Linux安装RabbitMQ

官网地址：https://www.rabbitmq.com/

学习自：http://www.imooc.com/article/305858

Linux版本：CentOS7

RabbitMQ：3.8.5

仅供参考，不同版本安装略有不同

进入官网，首先点击Get Started

![image-20200807093416002](Linux安装RabbitMQ.assets/image-20200807093416002.png)

然后点击Download + Installation

![image-20200807093441535](Linux安装RabbitMQ.assets/image-20200807093441535.png)

点击下载安装linux版本

![image-20200807093532199](Linux安装RabbitMQ.assets/image-20200807093532199.png)

1. RabbitMQ是用erlang写的，所以首先安装erlang

官网解释直接yum可能无法安装erlang的最新版本，所以我们要先锁定erlang在yum库中的版本

下拉网页找到

![image-20200807094434130](Linux安装RabbitMQ.assets/image-20200807094434130.png)

选择rpm运行这条命令

```
curl -s https://packagecloud.io/install/repositories/rabbitmq/erlang/script.rpm.sh | sudo bash
```

![image-20200807094503680](Linux安装RabbitMQ.assets/image-20200807094503680.png)

提示安装成功

![image-20200807094701273](Linux安装RabbitMQ.assets/image-20200807094701273.png)

```
yum install erlang
```

提示Complete!安装成功

![image-20200807100055880](Linux安装RabbitMQ.assets/image-20200807100055880.png)

![image-20200807100113548](Linux安装RabbitMQ.assets/image-20200807100113548.png)

运行erlang查看版本是否正确

```
erl
```

2. 安装rabbitmq-server

首先安装下面三个key，不报错就是安装成功了

![image-20200807100833984](Linux安装RabbitMQ.assets/image-20200807100833984.png)

```bash
rpm --import https://packagecloud.io/rabbitmq/rabbitmq-server/gpgkey

rpm --import https://packagecloud.io/gpg.key
```

![image-20200807103616174](Linux安装RabbitMQ.assets/image-20200807103616174.png)

```bash
rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc
```



随后，点击这里锁定版本库

![image-20200807101143409](Linux安装RabbitMQ.assets/image-20200807101143409.png)

![image-20200807101231832](Linux安装RabbitMQ.assets/image-20200807101231832.png)

提示The repository is setup! You can now install packages.则为安装成功

下载软件包，我这里是CentOS7.x版本

```
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.8.6/rabbitmq-server-3.8.6-1.el7.noarch.rpm
```

![image-20200807103124110](Linux安装RabbitMQ.assets/image-20200807103124110.png)

安装前还需要安装EPEL和socat

```
yum -y install epel-release
yum -y install socat
```

正式开始安装rabbitmq-server

```
rpm -ivh rabbitmq-server-3.8.6-1.el7.noarch.rpm
```

![image-20200807104608043](Linux安装RabbitMQ.assets/image-20200807104608043.png)

3. 配置RabbitMQ

这里官网给了一个参考配置：https://github.com/rabbitmq/rabbitmq-server/blob/master/docs/advanced.config.example

在/etc/rabbitmq/目录下

```
tounch rabbitmq.conf
```

然后将参考配置粘贴进去

找到{loopback_users, [<<"guest">>]},在下面添加opback_users = none，该配置表示用户只能通过localhost访问，默认guest用户，将其设为none就让guest用户可以远程连接访问

至此，启动rabbitmq-server

```
service rabbitmq-server start
```

显示Redirecting to /bin/systemctl start rabbitmq-server.service就是成功了