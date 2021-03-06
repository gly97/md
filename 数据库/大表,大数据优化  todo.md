# 大表,大数据优化

## 1.sql语句优化



sql查询过程（Mysql8及之后移除了缓存）

 ![mysql_sql_execute](https://user-gold-cdn.xitu.io/2020/6/15/172b834c40364b80?imageView2/0/w/1280/h/960/format/webp/ignore-error/1) 

MySQL查询过程如下：

- 客户端发送一条查询给服务器。
- 服务器先检查查询缓存，如果命中了缓存，则立刻返回存储在缓存中的结果。否则进入下一阶段。
- 服务器端进行SQL解析、预处理，再由优化器生成对应的执行计划。
- MySQL根据优化器生成的执行计划，再调用存储引擎的API来执行查询。
- 将结果返回给客户端。

## SQL 语法顺序：

```sql
SELECT [DISTINCT]
FROM
WHERE
GROUP BY
HAVING
UNION
ORDER BY
LIMIT/ROWNUM
```

### 关键字explain

![img](https://user-gold-cdn.xitu.io/2019/3/17/1698a4aa45721263?imageslim)

- id: 查询的唯一标识

- select_type: 查询的类型

- table: 查询的表, 可能是数据库中的表/视图，也可能是 FROM 中的子查询

- type: 搜索数据的方法

   ALL< index< range< ref< eq_ref< const< system< NULL 

  -  **ALL：Full Table Scan， MySQL将遍历全表以找到匹配的行** 
  -  **index：Full Index Scan，index与ALL区别为index类型只遍历索引树** 
  -  **range:索引范围扫描，对索引的扫描开始于某一点，返回匹配值域的行。** 
  -  **ref：使用非唯一索引扫描或者唯一索引的前缀扫描，返回匹配某个单独值的记录行** 
  -  **eq_ref：类似ref，区别就在使用的索引是唯一索引，对于每个索引键值，表中只有一条记录匹配，简单来说，就是多表连接中使用primary key或者 unique key作为关联条件** 
  - **const 表示通过索引一次就找到了** 
  -  **表只有一行记录（等于系统表），这是const类型的特列，平时不大会出现，可以忽略** 
  -  **MySQL能够在优化阶段分解查询语句，在执行阶段用不着再访问表或索引** 

- possible_keys: 可能使用的索引(不一定使用)

- key: 最终决定要使用的key(实际使用的索引, 如果为NULL，则没有使用索引 )

- key_len: 查询索引使用的字节数。通常越少越好

- ref:   显示索引的哪一列被使用了，如果可能的话，是一个常数，哪些列或常量被用于查找索引列上的值 

- rows:   根据表统计信息及索引选用情况，大致估算出找到所需的记录所需读取的行数 ，估计值。通常越少越好

- extra: 额外的信息

### sql语句

- 查询语句无论是使用哪种判断条件 **等于、小于、大于**， WHERE 左侧的条件查询字段不要使用函数或者表达式
- 使用 **EXPLAIN**命令优化你的 SELECT 查询，对于复杂、效率低的 sql 语句，我们通常是使用 explain sql 来分析这条 sql 语句，这样方便我们分析，进行优化。
- 当你的 SELECT 查询语句只需要使用一条记录时，要使用 LIMIT 1
- 不要直接使用 **SELECT ***，而应该使用具体需要查询的表字段，因为使用 EXPLAIN 进行分析时，SELECT * 使用的是全表扫描，也就是 type = all。
- 为每一张表设置一个 ID 属性(主键索引)
- 避免在 WHERE中使用! = 或 <> 操作符(将导致引擎放弃使用索引而进行全表扫描 )
-  应尽量避免在 where 子句中使用 or 来连接条件，如果一个字段有索引，一个字段没有索引，将导致引擎放弃使用索引而进行全表扫描 
- 使用 BETWEEN AND 替代 IN( exists 代替 in  )
- 为搜索字段创建索引
- 选择正确的存储引擎，InnoDB 、MyISAM 、MEMORY 等
- 使用 LIKE %abc 不会走索引，而使用 LIKE abc%会走索引
- 对于枚举类型的字段(即有固定罗列值的字段)，建议使用ENUM而不是VARCHAR，如性别、星期、类型、类别等
- 拆分大的 DELETE 或 INSERT 语句
- 选择合适的字段类型，选择标准是 **尽可能小、尽可能定长、尽可能使用整数**。
-  最好不要给数据库留NULL，尽可能的使用 NOT NULL填充数据库. ( where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描 )
-  避免在 where 子句中对字段进行表达式操作/ 函数操作  (select id from t where num/2 = 100  会导致放弃索引)
-  尽可能的使用 varchar/nvarchar 代替 char/nchar ，因为首先变长字段存储空间小， 

## 2.使用索引

~~~sql
代码示例：

-- 创建书籍表

create table tb_book

(
-- 创建主键索引
id int primary key,

-- 创建唯一索引
title varchar(100) unique,

-- 普通索引
index ix_title (title),

-- 全文索引
fulltext index ix_content(content),

-- 组合索引
index ix_title_author(title,author)

);

-- 建表后添加主键索引
ALTER TABLE tb_book ADD PRIMARY KEY pk_id(id)；

-- 建表后添加唯一索引
ALTER TABLE tb_book ADD UNIQUE index ix_title(title)；

-- 建表后添加全文索引
ALTER TABLE tb_book ADD FULLTEXT index ix_content(content)；

-- 查询时使用全文索引
SELECT * FROM tb_book MATCH(content) ANGAINST(‘胜利’);

-- 建表后添加组合索引
ALTER TABLE tb_book ADD INDEX ix_book(title,author)；
~~~

**什么情况下索引会失效？**

- 模糊查询时，使用like ‘%张%’会失效，而like ‘张%’不会
- 使用is null或is not null查询时
- 使用组合索引时，某个字段为null
- 使用or查询多个条件时
- 在函数中使用字段时，如where year(time) = 2019

**索引的结构**

不同的存储引擎使用不同结构的索引：

聚簇索引，InnoDB支持，索引的顺序和数据的物理顺序一致，类似新华字典中的拼音目录排列和汉字排列顺序一致，聚簇索引一个表中只能有一个。

非聚簇索引，MyISAM支持，索引顺序和数据的物理顺序不一致，类似新华字典中的偏旁部首目录和汉字排列顺序不一致，非聚簇索引表可以有多个。

索引的数据结构主要是：BTree和B+Tree

BTree的数据结构如下，是一种平衡搜索多叉树，每个节点由key和data组成，key是索引的键，data是键对应的数据，在节点的两边是两个指针，指向另外的索引位置，而所有的键都是排序过的，这样在搜索索引时，可以使用二分查找，速度比较快，时间复杂度是h*log(n)，h是树的高度，BTree是一种比较高效的搜索结构。

![img](https://user-gold-cdn.xitu.io/2019/6/14/16b54d0c52fa2e20?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

B+Tree的数据结构如下，是BTree的升级版，区别是**非叶子节点不在存储具体的数据，只保存索引的键**，数据保存到叶子节点中，并且叶子节点中没有指针只有键和数据。B+Tree的优点是：搜索效率更高，因为非叶子节点中没有保存数据，就可以保存更多的键，每一层的键越多，树的高度就会减少，这样查询速度就会提升。![img](https://user-gold-cdn.xitu.io/2019/6/14/16b54d0c3f4c17c0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

## 3.引入缓存数据库

## 4.分库分表

当MySQL单表记录数过大时，数据库的CRUD性能会明显下降，一些常见的优化措施如下：

#### 1. 限定数据的范围

务必禁止不带任何限制数据范围条件的查询语句。比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内；

#### 2. 读/写分离

经典的数据库拆分方案，主库负责写，从库负责读；

#### 3. 垂直分区

**根据数据库里面数据表的相关性进行拆分。** 例如，用户表中既有用户的登录信息又有用户的基本信息，可以将用户表拆分成两个单独的表，甚至放到单独的库做分库。

**简单来说垂直拆分是指数据表列的拆分，把一张列比较多的表拆分为多张表。** 如下图所示，这样来说大家应该就更容易理解了。 [![数据库垂直分区](https://camo.githubusercontent.com/3045b900b49be903108147297e99499ee4168b94/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545362539352542302545362538442541452545352542412539332545352539452538322545372539422542342545352538382538362545352538432542412e706e67)](https://camo.githubusercontent.com/3045b900b49be903108147297e99499ee4168b94/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545362539352542302545362538442541452545352542412539332545352539452538322545372539422542342545352538382538362545352538432542412e706e67)

- **垂直拆分的优点：** 可以使得列数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区可以简化表的结构，易于维护。
- **垂直拆分的缺点：** 主键会出现冗余，需要管理冗余列，并会引起Join操作，可以通过在应用层进行Join来解决。此外，垂直分区会让事务变得更加复杂；

#### 4. 水平分区

**保持数据表结构不变，通过某种策略存储数据分片。这样每一片数据分散到不同的表或者库中，达到了分布式的目的。 水平拆分可以支撑非常大的数据量。**

水平拆分是指数据表行的拆分，表的行数超过200万行时，就会变慢，这时可以把一张的表的数据拆成多张表来存放。举个例子：我们可以将用户信息表拆分成多个用户信息表，这样就可以避免单一表数据量过大对性能造成影响。

[![数据库水平拆分](https://camo.githubusercontent.com/3e93efc1212224996fe7d54a89a4894ab99c853c/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545362539352542302545362538442541452545352542412539332545362542302542342545352542392542332545362538422538362545352538382538362e706e67)](https://camo.githubusercontent.com/3e93efc1212224996fe7d54a89a4894ab99c853c/68747470733a2f2f6d792d626c6f672d746f2d7573652e6f73732d636e2d6265696a696e672e616c6979756e63732e636f6d2f323031392d362f2545362539352542302545362538442541452545352542412539332545362542302542342545352542392542332545362538422538362545352538382538362e706e67)

水平拆分可以支持非常大的数据量。需要注意的一点是：分表仅仅是解决了单一表数据过大的问题，但由于表的数据还是在同一台机器上，其实对于提升MySQL并发能力没有什么意义，所以 **水平拆分最好分库** 。

水平拆分能够 **支持非常大的数据量存储，应用端改造也少**，但 **分片事务难以解决** ，跨节点Join性能较差，逻辑复杂。《Java工程师修炼之道》的作者推荐 **尽量不要对数据进行分片，因为拆分会带来逻辑、部署、运维的各种复杂度** ，一般的数据表在优化得当的情况下支撑千万以下的数据量是没有太大问题的。如果实在要分片，尽量选择客户端分片架构，这样可以减少一次和中间件的网络I/O。

**下面补充一下数据库分片的两种常见方案：**

- **客户端代理：** **分片逻辑在应用端，封装在jar包中，通过修改或者封装JDBC层来实现。** 当当网的 **Sharding-JDBC** 、阿里的TDDL是两种比较常用的实现。
- **中间件代理：** **在应用和数据中间加了一个代理层。分片逻辑统一维护在中间件服务中。** 我们现在谈的 **Mycat** 、360的Atlas、网易的DDB等等都是这种架构的实现。

详细内容可以参考： MySQL大表优化方案: https://segmentfault.com/a/1190000006158186

### 分库分表之后,id 主键如何处理？

因为要是分成多个表之后，每个表都是从 1 开始累加，这样是不对的，我们需要一个全局唯一的 id 来支持。

生成全局 id 有下面这几种方式：

1. - **UUID**：不适合作为主键，因为太长了，并且无序不可读，查询效率低。比较适合用于生成唯一的名字的标示比如文件的名字。

   - **数据库自增 id** : 两台数据库分别设置不同步长，生成不重复ID的策略来实现高可用。这种方式生成的 id 有序，但是需要独立部署数据库实例，成本高，还会有性能瓶颈。

   - **利用 redis 生成 id :** 性能比较好，灵活方便，不依赖于数据库。但是，引入了新的组件造成系统更加复杂，可用性降低，编码更加复杂，增加了系统成本。

   - **Twitter的snowflake算法** ：Github 地址：https://github.com/twitter-archive/snowflake。

   - **美团的[Leaf](https://tech.meituan.com/2017/04/21/mt-leaf.html)分布式ID生成系统** ：Leaf 是美团开源的分布式ID生成器，能保证全局唯一性、趋势递增、单调递增、信息安全，里面也提到了几种分布式方案的对比，但也需要依赖关系数据库、Zookeeper等中间件。感觉还不错。美团技术团队的一篇文章：https://tech.meituan.com/2017/04/21/mt-leaf.html 。

     