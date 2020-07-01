# Shiro



 Apache Shiro™是一个强大且易用的 Java 安全框架，可以执行身份验证、授权、加密和会话管理等。基于 Shiro 的易于理解的API，您可以快速、轻松地使任何应用程序变得安全（从最小的移动应用到最大的网络和企业应用）。 



![img](https://user-gold-cdn.xitu.io/2019/8/8/16c6ec40d1fff54c?imageslim)



## shiro核心

#### Subject

它是一个主体，代表了当前“用户”，这个用户不一定是一个具体的人，与当前应用交互的任何东西都是Subject，如网络爬虫，机器人等；即一个抽象概念；所有 Subject 都绑定到 SecurityManager，与 Subject 的所有交互都会委托给SecurityManager；可以把 Subject 认为是一个门面；SecurityManager 才是实际的执行者

#### SecurityManager

安全管理器；即所有与安全有关的操作都会与 SecurityManager 交互；且它管理着所有 Subject；可以看出它是 Shiro 的核心，它负责与后边介绍的其他组件进行交互，如果学习过 SpringMVC，你可以把它看成 DispatcherServlet前端控制器

#### Realm

域，Shiro 从 Realm 获取安全数据（如用户、角色、权限），就是说 SecurityManager 要验证用户身份，那么它需要从 Realm 获取相应的用户进行比较以确定用户身份是否合法；也需要从 Realm 得到用户相应的角色/权限进行验证用户是否能进行操作；可以把 Realm 看成 DataSource，即安全数据源。



## shiro详解




![img](https://user-gold-cdn.xitu.io/2019/8/8/16c6ec40f885f098?imageslim)

#### Subject

主体，可以看到主体可以是任何可以与应用交互的 “用户”

#### SecurityManager

相当于 SpringMVC 中的 DispatcherServlet；是 Shiro 的心脏；所有具体的交互都通过 SecurityManager 进行控制；它管理着所有 Subject、且负责进行认证和授权、及会话、缓存的管理

#### Authenticator

认证器，负责主体认证的，这是一个扩展点，如果用户觉得 Shiro 默认的不好，可以自定义实现；需要自定义认证策略（Authentication Strategy），即什么情况下算用户认证通过了

#### Authrizer

授权器，或者访问控制器，用来决定主体是否有权限进行相应的操作；即控制着用户能访问应用中的哪些功能

#### Realm

可以有 1 个或多个 Realm，可以认为是安全实体数据源，即用于获取安全实体的；可以是JDBC实现，也可以是LDAP实现，或者内存实现等等；由用户提供；注意：Shiro 不知道你的用户/权限存储在哪及以何种格式存储；所以我们一般在应用中都需要实现自己的Realm

#### SessionManager

如果写过 Servlet 就应该知道 Session 的概念，Session 需要有人去管理它的生命周期，这个组件就是 SessionManager；而Shiro 并不仅仅可以用在 Web 环境，也可以用在如普通的 JavaSE 环境、EJB等环境；所以，Shiro 就抽象了一个自己的Session 来管理主体与应用之间交互的数据；这样的话，比如我们在 Web 环境用，刚开始是一台Web服务器；接着又上了台EJB 服务器；这时又想把两台服务器的会话数据放到一个地方，我们就可以实现自己的分布式会话（如把数据放到Memcached 服务器）

#### SessionDAO

DAO大家都用过，数据访问对象，用于会话的 CRUD，比如我们想把 Session 保存到数据库，那么可以实现自己的SessionDAO，通过如JDBC写到数据库；比如想把 Session 放到 Memcached 中，可以实现自己的 Memcached SessionDAO；另外 SessionDAO 中可以使用 Cache 进行缓存，以提高性能；

#### CacheManager

缓存控制器，来管理如用户、角色、权限等的缓存的；因为这些数据基本上很少去改变，放到缓存中后可以提高访问的性能

#### Cryptography

密码模块，Shiro提高了一些常见的加密组件用于如密码「加密/解密」的



