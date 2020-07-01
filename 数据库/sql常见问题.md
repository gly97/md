&这个符号不能代表and!!!!

~~~ sql
select name from student where english>=80 & english<=90; 
-- &这个符号不能代表and!!!!
select name from student where english>=80 OR english<=90;
-- 和上一条数据查询结果相同  为什么???
select name from student where english>=80 and english<=90;
select name from student where english between 80 and 90;
~~~





as别名在什么什么时候使用  什么时候可以省略???







~~~sql
-- 有多个列要修改的时候不用and连接列,用逗号!!!
update employee set salary = 4000 and sex = 'female' where name = 'lisi' ; -- 数据倒是变了,但是不知道变成了什么样    ???
update employee set salary = 4000,sex = 'female' where name = 'lisi' ;
~~~



~~~sql
-- update 是修改表中的数据,就不用在其后加上talbe关键字了.

update table employee set salary = 5000; -- 报错

update employee set salary = 5000;

~~~

select * from emp where ename like "A%","B%","S%";要写多个like语句.

select * from emp where ename like 'A%' OR ENAME LIKE 'B%' OR ENAME LIKE 'S%';