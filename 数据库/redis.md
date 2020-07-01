# Redis

## 1.Nosql 基本概念

为了解决高并发、高可用、高可扩展，大数据存储等一系列问题而产生的数据库解决方案，就是NoSql。

NoSql，叫非关系型数据库，它的全名Not only sql。它不能替代关系型数据库，只能作为关系型数据库的一个良好补充。

**Redis**和**MongoDB**是当前使用最广泛的NoSQL，而就Redis技术而言，它的性能十分优越，可以**支持每秒十几万此的读/写操作**，其性能远超数据库，并且还**支持集群、分布式、主从同步等**配置，原则上可以无限扩展，让更多的数据存储在内存中，更让人欣慰的是它还**支持一定的事务能力**，这保证了高并发的场景下数据的安全和一致性。



### Redis和Memcache的 区别

1 、Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，zset，hash等数据结构的存储。memcache支持简单的数据类型，String。

2 、Redis支持数据的备份，即master-slave模式的数据备份。

3 、Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用,而Memecache把数据全部存在内存之中

4、 redis的速度比memcached快很多

5、Memcached是多线程，非阻塞IO复用的网络模型；Redis使用单线程的IO复用模型。



![Redis与Memcached的区别与比较](https://user-gold-cdn.xitu.io/2018/4/18/162d7773080d4570?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### Redis 在 Java Web 中的应用

 `Redis` 是一个使用 `ANSI C` 编写的开源、支持 **网络**、基于 **内存**、**单线程模型**、**可选持久性** 的 **键值对存储数据库**。 

Redis 在 Java Web 主要有两个应用场景：

- 存储 **缓存** 用的数据；
- 需要高速读/写的场合**使用它快速读/写**；



## **2.redis数据结构 **

redis是一种高级的key:value存储系统，其中value支持五种数据类型：

#### 1. String

> **常用命令:** set,get,decr,incr,mget 等。

String数据结构是简单的key-value类型，value其实不仅可以是String，也可以是数字。 常规key-value缓存应用； 常规计数：微博数，粉丝数等。 string 就像是 java 中的 map 一样，**一个 key 对应一个 value** 

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Naw09lVL3GTpIRm1yTUAqajCwjtaLkWMaZIibCB9LzwkXB9VKYj0VIjPUFA2qSibWCMjTjib5iaHAJ9iaqj0MMlIxxA/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

~~~shell
127.0.0.1:6379> set hello world
OK
127.0.0.1:6379> get hello
"world"
~~~



#### 2.Hash

> **常用命令：** hget,hset,hgetall 等。

Hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。 比如我们可以Hash数据结构来存储用户信息，商品信息等等。

**理解：可以将 hash 看成一个 key - value 的集合。也可以将其想成一个 hash 对应着多个 string。**

**与 string 区别：string 是 一个 key - value 键值对，而 hash 是多个 key - value 键值对。**

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Naw09lVL3GTpIRm1yTUAqajCwjtaLkWMsvjnTMqwaVHLyb48B2kSdj0UaAudN8CJSPj4Fdfact7QseicKN1K19A/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

~~~shell
// hash-key 可以看成是一个键值对集合的名字,在这里分别为其添加了 sub-key1 : value1、
sub-key2 : value2、sub-key3 : value3 这三个键值对
127.0.0.1:6379> hset hash-key sub-key1 value1
(integer) 1
127.0.0.1:6379> hset hash-key sub-key2 value2
(integer) 1
127.0.0.1:6379> hset hash-key sub-key3 value3
(integer) 1
// 获取 hash-key 这个 hash 里面的所有键值对
127.0.0.1:6379> hgetall hash-key
1) "sub-key1"
2) "value1"
3) "sub-key2"
4) "value2"
5) "sub-key3"
6) "value3"
// 删除 hash-key 这个 hash 里面的 sub-key2 键值对
127.0.0.1:6379> hdel hash-key sub-key2
(integer) 1
127.0.0.1:6379> hget hash-key sub-key2
(nil)
127.0.0.1:6379> hget hash-key sub-key1
"value1"
127.0.0.1:6379> hgetall hash-key
1) "sub-key1"
2) "value1"
3) "sub-key3"
4) "value3"
~~~



**举个例子：** 最近做的一个电商网站项目的首页就使用了redis的hash数据结构进行缓存，因为一个网站的首页访问量是最大的，所以通常网站的首页可以通过redis缓存来提高性能和并发量。我用**jedis客户端**来连接和操作我搭建的redis集群或者单机redis，利用jedis可以很容易的对redis进行相关操作，总的来说从搭一个简单的集群到实现redis作为缓存的整个步骤不难。感兴趣的可以看我昨天写的这篇文章：

 **《一文轻松搞懂redis集群原理及搭建与使用》：** [juejin.im/post/5ad54d…](https://juejin.im/post/5ad54d76f265da23970759d3) 

#### 3.List

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Naw09lVL3GTpIRm1yTUAqajCwjtaLkWMK9vhfsBibcHFArO0V5VLTbWIhly26WkB34wsf6MBx0uTFHNgktMfMzg/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



~~~shell
127.0.0.1:6379> rpush list-key v1
(integer) 1
127.0.0.1:6379> rpush list-key v2
(integer) 2
127.0.0.1:6379> rpush list-key v1
(integer) 3
127.0.0.1:6379> lrange list-key 0 -1
1) "v1"
2) "v2"
3) "v1"
127.0.0.1:6379> lindex list-key 1
"v2"
127.0.0.1:6379> lpop list
(nil)
127.0.0.1:6379> lpop list-key
"v1"
127.0.0.1:6379> lrange list-key 0 -1
1) "v2"
2) "v1"
~~~



> **常用命令:** lpush,rpush,lpop,rpop,lrange等

list就是链表，Redis list的应用场景非常多，也是Redis最重要的数据结构之一，比如微博的关注列表，粉丝列表，最新消息排行等功能都可以用Redis的list结构来实现。

Redis list的实现为一个双向链表，即可以支持反向查找和遍历，更方便操作，不过带来了部分额外的内存开销。

#### 4.Set

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Naw09lVL3GTpIRm1yTUAqajCwjtaLkWMArjnt78jD5uVQ4av1lCghicMEkz5obrQ3xibmqoDpUb1q5KCScEcUC2g/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

```shell
127.0.0.1:6379> sadd k1 v1(integer) 1127.0.0.1:6379> sadd k1 v2(integer) 1127.0.0.1:6379> sadd k1 v3(integer) 1127.0.0.1:6379> sadd k1 v1(integer) 0127.0.0.1:6379> smembers k11) "v3"2) "v2"3) "v1"127.0.0.1:6379>127.0.0.1:6379> sismember k1 k4(integer) 0127.0.0.1:6379> sismember k1 v1(integer) 1127.0.0.1:6379> srem k1 v2(integer) 1127.0.0.1:6379> srem k1 v2(integer) 0127.0.0.1:6379> smembers k11) "v3"2) "v1"
```

> 
>
> 常用命令：** sadd,spop,smembers,sunion 等

 redis 的 set 是字符串类型的**无序集合**。集合是通过哈希表实现的，因此添加、删除、查找的复杂度都是 O（1）。 

set对外提供的功能与list类似是一个列表的功能，特殊之处在于set是可以自动**排重**的。 当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口，这个也是list所不能提供的。

在微博应用中，可以将一个用户所有的关注人存在一个集合中，将其所有粉丝存在一个集合。Redis可以非常方便的实现如共同关注、共同喜好、二度好友等功能。

#### 5.Sorted Set(Zset)

![img](https://mmbiz.qpic.cn/mmbiz_jpg/Naw09lVL3GTpIRm1yTUAqajCwjtaLkWMArjnt78jD5uVQ4av1lCghicMEkz5obrQ3xibmqoDpUb1q5KCScEcUC2g/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

> **常用命令：** zadd,zrange,zrem,zcard等

和set相比，sorted set增加了一个权重参数score，使得集合中的元素能够按score进行有序排列。

**举例：** 在直播系统中，实时排行信息包含直播间在线用户列表，各种礼物排行榜，弹幕消息（可以理解为按消息维度的消息排行榜）等信息，适合使用Redis中的SortedSet结构进行存储。

## 3.redis常见问题

### 缓存穿透

![redis-caching-penetration](https://github.com/doocs/advanced-java/raw/master/docs/high-concurrency/images/redis-caching-penetration.png)

缓存穿透是指查询一个一定不存在的数据，由于缓存是不命中时被动写的，并且出于容错考虑，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到存储层去查询，失去了缓存的意义。在流量大时，可能DB就挂掉了，要是有人利用不存在的key频繁攻击我们的应用，这就是漏洞。

**解决方案**
有很多种方法可以有效地解决缓存穿透问题，最常见的则是采用**布隆过滤器**，将所有可能存在的数据哈希到一个足够大的bitmap中，一个一定不存在的数据会被 这个bitmap拦截掉，从而避免了对底层存储系统的查询压力。

另外也有一个更为**简单粗暴**的方法（我们采用的就是这种），如果一个**查询返回的数据为空**（不管是数 据不存在，还是系统故障），我们仍然把这个空结果**进行缓存**，但它的过期时间会很短，最长不超过五分钟。

###  **缓存雪崩** 



缓存雪崩是指在我们设置缓存时采用了**相同的过期时间，导致缓存在某一时刻同时失效**，请求全部转发到DB，DB瞬时压力过重雪崩。

**解决方案**
缓存失效时的雪崩效应对底层系统的冲击非常可怕。大多数系统设计者考虑用加锁或者队列的方式保证缓存的单线 程（进程）写，从而避免失效时大量的并发请求落到底层存储系统上。这里分享一个简单方案就时讲缓存失效时间分散开，比如我们可以在**原有的失效时间基础上增加一个随机值**，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。

### 缓存击穿

对于一些设置了过期时间的key，如果这些key可能会在某些时间点被超高并发地访问，是一种非常“热点”的数据。这个时候，需要考虑一个问题：缓存被“击穿”的问题，这个和缓存雪崩的区别在于这里针对某一key缓存，前者则是很多key。

缓存在某个时间点过期的时候，恰好在这个时间点对这个Key有大量的并发请求过来，这些请求发现缓存过期一般都会从后端DB加载数据并回设到缓存，这个时候大并发的请求可能会瞬间把后端DB压垮。



- 若缓存的数据是基本不会发生更新的，则可尝试将该热点数据**设置为永不过期**。
- 若缓存的数据更新不频繁，且缓存刷新的整个流程耗时较少的情况下，则可以采用基于 Redis、zookeeper 等分布式中间件的**分布式互斥锁**，或者本地互斥锁以保证仅少量的请求能请求数据库并重新构建缓存，其余线程则在锁释放后能访问到新缓存。
- 若缓存的数据更新频繁或者在缓存刷新的流程耗时较长的情况下，可以利用定时线程在缓存过期前主动地重新构建缓存或者延后缓存的过期时间，以保证所有的请求能一直访问到对应的缓存。

1.使用**互斥锁**(mutex key)
业界比较常用的做法，是使用mutex。简单地来说，就是在缓存失效的时候（判断拿出来的值为空），不是立即去load db，而是先使用缓存工具的某些带成功操作返回值的操作（比如Redis的SETNX或者Memcache的ADD）去set一个mutex key，当操作返回成功时，再进行load db的操作并回设缓存；否则，就重试整个get缓存的方法。

SETNX，是「SET if Not eXists」的缩写，也就是只有不存在的时候才设置，可以利用它来实现锁的效果。在redis2.6.1之前版本未实现setnx的过期时间，所以这里给出两种版本代码参考：

~~~java
//2.6.1前单机版本锁
String get(String key) {  
   String value = redis.get(key);  
   if (value  == null) {  
    if (redis.setnx(key_mutex, "1")) {  
        // 3 min timeout to avoid mutex holder crash  
        redis.expire(key_mutex, 3 * 60)  
        value = db.get(key);  
        redis.set(key, value);  
        redis.delete(key_mutex);  
    } else {  
        //其他线程休息50毫秒后重试  
        Thread.sleep(50);  
        get(key);  
    }  
  }  


~~~

~~~java
public String get(key) {
      String value = redis.get(key);
      if (value == null) { //代表缓存值过期
          //设置3min的超时，防止del操作失败的时候，下次缓存过期一直不能load db
		  if (redis.setnx(key_mutex, 1, 3 * 60) == 1) {  //代表设置成功
               value = db.get(key);
                      redis.set(key, value, expire_secs);
                      redis.del(key_mutex);
            } else {  //这个时候代表同时候的其他线程已经load db并回设到缓存了，这时候重试获取缓存值即可
                      sleep(50);
                      get(key);  //重试
              }
          } else {
              return value;      
          }
 }
~~~



###  如何保证缓存与数据库的双写一致性 

**问题**

1.如果删除了缓存Redis，还没有来得及写库MySQL，另一个线程就来读取，发现缓存为空，则去数据库中读取数据写入缓存，此时缓存中为脏数据。

2.如果先写了库，在删除缓存前，写库的线程宕机了，没有删除掉缓存，则也会出现数据不一致情况。

**解决方案**

 **第一种方案：采用延时双删策略** 

**具体的步骤就是：**

1. 先删除缓存
2. 再写数据库
3. 休眠500毫秒
4. 再次删除缓存

**为什么是删除缓存，而不是更新缓存？**

原因很简单，很多时候，在复杂点的缓存场景，缓存不单单是数据库中直接取出来的值。

比如可能更新了某个表的一个字段，然后其对应的缓存，是需要查询另外两个表的数据并进行运算，才能计算出缓存最新的值的。

另外更新缓存的代价有时候是很高的。是不是说，每次修改数据库的时候，都一定要将其对应的缓存更新一份？也许有的场景是这样，但是对于**比较复杂的缓存数据计算的场景**，就不是这样了。如果你频繁修改一个缓存涉及的多个表，缓存也频繁更新。但是问题在于，**这个缓存到底会不会被频繁访问到？**

举个栗子，一个缓存涉及的表的字段，在 1 分钟内就修改了 20 次，或者是 100 次，那么缓存更新 20 次、100 次；但是这个缓存在 1 分钟内只被读取了 1 次，有**大量的冷数据**。实际上，如果你只是删除缓存的话，那么在 1 分钟内，这个缓存不过就重新计算一次而已，开销大幅度降低。**用到缓存才去算缓存。**

其实删除缓存，而不是更新缓存，就是一个 lazy 计算的思想，不要每次都重新做复杂的计算，不管它会不会用到，而是让它到需要被使用的时候再重新计算。像 mybatis，hibernate，都有懒加载思想。查询一个部门，部门带了一个员工的 list，没有必要说每次查询部门，都把里面的 1000 个员工的数据也同时查出来啊。80% 的情况，查这个部门，就只是要访问这个部门的信息就可以了。先查部门，同时要访问里面的员工，那么这个时候只有在你要访问里面的员工的时候，才会去数据库里面查询 1000 个员工。

### 

### Redis并发竞争key

 多客户端同时并发写一个key，一个key的值是1，本来按顺序修改为2,3,4，最后是4，但是顺序变成了4,3,2，最后变成了2。 

![img](https://user-gold-cdn.xitu.io/2019/5/19/16aceb58b476a1ea?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**1.整体技术方案**

这种情况，主要是准备一个分布式锁，大家去抢锁，抢到锁就做set操作。
加锁的目的实际上就是把并行读写改成串行读写的方式，从而来避免资源竞争。

**2.Redis分布式锁的实现**

主要用到的redis函数是setnx()

**用SETNX实现分布式锁**

利用SETNX非常简单地实现分布式锁。例如：某客户端要获得一个名字youzhi的锁，客户端使用下面的命令进行获取：
SETNX lock.youzhi<current Unix time + lock timeout + 1>

- 如返回1，则该客户端获得锁，把lock.youzhi的键值设置为时间值表示该键已被锁定，该客户端最后可以通过DEL lock.foo来释放该锁。
- 如返回0，表明该锁已被其他客户端取得，这时我们可以先返回或进行重试等对方完成或等待锁超时。

**2.时间戳**

由于上面举的例子，要求key的操作需要顺序执行，所以需要保存一个时间戳判断set顺序。

```
系统A key 1 {ValueA 7:00}
系统B key 1 { ValueB 7:05}
复制代码
```

假设系统B先抢到锁，将key1设置为{ValueB 7:05}。接下来系统A抢到锁，发现自己的key1的时间戳早于缓存中的时间戳（7:00<7:05），那就不做set操作了。

**3.什么是分布式锁**

因为传统的加锁的做法（如java的synchronized和Lock）这里没用，只适合单点。因为这是分布式环境，需要的是分布式锁。
当然，分布式锁可以基于很多种方式实现，比如zookeeper、redis等，不管哪种方式实现，基本原理是不变的：用一个状态值表示锁，对锁的占用和释放通过状态值来标识。

**03. 第二种方案：利用消息队列**

在并发量过大的情况下,可以通过消息中间件进行处理,把并行读写进行串行化。
把Redis.set操作放在队列中使其串行化,必须的一个一个执行。

这种方式在一些高并发的场景中算是一种通用的解决方案。

**2、第二种方案：异步更新缓存(基于订阅binlog的同步机制)**

​    **1.技术整体思路：**

MySQL binlog增量订阅消费+消息队列+增量数据更新到redis

- **读Redis**：热数据基本都在Redis
- **写MySQL**:增删改都是操作MySQL
- **更新Redis数据**：MySQ的数据操作binlog，来更新到Redis

​    **2.Redis更新**

**(1）数据操作主要分为两大块：**

- 一个是全量(将全部数据一次写入到redis)
- 一个是增量（实时更新）

这里说的是增量,指的是mysql的update、insert、delate变更数据。

**(2）读取binlog后分析 ，利用消息队列,推送更新各台的redis缓存数据。**

这样一旦MySQL中产生了新的写入、更新、删除等操作，就可以把binlog相关的消息推送至Redis，Redis再根据binlog中的记录，对Redis进行更新。

其实这种机制，很类似MySQL的主从备份机制，因为MySQL的主备也是通过binlog来实现的数据一致性。

这里可以结合使用canal(阿里的一款开源框架)，通过该框架可以对MySQL的binlog进行订阅，而canal正是模仿了mysql的slave数据库的备份请求，使得Redis的数据更新达到了相同的效果。

当然，这里的消息推送工具你也可以采用别的第三方：kafka、rabbitMQ等来实现推送更新Redis!

## 4.Redis集群

### 介绍

Redis 集群是一个可以在多个 Redis 节点之间进行数据共享的设施（installation）。

Redis 集群不支持那些需要同时处理多个键的 Redis 命令， 因为执行这些命令需要在多个 Redis 节点之间移动数据， 并且在高负载的情况下， 这些命令将降低 Redis 集群的性能， 并导致不可预测的错误。

Redis 集群通过分区（partition）来提供一定程度的可用性（availability）： 即使集群中有一部分节点失效或者无法进行通讯， 集群也可以继续处理命令请求。

Redis 集群提供了以下两个好处：

- 将数据自动切分（split）到多个节点的能力。
- 当集群中的一部分节点失效或者无法进行通讯时， 仍然可以继续处理命令请求的能力。

### 数据分片

Redis 集群使用数据分片（sharding）而非一致性哈希（consistency hashing）来实现： 一个 Redis 集群包含 16384 个哈希槽（hash slot）， 数据库中的每个键都属于这 16384 个哈希槽的其中一个， 集群使用公式 CRC16(key) % 16384 来计算键 key 属于哪个槽， 其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。

## 5.redis持久化

Redis为持久化提供了两种方式：

- **RDB文件**(快照)：在指定的时间间隔能对你的数据进行快照存储。
- **AOF**文件(追加式文件)：记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据。

### RDB(  快照,默认 )

#### 工作原理

- Redis调用fork()，产生一个子进程。
- 子进程把数据写到一个临时的RDB文件。
- 当子进程写完新的RDB文件后，把旧的RDB文件替换掉。

#### 优点

- RDB文件是一个很简洁的单文件，它保存了某个时间点的Redis数据，很适合用于做备份。你可以设定一个时间点对RDB文件进行归档，这样就能在需要的时候很轻易的把数据恢复到不同的版本。
- 基于上面所描述的特性，RDB很适合用于灾备。单文件很方便就能传输到远程的服务器上。
- RDB的性能很好，需要进行持久化时，主进程会fork一个子进程出来，然后把持久化的工作交给子进程，自己不会有相关的I/O操作。
- 比起AOF，在数据量比较大的情况下，RDB的启动速度更快。

#### 缺点

- RDB容易造成数据的丢失。假设每5分钟保存一次快照，如果Redis因为某些原因不能正常工作，那么从上次产生快照到Redis出现问题这段时间的数据就会丢失了。
- RDB使用`fork()`产生子进程进行数据的持久化，如果数据比较大的话可能就会花费点时间，造成Redis停止服务几毫秒。如果数据量很大且CPU性能不是很好的时候，停止服务的时间甚至会到1秒。

### AOF(追加式文件)

快照并不是很可靠。如果你的电脑突然宕机了，或者电源断了，又或者不小心杀掉了进程，那么最新的数据就会丢失。而AOF文件则提供了一种更为可靠的持久化方式。每当Redis接受到会修改数据集的命令时，就会把命令追加到AOF文件里，当你重启Redis时，AOF里的命令会被重新执行一次，重建数据。

#### 优点

- 比RDB可靠。你可以制定不同的fsync策略：不进行fsync、每秒fsync一次和每次查询进行fsync。默认是每秒fsync一次。这意味着你最多丢失一秒钟的数据。
- AOF日志文件是一个纯追加的文件。就算是遇到突然停电的情况，也不会出现日志的定位或者损坏问题。甚至如果因为某些原因（例如磁盘满了）命令只写了一半到日志文件里，我们也可以用`redis-check-aof`这个工具很简单的进行修复。
- 当AOF文件太大时，Redis会自动在后台进行重写。重写很安全，因为重写是在一个新的文件上进行，同时Redis会继续往旧的文件追加数据。新文件上会写入能重建当前数据集的最小操作命令的集合。当新文件重写完，Redis会把新旧文件进行切换，然后开始把数据写到新文件上。
- AOF把操作命令以简单易懂的格式一条接一条的保存在文件里，很容易导出来用于恢复数据。例如我们不小心用`FLUSHALL`命令把所有数据刷掉了，只要文件没有被重写，我们可以把服务停掉，把最后那条命令删掉，然后重启服务，这样就能把被刷掉的数据恢复回来。

#### 缺点

- 在相同的数据集下，AOF文件的大小一般会比RDB文件大。
- 在某些fsync策略下，AOF的速度会比RDB慢。通常fsync设置为每秒一次就能获得比较高的性能，而在禁止fsync的情况下速度可以达到RDB的水平。
- 在过去曾经发现一些很罕见的BUG导致使用AOF重建的数据跟原数据不一致的问题。s

## 6.分布式锁

1. 基于数据库实现分布式锁
2. 基于缓存，实现分布式锁，如redis
3. 基于Zookeeper实现分布式锁

### 基于数据库表的增删

基于数据库表增删是最简单的方式，首先创建一张锁的表主要包含下列字段：方法名，时间戳等字段。

具体使用的方法，当需要锁住某个方法时，往该表中插入一条相关的记录。这边需要注意，方法名是有唯一性约束的，如果有多个请求同时提交到数据库的话，数据库会保证只有一个操作可以成功，那么我们就可以认为操作成功的那个线程获得了该方法的锁，可以执行方法体内容。

执行完毕，需要delete该记录。

优化，如应用主从数据库，数据之间双向同步。一旦挂掉快速切换到备库上；做一个定时任务，每隔一定时间把数据库中的超时数据清理一遍；使用while循环，直到insert成功再返回成功，虽然并不推荐这样做；还可以记录当前获得锁的机器的主机信息和线程信息，那么下次再获取锁的时候先查询数据库，如果当前机器的主机信息和线程信息在数据库可以查到的话，直接把锁分配给他就可以了，实现可重入锁。

### 基于数据库排他锁

我们还可以通过数据库的排他锁来实现分布式锁。基于MySql的InnoDB引擎，可以使用以下方法来实现加锁操作：

```java
public void lock(){
    connection.setAutoCommit(false)
    int count = 0;
    while(count < 4){
        try{
            select * from lock where lock_name=xxx for update;
            if(结果不为空){
                //代表获取到锁
                return;
            }
        }catch(Exception e){
        }
        //为空或者抛异常的话都表示没有获取到锁
        sleep(1000);
        count++;
    }
    throw new LockException();
}
```

在查询语句后面增加for update，数据库会在查询过程中给数据库表增加排他锁。当某条记录被加上排他锁之后，其他线程无法再在该行记录上增加排他锁。其他没有获取到锁的就会阻塞在上述select语句上，可能的结果有2种，在超时之前获取到了锁，在超时之前仍未获取到锁。

获得排它锁的线程即可获得分布式锁，当获取到锁之后，可以执行方法的业务逻辑，执行完方法之后，释放锁`connection.commit()`。

存在的问题主要是性能不高和sql超时的异常。

### 基于数据库锁的优缺点

上面两种方式都是依赖数据库的一张表，一种是通过表中的记录的存在情况确定当前是否有锁存在，另外一种是通过数据库的排他锁来实现分布式锁。

- 优点是直接借助数据库，简单容易理解。
- 缺点是操作数据库需要一定的开销，性能问题需要考虑。

### 基于Zookeeper

基于zookeeper临时有序节点可以实现的分布式锁。每个客户端对某个方法加锁时，在zookeeper上的与该方法对应的指定节点的目录下，生成一个唯一的瞬时有序节点。 判断是否获取锁的方式很简单，只需要判断有序节点中序号最小的一个。 当释放锁的时候，只需将这个瞬时节点删除即可。同时，其可以避免服务宕机导致的锁无法释放，而产生的死锁问题。

提供的第三方库有[curator](https://curator.apache.org/)，具体使用读者可以自行去看一下。Curator提供的InterProcessMutex是分布式锁的实现。acquire方法获取锁，release方法释放锁。另外，锁释放、阻塞锁、可重入锁等问题都可以有有效解决。讲下阻塞锁的实现，客户端可以通过在ZK中创建顺序节点，并且在节点上绑定监听器，一旦节点有变化，Zookeeper会通知客户端，客户端可以检查自己创建的节点是不是当前所有节点中序号最小的，如果是就获取到锁，便可以执行业务逻辑。

最后，Zookeeper实现的分布式锁其实存在一个缺点，那就是性能上可能并没有缓存服务那么高。因为每次在创建锁和释放锁的过程中，都要动态创建、销毁瞬时节点来实现锁功能。ZK中创建和删除节点只能通过Leader服务器来执行，然后将数据同不到所有的Follower机器上。并发问题，可能存在网络抖动，客户端和ZK集群的session连接断了，zk集群以为客户端挂了，就会删除临时节点，这时候其他客户端就可以获取到分布式锁了。

### 基于redis的分布式锁实现

#### 1. 利用setnx+expire命令 (错误的做法)

Redis的SETNX命令，setnx key value，将key设置为value，当键不存在时，才能成功，若键存在，什么也不做，成功返回1，失败返回0 。 SETNX实际上就是SET IF NOT Exists的缩写

因为分布式锁还需要超时机制，所以我们利用expire命令来设置，所以利用setnx+expire命令的核心代码如下：

```java
public boolean tryLock(String key,String requset,int timeout) {
    Long result = jedis.setnx(key, requset);
    // result = 1时，设置成功，否则设置失败
    if (result == 1L) {
        return jedis.expire(key, timeout) == 1L;
    } else {
        return false;
    }
}
复制代码
```

实际上上面的步骤是有问题的，setnx和expire是分开的两步操作，**不具有原子性**，如果执行完第一条指令应用异常或者重启了，锁将无法过期。

#### 2.使用 set key value [EX seconds][PX milliseconds][NX|XX] 命令  (错误的做法)

Redis在 2.6.12 版本开始，为 SET 命令增加一系列选项：

```
SET key value[EX seconds][PX milliseconds][NX|XX]
复制代码
```

- EX seconds: 设定过期时间，单位为秒
- PX milliseconds: 设定过期时间，单位为毫秒
- NX: 仅当key不存在时设置值
- XX: 仅当key存在时设置值

set命令的nx选项，就等同于setnx命令，代码过程如下：

```
public boolean tryLock_with_set(String key, String UniqueId, int seconds) {
    return "OK".equals(jedis.set(key, UniqueId, "NX", "EX", seconds));
}
复制代码
```

value必须要具有唯一性，我们可以用UUID来做，设置随机字符串保证唯一性，至于为什么要保证唯一性？假如value不是随机字符串，而是一个固定值，那么就可能存在下面的问题：

- 1.客户端1获取锁成功
- 2.客户端1在某个操作上阻塞了太长时间
- 3.设置的key过期了，锁自动释放了
- 4.客户端2获取到了对应同一个资源的锁
- 5.客户端1从阻塞中恢复过来，因为value值一样，所以执行释放锁操作时就会释放掉客户端2持有的锁，这样就会造成问题

所以通常来说，在释放锁时，我们需要对value进行验证

问题: 如果**进程A**又不讲道理，操作锁内资源**超过笔者设置的超时时间**，那么就会导致**其他进程拿到锁**，等**进程A**回来了，回手就是把其他进程的锁删了，如图： 

![img](https://mmbiz.qpic.cn/mmbiz/R3InYSAIZkG5Nyw1nrIEibk6icobARemxScZAEia61Zb6ZI0lT0ukmNrzmCsTOsLEwyKIxVDiaekSvxbc6t9ic2CUUw/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

还是刚才那张图，将**T5**时刻改成了锁超时，被redis释放。

**进程B**在**T6**开开心心拿到锁不到一会，**进程A**操作完成，回手一个**del**，就把锁释放了。

当**进程B**操作完成，去释放锁的时候（图中T8时刻）没锁了(被A删了)



找不到锁其实还算好的，万一**T7**时刻有个**进程C**过来加锁成功，那么**进程B**就把**进程C**的锁释放了。以此类推，**进程C**可能释放**进程D**的锁，**进程D**....(禁止套娃)，具体什么后果就不得而知了。

所以在用**setnx**的时候，key虽然是主要作用，但是**value**也不能闲着，可以设置一个**唯一的客户端ID**，或者用**UUID**这种随机数。

当解锁的时候，先获取value判断是否是当前进程加的锁，再去删除。**伪代码**：

```java
String uuid = xxxx;
// 伪代码，具体实现看项目中用的连接工具
// 有的提供的方法名为set 有的叫setIfAbsent
set Test uuid NX PX 3000
try{
// biz handle....
} finally {
    // unlock
    if(uuid.equals(redisTool.get('Test')){
        redisTool.del('Test');
    }
}

```

这回看起来是不是稳了。

相反，这回的问题更明显了，在finally代码块中，**get和del并非原子操作**，还是有进程安全问题。

#### 3.基于lua脚本实现(正确)

 就是可以使用**lua**脚本，通过redis的**eval**/**evalsha**命令来运行 

~~~shell
-- lua删除锁：
-- KEYS和ARGV分别是以集合方式传入的参数，对应上文的Test和uuid。
-- 如果对应的value等于传入的uuid。
if redis.call('get', KEYS[1]) == ARGV[1]
    then
 -- 执行删除操作
        return redis.call('del', KEYS[1])
    else
 -- 不成功，返回0
        return 0
end

~~~

通过**lua**脚本能保证原子性的原因说的通俗一点：

**就算你在lua里写出花，执行也是一个命令(eval/evalsha)去执行的，一条命令没执行完，其他客户端是看不到的。**

那么既然这么麻烦，有没有比较好的工具呢？就要说到**redisson**了。

------

介绍**redisson**之前，笔者简单解释一下为什么现在的**setnx**默认是指**set**命令带上**nx**参数，而不是直接说是**setnx**这个命令。

因为redis版本在**2.6.12**之前，set是不支持nx参数的，如果想要完成一个锁，那么需要两条命令：

~~~shell
1. setnx Test uuid
2. expire Test 30
~~~

 即放入Key和设置有效期，是分开的两步，理论上会出现**1**刚执行完，程序挂掉，无法保证原子性。 



#### 4.redisson(正确)

 Redisson是**java的redis客户端之一** 

 Redisson普通的锁实现源码主要是**RedissonLock**这个类 

 源码中**加锁/释放锁**操作都是用**lua**脚本完成的，封装的非常完善，开箱即用。 

 **redLock** ( 红锁,redis官方提出的一种分布式锁的**算法** )

![img](https://mmbiz.qpic.cn/mmbiz/R3InYSAIZkG5Nyw1nrIEibk6icobARemxSJbicmaltAVwGHdmMBzD5m1Jf6EibiaD1uLzicj48ylGb72fAcvQgKlusyg/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

红锁算法认为，只要(N/2) + 1个节点加锁成功，那么就认为获取了锁， 解锁时将所有实例解锁。流程为：

1. 顺序向五个节点请求加锁
2. 根据一定的**超时时间**来推断是不是跳过该节点
3. 三个节点加锁成功并且花费时间小于锁的有效期
4. 认定加锁成功

 也就是说，假设锁**30秒**过期，三个节点加锁花了31秒，自然是加锁失败了。 

 这只是举个例子，实际上并不应该等每个节点那么长时间，就像官网所说的那样，假设有效期是10**秒**，那么单个redis实例操作超时时间，应该在5到50**毫秒**(注意时间单位) 

## 7.Redis  主从 哨兵  集群

### 主从

`Redis` **主从复制** 可将 **主节点** 数据同步给 **从节点**，从节点此时有两个作用：

1. 一旦 **主节点宕机**，**从节点** 作为 **主节点** 的 **备份** 可以随时顶上来。
2. 扩展 **主节点** 的 **读能力**，分担主节点读压力。

![img](https://user-gold-cdn.xitu.io/2018/8/22/16560ce61dbb9d7e?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**主从复制** 同时存在以下几个问题：

1. 一旦 **主节点宕机**，**从节点** 晋升成 **主节点**，同时需要修改 **应用方** 的 **主节点地址**，还需要命令所有 **从节点** 去 **复制** 新的主节点，整个过程需要 **人工干预**。
2. **主节点** 的 **写能力** 受到 **单机的限制**。
3. **主节点** 的 **存储能力** 受到 **单机的限制**。
4. **原生复制** 的弊端在早期的版本中也会比较突出，比如：`Redis` **复制中断** 后，**从节点** 会发起 `psync`。此时如果 **同步不成功**，则会进行 **全量同步**，**主库** 执行 **全量备份** 的同时，可能会造成毫秒或秒级的 **卡顿**。



**复制原理**
当从数据库启动时，会向主数据库发送sync命令，主数据库接收到sync后开始在后台保存快照rdb，在保存快照期间受到的命名缓存起来，当快照完成时，主数据库会将快照和缓存的命令一块发送给从。复制初始化结束。
之后，主每受到1个命令就同步发送给从。
当出现断开重连后，2.8之后的版本会将断线期间的命令传给重数据库。增量复制

主从复制是乐观复制，当客户端发送写执行给主，主执行完立即将结果返回客户端，并异步的把命令发送给从，从而不影响性能。也可以设置至少同步给多少个从主才可写。
无硬盘复制:如果硬盘效率低将会影响复制性能，2.8之后可以设置无硬盘复制，repl-diskless-sync yes

### 哨兵 ( Redis Sentinel)

![img](https://user-gold-cdn.xitu.io/2018/8/22/16560ce611d8c4a5?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

`Sentinel` 的主要功能包括 **主节点存活检测**、**主从运行情况检测**、**自动故障转移** （`failover`）、**主从切换**。`Redis` 的 `Sentinel` 最小配置是 **一主一从**。

`Redis` 的 `Sentinel` 系统可以用来管理多个 `Redis` 服务器，该系统可以执行以下四个任务：

- **监控**

`Sentinel` 会不断的检查 **主服务器** 和 **从服务器** 是否正常运行。

- **通知**

当被监控的某个 `Redis` 服务器出现问题，`Sentinel` 通过 `API` **脚本** 向 **管理员** 或者其他的 **应用程序** 发送通知。

- **自动故障转移**

当 **主节点** 不能正常工作时，`Sentinel` 会开始一次 **自动的** 故障转移操作，它会将与 **失效主节点** 是 **主从关系** 的其中一个 **从节点** 升级为新的 **主节点**，并且将其他的 **从节点** 指向 **新的主节点**。

- **配置提供者**

在 `Redis Sentinel` 模式下，**客户端应用** 在初始化时连接的是 `Sentinel` **节点集合**，从中获取 **主节点** 的信息。







### 集群

#### 客户端分区方案

**客户端** 就已经决定数据会被 **存储** 到哪个 `redis` 节点或者从哪个 `redis` 节点 **读取数据**。其主要思想是采用 **哈希算法** 将 `Redis` 数据的 `key` 进行散列，通过 `hash` 函数，特定的 `key`会 **映射** 到特定的 `Redis` 节点上。

![img](https://user-gold-cdn.xitu.io/2018/9/4/165a4f9e74a09b36?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**客户端分区方案** 的代表为 `Redis Sharding`，`Redis Sharding` 是 `Redis Cluster` 出来之前，业界普遍使用的 `Redis` **多实例集群** 方法。`Java` 的 `Redis` 客户端驱动库 `Jedis`，支持 `Redis Sharding` 功能，即 `ShardedJedis` 以及 **结合缓存池** 的 `ShardedJedisPool`。

- **优点**

不使用 **第三方中间件**，**分区逻辑** 可控，**配置** 简单，节点之间无关联，容易 **线性扩展**，灵活性强。

- **缺点**

**客户端** 无法 **动态增删** 服务节点，客户端需要自行维护 **分发逻辑**，客户端之间 **无连接共享**，会造成 **连接浪费**。

#### 代理分区方案

**客户端** 发送请求到一个 **代理组件**，**代理** 解析 **客户端** 的数据，并将请求转发至正确的节点，最后将结果回复给客户端。

- **优点**：简化 **客户端** 的分布式逻辑，**客户端** 透明接入，切换成本低，代理的 **转发** 和 **存储** 分离。
- **缺点**：多了一层 **代理层**，加重了 **架构部署复杂度** 和 **性能损耗**。

![img](https://user-gold-cdn.xitu.io/2018/9/4/165a4f9e6f8b3a44?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



 **代理分区** 主流实现的有方案有 `Twemproxy` 和 `Codis`。 

#### Twemproxy

`Twemproxy` 也叫 `nutcraker`，是 `twitter` 开源的一个 `redis` 和 `memcache` 的 **中间代理服务器** 程序。`Twemproxy` 作为 **代理**，可接受来自多个程序的访问，按照 **路由规则**，转发给后台的各个 `Redis` 服务器，再原路返回。`Twemproxy` 存在 **单点故障** 问题，需要结合 `Lvs` 和 `Keepalived` 做 **高可用方案**。

![img](https://user-gold-cdn.xitu.io/2018/9/4/165a4f9e751d0773?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- **优点**：应用范围广，稳定性较高，中间代理层 **高可用**。
- **缺点**：无法平滑地 **水平扩容/缩容**，无 **可视化管理界面**，运维不友好，出现故障，不能 **自动转移**。

#### Codis

`Codis` 是一个 **分布式** `Redis` 解决方案，对于上层应用来说，连接 `Codis-Proxy` 和直接连接 **原生的** `Redis-Server` 没有的区别。`Codis` 底层会 **处理请求的转发**，不停机的进行 **数据迁移** 等工作。`Codis` 采用了无状态的 **代理层**，对于 **客户端** 来说，一切都是透明的。

![img](https://user-gold-cdn.xitu.io/2018/9/4/165a4f9e7509b300?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- **优点**

实现了上层 `Proxy` 和底层 `Redis` 的 **高可用**，**数据分片** 和 **自动平衡**，提供 **命令行接口** 和 `RESTful API`，提供 **监控** 和 **管理** 界面，可以动态 **添加** 和 **删除** `Redis` 节点。

- **缺点**

**部署架构** 和 **配置** 复杂，不支持 **跨机房** 和 **多租户**，不支持 **鉴权管理**。







