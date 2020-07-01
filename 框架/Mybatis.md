# Mybatis

 MyBatis 是支持普通 SQL查询，存储过程和高级映射的优秀持久层框架。MyBatis 消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis 使用简单的 XML或注解用于配置和原始映射，将接口和 Java 的POJOs（Plain Old Java Objects，普通的 Java对象）映射成数据库中的记录 

## `mapper`接口是怎么映射成SQL语句的

目前多数开发者还是会使用XML来进行MyBatis的配置，包括MyBatis的核心配置和SQL映射配置。其实和注解一样，XML本身只不过是一个元数据的载体，最终起作用的还是MyBatis的核心类。其中有这样几个比较重要的：

1. `SqlSessionFactoryBuilder`，用来创建`SqlSessionFactory`的实例，之后就没有用了，其生命周期只是在初始化的时候有作用。
2. `SqlSessionFactory`，MyBatis最基础的类，用来创建会话（即`SqlSession`的实例），其生命周期与整个系统的生命周期相同，在系统运行的任何时候都可以使用它查询到当前数据库的配置信息等。
3. `SqlSession`，真正的和数据库之间的会话，线程不安全，所以其生命周期和使用它的线程相同。
4. 各种`Mapper`，承载了实际的业务逻辑，其生命周期比较短，由`SqlSession`创建。

## `#{}和${}`的区别是什么？

> ```
> #{}和${}的区别是什么？
> ```

在Mybatis中，有两种占位符

- `#{}`解析传递进来的参数数据
- ${}对传递进来的参数**原样**拼接在SQL中
- **`#{}`是预编译处理，${}是字符串替换**。
- 使用#{}可以有效的防止SQL注入，提高系统安全性。

## Mybatis是如何进行分页的？分页插件的原理是什么？

> Mybatis是如何进行分页的？分页插件的原理是什么？

Mybatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页，可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。

**分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。**

举例：`select * from student，拦截sql后重写为：select t.* from （select * from student）t limit 0，10`

#### Hibernate和MyBatis

Hibernate和Mybatis都是ORM模型，Hibernate提供的是一种全表映射的模型，对JDBC的封装程度比较高。但Hibernate也有不少缺点，列举如下：

- 全表映射带来的不便，比如更新时需要发送所有的字段；
- 无法根据不同的条件组装不同的SQL；
- 对多表关联和复杂SQL查询支持较差，需要自己写SQL，返回后，需要自己将数据组装为POJO；
- 不能有效支持存储过程；
- 虽然有HQL，但性能较差，大型互联网系统往往需要优化SQL，而Hibernate做不到。

大型互联网环境中，灵活、SQL优化，减少数据的传递是最基本的优化方法，Hibernate无法满足要求，而MyBatis提哦给你了灵活、方便的方式，是一个半自动映射的框架。

MyBatis需要手工匹配提供POJO、SQL和映射关系，而全表映射的Hibernate只需要提供POJO和映射关系。

MyBatis可以配置动态SQL，可以解决Hibernate的表名根据时间变化，不同的条件下列明不一样的问题。可以优化SQL，通过配置决定SQL映射规则，也能支持存储过程，对于一些复杂和需要优化性能的SQL的查询它更加方便。



### 动态SQL

~~~java
 public List<Emp> getEmps(Emp emp);
    
    public void updateEmp(Emp emp);
    
    public List<Emp> getEmpsByIds(@Param("ids") List<Integer> ids);
    
    public void addEmps(@Param("emp") List<Emp> emp);
~~~

#### if + where

```xml

<resultMap type="com.eu.bean.Emp" id="emp">
          <id column="id" property="id"/>
          <result column="last_name" property="lastName"/>
          <result column="gender" property="geder"/>
          <result column="email" property="email"/>
      </resultMap>
  
    <select id="getEmps" resultMap="emp">
        SELECT *FROM emp
        <where>
            <if test="id != null">
                id = #{id}
            </if>
            <if test="lastName != null">
                and last_name = #{lastName}
            </if>
            <if test="geder != null">
                 and gender = #{geder}
            </if>
        </where>
    </select>
```

####  **choose，when **

**有一个when标签成立 ，其余的when标签中的内容不再执行。** 

~~~xml

<select id="getEmps" resultMap="emp">
        SELECT *FROM emp
        <where>
            <choose>
                <when test="id != null">
                    id = #{id}
                </when>
                <when test="lastName != null">
                    and last_name = #{lastName}
                </when>
                <when test="geder != null">
                    and gender = #{geder}
                </when>
                <otherwise>
                    id=1
                </otherwise>
            </choose>
        </where>
    </select>
~~~

####  set

~~~xml
<update id="updateEmp">
        UPDATE emp
        <set>
            <if test="lastName != null">
                last_name=#{lastName},
            </if>
            <if test="geder != null">
                gender = #{geder}
            </if>
        </set> 
        WHERE id=#{id}
    </update>
~~~

####  for each

~~~xml
<!-- SELECT *FROM emp
         WHERE id IN (1,5,6) -->
      <select id="getEmpsByIds" resultMap="emp">
              SELECT *FROM empf
              WHERE id IN 
              <foreach collection="ids" item="item_id" separator="," 
                  open="(" close=")">
                   #{item_id}
              </foreach>
      </select>
~~~

####  for each批量插入

~~~xml
<insert id="addEmps">
              INSERT INTO emp(last_name,gender,email)
            VALUES
            <foreach collection="emp" item="emps" separator=",">
                (#{emps.lastName},#{emps.geder},#{emps.email})
            </foreach>
  </insert>
~~~

