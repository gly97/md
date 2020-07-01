# JDBC

## 1. 概念

Java DataBase Connectivity  Java 数据库连接， Java语言操作数据库

JDBC本质：其实是官方（sun公司）定义的一套操作所有关系型数据库的规则，即接口。各个数据库厂商去实现这套接口，提供数据库驱动jar包。我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类。

### 1.1 快速入门：

步骤：
1. 导入驱动jar包 mysql-connector-java-8.0.18.jar    maven中央仓库
	1.复制mysql-connector-java-8.0.18.jar到项目的libs目录下
	2.右键-->Add As Library
2. 注册驱动
3. 获取数据库连接对象 Connection
4. 定义sql
5. 获取执行sql语句的对象 Statement
6. 执行sql，接受返回结果
7. 处理结果
8. 释放资源

代码实现：
	
~~~java
//1. 导入驱动jar包
  //2.注册驱动 没必要
  Class.forName("com.mysql.jdbc.Driver");
  //3.获取数据库连接对象  jdbc:mysql://ip地址(域名):端口号/数据库名称,账号,密码   localhost :3306是本地默认
  Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/demo", "root", "root");
  //4.自定义sql语句
  String sql = "update account set name ="lizi" where id = "1";
  //5.获取执行sql的对象 Statement
  Statement stmt = conn.createStatement();
  //6.执行sql
  int count = stmt.executeUpdate(sql);
  //7.处理结果
  System.out.println(count);
  //8.释放资源
stmt.close();
   conn.close();
~~~

 

Connection：数据库连接对象
1. 功能：
	1. 获取执行sql 的对象
		* Statement createStatement()(不常用)
		* PreparedStatement prepareStatement(String sql)  
	2. **管理事务**：
		* 开启事务：**setAutoCommit(boolean autoCommit)** ：调用该方法设置参数为false，即开启事务
		* 提交事务：commit() 
		* 回滚事务：rollback() 



3. Statement：执行sql的对象
	1. 执行sql
		1. boolean execute(String sql) ：可以执行任意的sql 
		2. int executeUpdate(String sql) ：执行DML（insert、update、delete）语句、DDL(create，alter、drop)语句
			
			返回值：影响的行数，可以通过这个影响的行数判断DML语句是否执行成功 返回值>0的则执行成功，反之，则失败。
		3. ResultSet executeQuery(String sql)  ：执行DQL（select)语句
		
	
	
	
2. ResultSet：结果集对象,封装查询结果
  * boolean next(): 游标向下移动一行，判断当前行是否是最后一行末尾(是否有数据)，如果是，则返回false，如果不是则返回true
  * getXxx(参数):获取数据
  	* Xxx：代表数据类型   如： int getInt() ,	String getString()
  	* 参数：
  		1. int：代表列的编号,从1开始   如： getString(1)
        		2. String：代表列名称。 如： getDouble("balance")

  * 注意：
  	* 使用步骤：
  		1. 游标向下移动一行
        		2. 判断是否有数据
            		3. 获取数据




~~~java
     //循环判断游标是否是最后一行末尾。
     while(rs.next()){
         //获取数据
         //6.2 获取数据
         int id = rs.getInt(1);
         String name = rs.getString("name");
  double balance = rs.getDouble(3);
     System.out.println(id + "---" + name + "---" + balance);
 }
~~~


5. PreparedStatement：执行sql的对象
	1. SQL注入问题：在拼接sql时，有一些sql的特殊关键字参与**字符串的拼接**。会造成安全性问题输入用户随便，输入密码：a' or 'a' = 'a

```sql

sql：select * from user where username = 'admin' and password = '任意' or 'a' = 'a' 
-- 即 '任意' or 'a' = 'a' 为true所以通过

```

2. **解决sql注入问题**：使用PreparedStatement对象来解决(**预编译的SQL**：参数使用?作为占位符)
4. 步骤：
	1. 导入驱动jar包 mysql-connector-java-8.0.18.jar
	2. 注册驱动
	3. 获取数据库连接对象 Connection
	4. 定义sql
		* 注意：sql的参数使用？作为占位符。 如：select * from user where username = ? and password = ?;
	5. 获取执行sql语句的对象 PreparedStatement  Connection.prepareStatement(String sql) 
	6. 给？赋值：
		* 方法： setXxx(参数1,参数2)
			* 参数1：？的位置编号 从1 开始
			* 参数2：？的值
7. 执行sql，接受返回结果，不需要传递sql语句
	8. 处理结果
	9. 释放资源

5. 注意：后期都会使用PreparedStatement来完成增删改查的所有操作
	1. 可以防止SQL注入
	2. 效率更高



# 2 数据库连接池

1. 概念：其实就是一个容器(集合)，存放数据库连接的容器。
	    当系统初始化好后，容器被创建，容器中会申请一些连接对象，当用户来访问数据库时，从容器中获取连接对象，用户访问完之后，会将连接对象归还给容器。

2. 好处：
	1. 节约资源
	2. 用户访问高效

3. 实现：
	1. 标准接口：DataSource   javax.sql包下的
		1. 方法：
			* 获取连接：getConnection()
			* 归还连接：Connection.close()。如果连接对象Connection是从连接池中获取的，那么调用Connection.close()方法，则不会再关闭连接了。而是归还连接

	2. 一般我们不去实现它，有数据库厂商来实现
		1. C3P0：数据库连接池技术
		2. Druid：数据库连接池实现技术，由阿里巴巴提供的



C3P0：数据库连接池技术
* 步骤：
	1. 导入jar包 (两个) c3p0-0.9.5.2.jar mchange-commons-java-0.2.12.jar ，
		* 不要忘记导入数据库驱动jar包
	2. 定义配置文件：
		* 名称： c3p0.properties 或者 c3p0-config.xml
		* 路径：直接将文件放在src目录下即可。
3. 创建核心对象 数据库连接池对象 ComboPooledDataSource
	4. 获取连接： getConnection

~~~java
//1.创建数据库连接池对象
  DataSource ds  = new ComboPooledDataSource();
  //2. 获取连接对象
  Connection conn = ds.getConnection();
~~~



5. Druid：数据库连接池实现技术，由阿里巴巴提供的
	1. 步骤：
		1. 导入jar包 druid-1.0.9.jar
		2. 定义配置文件：
			* 是properties形式的
			* 可以叫任意名称，可以放在任意目录下
		3. 加载配置文件。Properties
		4. 获取数据库连接池对象：通过工厂来来获取  DruidDataSourceFactory
		5. 获取连接：getConnection
	~~~java
	//3.加载配置文件
     Properties pro = new Properties();
     InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
     pro.load(is);
     //4.获取连接池对象
     DataSource ds = DruidDataSourceFactory.createDataSource(pro);
     //5.获取连接
     Connection conn = ds.getConnection();
	~~~
	
	
	
	
	
	2. 定义工具类
		1. 定义一个类 JDBCUtils
		2. 提供静态代码块加载配置文件，初始化连接池对象
		3. 提供方法
			1. 获取连接方法：通过数据库连接池获取连接
			2. 释放资源
			3. 获取连接池的方法

~~~java
import java.io.IOException;
import java.sql.ResultSet;
import java.sql.SQLException;

public class JDBCUtils {

    //1.定义成员变量 DataSource
    private static DataSource ds;

    static {
        try {
            //1.加载配置文件
            Properties pro = new Properties();
            pro.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
            //2.获取DataSource
            ds = DruidDataSourceFactory.createDataSource(pro);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

    /**
     * 获取连接
     */
    public static Connection getConnection() throws SQLException {
        return ds.getConnection();
    }

    /**
     * 释放资源
     */
    public static void close(Statement stmt, Connection conn) {
		       /* if(stmt != null){
		            try {
		                stmt.close();
		            } catch (SQLException e) {
		                e.printStackTrace();
		            }
		        }
		
		        if(conn != null){
		            try {
		                conn.close();//归还连接
		            } catch (SQLException e) {
		                e.printStackTrace();
		            }
		        }*/

        close(null, stmt, conn);
    }

    public static void close(ResultSet rs, Statement stmt, Connection conn) {
        if (rs != null) {
            try {
                rs.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }


        if (stmt != null) {
            try {
                stmt.close();
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }

        if (conn != null) {
            try {
                conn.close();//归还连接
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }

    /**
     * 获取连接池方法
     */

    public static DataSource getDataSource() {
        return ds;
    }

}
~~~

## 前言

平时接触过多线程开发的童鞋应该都或多或少了解过线程池，之前发布的《阿里巴巴 Java 手册》里也有一条：

[![img](https://camo.githubusercontent.com/4fe5fbafcc85c791f8e173b2c0ff992c0b31bafa/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667470786633783165706a33306c6130337330746c2e6a7067)](https://camo.githubusercontent.com/4fe5fbafcc85c791f8e173b2c0ff992c0b31bafa/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667470786633783165706a33306c6130337330746c2e6a7067)

可见线程池的重要性。

简单来说使用线程池有以下几个目的：

- 线程是稀缺资源，不能频繁的创建。
- 解耦作用；线程的创建于执行完全分开，方便维护。
- 应当将其放入一个池子中，可以给其他任务进行复用。

## 线程池原理

谈到线程池就会想到池化技术，其中最核心的思想就是把宝贵的资源放到一个池子中；每次使用都从里面获取，用完之后又放回池子供其他人使用，有点吃大锅饭的意思。

那在 Java 中又是如何实现的呢？

在 JDK 1.5 之后推出了相关的 api，常见的创建线程池方式有以下几种：

- `Executors.newCachedThreadPool()`：无限线程池。
- `Executors.newFixedThreadPool(nThreads)`：创建固定大小的线程池。
- `Executors.newSingleThreadExecutor()`：创建单个线程的线程池。

其实看这三种方式创建的源码就会发现：

```
    public static ExecutorService newCachedThreadPool() {
        return new ThreadPoolExecutor(0, Integer.MAX_VALUE,
                                      60L, TimeUnit.SECONDS,
                                      new SynchronousQueue<Runnable>());
    }
```

实际上还是利用 `ThreadPoolExecutor` 类实现的。

所以我们重点来看下 `ThreadPoolExecutor` 是怎么玩的。

首先是创建线程的 api：

```
ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, RejectedExecutionHandler handler) 
```

这几个核心参数的作用：

- `corePoolSize` 为线程池的基本大小。
- `maximumPoolSize` 为线程池最大线程大小。
- `keepAliveTime` 和 `unit` 则是线程空闲后的存活时间。
- `workQueue` 用于存放任务的阻塞队列。
- `handler` 当队列和最大线程池都满了之后的饱和策略。

了解了这几个参数再来看看实际的运用。

通常我们都是使用:

```
threadPool.execute(new Job());
```

这样的方式来提交一个任务到线程池中，所以核心的逻辑就是 `execute()` 函数了。

在具体分析之前先了解下线程池中所定义的状态，这些状态都和线程的执行密切相关：

[![img](https://camo.githubusercontent.com/3217f32a3a8f10bf9b6f5076c7e1d44a81ad19a9/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744b665463677931667471316b73357179776a33306a6e303369337a612e6a7067)](https://camo.githubusercontent.com/3217f32a3a8f10bf9b6f5076c7e1d44a81ad19a9/68747470733a2f2f7773332e73696e61696d672e636e2f6c617267652f303036744b665463677931667471316b73357179776a33306a6e303369337a612e6a7067)

- `RUNNING` 自然是运行状态，指可以接受任务执行队列里的任务
- `SHUTDOWN` 指调用了 `shutdown()` 方法，不再接受新任务了，但是队列里的任务得执行完毕。
- `STOP` 指调用了 `shutdownNow()` 方法，不再接受新任务，同时抛弃阻塞队列里的所有任务并中断所有正在执行任务。
- `TIDYING` 所有任务都执行完毕，在调用 `shutdown()/shutdownNow()` 中都会尝试更新为这个状态。
- `TERMINATED` 终止状态，当执行 `terminated()` 后会更新为这个状态。

用图表示为：

[![img](https://camo.githubusercontent.com/7aaf3ec3d5b3f0a76786b4ca30fac43083f13690/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b665463677931667471326e786c7765356a333073703062613074732e6a7067)](https://camo.githubusercontent.com/7aaf3ec3d5b3f0a76786b4ca30fac43083f13690/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b665463677931667471326e786c7765356a333073703062613074732e6a7067)

然后看看 `execute()` 方法是如何处理的：

[![img](https://camo.githubusercontent.com/a2477af411a2d9effded4361aa117549f5856af3/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b6654636779316674713238337a6939316a33306b7930386d7767622e6a7067)](https://camo.githubusercontent.com/a2477af411a2d9effded4361aa117549f5856af3/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b6654636779316674713238337a6939316a33306b7930386d7767622e6a7067)

1. 获取当前线程池的状态。
2. 当前线程数量小于 coreSize 时创建一个新的线程运行。
3. 如果当前线程处于运行状态，并且写入阻塞队列成功。
4. 双重检查，再次获取线程池状态；如果线程池状态变了（非运行状态）就需要从阻塞队列移除任务，并尝试判断线程是否全部执行完毕。同时执行拒绝策略。
5. 如果当前线程池为空就新创建一个线程并执行。
6. 如果在第三步的判断为非运行状态，尝试新建线程，如果失败则执行拒绝策略。

这里借助《聊聊并发》的一张图来描述这个流程：

[![img](https://camo.githubusercontent.com/bdcb761059ed70b56b106e0c77e79c86f56c7a05/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b66546367793166747132767a757635726a333064773038357133692e6a7067)](https://camo.githubusercontent.com/bdcb761059ed70b56b106e0c77e79c86f56c7a05/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b66546367793166747132767a757635726a333064773038357133692e6a7067)

### 如何配置线程

流程聊完了再来看看上文提到了几个核心参数应该如何配置呢？

有一点是肯定的，线程池肯定是不是越大越好。

通常我们是需要根据这批任务执行的性质来确定的。

- IO 密集型任务：由于线程并不是一直在运行，所以可以尽可能的多配置线程，比如 CPU 个数 * 2
- CPU 密集型任务（大量复杂的运算）应当分配较少的线程，比如 CPU 个数相当的大小。

当然这些都是经验值，最好的方式还是根据实际情况测试得出最佳配置。

### 优雅的关闭线程池

有运行任务自然也有关闭任务，从上文提到的 5 个状态就能看出如何来关闭线程池。

其实无非就是两个方法 `shutdown()/shutdownNow()`。

但他们有着重要的区别：

- `shutdown()` 执行后停止接受新任务，会把队列的任务执行完毕。
- `shutdownNow()` 也是停止接受新任务，但会中断所有的任务，将线程池状态变为 stop。

> 两个方法都会中断线程，用户可自行判断是否需要响应中断。

`shutdownNow()` 要更简单粗暴，可以根据实际场景选择不同的方法。

我通常是按照以下方式关闭线程池的：

```
        long start = System.currentTimeMillis();
        for (int i = 0; i <= 5; i++) {
            pool.execute(new Job());
        }

        pool.shutdown();

        while (!pool.awaitTermination(1, TimeUnit.SECONDS)) {
            LOGGER.info("线程还在执行。。。");
        }
        long end = System.currentTimeMillis();
        LOGGER.info("一共处理了【{}】", (end - start));
```

`pool.awaitTermination(1, TimeUnit.SECONDS)` 会每隔一秒钟检查一次是否执行完毕（状态为 `TERMINATED`），当从 while 循环退出时就表明线程池已经完全终止了。

## SpringBoot 使用线程池

2018 年了，SpringBoot 盛行；来看看在 SpringBoot 中应当怎么配置和使用线程池。

既然用了 SpringBoot ，那自然得发挥 Spring 的特性，所以需要 Spring 来帮我们管理线程池：

```
@Configuration
public class TreadPoolConfig {


    /**
     * 消费队列线程
     * @return
     */
    @Bean(value = "consumerQueueThreadPool")
    public ExecutorService buildConsumerQueueThreadPool(){
        ThreadFactory namedThreadFactory = new ThreadFactoryBuilder()
                .setNameFormat("consumer-queue-thread-%d").build();

        ExecutorService pool = new ThreadPoolExecutor(5, 5, 0L, TimeUnit.MILLISECONDS,
                new ArrayBlockingQueue<Runnable>(5),namedThreadFactory,new ThreadPoolExecutor.AbortPolicy());

        return pool ;
    }



}
```

使用时：

```
    @Resource(name = "consumerQueueThreadPool")
    private ExecutorService consumerQueueThreadPool;


    @Override
    public void execute() {

        //消费队列
        for (int i = 0; i < 5; i++) {
            consumerQueueThreadPool.execute(new ConsumerQueueThread());
        }

    }
```

其实也挺简单，就是创建了一个线程池的 bean，在使用时直接从 Spring 中取出即可。

## 监控线程池

谈到了 SpringBoot，也可利用它 actuator 组件来做线程池的监控。

线程怎么说都是稀缺资源，对线程池的监控可以知道自己任务执行的状况、效率等。

关于 actuator 就不再细说了，感兴趣的可以看看[这篇](http://t.cn/ReimM0o)，有详细整理过如何暴露监控端点。

其实 ThreadPool 本身已经提供了不少 api 可以获取线程状态：

[![img](https://camo.githubusercontent.com/ebd9e743f998de76dc1b5c5a88d0d3abca9cb97b/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b665463677931667471337873726273366a33306267306270676e622e6a7067)](https://camo.githubusercontent.com/ebd9e743f998de76dc1b5c5a88d0d3abca9cb97b/68747470733a2f2f7773312e73696e61696d672e636e2f6c617267652f303036744b665463677931667471337873726273366a33306267306270676e622e6a7067)

很多方法看名字就知道其含义，只需要将这些信息暴露到 SpringBoot 的监控端点中，我们就可以在可视化页面查看当前的线程池状态了。

甚至我们可以继承线程池扩展其中的几个函数来自定义监控逻辑：

[![img](https://camo.githubusercontent.com/5804ea0ece95bddc3c1a86119d99ac66ef3ef2be/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b66546367793166747134306c6b77396a6a33306d713037726d79742e6a7067)](https://camo.githubusercontent.com/5804ea0ece95bddc3c1a86119d99ac66ef3ef2be/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b66546367793166747134306c6b77396a6a33306d713037726d79742e6a7067)

[![img](https://camo.githubusercontent.com/9a1e3cb2f6c4d2028c4cb65e0250416b1243bf98/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b665463677931667471343161736638726a33306b713037636162642e6a7067)](https://camo.githubusercontent.com/9a1e3cb2f6c4d2028c4cb65e0250416b1243bf98/68747470733a2f2f7773342e73696e61696d672e636e2f6c617267652f303036744b665463677931667471343161736638726a33306b713037636162642e6a7067)

看这些名称和定义都知道，这是让子类来实现的。

可以在线程执行前、后、终止状态执行自定义逻辑。

## 线程池隔离

> 线程池看似很美好，但也会带来一些问题。

如果我们很多业务都依赖于同一个线程池,当其中一个业务因为各种不可控的原因消耗了所有的线程，导致线程池全部占满。

这样其他的业务也就不能正常运转了，这对系统的打击是巨大的。

比如我们 Tomcat 接受请求的线程池，假设其中一些响应特别慢，线程资源得不到回收释放；线程池慢慢被占满，最坏的情况就是整个应用都不能提供服务。

所以我们需要将线程池**进行隔离**。

通常的做法是按照业务进行划分：

> 比如下单的任务用一个线程池，获取数据的任务用另一个线程池。这样即使其中一个出现问题把线程池耗尽，那也不会影响其他的任务运行。

### hystrix 隔离

这样的需求 [Hystrix](https://github.com/Netflix/Hystrix) 已经帮我们实现了。

> Hystrix 是一款开源的容错插件，具有依赖隔离、系统容错降级等功能。

下面来看看 `Hystrix` 简单的应用：

首先需要定义两个线程池，分别用于执行订单、处理用户。

```java
/**
 * Function:订单服务
 *
 * @author crossoverJie
 *         Date: 2018/7/28 16:43
 * @since JDK 1.8
 */
public class CommandOrder extends HystrixCommand<String> {

    private final static Logger LOGGER = LoggerFactory.getLogger(CommandOrder.class);

    private String orderName;

    public CommandOrder(String orderName) {


        super(Setter.withGroupKey(
                //服务分组
                HystrixCommandGroupKey.Factory.asKey("OrderGroup"))
                //线程分组
                .andThreadPoolKey(HystrixThreadPoolKey.Factory.asKey("OrderPool"))

                //线程池配置
                .andThreadPoolPropertiesDefaults(HystrixThreadPoolProperties.Setter()
                        .withCoreSize(10)
                        .withKeepAliveTimeMinutes(5)
                        .withMaxQueueSize(10)
                        .withQueueSizeRejectionThreshold(10000))

                .andCommandPropertiesDefaults(
                        HystrixCommandProperties.Setter()
                                .withExecutionIsolationStrategy(HystrixCommandProperties.ExecutionIsolationStrategy.THREAD))
        )
        ;
        this.orderName = orderName;
    }


    @Override
    public String run() throws Exception {

        LOGGER.info("orderName=[{}]", orderName);

        TimeUnit.MILLISECONDS.sleep(100);
        return "OrderName=" + orderName;
    }


}


/**
 * Function:用户服务
 *
 * @author crossoverJie
 *         Date: 2018/7/28 16:43
 * @since JDK 1.8
 */
public class CommandUser extends HystrixCommand<String> {

    private final static Logger LOGGER = LoggerFactory.getLogger(CommandUser.class);

    private String userName;

    public CommandUser(String userName) {


        super(Setter.withGroupKey(
                //服务分组
                HystrixCommandGroupKey.Factory.asKey("UserGroup"))
                //线程分组
                .andThreadPoolKey(HystrixThreadPoolKey.Factory.asKey("UserPool"))

                //线程池配置
                .andThreadPoolPropertiesDefaults(HystrixThreadPoolProperties.Setter()
                        .withCoreSize(10)
                        .withKeepAliveTimeMinutes(5)
                        .withMaxQueueSize(10)
                        .withQueueSizeRejectionThreshold(10000))

                //线程池隔离
                .andCommandPropertiesDefaults(
                        HystrixCommandProperties.Setter()
                                .withExecutionIsolationStrategy(HystrixCommandProperties.ExecutionIsolationStrategy.THREAD))
        )
        ;
        this.userName = userName;
    }


    @Override
    public String run() throws Exception {

        LOGGER.info("userName=[{}]", userName);

        TimeUnit.MILLISECONDS.sleep(100);
        return "userName=" + userName;
    }


}
```

------

`api` 特别简洁易懂，具体详情请查看官方文档。

然后模拟运行：

```java
    public static void main(String[] args) throws Exception {
        CommandOrder commandPhone = new CommandOrder("手机");
        CommandOrder command = new CommandOrder("电视");


        //阻塞方式执行
        String execute = commandPhone.execute();
        LOGGER.info("execute=[{}]", execute);

        //异步非阻塞方式
        Future<String> queue = command.queue();
        String value = queue.get(200, TimeUnit.MILLISECONDS);
        LOGGER.info("value=[{}]", value);


        CommandUser commandUser = new CommandUser("张三");
        String name = commandUser.execute();
        LOGGER.info("name=[{}]", name);
    }
```

------

运行结果：

[![img](https://camo.githubusercontent.com/f8ae2571dc4e0397a72090974b264fa3ee6a2a5f/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667471346530756b75626a3330707330346774616b2e6a7067)](https://camo.githubusercontent.com/f8ae2571dc4e0397a72090974b264fa3ee6a2a5f/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667471346530756b75626a3330707330346774616b2e6a7067)

可以看到两个任务分成了两个线程池运行，他们之间互不干扰。

获取任务任务结果支持同步阻塞和异步非阻塞方式，可自行选择。

它的实现原理其实容易猜到：

> 利用一个 Map 来存放不同业务对应的线程池。

通过刚才的构造函数也能证明：

[![img](https://camo.githubusercontent.com/5420e651871dd6189fe3631ef962cc1652b5757f/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667471346936787932716a3330756f3039616468702e6a7067)](https://camo.githubusercontent.com/5420e651871dd6189fe3631ef962cc1652b5757f/68747470733a2f2f7773322e73696e61696d672e636e2f6c617267652f303036744b665463677931667471346936787932716a3330756f3039616468702e6a7067)

还要注意的一点是：

> 自定义的 Command 并不是一个单例，每次执行需要 new 一个实例，不然会报 `This instance can only be executed once. Please instantiate a new instance.` 异常。

## 总结

池化技术确实在平时应用广泛，熟练掌握能提高不少效率。

文末的 hystrix 源码：

https://github.com/crossoverJie/Java-Interview/tree/master/src/main/java/com/crossoverjie/hystrix