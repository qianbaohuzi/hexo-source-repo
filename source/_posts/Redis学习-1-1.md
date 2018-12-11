---
title: Redis学习(1)
date: 2018-10-03 20:36:52
tags: redis
---
### Redis的安装
以CentOs7为为例,仅需要
```bash 
$ yum install redis  
$ systemctl start redis.service
$ redis 
127.0.0.1:6379>ping
PONG
```
如果返回PONG则代表正常

### Redis数据结构
string：可以是字符串、整数或者浮点数，对整数或者浮点型可以进行自增(increment)或者自减(decrement)
list:链表结构
set: 无序集合
hash: 键值对的无序集合
zset: 有序集合
### Redis中的字符串
get 获取存储在给定键中的值
set 设置存储在给定键中的值
del 删除存储在给定键中的值
```bash
$ redis-cli
127.0.0.1:6379>ping
PONG
127.0.0.1:6739>set hello world
OK
127.0.0.1:6739>get hello
"world"
127.0.0.1:6379>del hello
(integer)1
127.0.0.1:6379>get hello
(nil)
```
### Redis中的列表
push 将给定值推入链表 分为 lpush 和 rpush
lrange 获取链表在给定范围上的所有值
lindex 获取链表在给定位置的单个元素
pop  将链表一端弹出一个值 分为 lpop 和 rpop
```bash
127.0.0.1:6379> lpush list item 
(integer) 1
127.0.0.1:6379> rpush list item1 item2
(integer) 3
127.0.0.1:6379> lrange list 0 -1
1) "item"
2) "item1"
3) "item2"
127.0.0.1:6379> lindex list 1
"item1"
127.0.0.1:6379> lpop list 
"item"
127.0.0.1:6379> lrange list 0 -1
1) "item1"
2) "item2"
```
### Redis中的集合
sadd 将给定元素添加到集合
smembers 返回集合包含的所有元素
sismember 检查指定元素是否存在于集合中
srem 如果给定元素在集合中 那么移除这个元素
```bash
127.0.0.1:6379> sadd set item
(integer) 1
127.0.0.1:6379> sadd set item1 item2
(integer) 2
127.0.0.1:6379> smembers set
1) "item"
2) "item2"
3) "item1"
127.0.0.1:6379> sismember set item3
(integer) 0
127.0.0.1:6379> sismember set item
(integer) 1
127.0.0.1:6379> srem set item2
(integer) 1
127.0.0.1:6379> srem set item2
(intieger) 0
127.0.0.1:6379> smembers set
1) "item"
2) "item1"
```
### Redis的散列
hset 在散列里关联给定的键值对
hget 获取指定散列键的值
hgetall 获取散列包含的所有键值对
hdel 如果给定简直存在于散列里面，那么移除这个键
```bash
127.0.0.1:6379> hset hash sub-key1 value1
(integer) 1
127.0.0.1:6379> hset hash sub-key2 value2
(integer) 1
127.0.0.1:6379> hset hash sub-key1 value3
(integer) 0
127.0.0.1:6379> hgetall hash
1) "sub-key1"
2) "value1"
3) "sub-key2"
4) "value2"
127.0.0.1:6379> hdel hash sub-key1
(integer) 1
127.0.0.1:6379> hdel hash sub-key1
(integer) 0
127.0.0.1:6379> hget hash sub-key2
"value2"
127.0.0.1:6379> hgetall hash
1) "sub-key2"
2) "value2"
```
### Redis的有序集合
zadd 将一个带有给定分值的成员添加到有序集合里面
zrange 根据元素在有序排列中所处的位置，从有序集合里面获得多个元素
zrangebyscore 获取有序集合在给定分值范围内的所有元素
zrem 如果给定成员存在于有序集，那么移除这个成员
```bash
127.0.0.1:6379> zadd zset 728 member1
(integer) 1
127.0.0.1:6379> zadd zset 982 member2
(integer) 1
127.0.0.1:6379> zadd zset 728 member0
(integer) 1
127.0.0.1:6379> zrange zset 0 -1
1) "member1"
2) "member2"
3) "member0"
127.0.0.1:6379> zrange zset 0 -1 withscores
1) "member1"
2) "728"
3) "member2"
4) "982"
5) "member0"
6) "728"
127.0.0.1:6379> zrangebyscore zset 0 800
1) "member1"
2) "member0"
127.0.0.1:6379> zrem zset member1
(inieger) 1
127.0.0.1:6379> zrange zset 0 800
1) "member1"
2) "member0"
```