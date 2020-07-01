# 大表,大数据优化

## 1.sql语句优化

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