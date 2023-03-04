# Spring系列

## Spring

### IoC和AOP的理解

**IoC**（控制反转）
一种设计思想，是将原本在程序中手动创建对象的控制权、对象之间的相互依赖关系，交由Spring的ioc容器来管理；
IoC和DI有什么关系呢？其实它们是同一个概念的不同角度描述；
依赖注入指对象被动地接受依赖类而不用自己主动去找，实例化对象时主动将它依赖的类注入给它；
优点：无侵入式应用解耦，提高开发效率；
ioc注解：Controller、Service、Repository（dao层）、Component（通用）把类交给ioc容器管理；
ComponentScan：根据指定的扫描路径，把路径中符合扫描规则的类装配到ioc容器中；
di注解：Autowire（按类型注）或者Resource（按照Bean的id注入），用于获取属性的实例；
**AOP**（面向切面编程）代理模式的典型应用；
好处：可以在不修改现有代码的情况下对现有代码增加一些通用功能，增加程序的模块化，降低代码耦合度；

<img src="https://s2.loli.net/2023/03/04/QaKIePLFyO8wmNH.png" alt="Snipaste_2023-03-02_00-42-23.png" style="zoom:50%;" />
**Spring AOP核心概念**：
1、切面Aspect：切面一般定义为一个Java类，包含通知和切点；
2、切点Pointcut：可以插入增强处理的连接点；
3、连接点Joinpoint：连接点表示应用执行过程中能够插入切面的一个点，这个点可以是方法的调用；
4、通知Advice：描述了切面何时执行以及如何执行增强处理；Around类型需要主动去触发目标方法的执行；
常用注解：AfterThrowing，该Advice会在方法抛出异常以后执行；用于代码监控，异常后发送告警邮件；EnableAspectJAutoProxy
**Spring AOP使用流程**：
1、新建切面类，用Component注解将其交给ioc容器管理；
2、使用Aspect注解将其标记为一个切面；然后在该类中定义切点，通知；
**Spring AOP底层代理方式**
**JDK动态代理**：在运行期，目标类加载后，为接口动态生成代理类，将切面织入到代理类；
如果目标对象实现了接口，默认情况下会采用JDK的动态代理；
缺点：切入的关注点需要实现接口
**CGLIB动态代理**：在运行期，目标类加载后，动态生成目标类的子类（字节码提升），将切面逻辑加入到子类中；
如果目标对象没有实现了接口，必须采用CGLIB；
缺点：不能代理被final修饰的类，也不能重写final或者private修饰的方法；
**为什么JDK动态代理只能基于接口代理**
因为JDK动态代理生成的代理对象需要继承Proxy这个类，Java中类只能是单继承，无法再继承一个代理类；

### SpringBean的作用范围（作用域）

singleton单例：默认，Spring只为每个在IOC容器里声明的bean创建唯一实例，整个IOC容器范围内都能共享；
prototype原型：每次Bean请求都会创建一个新对象；线程安全
request请求：每次Http请求创建一个新对象；
session会话：同一个会话共享一个实例，不同会话使用不用的实例；
global-session全局会话：所有会话共享一个实例；

### Spring单例bean的线程安全问题

Spring中单例Bean是否是线程安全取决于Bean是有状态的Bean还是无状态的Bean；
有状态Bean跟无状态Bean
有状态的Bean：有数据存储功能；如本地属性存储传入的值
无状态的Bean：无数据存储功能；
解决方案：
1、声明该bean的作用域为prototype原型模式；
2、类中定义ThreadLocal成员变量（推荐的一种方式）；
ThreadLocal：线程的局部变量，也就是说一个ThreadLocal的变量只有当前自身线程可以访问，别的线程都访问不了，那么自然就避免了线程竞争；
ThreadLocal提供了一种与众不同的线程安全方式，它不是在发生线程冲突时想办法解决冲突，而是彻底的避免了冲突的发生；

## MyBatis

半自动ORM（对象关系映射）框架，它内部封装了JDBC，开发时只需要关注SQL语句本身；

### #{}和${}的区别

#{}是预编译处理，${}是字符串替换；
Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；
Mybatis在处理${}时，就是把${}替换成变量的值；
使用#{}可以有效的防止SQL注入，提高系统安全性。

### 结果集的映射方式

自动映射 ，通过resultType来指定要映射的类型即可。  
自定义映射 通过resultMap来完成具体的映射规则，指定将结果集中的哪个列映射到对象的哪个属性。

### 获取自动生成主键值

在<insert>标签中使用 useGeneratedKeys   和  keyProperty 两个属性来获取自动生成的主键值。

### 常用标签

set：配合update使用；
foreach：遍历传入的集合；
choose when otherwise：相当于if/elseif；

### MyBatis 执行批处理

使用 BatchExecutor 完成批处理

## SpringBoot

### Spring Boot和Spring 

SpringBoot是Spring开源组织下的子项目，是Spring组件一站式解决方案，主要是简化了使用Spring的难度，简省了繁重的配置；
核心注解SpringBootApplication ComponentScan
优点（相比Spring）简化配置 自带Tomcat 无代码生成和XML配置

### Spring Cloud（Nacos+Sentinel）

SpringBoot是快速开发的Spring框架，SpringCloud是完整的微服务框架，SpringCloud依赖于SpringBoot。

### Spring Boot和Spring Cloud是什么关系

Spring Boot 是 Spring 的一套快速配置脚手架，可以基于Spring Boot 快速开发单个微服务，Spring Cloud是一个基于Spring Boot实现的开发工具；Spring Boot专注于快速、方便集成的单个微服务个体，Spring Cloud关注全局的服务治理框架； Spring Boot使用了默认大于配置的理念，很多集成方案已经帮你选择好了，能不配置就不配置，Spring Cloud很大的一部分是基于Spring Boot来实现，必须基于Spring Boot开发。
 可以单独使用Spring Boot开发项目，但是Spring Cloud离不开 Spring Boot。

### 微服务优缺点

降低系统耦合
适合大型团队开发，个人可以只关注某个微服务中的业务；
容易维护，某个服务故障时，只需要对一个服务修改代码并重启；
微服务能使用不同的语言开发
缺点：开发人员需要理解概念，系统复杂度增加；

### Nacos（服务注册与发现、配置中心）

核心功能：
1、服务注册、服务发现：@EnableDiscoveryClient
2、服务健康检查：客户端通过发送心跳包，5秒发送一次，如果15秒没有回应，则说明服务出现了问题，如果30秒后没有回应，则说明服务已经停止则将该服务删除；
3、配置中心：使用原生注解@Value()直接读取，Nacos自动刷新配置需要在配置类加@RefreshScope注解；
Nacos与Eureka的区别：
Nacos无需自己构建注册中心服务；
健康检测：Nacos对临时实例采用心跳模式检测，对永久实例采用主动请求来检测；Eureka只支持心跳模式；
服务发现：Nacos支持定时拉取和订阅推送两种模式；Eureka只支持定时拉取模式；
Nacos配置中心宕机：以前已经调用过的接口依旧可以被调用，因为Nacos客户端会缓存调用信息；

### OpenFeign

底层内置了Ribbon
通过 Feign，我们可以像调用本地方法一样来调用远程服务，简化了调用流程；
OpenFeign 的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务；
启动类注解@EnableFeignClients开启openFeign功能
feign和openfeign的区别
feign：Netflix公司出品，不支持Spring MVC的注解，停止维护；
openfeign：Spring出品，支持Spring MVC的注解，springboot 2.0 以上基本上使用openfeign；

### Sentinel（服务熔断）

实现服务熔断降级处理，保护微服务，防止雪崩效应发生
把流量作为切入点，从流量控制、熔断降级、系统负载保护等多个维度保护服务的稳定性；
在@FeignClient中添加fallback属性，属性值是降级回调的类；
Sentinel的线程隔离与Hystix