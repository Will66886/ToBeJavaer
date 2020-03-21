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

   