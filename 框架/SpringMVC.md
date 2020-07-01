# SpringMVC

原理

 Spring Web MVC 框架提供 **模型-视图-控制器**(Modle-View-Controller) 架构和随时可用的组件，用于开发灵活且松散耦合的 Web 应用程序。 MVC 模式有助于分离应用程序的不同方面，如输入逻辑，业务逻辑和 UI 逻辑，同时在所有这些元素之间提供松散耦合 





## SpringMVC工作流程

![img](https://user-gold-cdn.xitu.io/2018/12/11/1679c3a59497593b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

![img](https://user-gold-cdn.xitu.io/2018/12/11/1679c3c9cf1def7b?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



![img](https://user-gold-cdn.xitu.io/2018/12/11/1679c3c51136aeb9?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)



1. 向服务器发送 HTTP 请求，请求被前端控制器 DispatcherServlet 捕获。
2. DispatcherServlet 根据 **-servlet.xml** 中的配置对请求的 URL 进行解析，得到请求资源标识符（URI）。然后根据该 URI，调用 HandlerMapping 获得该 Handler 配置的所有相关的对象（包括 Handler 对象以及 Handler 对象对应的拦截器），最后以HandlerExecutionChain 对象的形式返回。
3. DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter。（附注：如果成功获得HandlerAdapter后，此时将开始执行拦截器的 preHandler(...)方法）。
4. 提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller)。 在填充Handler的入参过程中，根据你的配置，Spring 将帮你做一些额外的工作：

- HttpMessageConveter： 将请求消息（如 Json、xml 等数据）转换成一个对象，将对象转换为指定的响应信息。
- 数据转换：对请求消息进行数据转换。如String转换成Integer、Double等。
- 数据根式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等。
- 数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中。

1. Handler(Controller)执行完成后，向 DispatcherServlet 返回一个ModelAndView 对象；
2. 根据返回的ModelAndView，选择一个适合的 ViewResolver（必须是已经注册到 Spring 容器中的ViewResolver)返回给DispatcherServlet。
3. ViewResolver 结合Model和View，来渲染视图。
4. 视图负责将渲染结果返回给客户端。



## Spring MVC和 struts 的区别

![img](https://user-gold-cdn.xitu.io/2017/10/28/ab2648fd768293a6fa61959ba1dbbc40?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)