#   SpringBoot2核心技术-基础入门

## 01、Spring与SpringBoot

### 1、Spring能做什么

#### 1.1、Spring的能力

> - 微服务开发
> - 响应式编程
>   - 异步非阻塞
> - 分布式云开发
> - web应用
>   - SpringMVC
> - 无服务开发
>   - Faas(函数式服务)
> - 事件驱动
> - 批处理业务

![img](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602641710418-5123a24a-60df-4e26-8c23-1d93b8d998d9.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_41%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

#### 1.2、Spring的生态

https://spring.io/projects/spring-boot

覆盖了：

web开发

数据访问

安全控制

分布式

消息服务

移动开发

批处理

.....

#### 1.3、Spring5重大升级

##### 1.3.1、响应式编程

> - 响应式开发数据访问
> - 响应式开发web应用
> - 响应式开发安全应用
> - 使用少量的线程，占用少量的资源，处理大量的并发

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602642309979-eac6fe50-dc84-49cc-8ab9-e45b13b90121.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

##### 1.3.2、内部源码设计

基于Java8的一些新特性，如：接口默认实现（颠覆了适配器设计模式）。重新设计源码架构。

### 2、为什么用SpringBoot

> Spring Boot makes it easy to create stand-alone, production-grade Spring based Applications that you can "just run".
>
> ##### 能快速创建出生产级别的Spring应用

#### 2.1、SpringBoot优点

- Create stand-alone Spring applications

- - 创建独立Spring应用，替换原有的Spring开发的繁琐流程

- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)

- - 内嵌web服务器

- Provide opinionated 'starter' dependencies to simplify your build configuration

- - 自动starter依赖，简化构建配置
  - 版本控制，简化依赖配置

- Automatically configure Spring and 3rd party libraries whenever possible

- - 自动配置Spring以及第三方功能（连redis、MySQL）
  - 不用像以前整合MVC和Mybatis这些配置

- Provide production-ready features such as metrics, health checks, and externalized configuration

- - 提供生产级别的监控、健康检查及外部化配置

- Absolutely no code generation and no requirement for XML configuration

- - 无代码生成、无需编写XM

> SpringBoot是整合Spring技术栈的一站式框架
>
> SpringBoot是简化Spring技术栈的快速开发脚手架

#### 2.2、SpringBoot缺点

- 人称版本帝，迭代快，需要时刻关注变化
- 封装太深，内部原理复杂，不容易精通

### 3、时代背景

#### 3.1、微服务

[James Lewis and Martin Fowler (2014)](https://martinfowler.com/articles/microservices.html)  提出微服务完整概念。https://martinfowler.com/microservices/

> In short, the **microservice architectural style** is an approach to developing a single application as a **suite of small services**, each **running in its own process** and communicating with **lightweight** mechanisms, often an **HTTP** resource API. These services are **built around business capabilities** and **independently deployable** by fully **automated deployment** machinery. There is a **bare minimum of centralized management** of these services, which may be **written in different programming languages** and use different data storage technologies.-- [James Lewis and Martin Fowler (2014)](https://martinfowler.com/articles/microservices.html)

- 微服务是一种架构风格
- 一个应用拆分为一组小型服务
- 每个服务运行在自己的进程内，也就是可独立部署和升级
- 服务之间使用轻量级HTTP交互
- 服务围绕业务功能拆分
- 可以由全自动部署机制独立部署
- 去中心化，服务自治。服务可以使用不同的语言、不同的存储技术

#### 3.2、分布式

![img](https://cdn.nlark.com/yuque/0/2020/png/1613913/1599562347965-a617a866-4270-44e9-9c5b-ced552683eda.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_37%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fresize%2Cw_750%2Climit_0)

**分布式的困难**

- 远程调用
- 服务发现
- 负载均衡
- 服务容错
- 配置管理
- 服务监控
- 链路追踪
- 日志管理
- 任务调度
- ......

**分布式的解决**

- SpringBoot + SpringCloud

![img](https://cdn.nlark.com/yuque/0/2020/png/1613913/1599799119457-841ef47a-6585-4ca4-8e3d-8298e796012c.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_10%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_25%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fresize%2Cw_750%2Climit_0)

#### 3.3、云原生

原生应用如何上云。 Cloud Native

**上云的困难**

- 服务自愈
- 弹性伸缩
- 服务隔离
- 自动化部署
- 灰度发布
- 流量治理
- ......

**上云的解决**

![img](https://cdn.nlark.com/yuque/0/2020/png/1613913/1599563498261-8b0b4d86-bd9b-49a3-aefc-89696a375dcb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_14%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_29%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fresize%2Cw_750%2Climit_0)

### 4、如何学习SpringBoot

#### 4.1、官网文档架构

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602654700738-b6c50c90-0649-4d62-98d3-57658caf0fdb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_50%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10%2Fresize%2Cw_750%2Climit_0)

查看版本新特性；

https://github.com/spring-projects/spring-boot/wiki#release-notes

## 02、SpringBoot2入门

### 1、系统要求

- [Java 8](https://www.java.com/) & 兼容java14 .
- Maven 3.3+
- idea 2019.1.2

#### 1.1、maven设置

> 在maven路径的conf里的setting.xml设置

```xml
<mirrors>
  <mirror>
    <id>nexus-aliyun</id>
    <mirrorOf>central</mirrorOf>
    <name>Nexus aliyun</name>
    <url>http://maven.aliyun.com/nexus/content/groups/public</url>
  </mirror>
</mirrors>
 
<profiles>
     <profile>
          <id>jdk-1.8</id>
          <activation>
            <activeByDefault>true</activeByDefault>
            <jdk>1.8</jdk>
          </activation>
          <properties>
            <maven.compiler.source>1.8</maven.compiler.source>
            <maven.compiler.target>1.8</maven.compiler.target>
            <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>
          </properties>
     </profile>
</profiles>
```

### 2、HelloWorld

#### 2.1、创建maven工程

#### 2.2、引入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cjj</groupId>
    <artifactId>boot-01-helloworld</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <!--父工程-->
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.3.4.RELEASE</version>
    </parent>

    <!--导入依赖：web场景启动器-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>

    <build>
        <!--加入插件后，可以直接将boot项目打成jar包在服务器上运行-->
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.3.4.RELEASE</version>
            </plugin>
        </plugins>
    </build>
</project>
```

#### 2.3、创建主程序

> 主程序类一定要在最外侧，他只会扫描它所在的那个包

```java
package com.cjj;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

/**
 * 主程序类
 * @SpringBootApplication：这是一个SpringBoot应用
 */
@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class,args);
    }
}

```

#### 2.4、编写业务

```java
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String handle01(){
        return "Hello, Spring Boot 2";
    }

}
```

#### 2.5、测试

直接运行main方法

#### 2.6、简化配置

> 在resource包下创建application.properties文件

```properties
server.port=8888
```

#### 2.7、简化部署

```xml
<!--加入插件后，可以直接将boot项目打成jar包在服务器上运行-->
<plugins>
    <plugin>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-maven-plugin</artifactId>
        <version>2.3.4.RELEASE</version>
    </plugin>
</plugins>
```

把项目打成jar包，直接在目标服务器执行即可。

## 03、了解自动配置原理

### 1、SpringBoot特点

#### 1.1、依赖管理

- 父项目做依赖管理

```xml
<!--父工程-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.4.RELEASE</version>
</parent> 
<!--父工程的父项目,内部几乎声明了所有开发中常用的依赖的版本号,自动版本仲裁机制-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-dependencies</artifactId>
    <version>2.3.4.RELEASE</version>
</parent>
```

- 开发导入starter场景启动器

> starter是一组依赖的描述

```xml
<!--
1、见到很多 spring-boot-starter-* ： *代表某种场景
2、只要引入starter，这个场景的所有常规需要的依赖我们都自动引入（Maven的特性）
3、SpringBoot所有支持的场景
https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter
4、见到的  *-spring-boot-starter： 第三方为我们提供的简化开发的场景启动器。
5、所有场景启动器最底层的依赖
-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter</artifactId>
  <version>2.3.4.RELEASE</version>
  <scope>compile</scope>
</dependency>
```

![image-20230701221059105](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230701221059105.png)

- 无需关注版本号，自动版本仲裁

```xml
<!--
1、引入依赖默认都可以不写版本
2、引入非版本仲裁的jar，要写版本号。
——>
```

- 可以修改默认版本号

```xml
<!--如果想自定义依赖版本可以使用以下标签-->
<properties>
    <mysql.version>5.1.43</mysql.version>
</properties>
```

#### 1.2、自动配置

- 自动配好Tomcat

- - 引入web开发场景后默认引入Tomcat依赖。

- ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
  </dependency>
  ```

- - 配置Tomcat

- 自动配好SpringMVC
  
- 引入web开发场景后自动帮我们引入SpringMVC开发的全套组件。
  
- - 引入SpringMVC全套组件

- - 自动配好SpringMVC常用组件（功能）

- 自动配好Web常见功能，如：字符编码问题

- - SpringBoot帮我们配置好了所有web开发的常见场景

    - DispatcherServlet
    - 编码过滤器
    - 文件上传解析器
    - ...

    ```java
    /**
     * 主程序类
     * @SpringBootApplication：这是一个SpringBoot应用
     */
    @SpringBootApplication
    public class MainApplication {
        public static void main(String[] args) {
            // 获取IOC容器
            ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
            // 查看容器里的组件
            String[] names = run.getBeanDefinitionNames();
            for (String name : names) {
                System.out.println(name);
            }
        }
    }
    ```

- 默认的包扫描

- - 主程序所在包及其下面的所有子包里面的组件都会被默认扫描进来

- - 无需以前的包扫描配置

- - 想要改变扫描路径

    - 使用`@SpringBootApplication(scanBasePackages="com.atguigu")`指定路径
    - 或者`@ComponentScan `指定扫描路径

    ```java
    // 这三个注解式合成注解 相当于 @SpringBootApplication
    @SpringBootConfiguration
    @EnableAutoConfiguration
    @ComponentScan("com.cjj")
    public class MainApplication {
        public static void main(String[] args) {
            // 获取IOC容器
            ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
            // 查看容器里的组件
            String[] names = run.getBeanDefinitionNames();
            for (String name : names) {
                System.out.println(name);
            }
        }
    }
    ```

- 各种配置拥有默认值

- - 默认配置最终都是映射到某个类上或属性里，如：MultipartProperties
  - `application.properties`配置文件的键值对最终会以某种规则被识别然后绑定每个类或类中的方法中，这个类会在容器中创建对象

- 按需加载所有自动配置项

- - 有非常多的starter场景
  - 引入了哪些场景这个场景的自动配置才会开启
  - SpringBoot所有的自动配置功能都在 `spring-boot-autoconfigure `包里面

- ```xml
  <dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-autoconfigure</artifactId>
    <version>2.3.4.RELEASE</version>
    <scope>compile</scope>
  </dependency>
  ```

- ![image-20230701230205258](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230701230205258.png)

### 2、容器功能

#### 2.1、组件添加（注册组件）

使用传统xml文件配置bean如下所示，但是在SprintBoot中我们可以给一个类上加上@Configuration注解来作为我们的配置类，这个配置类就相当于我们的xml配置文件。可以给配置类中的方法加上@Bean注解，以方法名作为bean标签的id，返回类型作为bean标签的class类型，return作为组件在容器中的实例。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="user01" class="com.cjj.boot.bean.User">
        <property name="name" value="user01"></property>
        <property name="age" value="18"></property>
    </bean>
    
    <bean id="cat" class="com.cjj.boot.bean.Pet">
        <property name="name" value="tomcat"></property>
    </bean>
</beans>
```

##### 1、@Configuration

- 基本使用

```java
// 配置类也是容器内的一个组件
@Configuration
public class MyConfig {

    // 给容器中添加组件，方法名作为组件的id，返回类型就是组件类型，return的值就是组件在容器中的实例
    @Bean
    public User user01(){
        return new User("zhangsan",19);
    }

    // @Bean标签的属性值value:指定id
    @Bean("tom")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }

}
```

- **Full模式与Lite模式**

  > 当在Spring的配置类中设置`proxyBeanMethods = true`时，默认情况下，`@Bean`注解标记的方法将使用CGLIB代理进行增强。这意味着Spring将在容器中创建代理对象，并拦截对该方法的调用。
  >
  > 代理对象的创建通常是在首次调用代理对象的方法时发生的，而不是在容器初始化时。所以，当你在配置类中的不同方法上使用`@Bean`注解时，每个方法都会创建一个独立的代理对象。
  >
  > 这里涉及到两个关键概念：
  >
  > 1. CGLIB代理：在默认情况下，当使用`proxyBeanMethods = true`时，Spring会使用CGLIB库来创建代理对象。CGLIB代理是一种基于继承的动态代理技术，它创建一个子类来扩展原始类，并覆盖其中的方法以提供额外的功能，比如实现单例、AOP增强等。
  > 2. 方法调用：当调用带有`@Bean`注解的方法时，实际上是通过代理对象来调用的。代理对象会拦截这些方法调用，并根据需要执行相应的逻辑。在这种情况下，代理对象会检查容器中是否已经存在该组件的实例。如果存在，则返回现有的实例；如果不存在，则调用相应的方法创建一个新的实例，并将其保存在容器中以便后续使用。
  >
  > 总结一下，当你在配置类中使用`@Bean`注解并设置`proxyBeanMethods = true`时，每个带有`@Bean`注解的方法都会创建一个独立的代理对象。这些代理对象负责管理组件的生命周期，并提供单例实例的功能。所以，虽然两个方法可能返回相同类型的对象，但它们实际上是由不同的代理对象管理的，因此在容器中是不同的实例。

  - 示例

  ```java
  /**
   *  1.配置类也是容器内的一个组件
   *  2.配置类里使用@Bean注解在方法上给容器注册组件，默认也是single单例的
   *  3.@Configuration的属性：proxyBeanMethods默认为true:是不是代理bean的方法
   *      Full全配置:(proxyBeanMethods = true) 单例，返回值指向的都是同一个对象
   *      Lite轻量级配置:(proxyBeanMethods = false) 非单例
    */
  
  @Configuration(proxyBeanMethods = true)
  public class MyConfig {
  
      // 给容器中添加组件，方法名作为组件的id，返回类型就是组件类型，return的值就是组件在容器中的实例
      @Bean
      public User user01(){
          User zhangsan = new User("zhangsan",19);
          // user组件依赖Pet组件
          zhangsan.setPet(tomcatPet());
          return zhangsan;
      }
  
      // @Bean标签的属性值value:指定id
      @Bean("tom")
      public Pet tomcatPet(){
          return new Pet("tomcat");
      }
  
  }
  ```

  ```java
  /**
   * 主程序类
   * @SpringBootApplication：这是一个SpringBoot应用
   */
  @SpringBootApplication
  public class MainApplication {
      public static void main(String[] args) {
          // 获取IOC容器
          ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
          // bean是一个被spring增强了的代理对象
          MyConfig bean = run.getBean(MyConfig.class);
          // com.cjj.boot.config.MyConfig$$EnhancerBySpringCGLIB$$36877fc5@4e6d7365
          System.out.println(bean);
          User user = bean.user01();
          User user1 = bean.user01();
          System.out.println(user == user1); // ture
  
          User user01 = run.getBean("user01",User.class);
          Pet tom = run.getBean("tom",Pet.class);
          System.out.println("用户的宠物："+(user01.getPet() == tom))// true
      }
  }
  ```

  - @Configuration(proxyBeanMethods = true)时，最后输出为true
  - @Configuration(proxyBeanMethods = false)时，最后输出为false

- 最佳实战

  - Lite：SpringBoot就不会检查@Bean标识方法的返回值在容器中有没有，加速了容器启动过程，减少判断
  - Full：配置类组件之间有依赖关系，方法会被调用得到之前单实例组件，用Full模式

##### 2、其它注册组件注解

> - @Bean
>
>   ```java
>   @Configuration
>   public class MyConfig {
>   
>       @Bean
>       public User user() {
>           return new User();
>       }
>   
>       @Bean("user01")
>       public User user01() {
>           return new User();
>       }
>   }
>   ```
>
>   在Spring中，`@Bean`方法的ID默认情况下是根据方法名生成的，因此在你的示例中，`user()`方法和`user01()`方法实际上注册了相同的ID `"user"`。这种情况下，后面定义的方法会覆盖前面的方法。
>
>   因此，你在使用`run.getBean("user01", User.class)`获取组件时，实际上获取的是`user01()`方法返回的`User`组件，而不是`user()`方法返回的组件。所以输出的结果是正常的。
>
>   这是由于Spring使用方法名作为默认的组件ID，并且ID必须是唯一的。在同一个配置类中，不能有两个方法具有相同的方法名，并且使用`@Bean`注解注册组件时，也不能有两个组件具有相同的ID。
>
> - @Component 
> - @Controller 
> - @Service 
> - @Repository

> - @ComponentScan
>
> - @Import：在组件类像controller类、config配置类上使用该注解，给容器中导入组件  
>
>   ```java
>   @Import({User.class}) 
>   @Configuration(proxyBeanMethods = true)
>   public class MyConfig {
>   
>       // 给容器中添加组件，方法名作为组件的id，返回类型就是组件类型，return的值就是组件在容器中的实例
>       @Bean
>       public User user(){
>           User zhangsan = new User("zhangsan",19);
>           // user组件依赖Pet组件
>           zhangsan.setPet(tomcatPet());
>           return zhangsan;
>       }
>   
>       // @Bean标签的属性值value:指定id
>       @Bean("tom")
>       public Pet tomcatPet(){
>           return new Pet("tomcat");
>       }
>   
>   }
>   ```
>
>   - import导入的组件id名默认为该类的`全类名`
>   - @Import 高级用法： https://www.bilibili.com/video/BV1gW411W7wy?p=8

> - @Conditional：条件装配，满足条件时才进行组件装配
>
>   - 扫描组件从上往下依次扫描，注意配置顺序
>   - 注解加在类上，则所有方法生效
>     - @ConditionalOnBean：容器中有指定bean则加载组件
>     - @ConditionalOnMissingBean：容器中没有指定bean则加载组件
>
>   ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602835786727-28b6f936-62f5-4fd6-a6c5-ae690bd1e31d.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_17%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)
>
>   ```java
>   @Import({User.class})
>   @Configuration(proxyBeanMethods = true)
>   public class MyConfig {
>   
>       // 给容器中添加组件，方法名作为组件的id，返回类型就是组件类型，return的值就是组件在容器中的实例
>       @Bean
>       // 条件注解：代码执行到此处会先记录忽略，等容器加载完其他组件，在来判断是否注册此组件
>       @ConditionalOnBean(name = "tom")  // 容器中有tom则加载，反之则不加载
>       public User user(){
>           User zhangsan = new User("zhangsan",19);
>           // user组件依赖Pet组件
>           zhangsan.setPet(tomcatPet());
>           return zhangsan;
>       }
>   
>       // @Bean标签的属性值value:指定id
>       @Bean("tom")
>       public Pet tomcatPet(){
>           return new Pet("tomcat");
>       }
>   
>   }
>   
>   public static void main(String[] args) {
>           //1、返回我们IOC容器
>           ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
>   
>           //2、查看容器里面的组件
>           String[] names = run.getBeanDefinitionNames();
>           for (String name : names) {
>               System.out.println(name);
>           }
>   
>           boolean tom = run.containsBean("tom");
>           System.out.println("容器中Tom组件："+tom);
>   
>           boolean user01 = run.containsBean("user01");
>           System.out.println("容器中user01组件："+user01);
>   
>           boolean tom22 = run.containsBean("tom22");
>           System.out.println("容器中tom22组件："+tom22);
>   
>   
>       }
>   ```

#### 2.2、原生配置文件引入

> 如果对于一些配置好的xml文件，里面的bean程序员不想自己一个个迁移成@Bean，就可以使用`@ImportResource`注解，由SpringBoot为我们解析并将xml中的bean放到容器里面

##### 1、@ImportResource

```java
@Import({User.class})
@Configuration(proxyBeanMethods = true)
@ImportResource("classpath:test.xml") // 类路径
public class MyConfig {

    // 给容器中添加组件，方法名作为组件的id，返回类型就是组件类型，return的值就是组件在容器中的实例
    @Bean
    // 条件注解：代码执行到此处会先记录忽略，等容器加载完其他组件，在来判断是否注册此组件
    @ConditionalOnBean(name = "tom")  // 容器中有tom则加载，反之则不加载
    public User user(){
        User zhangsan = new User("zhangsan",19);
        // user组件依赖Pet组件
        zhangsan.setPet(tomcatPet());
        return zhangsan;
    }

    // @Bean标签的属性值value:指定id
    @Bean("tom")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }

}
```

```java
package com.cjj.boot;

import com.cjj.boot.bean.Pet;
import com.cjj.boot.bean.User;
import com.cjj.boot.config.MyConfig;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

/**
 * 主程序类
 * @SpringBootApplication：这是一个SpringBoot应用
 */
@SpringBootApplication
// @SpringBootConfiguration
// @EnableAutoConfiguration
// @ComponentScan("com.cjj")
public class MainApplication {
    public static void main(String[] args) {
        // 获取IOC容器
        ConfigurableApplicationContext run = SpringApplication.run(MainApplication.class, args);
        boolean haha = run.containsBean("haha");
        boolean hehe = run.containsBean("hehe");
        System.out.println("haha："+haha);//true
        System.out.println("hehe："+hehe);//true
    }
}
```

#### 2.3、配置绑定

如何使用Java读取到properties文件中的内容，并且把它封装到JavaBean中，以供随时使用；

```java
// 原生的做法
public class getProperties {
     public static void main(String[] args) throws FileNotFoundException, IOException {
         Properties pps = new Properties(); // 获取pro对象
         pps.load(new FileInputStream("a.properties")); // 读取加载到内存中
         Enumeration enum1 = pps.propertyNames();//得到配置文件的名字
         while(enum1.hasMoreElements()) {
             String strKey = (String) enum1.nextElement();
             String strValue = pps.getProperty(strKey);
             System.out.println(strKey + "=" + strValue);
             //封装到JavaBean。
         }
     }
 }
```

##### 1、@ConfigurationProperties

> - 使用注解前需要给要标识的类加载到容器中，这样它才能使用Spring提供的强大的功能
> - @ConfigurationProperties的属性：prefix
>   - 绑定properties文件里的前缀
>   - 这样properties文件里的键值对就能够自动和JavaBean里的属性一一绑定

```java
@Component
// prefix:properties文件里的前缀
@ConfigurationProperties(prefix = "mycar")
public class Car {

    private String brand;

    private Double price;

    public Car() {
    }

    public Car(String brand, Double price) {
        this.brand = brand;
        this.price = price;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}
```

##### 2、@EnableConfigurationProperties + @ConfigurationProperties

> - @EnableConfigurationProperties：开启属性配置功能，写在配置类上。
>   - 属性：Class类型，开启该类的配置绑定功能，把这个组件自动注册到容器中。
>   - 一般用于注册第三方包

```java
@Import({User.class})
@Configuration(proxyBeanMethods = true)
@ImportResource("classpath:test.xml")
// 开启Car类的属性自动绑定功能，并注册该组件到容器中
@EnableConfigurationProperties(Car.class)
public class MyConfig {

    // 给容器中添加组件，方法名作为组件的id，返回类型就是组件类型，return的值就是组件在容器中的实例
    @Bean
    // 条件注解：代码执行到此处会先记录忽略，等容器加载完其他组件，在来判断是否注册此组件
    @ConditionalOnBean(name = "tom")  // 容器中有tom则加载，反之则不加载
    public User user(){
        User zhangsan = new User("zhangsan",19);
        // user组件依赖Pet组件
        zhangsan.setPet(tomcatPet());
        return zhangsan;
    }

    // @Bean标签的属性值value:指定id
    @Bean("tom")
    public Pet tomcatPet(){
        return new Pet("tomcat");
    }
}
```

```java
// prefix:properties文件里的前缀
@ConfigurationProperties(prefix = "mycar")
public class Car {

    private String brand;

    private Double price;

    public Car() {
    }

    public Car(String brand, Double price) {
        this.brand = brand;
        this.price = price;
    }

    public String getBrand() {
        return brand;
    }

    public void setBrand(String brand) {
        this.brand = brand;
    }

    public Double getPrice() {
        return price;
    }

    public void setPrice(Double price) {
        this.price = price;
    }

    @Override
    public String toString() {
        return "Car{" +
                "brand='" + brand + '\'' +
                ", price=" + price +
                '}';
    }
}
```

### 3、自动配置原理入门

@SpringBootApplication注解详解

#### 3.1、引导加载自动配置类

> 它是一个合成注解，如下

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    Class<?>[] exclude() default {};

    @AliasFor(
        annotation = EnableAutoConfiguration.class
    )
    String[] excludeName() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackages"
    )
    String[] scanBasePackages() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "basePackageClasses"
    )
    Class<?>[] scanBasePackageClasses() default {};

    @AliasFor(
        annotation = ComponentScan.class,
        attribute = "nameGenerator"
    )
    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

    @AliasFor(
        annotation = Configuration.class
    )
    boolean proxyBeanMethods() default true;
}
```

1. `@SpringBootConfiguration`

   > 其内部@Configuration标识其本质上还是一个配置类

   ```java
   @Configuration
   public @interface SpringBootConfiguration {
       @AliasFor(
           annotation = Configuration.class
       )
       boolean proxyBeanMethods() default true;
   }
   ```

2. `@ComponentScan`

   > 指定要扫描哪些包

3. `@EnableAutoConfiguration`

   ```java
   @AutoConfigurationPackage
   @Import({AutoConfigurationImportSelector.class})
   public @interface EnableAutoConfiguration {
       String ENABLED_OVERRIDE_PROPERTY = "spring.boot.enableautoconfiguration";
   
       Class<?>[] exclude() default {};
   
       String[] excludeName() default {};
   }
   ```

   - `@AutoConfigurationPackage`：自动配置包?指定了默认的包规则

     ```java
     @Import({Registrar.class}) // import 给容器中导入一个组件
     public @interface AutoConfigurationPackage {
         String[] basePackages() default {};
     
         Class<?>[] basePackageClasses() default {};
     }
     ```

     > - 利用Registrar组件的方法给容器中导入一系列组件。
     >
     >   ```java
     >   static class Registrar implements ImportBeanDefinitionRegistrar, DeterminableImports {
     >       Registrar() {
     >       }
     >       // 获取该注解的源信息：注解标识的哪个类？
     >       public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
     >           // 注解标识在主类上，可以获取主类所在的包名并封装到一个数组中
     >           // 把该包下的所有组件给注册进来
     >           AutoConfigurationPackages.register(registry, (String[])(new AutoConfigurationPackages.PackageImports(metadata)).getPackageNames().toArray(new String[0]));
     >       }
     >       public Set<Object> determineImports(AnnotationMetadata metadata) {
     >           return Collections.singleton(new AutoConfigurationPackages.PackageImports(metadata));
     >       }
     >   }
     >   ```
     >
     >   

   - `@Import({AutoConfigurationImportSelector.class})`：在Spring Boot中，AutoConfigurationEntry是用来指定在启动应用程序时所执行的自动配置代码的方式和规则。这些自动配置代码负责根据应用程序的配置和依赖自动初始化各种组件、库、数据库连接等。

     > - 1、利用getAutoConfigurationEntry(annotationMetadata);给容器中批量导入一些组件
     > - 2、调用List<String> configurations = getCandidateConfigurations(annotationMetadata, attributes)获取到所有需要导入到容器中的配置类
     > - 3、利用工厂加载 Map<String, List<String>> loadSpringFactories(@Nullable ClassLoader classLoader)；得到所有的组件
     > - 4、从META-INF/spring.factories位置来加载一个文件
     >   - 默认扫描我们当前系统里面所有META-INF/spring.factories位置的文件
     >   - spring-boot-autoconfigure-2.3.4.RELEASE.jar包里面也有META-INF/spring.factorie

     > - 文件里面写死了spring-boot一启动就要给容器中加载的所有配置类127个
     >
     > ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602845382065-5c41abf5-ee10-4c93-89e4-2a9b831c3ceb.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_29%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

     ```java
     // 选择导入
     public String[] selectImports(AnnotationMetadata annotationMetadata) {
         if (!this.isEnabled(annotationMetadata)) {
             return NO_IMPORTS;
         } else {
             // 获取自动配置集合，给容器中批量导入一些组件
             AutoConfigurationImportSelector.AutoConfigurationEntry autoConfigurationEntry = this.getAutoConfigurationEntry(annotationMetadata);
             return StringUtils.toStringArray(autoConfigurationEntry.getConfigurations());
         }
     }
     ```

     ```java
     // 获取自动配置集合
     protected AutoConfigurationImportSelector.AutoConfigurationEntry getAutoConfigurationEntry(AnnotationMetadata annotationMetadata) {
         if (!this.isEnabled(annotationMetadata)) {
             return EMPTY_ENTRY;
         } else {
             AnnotationAttributes attributes = this.getAttributes(annotationMetadata);
            // 获取所有候选的配置
             List<String> configurations = this.getCandidateConfigurations(annotationMetadata, attributes);
             configurations = this.removeDuplicates(configurations);
             Set<String> exclusions = this.getExclusions(annotationMetadata, attributes);
             this.checkExcludedClasses(configurations, exclusions);
             configurations.removeAll(exclusions);
             configurations = this.getConfigurationClassFilter().filter(configurations);
             this.fireAutoConfigurationImportEvents(configurations, exclusions);
             // 最后返回的configurations是所有需要导入到容器中的配置类（组件）
             return new AutoConfigurationImportSelector.AutoConfigurationEntry(configurations, exclusions);
         }
     }
     ```

#### 3.2、按需开启自动配置项

虽然我们127个场景的所有自动配置启动的时候默认全部加载。xxxxAutoConfiguration按照条件装配规则（@Conditional），最终会按需配置。

像我们的`spring-boot-autoconfigure-2.3.4.RELEASE.jar`下的DispatcherSevletAutoconfiguration自动配置类，满足条件则开启了此配置项，由SpringBoot帮我们创建了该类型的对象并注册到了容器中。

```java
@AutoConfigureOrder(-2147483648) // 优先级
@Configuration( // 配置类
    proxyBeanMethods = false
)
@ConditionalOnWebApplication( // 条件装配
    type = Type.SERVLET
)
@ConditionalOnClass({DispatcherServlet.class}) // 条件装配，有DispatcherServlet类才装配
@AutoConfigureAfter({ServletWebServerFactoryAutoConfiguration.class}) // 在加载完指定类后再装配
// 配置类
public class DispatcherServletAutoConfiguration {
    public static final String DEFAULT_DISPATCHER_SERVLET_BEAN_NAME = "dispatcherServlet";
    public static final String DEFAULT_DISPATCHER_SERVLET_REGISTRATION_BEAN_NAME = "dispatcherServletRegistration";

    public DispatcherServletAutoConfiguration() {
    }
	
    @Configuration( // 配置类
    proxyBeanMethods = false
)
    // 条件装配
@Conditional({DispatcherServletAutoConfiguration.DefaultDispatcherServletCondition.class})
@ConditionalOnClass({ServletRegistration.class})
    // 开启WebMvcProperties类的属性自动绑定功能，并注册该组件到容器中
@EnableConfigurationProperties({WebMvcProperties.class})
    // 内部类
protected static class DispatcherServletConfiguration {
    protected DispatcherServletConfiguration() {
    }
    @Bean(
        name = {"dispatcherServlet"}
    )
    // 进行初始化操作，并给容器中注册DispatcherServlet类型的组件
    public DispatcherServlet dispatcherServlet(WebMvcProperties webMvcProperties) {
        DispatcherServlet dispatcherServlet = new DispatcherServlet();
       dispatcherServlet.setDispatchOptionsRequest(webMvcProperties.isDispatchOptionsRequest());
        dispatcherServlet.setDispatchTraceRequest(webMvcProperties.isDispatchTraceRequest());
        dispatcherServlet.setThrowExceptionIfNoHandlerFound(webMvcProperties.isThrowExceptionIfNoHandlerFound());
        dispatcherServlet.setPublishEvents(webMvcProperties.isPublishRequestHandledEvents());
        dispatcherServlet.setEnableLoggingRequestDetails(webMvcProperties.isLogRequestDetails());
        return dispatcherServlet;
    }
}
```

#### 3.3 、修改默认配置

修改用户自己注册的组件名称

```java
    @Bean
	@ConditionalOnBean(MultipartResolver.class)  //容器中有这个类型组件
	@ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME) //容器中没有这个名字 multipartResolver 的组件
	public MultipartResolver multipartResolver(MultipartResolver resolver) {
        //给@Bean标注的方法传入了对象参数，这个参数的值就会从容器中找。
        //SpringMVC multipartResolver。防止有些用户配置的文件上传解析器不符合规范
		// Detect if the user has created a MultipartResolver but named it incorrectly
		return resolver;
	}
```
SpringBoot默认会在底层配好所有的组件，但是如果用户自己配置了以用户的优先

```java
// 用户可以自己在配置类中写自己的编码过滤器，这样spring就会加载用户配置的bean而不是默认自动配置的了
@Bean
@ConditionalOnMissingBean
public CharacterEncodingFilter characterEncodingFilter() {
    
}
```

总结：

- SpringBoot先加载所有的自动配置类  xxxxxAutoConfiguration
- 每个自动配置类按照条件进行生效，默认都会绑定配置文件指定的值。xxxxProperties.class里面拿（在配置类的方法新参中定义XxxProperties xxxProperties来获取配置文件里的值）。
- 生效的配置类就会给容器中装配很多组件
- 只要容器中有这些组件，相当于这些功能就有了
- 定制化配置

- - 用户直接自己@Bean替换底层的组件
  - 用户去看这个组件是获取的配置文件什么值就去修改。

**xxxxxAutoConfiguration 导入---> 组件  --->** 从xxxxProperties里面拿值  ---->xxxxProperties封装的值从application.properties获取

#### 3.4、最佳实践

- 引入场景依赖

- - https://docs.spring.io/spring-boot/docs/current/reference/html/using-spring-boot.html#using-boot-starter

- 查看自动配置了哪些（选做）

- - 自己分析，引入场景对应的自动配置一般都生效了
    - 引入web场景，web场景相关的配置就自动配好了
  - application.properties配置文件中添加debug=true开启自动配置报告。
    - Negative（不生效）\Positive（生效）

- 是否需要修改

- - 参照文档修改配置项

- - - https://docs.spring.io/spring-boot/docs/current/reference/html/appendix-application-properties.html#common-application-properties
    - 自己分析。xxxxProperties绑定了配置文件的哪些前缀。

- - 自定义加入或者替换组件

- - - @Bean、@Component。。。

- - 自定义器  **XXXXXCustomizer**；
  - ......

### 4、开发小技巧

#### 4.1、Lombok

简化JavaBean开发

```xml
<!--lombok 要安装idea插件lombok-->
<!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
```

```java
===============================简化JavaBean开发===================================
@NoArgsConstructor // 无参构造器
//@AllArgsConstructor // 全参构造器
@Data // 相当于getter和setter + toString
@ToString // toString
@EqualsAndHashCode
public class User {

    private String name;
    private Integer age;

    private Pet pet;

    public User(String name,Integer age){
        this.name = name;
        this.age = age;
    }


}



================================简化日志开发===================================
@Slf4j // 引入日志类
@RestController 
public class HelloController {
    @RequestMapping("/hello")
    public String handle01(@RequestParam("name") String name){
        
        // 直接使用log属性打印日志信息，不占性能
        log.info("请求进来了....");
        
        return "Hello, Spring Boot 2!"+"你好："+name;
    }
}
```

#### 4.2、dev-tools

项目或者页面修改以后：Ctrl+F9；重新部署、

`JRebel`付费插件可以真正做到web项目的热更新

```xml
<!--devtools-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

#### 4.3、Spring Initailizr（项目初始化向导）

建项目、导依赖

![image-20230703221554342](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230703221554342.png)

##### 0、选择我们需要的开发场景

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602922147241-73fb2496-e795-4b5a-b909-a18c6011a028.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_37%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

##### 1、自动依赖引入

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602921777330-8fc5c198-75da-4ff9-b82c-71ee3fe18af8.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_28%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

##### 2、自动创建项目结构

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602921758313-5099fe18-4c7b-4417-bf6f-2f40b9028296.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_18%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

##### 3、自动编写好主配置类

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1354552/1602922039074-79e98aad-8158-4113-a7e7-305b57b0a6bf.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_29%2Ctext_YXRndWlndS5jb20g5bCa56GF6LC3%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)

# SpringBoot2核心技术-核心功能

![image-20230703223015424](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230703223040445.png)

## 04、配置文件

优先读取properties文件里的内容

### 1、文件类型

#### 1.1、properties

同以前的properties用法

#### 1.2、yaml

##### 1.2.1、简介

YAML 是 "YAML Ain't Markup Language"（YAML 不是一种标记语言）的递归缩写。在开发的这种语言时，YAML 的意思其实是："Yet Another Markup Language"（仍是一种标记语言）。 

非常适合用来做以数据为中心的配置文件

##### 1.2.2、基本语法

- key: value；kv之间有空格
- 大小写敏感
- 使用缩进表示层级关系
- 缩进不允许使用tab，只允许空格
- 缩进的空格数不重要，只要相同层级的元素左对齐即可
- '#'表示注释
- 字符串无需加引号，如果要加，''与""表示字符串内容 会被 转义/不转义

##### 1.2.3、数据类型

- 字面量：单个的、不可再分的值。date、boolean、string、number、null

```yaml
k: v
```

- 对象：键值对的集合。map、hash、set、object 

```yaml
行内写法：k: {k1:v1,k2:v2,k3:v3}
#或
k: 
  k1: v1
  k2: v2
  k3: v3
  
user:
  username: cjj
  age: 18
```

- 数组：一组按次序排列的值。array、list、queue

```yaml
行内写法：  k: [v1,v2,v3]
#或者 -:表示元素 v:表示元素值
k:
 - v1
 - v2
 - v3
```

##### 1.2.4、示例

> Person类

```yaml
person:
  userName: chenjiujia
  boss: true
  birth: 2002/10/5
  age: 20
  # String[] interests || 行内写法: interests: [篮球,足球]
  interests:
    - 篮球
    - 足球
    - 111
  # List<String> animal
  animal: [猫猫,狗狗]
  # Map<String, Object> score
  score: {english: 80,math: 100}
    #english: 80
    #math: 100
  #  Set<Double> salary
  salarys:
    - 1000
    - 1001
  pet:
    name: tom
    weight: 20
  #  Map<String, List<Pet>> allPets
  allPets:
    sick:
      - {name: tom,weight: 10}
      - name: 阿猫
        weight: 100
      - name: 阿狗
        weight: 100
    health:
      - {name: mark,weight: 10}
      - {name: mark,weight: 10}
```

![image-20230704160626608](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230704160626608.png)

### 2、配置提示

自定义的类和配置文件绑定一般没有提示

```xml
    <!--springboot配置注解处理器-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
    </dependency>
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <!--在项目打包的时候去掉这个类，因为它只在编写代码的时候帮助了我们代码补全-->
                <excludes>
                    <exclude>
                        <groupId>org.springframework.boot</groupId>
                        <artifactId>spring-boot-configuration-processor</artifactId>
                    </exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

## 05、Web开发

![image-20230704161449560](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230704161449560.png)

### 1、SpringMVC自动配置概览

Spring Boot provides `auto-configuration` for Spring MVC that **works well with most applications.(大多场景我们都无需自定义配置)**

自动配置包括以下特性：

- Inclusion of `ContentNegotiatingViewResolver` and `BeanNameViewResolver` beans.

- - 内容协商、视图解析器和BeanName视图解析器

- Support for serving static resources, including support for WebJars (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content))).

- - 静态资源（包括webjars），以前需要配mvc default-servlet-handler之类的

- Automatic registration of `Converter`, `GenericConverter`, and `Formatter` beans.

- - 自动注册 `Converter转换器，GenericConverter，Formatter格式化器 (日期)`

- Support for `HttpMessageConverters` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-message-converters)).

- - 支持 `HttpMessageConverters` （后来我们配合内容协商理解原理）

- Automatic registration of `MessageCodesResolver` (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-message-codes)).

- - 自动注册 `MessageCodesResolver` （国际化用的，一般开发两套网站，不怎么用）

- Static `index.html` support.

- - 静态index.html 页支持，自动发现欢迎页

- Custom `Favicon` support (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-favicon)).

- - 自定义 `Favicon`  

- Automatic use of a `ConfigurableWebBindingInitializer` bean (covered [later in this document](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-spring-mvc-web-binding-initializer)).

- - 自动使用 `ConfigurableWebBindingInitializer数据绑定器` ，（DataBinder负责将请求数据绑定到JavaBean上）

> 不用`@EnableWebMvc`注解。使用 `@Configuration` + `WebMvcConfigurer `自定义规则

>声明 `WebMvcRegistrations` 改变默认底层组件

> 使用` @EnableWebMvc`+`@Configuration`+`DelegatingWebMvcConfiguration` 全面接管SpringMVC

### 2、简单功能分析

#### 2.1、静态资源访问

> - 静态资源：js、css、图片、视频

##### 1、静态资源目录

只要静态资源放在类路径下： called `/static` (or `/public` or `/resources` or `/META-INF/resources`

![image-20230704163922258](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230704163922258.png)

访问 ： 当前项目根路径/ + 静态资源名 

原理： 静态映射默认`/**`。拦截所有请求，只需写资源名，会自动找到资源所在位置

请求进来，先去找Controller看能不能处理，不能处理的所有请求又都交给`静态资源处理器`，静态资源也找不到则响应404页面

改变默认的静态资源路径

```yaml
spring:
  mvc:
    static-path-pattern: /res/** #一般静态资源路径都有前缀，方便拦截器放行

  resources:
    # 指定静态资源路径
    static-locations: [classpath:/haha/]
```

##### 2、静态资源访问前缀

默认无前缀

```yaml
spring:
  mvc:
    static-path-pattern: /res/**
```

当前项目 + static-path-pattern + 静态资源名 = 静态资源文件夹下找

##### 3、webjar

自动映射 /[webjars](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)/**

https://www.webjars.org/

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.5.1</version>
</dependency>
```

访问地址：[http://localhost:8080/webjars/**jquery/3.5.1/jquery.js**](http://localhost:8080/webjars/jquery/3.5.1/jquery.js)   后面地址要按照依赖里面的包路径

#### 2.2、欢迎页支持

- 静态资源路径下  index.html

- - 可以配置静态资源路径static-locations
  - 但是不可以配置静态资源的访问前缀static-path-pattern。否则导致 index.html不能被默认访问

- ```yaml
  spring:
  #  mvc:
  #    static-path-pattern: /res/**   这个会导致welcome page功能失效
  
    resources:
      static-locations: [classpath:/haha/]
  ```

- controller能处理/index

#### 2.3、自定义 `Favicon`

favicon.ico 放在静态资源目录下即可。

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**   这个会导致 Favicon 功能失效
```

#### 2.4、静态资源配置原理

- SpringBoot启动默认加载  xxxAutoConfiguration 类（自动配置类）
- SpringMVC功能的自动配置类 WebMvcAutoConfiguration，条件注解生效

```java
// 配置类，且被@bean标识的方法返回的对象没有被代理
@Configuration(proxyBeanMethods = false) 
// 是否为Servlet:true
@ConditionalOnWebApplication(type = Type.SERVLET)
// 加载了这些类:true
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class, WebMvcConfigurer.class })
// 没有加载此类：true
@ConditionalOnMissingBean(WebMvcConfigurationSupport.class)
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE + 10)
@AutoConfigureAfter({ DispatcherServletAutoConfiguration.class, TaskExecutionAutoConfiguration.class,
		ValidationAutoConfiguration.class })
// MVC自动配置类
public class WebMvcAutoConfiguration {}
```

- 配置类生效之后，给容器中配了什么组件。

> - `public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter()`
>   - SpringMVC用来兼容RESTFUL风格，使其可以GET、PUT...等请求

```java
@Bean
@ConditionalOnMissingBean({HiddenHttpMethodFilter.class})
@ConditionalOnProperty(
    prefix = "spring.mvc.hiddenmethod.filter",
    name = {"enabled"},
    matchIfMissing = false
)
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
    return new OrderedHiddenHttpMethodFilter();
}
```

> - `public OrderedFormContentFilter formContentFilter()`
>   - 表单内容过滤器

```java
@Bean
@ConditionalOnMissingBean({FormContentFilter.class})
@ConditionalOnProperty(
    prefix = "spring.mvc.formcontent.filter",
    name = {"enabled"},
    matchIfMissing = true
)
public OrderedFormContentFilter formContentFilter() {
    return new OrderedFormContentFilter();
}
```

##### 1、配置类只有一个有参构造器

> - `WebMvcAutoConfigurationAdapter`
>   - 内部类-配置类

```java
@Configuration(
    proxyBeanMethods = false
)
@Import({WebMvcAutoConfiguration.EnableWebMvcConfiguration.class})
// 配置文件的相关属性和xxx.class里的属性绑定
// WebMvcProperties==spring.mvc
// ResourceProperties==spring.resources
@EnableConfigurationProperties({WebMvcProperties.class, ResourceProperties.class})
@Order(0)
public static class WebMvcAutoConfigurationAdapter implements WebMvcConfigurer {
    private static final Log logger = LogFactory.getLog(WebMvcConfigurer.class);
    private final ResourceProperties resourceProperties;
    private final WebMvcProperties mvcProperties;
    private final ListableBeanFactory beanFactory;
    private final ObjectProvider<HttpMessageConverters> messageConvertersProvider;
    private final ObjectProvider<DispatcherServletPath> dispatcherServletPath;
    private final ObjectProvider<ServletRegistrationBean<?>> servletRegistrations;
    final WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer resourceHandlerRegistrationCustomizer;
    // 一个配置类只有一个有参构造器，有参构造器所有参数的值都会从容器中确定
    public WebMvcAutoConfigurationAdapter(
        // 获取和配置文件绑定过了值的resourceProperties对象
        ResourceProperties resourceProperties, 
        // 获取和配置文件绑定过了值的mvcProperties对象
        WebMvcProperties mvcProperties,
        // beanFactory:Spring的bean工厂也就是IOC容器
        ListableBeanFactory beanFactory,
        // ObjectProvider:对象提供器
        // 从容器中找到所有的messageConvertersProvider
        ObjectProvider<HttpMessageConverters> messageConvertersProvider, 
        // resourceHandlerRegistrationCustomizerProvider:找到资源处理器的自定义器
        ObjectProvider<WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer> resourceHandlerRegistrationCustomizerProvider,
        // dispatcherServletPath:dispatcherServlet能处理的路径
        ObjectProvider<DispatcherServletPath> dispatcherServletPath, 
        // servletRegistrations:给应用注册原生的Servlet、Filter...
        ObjectProvider<ServletRegistrationBean<?>> servletRegistrations
    ) {
        this.resourceProperties = resourceProperties;
        this.mvcProperties = mvcProperties;
        this.beanFactory = beanFactory;
        this.messageConvertersProvider = messageConvertersProvider;
        this.resourceHandlerRegistrationCustomizer = (WebMvcAutoConfiguration.ResourceHandlerRegistrationCustomizer)resourceHandlerRegistrationCustomizerProvider.getIfAvailable();
        this.dispatcherServletPath = dispatcherServletPath;
        this.servletRegistrations = servletRegistrations;
    }
```

##### 2、资源处理的默认规则

> - `public void addResourceHandlers(ResourceHandlerRegistry registry) `
>   - 添加资源处理器，负责所有的资源处理的默认规则

```java
// 容器启动时就要配置好
public void addResourceHandlers(ResourceHandlerRegistry registry) {
    // isAddMappings：返回一个 boolean addMappings;
    // false:禁用静态资源路径映射
    // true:放行
    if (!this.resourceProperties.isAddMappings()) {
        logger.debug("Default resource handling disabled");
    } else {
        // getCache()获取缓存策略对象:final ResourceProperties.Cache cache
        // 负责静态资源的缓存时间
        Duration cachePeriod = this.resourceProperties.getCache().getPeriod();
        CacheControl cacheControl = this.resourceProperties.getCache().getCachecontrol().toHttpCacheControl();
        // webjars的相关规则
        if (!registry.hasMappingForPattern("/webjars/**")) {
            this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{"/webjars/**"}).addResourceLocations(new String[]{"classpath:/META-INF/resources/webjars/"}).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
        }
        // 获取staticPathPattern:默认为/**，也可以在配置文件里修改
        String staticPathPattern = this.mvcProperties.getStaticPathPattern();
        // 静态资源路径配置以及缓存策略
        if (!registry.hasMappingForPattern(staticPathPattern)) {
            this.customizeResourceHandlerRegistration(registry.addResourceHandler(new String[]{staticPathPattern}).addResourceLocations(WebMvcAutoConfiguration.getResourceLocations(this.resourceProperties.getStaticLocations())).setCachePeriod(this.getSeconds(cachePeriod)).setCacheControl(cacheControl));
        }
    }
}

```

```yaml
spring:
#  mvc:
#    static-path-pattern: /res/**

  resources:
    add-mappings: false   # 禁用所有静态资源规则
```

```java
@ConfigurationProperties(prefix = "spring.resources", ignoreUnknownFields = false)
public class ResourceProperties {
	// 类路径下资源访问路径
	private static final String[] CLASSPATH_RESOURCE_LOCATIONS = { "classpath:/META-INF/resources/",
			"classpath:/resources/", "classpath:/static/", "classpath:/public/" };

	/**
	 * Locations of static resources. Defaults to classpath:[/META-INF/resources/,
	 * /resources/, /static/, /public/].
	 */
	private String[] staticLocations = CLASSPATH_RESOURCE_LOCATIONS;
}
```

##### 3、欢迎页的处理规则

> - `HandlerMapping`
>   - 处理器映射，保存了每一个Handler能处理哪些请求
>   - 接受到请求之后，HandlerMapping里会自动匹配控制器，通过反射调用方法

```java
// 注册为容器里的一个组件
@Bean
// 参数值自动从容器里匹配
public WelcomePageHandlerMapping welcomePageHandlerMapping(
    ApplicationContext applicationContext, 
    FormattingConversionService mvcConversionService,
    ResourceUrlProvider mvcResourceUrlProvider
) {
    WelcomePageHandlerMapping welcomePageHandlerMapping
        = new WelcomePageHandlerMapping(
        new TemplateAvailabilityProviders(applicationContext),
        applicationContext,
        this.getWelcomePage(),
        this.mvcProperties.getStaticPathPattern()
    );
    welcomePageHandlerMapping.setInterceptors(
        this.getInterceptors(mvcConversionService, mvcResourceUrlProvider)
    );
    welcomePageHandlerMapping.setCorsConfigurations(
        this.getCorsConfigurations()
    );
    return welcomePageHandlerMapping;
}

```

```java
WelcomePageHandlerMapping(
    TemplateAvailabilityProviders templateAvailabilityProviders,
    ApplicationContext applicationContext,
    Optional<Resource> welcomePage, String staticPathPattern
) {
    // 判断index欢迎页是否存在
    //要用欢迎页功能，必须是/**
    if (welcomePage.isPresent() && "/**".equals(staticPathPattern)) {
        logger.info("Adding welcome page: " + welcomePage.get());
        this.setRootViewName("forward:index.html");
    }
    // 不存在就调用controller
    else if (this.welcomeTemplateExists(templateAvailabilityProviders, applicationContext)) {
        // 调用Controller  /index
        logger.info("Adding welcome page template: index");
        this.setRootViewName("index");
    }

}
```

### 3、请求参数处理

#### 0、请求映射

##### 1、rest使用与原理

- @xxxMapping；
- Rest风格支持（使用HTTP请求方式动词来表示对资源的操作）
  - 以前：/getUser   获取用户     /deleteUser 删除用户    /editUser  修改用户       /saveUser 保存用户
  - 现在： /user    GET-获取用户    DELETE-删除用户     PUT-修改用户      POST-保存用户
  - 核心Filter；HiddenHttpMethodFilter
    - 用法： 表单method=post，隐藏域 _method=put
    - SpringBoot中需要手动开启Rest风格
    - 扩展：如何把_method 这个名字换成我们自己喜欢的。

```java
@RequestMapping(value = "/user",method = RequestMethod.GET)
public String getUser(){
    return "GET-张三";
}
@RequestMapping(value = "/user",method = RequestMethod.POST)
public String saveUser(){
    return "POST-张三";
}
@RequestMapping(value = "/user",method = RequestMethod.PUT)
public String putUser(){
    return "PUT-张三";
}
@RequestMapping(value = "/user",method = RequestMethod.DELETE)
public String deleteUser(){
    return "DELETE-张三";
}
```

```java
@Bean
@ConditionalOnMissingBean(HiddenHttpMethodFilter.class)
// 需要配置enabled的属性值，没有配置则默认不开启
@ConditionalOnProperty(prefix = "spring.mvc.hiddenmethod.filter", name = "enabled", matchIfMissing = false)
public OrderedHiddenHttpMethodFilter hiddenHttpMethodFilter() {
    return new OrderedHiddenHttpMethodFilter();
}


//自定义filter过滤器，请求过来会被拦截
@Bean
public HiddenHttpMethodFilter hiddenHttpMethodFilter(){
    HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();
    methodFilter.setMethodParam("_m");
    return methodFilter;
}
```

Rest原理（表单提交要使用REST的时候）

- 表单提交会带上input标签里的**_method=PUT**
- **请求过来被**HiddenHttpMethodFilter拦截

- - 请求是否正常，并且是POST请求

- - - 获取到**_method**的值。
    - 兼容以下请求；**PUT**.**DELETE**.**PATCH**
    - **原生request（post），包装模式requesWrapper重写了getMethod方法，返回的是传入的值。**
    - **过滤器链放行的时候用wrapper。以后的方法调用getMethod是调用****requesWrapper的。**

### 7、文件上传

#### 1、页面表单

## 06、数据访问

### 1、SQL

#### 1.1、数据源的自动配置-**HikariDataSource**

##### 1.1.1 导入JDBC场景



##### 1.1.2 

###### 1、自动配置的类

###### 