## Spring：

Spring 是一种轻量级开发框架，它是很多模块的集合，这些模块是：核心容器、数据访问/集成,、Web、AOP（面向切面编程）、工具、消息和测试模块。

Spring框架提供面向切面编程，可以很方便地对程序进行拦截，监控地声明式事务，比如日记管理，事务管理。而且只需要通过配置就可以完成对事务地管理，无需对手编程，还支持其他框架，另外一点就是IOC,将对象地创建权交给Spring容器进行管理，不需要自己去new一个对象出来。

![](C:\Users\祥仔\AppData\Roaming\marktext\images\2019-08-31-17-59-52-image.png)

- **ioc 控制反转**

将原本在程序中手动创建对象的控制权，交由Spring框架来管理。将对象之间的相互依赖关系交给 IoC 容器来管理，并由 IoC 容器完成对象的注入。这样可以很大程度上简化应用的开发，把应用从复杂的依赖关系中解放出来。

IoC 容器是 Spring 用来实现 IoC 的载体， IoC 容器实际上就是个Map（key，value）,Map 中存放的是各种对象。

通过配置xml文件或配置注解配置**Bean**

- **aop 面向切面编程**

AOP(Aspect-Oriented Programming:面向切面编程)能够将那些与业务无关，**却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来**，便于**减少系统的重复代码**，**降低模块间的耦合度**，并**有利于未来的可拓展性和可维护性**。

使用 AOP 之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样大大简化了代码量。我们需要增加新功能时也方便，这样也提高了系统扩展性。日志功能、事务管理等等场景都用到了 AOP 。

Spring AOP就是基于动态代理的,比如Cglib，AspectJ。AspectJ 应该算的上是 Java 生态系统中最完整的 AOP 框架了。

## Spring 中的 bean 的作用域有哪些?

- singleton : 唯一 bean 实例，Spring 中的 bean 默认都是单例的。
- prototype : 每次请求都会创建一个新的 bean 实例。
- request : 每一次HTTP请求都会产生一个新的bean，该bean仅在当前HTTP request内有效。
- session : 每一次HTTP请求都会产生一个新的 bean，该bean仅在当前 HTTP session 内有效。
- global-session： 全局session作用域，仅仅在基于portlet的web应用中才有意义，Spring5已经没有了。Portlet是能够生成语义代码(例如：HTML)片段的小型Java Web插件。它们基于portlet容器，可以像servlet一样处理HTTP请求。但是，与 servlet 不同，每个 portlet 都有不同的会话

## Spring 中的单例 bean 的线程安全问题

单例 bean 存在线程问题，主要是因为当多个线程操作同一个对象的时候，对这个对象的非静态成员变量的写操作会存在线程安全问题。

常见的有两种解决办法：

1. 在Bean对象中尽量避免定义可变的成员变量（不太现实）。

2. 在类中定义一个ThreadLocal成员变量，将需要的可变成员变量保存在 ThreadLocal 中（推荐的一种方式）。

## Spring 中的 bean 生命周期

## 对于 Spring MVC的 了解

MVC 是一种设计模式,Spring MVC 是一款很优秀的 MVC 框架。Spring MVC 可以帮助我们进行更简洁的Web层的开发，并且它天生与 Spring 框架集成。Spring MVC 下我们一般把后端项目分为 Service层（处理业务）、Dao层（数据库操作）、Entity层（实体类）、Controller层(控制层，返回数据给前台页面)。

**Spring MVC 的简单原理图如下：**

![](C:\Users\祥仔\AppData\Roaming\marktext\images\2019-08-31-13-41-22-image.png)

## SpringMVC 工作原理

![](C:\Users\祥仔\AppData\Roaming\marktext\images\2019-08-31-13-52-58-image.png)

**流程说明（重要）：**

1、  用户发送请求至前端控制器DispatcherServlet。

2、  DispatcherServlet收到请求调用HandlerMapping处理器映射器。

3、  处理器映射器找到具体的处理器(可以根据xml配置、注解进行查找)，生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet。

4、  DispatcherServlet调用HandlerAdapter处理器适配器。

5、  HandlerAdapter经过适配调用具体的处理器(Controller，也叫后端控制器)。

6、  Controller执行完成返回ModelAndView。

7、  HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet。

8、  DispatcherServlet将ModelAndView传给ViewReslover视图解析器。

9、  ViewReslover解析后返回具体View。

10、DispatcherServlet根据View进行渲染视图（即将模型数据填充至视图中）。

11、 DispatcherServlet响应用户。

## Spring 框架中用到了哪些设计模式？

关于下面一些设计模式的详细介绍，可以看笔主前段时间的原创文章[《面试官:“谈谈Spring中都用到了那些设计模式?”。》](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485303&idx=1&sn=9e4626a1e3f001f9b0d84a6fa0cff04a&chksm=cea248bcf9d5c1aaf48b67cc52bac74eb29d6037848d6cf213b0e5466f2d1fda970db700ba41&token=255050878&lang=zh_CN#rd)。

- **工厂设计模式**: Spring使用工厂模式通过`BeanFactory`、`ApplicationContext`创建 bean 对象。
- **代理设计模式**: Spring AOP 功能的实现。
- **单例设计模式**: Spring 中的 Bean 默认都是单例的。
- **模板方法模式**: Spring 中`jdbcTemplate`、`hibernateTemplate`等以 Template 结尾的对数据库操作的类，它们就使用到了模板模式。
- **包装器设计模式**: 我们的项目需要连接多个数据库，而且不同的客户在每次访问中根据需要会去访问不同的数据库。这种模式让我们可以根据客户的需求能够动态切换不同的数据源。
- **观察者模式**:Spring 事件驱动模型就是观察者模式很经典的一个应用。
- **适配器模式**:Spring AOP 的增强或通知(Advice)使用到了适配器模式、spring MVC 中也是用到了适配器模式适配`Controller`。

# Spring 生成 Bean 的时候三种方式

- 默认**无参的构造方法**

- **静态工厂** 提供一个工厂类 定义返回bean的静态函数 在xml文件配置

- **实例工厂** 提供一个工厂类 定义返回bean的非静态函数 在xml文件配置

# Spring 的 Bean 的属性注入（DI技术实现方式-向类里面注入属性）

- 构造方法注入

- set方法注入

- 接口注入（spring不支持）
