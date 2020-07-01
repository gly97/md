# spring

- **轻量级** - Spring 在代码量和透明度方面都很轻便。
- **IOC** - 控制反转
- **AOP** - 面向切面编程可以将应用业务逻辑和系统服务分离，以实现高内聚。
- **容器** - Spring 负责创建和管理对象（Bean）的生命周期和配置。
- **MVC** - 对 web 应用提供了高度可配置性，其他框架的集成也十分方便。
- **事务管理** - 提供了用于事务管理的通用抽象层。Spring 的事务支持也可用于容器较少的环境。
- **JDBC 异常** - Spring 的 JDBC 抽象层提供了一个异常层次结构，简化了错误处理策略。

 Spring是一个开源框架，是为了解决企业应用程序开发复杂性而创建的。框架的主要优势之一就是其分层架构，分层架构允许您选择使用哪一个组件，同时为 J2EE 应用程序开发提供集成的框架。 

![1、Spring特征.png](https://user-gold-cdn.xitu.io/2017/5/5/0d724099658747cb5d28bd7954c31330?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

> Sping架构

Spring框架是分模块存在，除了最核心的Spring Core Container(即Spring容器)是必要模块之外，其他模块都是可选，视需要而定。大约有20多个模块。



### Spring Framework 的功能![img](https://user-gold-cdn.xitu.io/2018/8/10/1652290e58b0cedb?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

- **Spring 核心容器** – 该层基本上是 Spring Framework 的核心。它包含以下模块：

- - Spring Core
  - Spring Bean
  - SpEL (Spring Expression Language)
  - Spring Context

- **数据访问/集成** – 该层提供与数据库交互的支持。它包含以下模块：

- - JDBC (Java DataBase Connectivity)
  - ORM (Object Relational Mapping)
  - OXM (Object XML Mappers)
  - JMS (Java Messaging Service)
  - Transaction

- **Web** – 该层提供了创建 Web 应用程序的支持。它包含以下模块：

- - Web
  - Web – Servlet
  - Web – Socket
  - Web – Portlet

- **AOP** – 该层支持面向切面编程

- **Instrumentation** – 该层为类检测和类加载器实现提供支持。

- **Test** – 该层为使用 JUnit 和 TestNG 进行测试提供支持。

- **几个杂项模块:**

- - Messaging – 该模块为 STOMP 提供支持。它还支持注解编程模型，该模型用于从 WebSocket 客户端路由和处理 STOMP 消息。
  - Aspects – 该模块为与 AspectJ 的集成提供支持。

## 1.IOC

### 什么是 IoC

IoC （Inversion of control ）**控制反转/反转控制**。它是一种思想不是一个技术实现。描述的是：Java 开发领域对象的创建以及管理的问题。

例如：现有类 A 依赖于类 B

- **传统的开发方式** ：往往是在类 A 中手动通过 new 关键字来 new 一个 B 的对象出来
- **使用 IoC 思想的开发方式** ：不通过 new 关键字来创建对象，而是通过 IoC 容器(Spring 框架) 来帮助我们实例化对象。我们需要哪个对象，直接从 IoC 容器里面过去即可。

从以上两种开发方式的对比来看：我们 “丧失了一个权力” (创建、管理对象的权力)，从而也得到了一个好处（不用再考虑对象的创建、管理等一系列的事情）

### 控制反转

**控制** ：指的是对象创建（实例化、管理）的权力

**反转** ：控制权交给外部环境（Spring 框架、IoC 容器）

![img](https://user-gold-cdn.xitu.io/2020/5/28/1725a71315d1da13?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### IoC 解决了什么问题

IoC 的思想就是两方之间不互相依赖，由第三方容器来管理相关资源。这样有什么好处呢？

1. 对象之间的耦合度或者说依赖程度降低；
2. 资源变的容易管理；比如你用 Spring 容器提供的话很容易就可以实现一个单例。

例如：现有一个针对 User 的操作，利用 Service 和 Dao 两层结构进行开发

在没有使用 IoC 思想的情况下，Service 层想要使用 Dao 层的具体实现的话，需要通过 new 关键字在`UserServiceImpl` 中手动 new 出 `IUserDao` 的具体实现类 `UserDaoImpl`（不能直接 new 接口类）。

![img](https://user-gold-cdn.xitu.io/2020/5/28/1725a71316001230?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

很完美，这种方式也是可以实现的，但是我们想象一下如下场景：

开发过程中突然接到一个新的需求，针对对`IUserDao` 接口开发出另一个具体实现类。因为 Server 层依赖了`IUserDao`的具体实现，所以我们需要修改`UserServiceImpl`中 new 的对象。如果只有一个类引用了`IUserDao`的具体实现，可能觉得还好，修改起来也不是很费力气，但是如果有许许多多的地方都引用了`IUserDao`的具体实现的话，一旦需要更换`IUserDao` 的实现方式，那修改起来将会非常的头疼。

![img](https://user-gold-cdn.xitu.io/2020/5/28/1725a6b2d58ea769?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 使用 IoC 的思想，我们将对象的控制权（创建、管理）交有 IoC 容器去管理，我们在使用的时候直接向 IoC 容器 “要” 就可以了 

![img](https://user-gold-cdn.xitu.io/2020/5/28/1725a6b2d57b064b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### IoC 和 DI 的区别

IoC（Inverse of Control:控制反转）是一种**设计思想** 或者说是某种模式。这个设计思想就是 **将原本在程序中手动创建对象的控制权，交由 Spring 框架来管理。** IoC 在其他语言中也有应用，并非 Spring 特有。**IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个 Map（key，value）,Map 中存放的是各种对象。**

IoC 最常见以及最合理的实现方式叫做依赖注入（Dependency Injection，简称 DI）。

### IOC注入方式

#### 依赖注入

在依赖注入中，您不必创建对象，但必须描述如何创建它们。您不是直接在代码中将组件和服务连接在一起，而是描述配置文件中哪些组件需要哪些服务。由 IoC 容器将它们装配在一起。

1.构造器注入

2.setter注入

3.接口注入（我们几乎不用）

#### Spring 循环依赖

 Spring内部如何解决循环依赖，一定是单默认的**单例**中，属性互相引用的场景。 

![img](https://mmbiz.qpic.cn/mmbiz/eQPyBffYbucDDhbXgfTTWqBWQNKVBpf92yc4HEnjCexIcVNtBib3JtpZTgZLIsy68Y2q7X6C9LRa6TjZT51ojRQ/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

 甚至自己“循环”依赖自己： 

![img](https://mmbiz.qpic.cn/mmbiz/eQPyBffYbucDDhbXgfTTWqBWQNKVBpf9KEUia6fcETVZ6DbEtjvrr9Rg8CWEJZz1f2GcnfyD09KI2bclVsWqYyA/640?wx_fmt=other&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



先说明前提：原型(Prototype)的场景是不支持循环依赖的，通常会走到[`AbstractBeanFactory`](http://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247493383&idx=2&sn=883c40b743d496e48dc2f99a083ec5e4&chksm=eb506231dc27eb272437a3dfd2b9a0ae91414c69a83c7e3fe22362cdef909e39122929af9884&scene=21#wechat_redirect)类中下面的判断，抛出异常。

- 
- 
- 

```java
if (isPrototypeCurrentlyInCreation(beanName)) {  throw new BeanCurrentlyInCreationException(beanName);}
```

原因很好理解，创建新的A时，发现要注入原型字段B，又创建新的B发现要注入原型字段A...

这就套娃了, 你猜是先[StackOverflow](http://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247493487&idx=2&sn=c81dc6fa03b6eba786eb439da7954cf8&chksm=eb506259dc27eb4f472fa50fb980285a2d6c533b20d948586f2f299485c4b67a1f782c0c3e3e&scene=21#wechat_redirect)还是[OutOfMemory](http://mp.weixin.qq.com/s?__biz=MzI3ODcxMzQzMw==&mid=2247489456&idx=1&sn=f24823b901bf4a697b7593737546e548&chksm=eb539286dc241b90529e3aa5efee03dc2cf41c9884c2e015ea0123ead6c02dda605e607d4c15&scene=21#wechat_redirect)？

Spring怕你不好猜，就先抛出了BeanCurrentlyInCreationException

#### Spring解决循环依赖

首先，Spring内部维护了三个Map，也就是我们通常说的三级缓存。

笔者翻阅Spring文档倒是没有找到三级缓存的概念，可能也是本土为了方便理解的词汇。

在Spring的`DefaultSingletonBeanRegistry`类中，你会赫然发现类上方挂着这三个Map：

- **singletonObjects** 它是我们最熟悉的朋友，俗称“单例池”“容器”，缓存创建完成单例Bean的地方。
- **singletonFactories** 映射创建Bean的原始工厂
- ***earlySingletonObjects*** 映射Bean的早期引用，也就是说在这个Map里的Bean不是完整的，甚至还不能称之为“Bean”，只是一个Instance.

后两个Map其实是“垫脚石”级别的，只是创建Bean的时候，用来借助了一下，创建完成就清掉了。

所以笔者前文对“三级缓存”这个词有些迷惑，可能是因为注释都是以Cache of开头吧。

为什么成为后两个Map为垫脚石，假设最终放在singletonObjects的Bean是你想要的一杯“*凉白开*”。

那么Spring准备了两个杯子，即*singletonFactories*和*earlySingletonObjects*来回“倒腾”几番，把热水晾成“*凉白开*”放到singletonObjects中。

闲话不说，都浓缩在图

![img](https://mmbiz.qpic.cn/mmbiz_gif/eQPyBffYbucDDhbXgfTTWqBWQNKVBpf9hpSzTCxNEgb7MwMdm4KqtjZua2fE9tZUey62kyST1AibiageWn8uQJWA/640?wx_fmt=gif&tp=webp&wxfrom=5&wx_lazy=1)





### IOC 容器

- BeanFactory - BeanFactory 就像一个包含 bean 集合的工厂类。它会在客户端要求时实例化 bean。
- ApplicationContext - ApplicationContext 接口扩展了 BeanFactory 接口。它在 BeanFactory 基础上提供了一些额外的功能。

  ![image-20200428210402245](https://user-gold-cdn.xitu.io/2020/6/10/1729ea364d208135?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### spring bean

- 它们是构成用户应用程序主干的对象。
- Bean 由 Spring IoC 容器管理。
- 它们由 Spring IoC 容器实例化，配置，装配和管理。
- Bean 是基于用户提供给容器的配置元数据创建。



### **Spring中的bean作用域**

我：Spring中的Bean有五种作用域：

1. **singleton**：唯一Bean实例，Spring中的Bean默认都是单例的。
2. **prototype**：每次请求都会创建一个新的bean实例。
3. **request**：每次HTTP请求都会产生一个新的Bean，该Bean仅在当前HTTP request内有效。
4. **session**：每次HTTP请产生一个新的Bean，该Bean仅在当前HTTP session内有效。
5. global-session：全局session作用域，仅仅在基于portlet的web应用中才有意义，**Spring5已经没有了**。

### springbean的生命周期

1. Bean容器找到配置文件中Spring Bean的定义。
2. Bean容器利用Java反射机制创建一个Bean的实例。
3. 如果涉及一些属性值，利用set()方法设置一些属性值。
4. 如果Bean实现了BeanNameAware接口，调用setBeanName（）方法，传入Bean的名称。
5. 如果Bean实现了BeanClassLoaderAware接口，调用setBeanClassLoader（）方法，传入ClassLoader对象的实例。
6. 如果Bean实现了BeanFactoryAware接口，调用setBeanClassLoader()方法，传入ClassLoader对象的实例。
7. 与上面类似，如果实现了其他*.Aware接口，就调用相应的方法。
8. 如果有和加载这个Bean的Spring容器相关的BeaPostProcessor对象，执行postProcessBeforeInitialization（）方法
9. 如果Bean实现了InitializingBean接口，执行afterPropertiesSet（）方法
10. 如果Bean在配置文件中的定义包含init-method属性，执行指定的方法。
11. 如果有和加载这个 Bean的 Spring 容器相关的 BeanPostProcessor 对象，执行postProcessAfterInitialization() 方法
12. 当要销毁Bean的时候，如果 Bean 实现了 DisposableBean 接口，执行 destroy() 方法。
13. 当要销毁 Bean 的时候，如果 Bean 在配置文件中的定义包含 destroy-method 属性，执行指定的方法。





### BeanFactory和ApplicationContext的区别

背景
BeanFactory：
 BeanFactory是spring中比较原始，比较古老的Factory。因为比较古老，所以BeanFactory无法支持spring插件，例如：AOP、Web应用等功能。

ApplicationContext
 ApplicationContext是BeanFactory的子类，因为古老的BeanFactory无法满足不断更新的spring的需求，于是ApplicationContext就基本上代替了BeanFactory的工作，以一种更面向框架的工作方式以及对上下文进行分层和实现继承，并在这个基础上对功能进行扩展：
<1>MessageSource, 提供国际化的消息访问
<2>资源访问（如URL和文件）
<3>事件传递
<4>Bean的自动装配
<5>各种不同应用层的Context实现



## 2.AOP

AOP：Aspect oriented programming 面向切面编程，AOP 是 OOP（面向对象编程）的一种延续。

下面我们先看一个 OOP 的例子。

例如：现有三个类，`Horse`、`Pig`、`Dog`，这三个类中都有 eat 和 run 两个方法。

通过 OOP 思想中的继承，我们可以提取出一个 Animal 的父类，然后将 eat 和 run 方法放入父类中，`Horse`、`Pig`、`Dog`通过继承`Animal`类即可自动获得 `eat()` 和 `run()` 方法。这样将会少些很多重复的代码。![img](https://user-gold-cdn.xitu.io/2020/5/28/1725a6b2d5a302a7?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

 OOP 编程思想可以解决大部分的代码重复问题。但是有一些问题是处理不了的。比如在父类 Animal 中的多个方法的相同位置出现了重复的代码，OOP 就解决不了。 

~~~java
/**
 * 动物父类
 */
public class Animal {

    /** 身高 */
    private String height;

    /** 体重 */
    private double weight;

    public void eat() {
        // 性能监控代码
        long start = System.currentTimeMillis();

        // 业务逻辑代码
        System.out.println("I can eat...");

        // 性能监控代码
        System.out.println("执行时长：" + (System.currentTimeMillis() - start)/1000f + "s");
    }

    public void run() {
        // 性能监控代码
        long start = System.currentTimeMillis();

        // 业务逻辑代码
        System.out.println("I can run...");

        // 性能监控代码
        System.out.println("执行时长：" + (System.currentTimeMillis() - start)/1000f + "s");
    }
}

~~~

 这部分重复的代码，一般统称为 **横切逻辑代码**。 

![img](https://user-gold-cdn.xitu.io/2020/5/28/1725a6b2d5a2c5ab?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

横切逻辑代码存在的问题：

- 代码重复问题
- 横切逻辑代码和业务代码混杂在一起，代码臃肿，不变维护

**AOP 就是用来解决这些问题的**

AOP 另辟蹊径，提出横向抽取机制，将横切逻辑代码和业务逻辑代码分离

![img](https://user-gold-cdn.xitu.io/2020/5/28/1725a6b2d5fad550?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

代码拆分比较容易，难的是如何在不改变原有业务逻辑的情况下，悄无声息的将横向逻辑代码应用到原有的业务逻辑中，达到和原来一样的效果。

### AOP 解决了什么问题

通过上面的分析可以发现，AOP 主要用来解决：在不改变原有业务逻辑的情况下，增强横切逻辑代码，根本上解耦合，避免横切逻辑代码重复。

### AOP 为什么叫面向切面编程

**切** ：指的是横切逻辑，原有业务逻辑代码不动，只能操作横切逻辑代码，所以面向横切逻辑

**面** ：横切逻辑代码往往要影响的是很多个方法，每个方法如同一个点，多个点构成一个面。这里有一个面的概念

### 什么是 Aspect？

aspect 由 pointcount 和 advice 组成, 它既包含了横切逻辑的定义, 也包括了连接点的定义. Spring AOP 就是负责实施切面的框架, 它将切面所定义的横切逻辑编织到切面所指定的连接点中.

AOP 的工作重心在于如何将增强编织目标对象的连接点上, 这里包含两个工作:

1. 如何通过 pointcut 和 advice 定位到特定的 joinpoint 上
2. 如何在 advice 中编写切面代码.

### 切点（JoinPoint）

程序运行中的一些时间点, 例如一个方法的执行, 或者是一个异常的处理.

在 Spring AOP 中, join point 总是方法的执行点。

### 通知（Advice）

特定 JoinPoint 处的 Aspect 所采取的动作称为 Advice。Spring AOP 使用一个 Advice 作为拦截器，在 JoinPoint “周围”维护一系列的拦截器。

**通知的类型**

- **Before** - 这些类型的 Advice 在 joinpoint 方法之前执行，并使用 @Before 注解标记进行配置。
- **After Returning** - 这些类型的 Advice 在连接点方法正常执行后执行，并使用@AfterReturning 注解标记进行配置。
- **After Throwing** - 这些类型的 Advice 仅在 joinpoint 方法通过抛出异常退出并使用 @AfterThrowing 注解标记配置时执行。
- **After (finally)** - 这些类型的 Advice 在连接点方法之后执行，无论方法退出是正常还是异常返回，并使用 @After 注解标记进行配置。
- **Around** - 这些类型的 Advice 在连接点之前和之后执行，并使用 @Around 注解标记进行配置。

### AOP 实现方式

实现 AOP 的技术，主要分为两大类：

- 静态代理 - 指使用 AOP 框架提供的命令进行编译，从而在编译阶段就可生成 AOP 代理类，因此也称为编译时增强；
- 编译时编织（特殊编译器实现）
- 类加载时编织（特殊的类加载器实现）。
- 动态代理 - 在运行时在内存中“临时”生成 AOP 动态代理类，因此也被称为运行时增强。
- JDK 动态代理
- CGLIB

#### 动态代理

在Java中动态代理有**两种**方式：

- JDK动态代理
- CGLib动态代理

JDK动态代理是需要实现某个接口了，而我们类未必全部会有接口，于是CGLib代理就有了~~

- CGLib代理其生成的动态代理对象是目标类的子类
- Spring AOP**默认是使用JDK动态代理**，如果代理的类**没有接口则会使用CGLib代理**。

### Spring AOP and AspectJ AOP 有什么区别？

Spring AOP 基于动态代理方式实现；AspectJ 基于静态代理方式实现。

Spring AOP 仅支持方法级别的 PointCut；提供了完全的 AOP 支持，它还支持属性级别的 PointCut。

## 3.常用注解

![4、常用注解.png](https://user-gold-cdn.xitu.io/2017/5/5/af7643e72876659fbde08b29f7561151?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

### 什么是基于注解的容器配置？

不使用 XML 来描述 bean 装配，开发人员通过在相关的类，方法或字段声明上使用注解将配置移动到组件类本身。它可以作为 XML 设置的替代方案。例如：

Spring 的 Java 配置是通过使用 @Bean 和 @Configuration 来实现。

- @Bean 注解扮演与
- 元素相同的角色。
- @Configuration 类允许通过简单地调用同一个类中的其他 @Bean 方法来定义 bean 间依赖关系。

例如：

```java
@Configuration  public class StudentConfig {  @Bean  public StudentBean myStudent() {  return new StudentBean();  }  }  
```



### 如何在 spring 中启动注解装配？

默认情况下，Spring 容器中未打开注解装配。因此，要使用基于注解装配，我们必须通过配置<context：annotation-config /> 元素在 Spring 配置文件中启用它。@Component, @Controller, @Repository, @Service 区别

- @Component：这将 java 类标记为 bean。它是任何 Spring 管理组件的通用构造型。spring 的组件扫描机制现在可以将其拾取并将其拉入应用程序环境中。
- @Controller：这将一个类标记为 Spring Web MVC 控制器。标有它的 Bean 会自动导入到 IoC 容器中。
- @Service：此注解是组件注解的特化。它不会对 @Component 注解提供任何其他行为。您可以在服务层类中使用 @Service 而不是 @Component，因为它以更好的方式指定了意图。
- @Repository：这个注解是具有类似用途和功能的 @Component 注解的特化。它为 DAO 提供了额外的好处。它将 DAO 导入 IoC 容器，并使未经检查的异常有资格转换为 Spring DataAccessException。

### @Autowired 注解有什么用？

@Autowired 可以更准确地控制应该在何处以及如何进行自动装配。此注解用于在 setter 方法，构造函数，具有任意名称或多个参数的属性或方法上自动装配 bean。默认情况下，它是类型驱动的注入。

~~~java
public class Employee {  private String name;  @Autowired  public void setName(String name) {  this.name=name;  }  public string getName(){  return name;  }  }  
~~~

###  @Required 注解有什么用？

@Required 应用于 bean 属性 setter 方法。此注解仅指示必须在配置时使用 bean 定义中的显式属性值或使用自动装配填充受影响的 bean 属性。如果尚未填充受影响的 bean 属性，则容器将抛出 BeanInitializationException。

~~~java
public class Employee {  private String name;  @Required  public void setName(String name){  this.name=name;  }  public string getName(){  return name;  }  }  
~~~

### @Qualifier 注解有什么用？

当您创建多个相同类型的 bean 并希望仅使用属性装配其中一个 bean 时，您可以使用@Qualifier 注解和 @Autowired 通过指定应该装配哪个确切的 bean 来消除歧义。

例如，这里我们分别有两个类，Employee 和 EmpAccount。在 EmpAccount 中，使用@Qualifier 指定了必须装配 id 为 emp1 的 bean。

~~~java
Employee.java  public class Employee {  private String name;  @Autowired  public void setName(String name) {  this.name=name;  }  public string getName() {  return name;  }  }  EmpAccount.java  public class EmpAccount {  private Employee emp;  @Autowired  @Qualifier(emp1)  public void showName() {  System.out.println(“Employee name : ”+emp.getName);  }  }  
~~~

### @RequestMapping 注解有什么用？

@RequestMapping 注解用于将特定 HTTP 请求方法映射到将处理相应请求的控制器中的特定类/方法。此注释可应用于两个级别：

- 类级别：映射请求的 URL
- 方法级别：映射 URL 以及 HTTP 请求方法





## **4.Spring框架中用到了哪些设计模式**



1. 工厂设计模式：Spring使用工厂模式通过BeanFactory、ApplicationContext创建Bean对象。
2. 代理设计模式：Spring AOP功能的实现。
3. 单例设计模式：Spring中的Bean默认都是单例的。
4. 模板方法模式：Spring中jdbcTemplate、hibernateTemplate等以Template结尾的对数据库操作的类，就是用到了模板模式。
5. 包装器设计模式：我们的项目需要链接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户需求都太切换不同的数据源。
6. 观察者模式：Spring事件驱动模型就是观察者模式很经典的一个应用。
7. 适配器模式：Spring AOP的增强或通知使用到了适配器模式。SpringMVC中也是用到了适配器模式适配Controller。

设计模式

## 5.spring事务

**原子性：** 事务是最小的执行单位，不允许分割。事务的原子性确保动作要么全部完成，要么完全不起作用；

**一致性：** 执行事务前后，数据保持一致；

**隔离性：** 并发访问数据库时，一个用户的事物不被其他事物所干扰，各并发事务之间数据库是独立的；

**持久性:**  一个事务被提交之后。它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有任何影响。

### spring事务隔离级别

TransactionDefinition 接口中定义了五个表示隔离级别的常量：

- **TransactionDefinition.ISOLATION_DEFAULT:**	使用后端数据库默认的隔离级别，Mysql 默认采用的 REPEATABLE_READ隔离级别 Oracle 默认采用的 READ_COMMITTED隔离级别.
- **TransactionDefinition.ISOLATION_READ_UNCOMMITTED:** 最低的隔离级别，允许读取尚未提交的数据变更，**可能会导致脏读、幻读或不可重复读**
- **TransactionDefinition.ISOLATION_READ_COMMITTED:** 	允许读取并发事务已经提交的数据，**可以阻止脏读，但是幻读或不可重复读仍有可能发生**
- **TransactionDefinition.ISOLATION_REPEATABLE_READ:** 	对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，**可以阻止脏读和不可重复读，但幻读仍有可能发生。**
- **TransactionDefinition.ISOLATION_SERIALIZABLE:** 	最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，**该级别可以防止脏读、不可重复读以及幻读**。但是这将严重影响程序的性能。通常情况下也不会用到该级别。

### 事务传播行为（为了解决业务层方法之间互相调用的事务问题）：

当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如：方法可能继续在现有事务中运行，也可能开启一个新事务，并在自己的事务中运行。在TransactionDefinition定义中包括了如下几个表示传播行为的常量：

**支持当前事务的情况：**

- **TransactionDefinition.PROPAGATION_REQUIRED：** 如果当前存在事务，则加入该事务；如果当前没有事务，则创建一个新的事务。
- **TransactionDefinition.PROPAGATION_SUPPORTS：** 如果当前存在事务，则加入该事务；如果当前没有事务，则以非事务的方式继续运行。
- **TransactionDefinition.PROPAGATION_MANDATORY：** 如果当前存在事务，则加入该事务；如果当前没有事务，则抛出异常。（mandatory：强制性）

**不支持当前事务的情况：**

- **TransactionDefinition.PROPAGATION_REQUIRES_NEW：** 创建一个新的事务，如果当前存在事务，则把当前事务挂起。
- **TransactionDefinition.PROPAGATION_NOT_SUPPORTED：** 以非事务方式运行，如果当前存在事务，则把当前事务挂起。
- **TransactionDefinition.PROPAGATION_NEVER：** 以非事务方式运行，如果当前存在事务，则抛出异常。

**其他情况：**

- **TransactionDefinition.PROPAGATION_NESTED：** 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务来运行；如果当前没有事务，则该取值等价于TransactionDefinition.PROPAGATION_REQUIRED。



### @Transactional事务注解

一、特性
先来了解一下@Transactional注解事务的特性吧，可以更好排查问题

1、service类标签(一般不建议在接口上)上添加@Transactional，可以将整个类纳入spring事务管理，在每个业务方法执行时都会开启一个事务，不过这些事务采用相同的管理方式。

2、@Transactional 注解只能应用到 public 可见度的方法上。 如果应用在protected、private或者 package可见度的方法上，也不会报错，不过事务设置不会起作用。

3、默认情况下，Spring会对unchecked异常进行事务回滚；如果是checked异常则不回滚。
辣么什么是checked异常，什么是unchecked异常

~~~
java里面将派生于Error或者RuntimeException（比如空指针，1/0）的异常称为unchecked异常，
其他继承自java.lang.Exception得异常统称为Checked Exception，如IOException、TimeoutException等
~~~

 你写代码出现的空指针等异常，会被回滚，文件读写，网络出问题，spring就没法回滚了。 

### Transactional注解不回滚/事务失效

- **方法是不是public**

  入口的方法必须是public，否则事务不起作用（这一点由Spring的AOP特性决定. 从原理上来说，动态代理是通过接口实现，所以自然不能支持private和protect方法的。 ）private方法，final方法和static方法不能添加事务，加了也不生效。

- 你的异常类型是**不是unchecked异常**

  Spring事务管理默认只对出**现运行时异常**（kava.lang.RuntimeException及其子类）进行回滚（至于Spring为什么这么设计：因为Spring认为Checked异常属于业务，程序员应该给出解决方案而不应该直接扔给框架）。
  如果我想check异常也想回滚怎么办，注解上面写明异常类型即可

~~~java
@Transactional(rollbackFor=Exception.class) 
~~~

类似的还有norollbackFor，自定义不回滚的异常数据库引擎要支持事务，如果是MySQL，注意表要使用支持事务的引擎，比如innodb，如果是MyISAM，事务是不起作用的



- ### catch异常无法回滚

在3.3节中我们说到只有抛出非检查异常或是`rollbackFor`中指定的异常才能触发回滚。如果我们把异常catch住，而且没抛出，则会导致无法触发回滚，这也是开发中常犯的错误。例如：

```java
@Transactional
public void insert(List<User> users) {
    try {
        JdbcTemplate jdbcTemplate = new JdbcTemplate(dataSource);
        for (User user : users) {
            String insertUserSql = "insert into User (id, name) values (?,?)";
            jdbcTemplate.update(insertUserSql, new Object[] { user.getId(),
                                                             user.getName() });
        }
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

- 是否开启了对注解的解析  @EnableTransactionManagement或xml配置

```xml
<tx:annotation-driven transaction-manager="transactionManager" proxy-target-class="true"/>
```



- spring是否扫描到你这个包，如下是扫描到org.test下面的包

~~~ xml
<context:component-scan base-package="org.test" ></context:component-scan>
~~~

检查是不是同一个类中的方法调用（如a方法调用同一个类中的b方法）

        - 异常是不是被你catch住了.

- 请确认你的类是否被代理了。（因为Spring的事务实现原理是AOP，只有通过代理对象调用方法才能被拦截，事务才能生效）。

- 请确保你的业务和事务入口在同一个线程里，否则事务也是不生效的，比如下面的代码：

~~~java
@Transactional
@Override
public void save(User user1, User user2) {
    new Thread(() -> {
          saveError(user1, user2);
          System.out.println(1 / 0);
    }).start();
}

~~~



- 同一个类中一个无事务的方法调用另一个有事务的方法，事务是不会起作用的。例如下面的代码：



![img](https://user-gold-cdn.xitu.io/2020/3/15/170dc223ca569107?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)







