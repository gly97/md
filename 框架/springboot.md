默认目录结构(spring默认扫描)

~~~
com    
  +- example
    +- myproject
      +- Application.java
      |
      +- domain  //com.example.myproject.domain包：用于定义实体映射关系与数据访问相关的接口和实现
      |  +- Customer.java
      |  +- CustomerRepository.java
      |
      +- service  //com.example.myproject.service包：用于编写业务逻辑相关的接口与实现
      |  +- CustomerService.java
      |
      +- web  //com.example.myproject.web：用于编写Web层相关的实现，比如：Spring MVC的Controller等
      |  +- CustomerController.java
      |
~~~

自定义的话,需要自己定义扫描

**方法一**：使用`@ComponentScan`注解指定具体的加载包，比如：

```java
@SpringBootApplication
@ComponentScan(basePackages="com.example")
public class Bootstrap {

    public static void main(String[] args) {
        SpringApplication.run(Bootstrap.class, args);
    }

}
```

这种方法通过注解直接指定要扫描的包，比较直观。如果有这样的需求也是可以用的，但是原则上还是推荐以上面的典型结构来定义，这样也可以少写一些注解，代码更加简洁。

**方法二**：使用`@Bean`注解来初始化，比如：

```
@SpringBootApplication
public class Bootstrap {

    public static void main(String[] args) {
        SpringApplication.run(Bootstrap.class, args);
    }

    @Bean
    public CustomerController customerController() {
        return new CustomerController();
    }

}
```

配置文件

配置文件位于src/main/resources 

 Spring Boot的配置文件除了可以使用传统的properties文件之外，还支持现在被广泛推荐使用的YAML文件。 

~~~ymal
environments:
    dev:
        url: http://dev.bar.com
        name: Developer Setup
    prod:
        url: http://foo.bar.com
        name: My Cool App
~~~

等价于

~~~
environments.dev.url=http://dev.bar.com
environments.dev.name=Developer Setup
environments.prod.url=http://foo.bar.com
environments.prod.name=My Cool App
~~~

- `@Controller`：修饰class，用来创建处理http请求的对象
- `@RestController`：Spring4之后加入的注解，原来在`@Controller`中返回json需要`@ResponseBody`来配合，如果直接用`@RestController`替代`@Controller`就不需要再配置`@ResponseBody`，默认返回json格式
- `@RequestMapping`：配置url映射。现在更多的也会直接用以Http Method直接关联的映射注解来定义，比如：`GetMapping`、`PostMapping`、`DeleteMapping`、`PutMapping`等

@Data  自动生成get/set  -- 出自Lombok插件 