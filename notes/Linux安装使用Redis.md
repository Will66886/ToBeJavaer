# Linux安装及使用Redis

## 一、环境及版本

Linux版本为CentOS7

Redis版本为6.0.6

GCC需升级为7.3.1-5

```
#第一步
sudo yum install centos-release-scl
#第二步
sudo yum install devtoolset-7-gcc*
#第三步
scl enable devtoolset-7 bash
#查看gcc版本
gcc -v
```

## 二、安装Redis

```
cd /usr/local
wget http://download.redis.io/releases/redis-6.0.6.tar.gz
tar xzf redis-6.0.6.tar.gz
cd redis-6.0.6
make
```

为了方便管理，创建bin目录和etc目录，分别用于存放启动项及配置文件

```
mkdir etc
mkdir bin
cp redis.conf /usr/local/redis-6.0.6/etc
cp redis-cli redis-server mkreleasehdr.sh redis-check-aof redis-check-rdb redis-benchmark /usr/local/redis-6.0.6/bin
```

## 三、运行Redis

在bin目录下执行`./redis-server`就可以启动redis啦

如果想要让redis后台运行，需修改配置文件redis.conf中的daemonize 由no改为yes

```
daemonize yes
```

可以在后台查看到

```
ps -A | grep redis
```

通过kill杀死进程

```
kill <进程号>
#强制关闭
kill -9 <进程号>
```

`./redis-cli`可以运行客户端，就可以在里面执行redis命令了

`./redis-server ../etc/redis.conf` 可以后台运行

## 三、远程连接

一般我们在Linux上安装redis都是作为服务器使用

所以想要外部访问服务器中的redis还需要做以下事情。

首先修改配置redis.conf中的protected-mode 把yes改为no

```
protected-mode no
```

其次，设置防火墙开放端口6379

```
#查看端口
firewall-cmd --query-port=6379/tcp
#新增端口
firewall-cmd --add-port=6379/tcp
#移除端口
firewall-cmd --remove-port=6379/tcp
```

查看IP

```
ifconfig
```

在远程客户端请求以上IP和端口就可以使用服务器上的Redis了

还有本地redis数据和远程redis数据是分开保存的，如果想要在本地客户端查看redis数据

```
./redis-cli -h <本机IP> -p <端口号>
```

