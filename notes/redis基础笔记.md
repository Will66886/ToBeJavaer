# redis基础笔记

redis是一种nosql 非关系型数据库 内存型数据库

redis的应用场景：

- 缓存（数据查询、短连接、新闻内容、商品内容等等）
- 聊天室的在线好友列表
- 任务队列。（秒杀、抢购、12306等等）
- 应用排行榜
- 网站访问统计
- 数据过期处理（可精确到毫秒）
- 分布式集群架构中的session分离

## 1、redis的数据结构



redis存储的是：key - value格式的数据格式

key都是字符串格式

value有五种数据结构：

- 字符串类型 string
- 哈希类型 hash：map格式
- 列表类型 list：可重复数据
- 集合类型 set：不可重复数据
- 有序集合类型 sortedset：不可重复数据并排序

##### 1、字符串类型 string 命令

1. 存储：set key value

2. 获取：get key

3. 删除：del key

##### 2、哈希类型 hash 命令

1. 存储：hset key field value
2. 获取：hget key field
3. 获取全部：hgetall key
4. 删除：hdel key field

##### 3、列表类型 list 命令

1. 添加：

   1. lpush key value:将元素加入列表左边
   2. rpush key value:将元素加入列表右边

2. 获取：lrange key start end ：范围获取

3. 删除：    

   1. lpop key：删除列表最左边的元素，并将元素返回
   2. rpop key：删除列表最右边的元素，并将元素返回

##### 4、集合类型 set 命令

1. 存储：sadd key value
2. 获取：smembers key：获取set集合中的所有元素
3. 删除：srem key value：删除set集合中的某个元素

##### 5、有序集合类型 sortedset 命令

不允许重复元素，且元素有顺序

1. 存储：zadd key score value
2. 获取：zrange key start end
3. 删除：zrem key value

## 2、通用命令

1. keys *：查询所有的键
2. type key：获取键对应的value的类型
3. del key：删除指定的key value

## 3、持久化

1. redis是一个内存数据库，当redis服务器重启，或者电脑重启，数据会对视，我们可以将redis内存中的数据持久化保存到硬盘的文件中
2. redis持久化机制：
   1. RDB：默认方式，不需要进行配置，默认就使用这种机制
      * 在一定的间隔时间中，检查key的变化情况，然后持久化数据
   2. AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据

## 4、Java客户端 Jedis

Jedis：一款java操作redis数据库的工具

快速入门步骤：

1. 下载jedis的jar包

2. 使用

   ```java
   //获取连接
   Jedis jedis = new Jedis("localhost",6379);
   //操作
   jedis.set("username","lisi");
   //关闭链接
   jedis.close();
   ```

### Jedis操作各种redis数据类型

##### 1、操作 字符串类型 string

```java
//获取连接
Jedis jedis = new Jedis();
//操作
jedis.set("username","王五");
//获取
String username = jedis.get("username");
System.out.println(username);
//可以使用setex()方法存储可以指定过期时间的key value
jedis.setex("activecode",20,"hehe");
//关闭链接
jedis.close();
```

##### 2、哈希类型 hash 命令

```java
//获取连接
Jedis jedis = new Jedis();
//操作
jedis.hset("user","name","liuliu");
jedis.hset("user","age","23");
jedis.hset("user","gender","male");
//获取
String username = jedis.hget("user","name");
System.out.println(username);

//获取hash中所有map中的数据
Map<String,String> user = jedis.hgetAll("user");
Set<String> set = user.keySet();
for (String key:set){
    String value = user.get(key);
    System.out.println(key + ":" + value);
}
//关闭链接
jedis.close();
```



##### 3、列表类型 list 命令

```java
//获取连接
Jedis jedis = new Jedis();
//list存储
jedis.lpush("mylist","a","b","c");
jedis.rpush("mylist","a","b","c");
//list范围获取
List<String> list = jedis.lrange("mylist",0,-1);
//list弹出
String element1 = jedis.lpop("mylist");
String element2 = jedis.rpop("mylist");
List<String> list2 = jedis.lrange("mylist",0,-1);
for (String str:list2){
    System.out.println(str);
}
//关闭链接
jedis.close();
```

##### 4、集合类型 set 命令

```java
//获取连接
Jedis jedis = new Jedis();
//set存储
jedis.sadd("myset","java","php","c++");
//set获取
Set<String> myset = jedis.smembers("myset");
System.out.println(myset);
//set删除
jedis.srem("myset", "c++");
//关闭链接
jedis.close();
```

##### 5、有序集合类型 sortedset 命令

```java
//获取连接
Jedis jedis = new Jedis();
//sortedset存储
jedis.zadd("mysortedset",55,"孙悟空");
jedis.zadd("mysortedset",30,"猪八戒");
jedis.zadd("mysortedset",25,"沙和尚");
//sortedset获取
Set<String> mysortedset = jedis.zrange("mysortedset",0,-1);
System.out.println(mysortedset);
jedis.zrem("mysortedset","孙悟空");
//关闭链接
jedis.close();
```