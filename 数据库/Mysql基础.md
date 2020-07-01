



# 1. 数据库常用知识及安装

基于MYsql

## 1.1数据库常用知识

数据库的英文单词： DataBase 简称 ： DB

什么数据库？


用于存储和管理数据的仓库。

数据库的特点：
1. 持久化存储数据的。其实数据库就是一个文件系统
2. 方便存储和管理数据
3. 使用了统一的方式操作数据库 -- SQLs

NOsql(not  only sql)

### 1.2三大范式

#### **第一范式（1NF）**

所谓第一范式（1NF）是指在关系模型中，对列添加的一个规范要求，所有的列都应该是原子性的，即**数据库表的每一列都是不可分割的原子数据项**，而不能是集合，数组，记录等非原子数据项。即实体中的某个属性有多个值时，必须拆分为不同的属性。

#### **第二范式（2NF）**

在1NF的基础上，非Key属性必须完全依赖于主键。**第二范式（2NF）是在第一范式（1NF）的基础上建立起来的**，即满足第二范式（2NF）必须先满足第一范式（1NF）。第二范式（2NF）要求数据库表中的每个实例或记录必须可以被唯一地区分。选取一个能区分每个实体的属性或属性组，作为实体的唯一标识。

第二范式（2NF）要求**实体的属性完全依赖于主关键字**。所谓完全依赖是指不能存在仅依赖主关键字一部分的属性，如果存在，那么这个属性和主关键字的这一部分应该分离出来形成一个新的实体，新实体与原实体之间是一对多的关系。为实现区分通常需要为表加上一个列，以存储各个实例的唯一标识。简而言之，第二范式就是在第一范式的基础上属性完全依赖于主键。

#### **第三范式（3NF）**

第三范式是在第二范式基础上，更进一层，第三范式的目标就是确保**表中各列与主键列直接相关**，而不是间接相关。即各列与主键列都是一种直接依赖关系，则满足第三范式。

第三范式要求各列与主键列直接相关，我们可以这样理解，假设张三是李四的兵，王五则是张三的兵，这时王五是不是李四的兵呢?从这个关系中我们可以看出，王五也是李四的兵，因为王五依赖于张三，而张三是李四的兵，所以王五也是。这中间就存在一种间接依赖的关系而非我们第三范式中强调的直接依赖。



##  1.3MySQL的安装

 推荐mysql8.0之后的免安装包,**记得修改时区**在my.ini中或者JDBC/database.properties连接数据库时指定时区.(以及无法使用版本太低的MySQL驱动)

- 下载太慢

参见   https://blog.csdn.net/zq019/article/details/78780202 

在MySQL的下载界面下载时,右键NO,thanks...  复制链接到迅雷等下载

- 安装流程

 https://www.jianshu.com/p/66507f8be32e

再解压后的bin目录下,即根目录新建my.ini

~~~sql
[mysqld]

#设置3306端口

port=3306

#设置mysql的安装目录

basedir=D:\\Sql Server\\mysql-8.0.12-winx64 

 # 切记此处一定要用双斜杠\\，单斜杠这里会出错。

#设置mysql数据库的数据的存放目录

datadir=D:\\Sql Server\\mysql-8.0.12-winx64\\Data 

 # 此处同上

#允许最大连接数

max_connections=200

#允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统max_connect_errors=10

#服务端使用的字符集默认为

UTF8character-set-server=utf8

#创建新表时将使用的默认存储引擎

default-storage-engine=INNODB

#默认使用“mysql_native_password”插件认证default_authentication_plugin=mysql_native_password

[mysql]

#设置mysql客户端默认字符集

default-character-set=utf8[client]

#设置mysql客户端连接服务端时默认使用的端口

port=3306default-character-set=utf8


#设置时区   重要   idea清理缓存
default-time_zone = '+8:00'

~~~

初始化mysql以管理员身份运行cmd命令，并将路径换到mysql的bin目录下运行命令：

~~~shell
mysqld --initialize --console
~~~



从你得到的数据找到下面这行 

~~~
 A temporary password is generated for root@localhost: rI5rvf5x5G
~~~

 然后将@localhost:后面的字段记录下来，这是随机密码



- 安装错误

如果没有得到密码出现错误，请看下面的错误指南。

安装服务，允许命令：

~~~shell
mysqld --install 
~~~

运行mysql,运行cmd,在bin目录下运行命令

~~~shell
net start mysql （如果服务无法启动就看错误指南）
~~~

~~~shell
mysql -u root -p 
~~~

输入上边记录的密码。 

修改mysql密码修改密码的命令

 ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

- 错误指南：	如果在修改密码或者初始化过程中出现问题。运行cmd。在bin目录下运行

net stop mysql 

mysqld remove

然后删除掉根目录的data文件夹重新从第三步开始操作

注:之前安装过MySQL的情况下,需卸载MySQL,删除注册表 参见

 https://jingyan.baidu.com/article/f96699bbaa8fc1894f3c1b5a.html 

## 1.4 常见问题

时区错误



记得重启MySQL服务,刷新idea缓存

# 2. SQL分类

1) DDL(Data Definition Language)数据定义语言
	用来**定义数据库对象**：数据库，表，列等。关键字：create, drop,alter 等
2) DML(Data Manipulation Language)数据操作语言
	用来**对数据库中表的数据进行增删改**。关键字：insert, delete, update 等
3) DQL(Data Query Language)数据查询语言(主要)
	用来**查询**数据库中表的记录(数据)。关键字：select, where 等
4) DCL(Data Control Language)数据控制语言(了解)
	用来定义**数据库的访问权限和安全级别**，及创建用户。关键字：GRANT， REVOKE 等

## 2.1 DDL



###  2.1.1操作数据库

~~~sql
	1. C(Create):创建
		* 创建数据库：
			* create database 数据库名称;
		* 创建数据库，判断不存在，再创建：
			* create database if not exists 数据库名称;
		* 创建数据库，并指定字符集
			* create database 数据库名称 character set 字符集名;

		* 练习： 创建db4数据库，判断是否存在，并制定字符集为gbk
			* create database if not exists db4 character set gbk;
	2. R(Retrieve)：查询
		* 查询所有数据库的名称:
			* show databases;
		* 查询某个数据库的字符集:查询某个数据库的创建语句
			* show create database 数据库名称;
	3. U(Update):修改
		* 修改数据库的字符集
			* alter database 数据库名称 character set 字符集名称;
	4. D(Delete):删除
		* 删除数据库
			* drop database 数据库名称;
		* 判断数据库存在，存在再删除
			* drop database if exists 数据库名称;
	5. 使用数据库
		* 查询当前正在使用的数据库名称
			* select database();
		* 使用数据库
			* use 数据库名称;
			
			
~~~

### 2.1.2操作表

~~~sql
	1. C(Create):创建
		1. 语法：
			create table 表名(
				列名1 数据类型1,
				列名2 数据类型2,
				....
				列名n 数据类型n
			);
			* 注意：最后一列，不需要加逗号（,）
			* 数据库类型：
				1. int：整数类型
					* age int,
				2. double:小数类型
					* score double(5,2)
				3. date:日期，只包含年月日，yyyy-MM-dd
				4. datetime:日期，包含年月日时分秒	 yyyy-MM-dd HH:mm:ss
				5. timestamp:时间错类型	包含年月日时分秒	 yyyy-MM-dd HH:mm:ss	
					* 如果将来不给这个字段赋值，或赋值为null，则默认使用当前的系统时间，来自动赋值

				6. varchar：字符串
					* name varchar(20):姓名最大20个字符
					* zhangsan 8个字符  张三 2个字符
					
					
		* 创建表
			create table student(
				id int,
				name varchar(32),
				age int ,
				score double(4,1),
				birthday date,
				insert_time timestamp
			);
		* 复制表：
			* create table 表名 like 被复制的表名;	  	
	2. R(Retrieve)：查询
		* 查询某个数据库中所有的表名称
			* show tables;
		* 查询表结构
			* desc 表名;
	3. U(Update):修改
		1. 修改表名
			alter table 表名 rename to 新的表名;
		2. 修改表的字符集
			alter table 表名 character set 字符集名称;
		3. 添加一列
			alter table 表名 add 列名 数据类型;
		4. 修改列名称 类型
			alter table 表名 change 列名 新列别 新数据类型;
			alter table 表名 modify 列名 新数据类型;
		5. 删除列
			alter table 表名 drop 列名;
	4. D(Delete):删除
		* drop table 表名;
		* drop table  if exists 表名 ;			
~~~

## 2.2 DML：增删改表中数据

~~~sql
1. 添加数据：
	* 语法：
		* insert into 表名(列名1,列名2,...列名n) values(值1,值2,...值n);
	* 注意：
		1. 列名和值要一一对应。
		2. 如果表名后，不定义列名，则默认给所有列添加值
			insert into 表名 values(值1,值2,...值n);
		3. 除了数字类型，其他类型需要使用引号(单双都可以)引起来
2. 删除数据：
	* 语法：
		* delete from 表名 [where 条件]
	* 注意：
		1. 如果不加条件，则删除表中所有记录。
		2. 如果要删除所有记录
			1. delete from 表名; -- 不推荐使用。有多少条记录就会执行多少次删除操作
			2. TRUNCATE TABLE 表名; -- 推荐使用，效率更高 先删除表，然后再创建一张一样的表。
3. 修改数据：
	* 语法：
		* update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件];

	* 注意：
		1. 如果不加任何条件，则会将表中所有记录全部修改。
~~~

## 2.3 DQL：查询表中的记录

~~~sql
* select * from 表名;

1. 语法：
	select
		字段列表
	from
		表名列表
	where
		条件列表
	group by
		分组字段
	having
		分组之后的条件
	order by
		排序
	limit
		分页限定
		
2. 基础查询
	1. 多个字段的查询
		select 字段名1，字段名2... from 表名；
		* 注意：
			* 如果查询所有字段，则可以使用*来替代字段列表。
	2. 去除重复：
		* distinct
	3. 计算列
		* 一般可以使用四则运算计算一些列的值。（一般只会进行数值型的计算）
		* ifnull(表达式1,表达式2)：null参与的运算，计算结果都为null
			* 表达式1：哪个字段需要判断是否为null
			* 如果该字段为null后的替换值。
	4. 起别名：
		* as：as也可以省略
		
3. 条件查询
	1. where子句后跟条件
	2. 运算符
		* > 、< 、<= 、>= 、= 、<>
		* BETWEEN...AND  
		* IN( 集合) 
		* LIKE：模糊查询
			* 占位符：
				* _:单个任意字符
				* %：多个任意字符
		* IS NULL  
		* and  或 &&
		* or  或 || 
		* not  或 !
		
			-- 查询年龄大于20岁

			SELECT * FROM student WHERE age > 20;
		-- 查询年龄不等于20岁
		
			SELECT * FROM student WHERE age <> 20;
			
			-- 查询年龄大于等于20 小于等于30
			
			SELECT * FROM student WHERE age >= 20 &&  age <=30;
			SELECT * FROM student WHERE age >= 20 AND  age <=30;
			SELECT * FROM student WHERE age BETWEEN 20 AND 30;
			
			-- 查询年龄22岁，18岁，25岁的信息
			SELECT * FROM student WHERE age = 22 OR age = 18 OR age = 25
			SELECT * FROM student WHERE age IN (22,18,25);
			
			-- 查询英语成绩为null
			
			SELECT * FROM student WHERE english IS NULL;
			
			-- 查询英语成绩不为null
			SELECT * FROM student WHERE english  IS NOT NULL;
			
			-- 查询姓马的有哪些？ like
			SELECT * FROM student WHERE NAME LIKE '马%';
			-- 查询姓名第二个字是化的人
			
			SELECT * FROM student WHERE NAME LIKE "_化%";
			
			-- 查询姓名是3个字的人
			SELECT * FROM student WHERE NAME LIKE '___';
			
		    -- 查询姓名中包含德的人
			SELECT * FROM student WHERE NAME LIKE '%德%';
~~~

## 3.3 DCL(管理用户,修改密码)

~~~sql
	1. 管理用户
		1. 添加用户：
			* 语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
		2. 删除用户：
			* 语法：DROP USER '用户名'@'主机名';
		3. 修改用户密码：
			
			UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
			UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lizi';
			
			SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
			SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');

			* mysql中忘记了root用户的密码？
				1. cmd -- > net stop mysql 停止mysql服务
					* 需要管理员运行该cmd

				2. 使用无验证方式启动mysql服务： mysqld --skip-grant-tables
				3. 打开新的cmd窗口,直接输入mysql命令，敲回车。就可以登录成功
				4. use mysql;
				5. update user set password = password('你的新密码') where user = 'root';
				6. 关闭两个窗口
				7. 打开任务管理器，手动结束mysqld.exe 的进程
				8. 启动mysql服务
				9. 使用新密码登录。
		4. 查询用户：
			-- 1. 切换到mysql数据库
			USE myql;
			-- 2. 查询user表
			SELECT * FROM USER;
			
			* 通配符： % 表示可以在任意主机使用用户登录数据库

	2. 权限管理：
		1. 查询权限：
			-- 查询权限
			SHOW GRANTS FOR '用户名'@'主机名';
			SHOW GRANTS FOR 'lisi'@'%';

		2. 授予权限：
			-- 授予权限
			grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
			-- 给张三用户授予所有权限，在任意数据库任意表上
			
			GRANT ALL ON *.* TO 'zhangsan'@'localhost';
		3. 撤销权限：
			-- 撤销权限：
			revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
			REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';
~~~

# 3 数据类型

| 分类     | 类型名称                  | 类型说明                                                     |
| -------- | ------------------------- | ------------------------------------------------------------ |
|          | tinyInt                   | 微整型：很小的整数(占8位二进制)                              |
|          | smallint                  | 小整型：小的整数(占16位二进制)                               |
|          | mediumint                 | 中整型：中等长度的整数(占24位二进制)                         |
| 整数     | **integer**               | 整型：整数类型(占32位二进制)                                 |
|          | float                     | 单精度浮点数，占4个字节                                      |
|          | **double**                | 双精度浮点数，占8个字节                                      |
| 小数     | **DECIMAL**               | 大数,用于精确运算                                            |
|          | time                      | 表示时间类型                                                 |
|          | date                      | 表示日期类型                                                 |
| 日期     | **datetime**              | 同时可以表示日期和时间类型                                   |
|          | **char(m)**               | **固定**长度的字符串，无论使用几个字符都占满全部，M为0~255之间的整数,空间换时间 |
| 字符串   | **varchar(m)**            | **可变**长度的字符串，使用几个字符就占用几个，M为0~65535之间的整数,时间换空间 |
|          | tinyblob Big Large Object | 允许长度0~255字节                                            |
|          | blob                      | 允许长度0~65535字节                                          |
| 大二进制 | longblob                  | 允许长度0~4294967295字节                                     |
|          | tinytext                  | 允许长度0~255字节                                            |
|          | **text**                  | 允许长度0~65535字节                                          |
| 大文本   | mediumtext                | 允许长度0~167772150字节                                      |
|          | longtext                  | 允许长度0~4294967295字节                                     |

# 4 DQL(重点)

## 4.1 排序查询

~~~sql

	* 语法：order by 子句
		* order by 排序字段1 排序方式1 ，  排序字段2 排序方式2...

	* 排序方式：
		* ASC：升序，默认的。
		* DESC：降序。

	* 注意：
		* 如果有多个排序条件，则当前边的条件值一样时，才会判断第二条件。

~~~

##  4.2 聚合函数

~~~sql

	1. count：计算个数
		1. 一般选择非空的列：主键
		2. count(*)
	2. max：计算最大值
	3. min：计算最小值
	4. sum：计算和
	5. avg：计算平均值
	* 注意：聚合函数的计算，排除null值。
		解决方案：
			1. 选择不包含非空的列进行计算
			2. IFNULL函数	
~~~

## 4.3 分组查询:

~~~sql

	1. 语法：group by 分组字段；
	2. 注意：
		1. 分组之后查询的字段：分组字段、聚合函数
		2. where 和 having 的区别？
			1. where 在分组之前进行限定，如果不满足条件，则不参与分组。having在分组之后进行限定，如果不满足结果，则不会被查询出来
			2. where 后不可以跟聚合函数，having可以进行聚合函数的判断。

		-- 按照性别分组。分别查询男、女同学的平均分

		SELECT sex , AVG(math) FROM student GROUP BY sex;
		
		-- 按照性别分组。分别查询男、女同学的平均分,人数
		
		SELECT sex , AVG(math),COUNT(*) FROM student GROUP BY sex;
		
		--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组
		SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex;
		
		--  按照性别分组。分别查询男、女同学的平均分,人数 要求：分数低于70分的人，不参与分组,分组之后。人数要大于2个人
		SELECT sex , AVG(math),COUNT(id) FROM student WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2;
		
		SELECT sex , AVG(math),COUNT(id) 人数 FROM student WHERE math > 70 GROUP BY sex HAVING 人数 > 2;
		
~~~

## 4.4 分页查询

~~~sql

	1. 语法：limit 开始的索引,每页查询的条数;
	2. 公式：开始的索引 = （当前的页码 - 1） * 每页显示的条数
		-- 每页显示3条记录 

		SELECT * FROM student LIMIT 0,3; -- 第1页
		
		SELECT * FROM student LIMIT 3,3; -- 第2页
        SELECT * FROM student LIMIT 3;   -- 查询前3条数据


	3. limit 是一个MySQL"方言"
~~~

## 4.5 约束

~~~sql
* 概念： 对表中的数据进行限定，保证数据的正确性、有效性和完整性。	
* 分类：
	1. 主键约束：primary key
	2. 非空约束：not null
	3. 唯一约束：unique 
	4. 外键约束：foreign key
~~~

## 4.5.1非空约束

~~~sql


* 非空约束：not null，值不能为null
	1. 创建表时添加约束
		CREATE TABLE stu(
			id INT,
			NAME VARCHAR(20) NOT NULL -- name为非空
		);
	2. 创建表完后，添加非空约束
		ALTER TABLE stu MODIFY NAME VARCHAR(20) NOT NULL;

	3. 删除name的非空约束
		ALTER TABLE stu MODIFY NAME VARCHAR(20);
		
		
	2. 删除唯一约束
	
		ALTER TABLE stu DROP INDEX phone_number;
	
	3. 在创建表后，添加唯一约束
		ALTER TABLE stu MODIFY phone_number VARCHAR(20) UNIQUE;
~~~

## 4.5.2 主键约束

~~~sql
* 主键约束：primary key。
	1. 注意：
		1. 含义：非空且唯一
		2. 一张表只能有一个字段为主键
		3. 主键就是表中记录的唯一标识

	2. 在创建表时，添加主键约束
		create table stu(
			id int primary key,-- 给id添加主键约束
			name varchar(20)
		);

	3. 删除主键
		-- 错误 alter table stu modify id int ;
		ALTER TABLE stu DROP PRIMARY KEY;

	4. 创建完表后，添加主键
		ALTER TABLE stu MODIFY id INT PRIMARY KEY;

	5. 自动增长：
		1.  概念：如果某一列是数值类型的，使用 auto_increment 可以来完成值得自动增长

		2. 在创建表时，添加主键约束，并且完成主键自增长
		create table stu(
			id int primary key auto_increment,-- 给id添加主键约束
			name varchar(20)
		);
		3. 删除自动增长
		ALTER TABLE stu MODIFY id INT;
		4. 添加自动增长
		ALTER TABLE stu MODIFY id INT AUTO_INCREMENT;
~~~

## 4.5.3 外键约束



~~~sql
	
* 外键约束：foreign key,让表于表产生关系，从而保证数据的正确性。
	1. 在创建表时，可以添加外键
		* 语法：
			create table 表名(
				....
				外键列
				constraint 外键名称 foreign key (外键列名称) references 主表名称(主表列名称)
			);

	2. 删除外键
		ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;

	3. 创建表之后，添加外键
		ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
	4. 级联操作
		1. 添加级联操作
			语法：ALTER TABLE 表名 ADD CONSTRAINT 外键名称 
					FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称) ON UPDATE CASCADE ON DELETE CASCADE  ;
		2. 分类：
			1. 级联更新：ON UPDATE CASCADE 
			2. 级联删除：ON DELETE CASCADE 
		
~~~

# 5事务

##  5.1 事务的基本介绍

~~~sql
1. 事务的基本介绍
	1. 概念：
		*  如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。
		
	2. 操作：
		1. 开启事务： start transaction;
		2. 回滚：rollback;
		3. 提交：commit;
~~~

~~~sql
	3. 例子：
		CREATE TABLE account (
			id INT PRIMARY KEY AUTO_INCREMENT,
			NAME VARCHAR(10),
			balance DOUBLE
		);
		-- 添加数据
		INSERT INTO account (NAME, balance) VALUES ('zhangsan', 1000), ('lisi', 1000);
		
		SELECT * FROM account;
		
		UPDATE account SET balance = 1000;
		-- 张三给李四转账 500 元
		
		-- 0. 开启事务
		START TRANSACTION;
		-- 1. 张三账户 -500
		
		UPDATE account SET balance = balance - 500 WHERE NAME = 'zhangsan';
		-- 2. 李四账户 +500
		-- 出错了...
		UPDATE account SET balance = balance + 500 WHERE NAME = 'lisi';
		
		-- 发现执行没有问题，提交事务
		COMMIT;
		
		-- 发现出问题了，回滚事务
		ROLLBACK;
~~~

## 5.2 事务的四大特征

~~~sql
	1. 原子性：是不可分割的最小操作单位，要么同时成功，要么同时失败。
	2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
	3. 隔离性：多个事务之间。相互独立。
	4. 一致性：事务操作前后，数据总量不变
~~~

## 5.3 事务的隔离级别

~~~sql
  * 概念：多个事务之间隔离的，相互独立的。但是如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。
	* 存在问题：
		1. 脏读：一个事务，读取到另一个事务中没有提交的数据
		2. 不可重复读(虚读)：在同一个事务中，两次读取到的数据不一样。
		3. 幻读：一个事务操作(DML)数据表中所有记录，另一个事务添加了一条数据，则第一个事务查询不到自己的修改。
	* 隔离级别：
		1. read uncommitted：读未提交
			* 产生的问题：脏读、不可重复读、幻读
		2. read committed：读已提交 （Oracle）
			* 产生的问题：不可重复读、幻读
		3. repeatable read：可重复读 （MySQL默认）
			* 产生的问题：幻读
		4. serializable：串行化
			* 可以解决所有的问题

		* 注意：隔离级别从小到大安全性越来越高，但是效率越来越低
		* 数据库查询隔离级别：
			* select @@tx_isolation;
		* 数据库设置隔离级别：
			* set global transaction isolation level  级别字符串;

	* 演示：
		set global transaction isolation level read uncommitted;
		start transaction;
		-- 转账操作
		update account set balance = balance - 500 where id = 1;
		update account set balance = balance + 500 where id = 2;
~~~

# 6. MySQL细节

## sql语句执行顺序

![img](https://mmbiz.qpic.cn/mmbiz_png/libYRuvULTdUjIID8CJZm0kCpUTFyVGvJk3dNXRhX7iaSe3hwYebNbtBmic8xcSbJia2y4NA4gmPfezluyBtYMrEAA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

**FROM 连接**

首先，对 SELECT 语句执行查询时，对`FROM` 关键字两边的表执行连接，会形成`笛卡尔积`，这时候会产生一个`虚表VT1(virtual table)`

解释一下什么是虚表

> “
>
> 在 MySQL 中，有三种类型的表
>
> 一种是`永久表`，永久表就是创建以后用来长期保存数据的表
>
> 一种是`临时表`，临时表也有两类，一种是和永久表一样，只保存临时数据，但是能够长久存在的；还有一种是临时创建的，SQL 语句执行完成就会删除。
>
> 一种是`虚表`，虚表其实就是`视图`，数据可能会来自多张表的执行结果。

**ON 过滤**

然后对 FROM 连接的结果进行 ON 筛选，创建 VT2，把符合记录的条件存在 VT2 中。

**JOIN 连接**

第三步，如果是 `OUTER JOIN(left join、right join)` ，那么这一步就将添加外部行，如果是 left join 就把 ON 过滤条件的左表添加进来，如果是 right join ，就把右表添加进来，从而生成新的虚拟表 VT3。

**WHERE 过滤**

第四步，是执行 WHERE 过滤器，对上一步生产的虚拟表引用 WHERE 筛选，生成虚拟表 VT4。

WHERE 和 ON 的区别

- 如果有外部列，ON 针对过滤的是关联表，主表(保留表)会返回所有的列;
- 如果没有添加外部列，两者的效果是一样的;

应用

- 对主表的过滤应该使用 WHERE;
- 对于关联表，先条件查询后连接则用 ON，先连接后条件查询则用 WHERE;

**GROUP BY**

根据 group by 字句中的列，会对 VT4 中的记录进行分组操作，产生虚拟机表 VT5。果应用了group by，那么后面的所有步骤都只能得到的 VT5 的列或者是聚合函数（count、sum、avg等）。

**HAVING**

紧跟着 GROUP BY 字句后面的是 HAVING，使用 HAVING 过滤，会把符合条件的放在 VT6

**SELECT**

第七步才会执行 SELECT 语句，将 VT6 中的结果按照 SELECT 进行刷选，生成 VT7

**DISTINCT**

在第八步中，会对 TV7 生成的记录进行去重操作，生成 VT8。事实上如果应用了 group by 子句那么 distinct 是多余的，原因同样在于，分组的时候是将列中唯一的值分成一组，同时只为每一组返回一行记录，那么所以的记录都将是不相同的。

**ORDER BY**

应用 order by 子句。按照 order_by_condition 排序 VT8，此时返回的一个游标，而不是虚拟表。sql 是基于集合的理论的，集合不会预先对他的行排序，它只是成员的逻辑集合，成员的顺序是无关紧要的。

## 常用关键字

SQL 通配符:

在搜索数据库中的数据时，SQL 通配符可以替代一个或多个字符。

SQL 通配符必须与 LIKE 运算符一起使用。

在 SQL 中，可使用以下通配符：

| 通配符                     | 描述                       |
| :------------------------- | :------------------------- |
| %                          | 替代一个或多个字符         |
| _                          | 仅替代一个字符             |
| [charlist]                 | 字符列中的任何单一字符     |
| [^charlist]或者[!charlist] | 不在字符列中的任何单一字符 |

eg

```sql
SELECT * FROM Persons WHERE Name LIKE '_liu';

SELECT * FROM Persons WHERE LastName LIKE 'C_r_er';
```

 "Persons" 表中选取居住的城市以 "A" 或 "L" 或 "N" 开头的人： 

```sql
SELECT * FROM Persons WHERE City LIKE '[ALN]%';
SELECT * FROM Persons WHERE City LIKE '[!ALN]%';
```



**join**

**![img](https://user-gold-cdn.xitu.io/2020/6/11/172a0e9a46f69586?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)**



**left ( outer) join**（左联接）：返回左表中的所有记录以及和右表中的联接字段相等的记录。

![clipboard.png](https://user-gold-cdn.xitu.io/2020/6/11/172a0e9b9df710d0?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**right ( outer)  join**（右联接）：返回右表中的所有记录以及和左表中的联接字段相等的记录。

![clipboard.png](https://user-gold-cdn.xitu.io/2020/6/11/172a0e9ba0a532f3?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

**(inner) join**（等值联接）：只返回两个表中联接字段相等的记录( A和B 的交集)。

![clipboard.png](https://user-gold-cdn.xitu.io/2020/6/11/172a0e9a98e0d541?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 **FULL [OUTER] JOIN**  : 产生A和B的并集。但是需要注意的是，对于没有匹配的记录，则会以null做为值。 



**UNION** 与 **UNION ALL** 

 UNION 内部的 SELECT 语句必须拥有**相同数量的列**。列也必须拥有相似的数据类型。同时，每条 SELECT 语句中的**列的顺序必须相同**。UNION 只选取 选取不同的值 ，而UNION ALL会列出所有记录。 

## 7.存储过程

MySQL 5.0 版本开始支持存储过程。

存储过程（Stored Procedure）是一种在数据库中存储复杂程序，以便外部程序调用的一种数据库对象。

存储过程是为了完成特定功能的SQL语句集，经编译创建并保存在数据库中，用户可通过指定存储过程的名字并给定参数(需要时)来调用执行。

存储过程思想上很简单，就是数据库 SQL 语言层面的代码封装与重用。

### 优点

- 存储过程可封装，并隐藏复杂的商业逻辑。
- 存储过程可以回传值，并可以接受参数。
- 存储过程无法使用 SELECT 指令来运行，因为它是子程序，与查看表，数据表或用户定义函数不同。
- 存储过程可以用在数据检验，强制实行商业逻辑等。

### 缺点

- 存储过程，往往定制化于特定的数据库上，因为支持的编程语言不同。当切换到其他厂商的数据库系统时，需要重写原有的存储过程。
- 存储过程的性能调校与撰写，受限于各种数据库系统。

## 8.视图

 视图是可视化的表。 