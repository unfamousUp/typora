# 一、MyBatis

## 1、MyBatis简介

### 1.1  MyBatis历史

> - Dao(Data Access Object)：数据访问对象
>   - MyBatis封装了JDBC，我们可以通过MyBatis连接数据库，访问或操作数据库中的数据

![image-20230506091347935](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702811.png)

### 1.2  MyBatis特性

> - ORM(Object Relation Mapping)：对象关系映射
>   - java中的对象和数据库中的记录创建映射关系

![image-20230506091418043](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702813.png)

### 1.3  MyBatis下载

> - 下载地址：`https://github.com/mybatis/mybatis-3`

### 1.3  和其它持久层技术对比

![image-20230506092337059](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702814.png)

![image-20230506092351539](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702815.png)

## 2、搭建MyBatis

### 2.1 开发环境

![image-20230506095735841](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702816.png)

> MySQL不同版本的注意事项
>
> 1、驱动类driver-class-name
>
> MySQL 5版本使用jdbc5驱动，驱动类使用：`com.mysql.jdbc.Driver`
>
> MySQL 8版本使用jdbc8驱动，驱动类使用：`com.mysql.cj.jdbc.Driver`
>
> 2、连接地址url
>
> MySQL 5版本的url：
>
> `jdbc:mysql://localhost:3306/ssm`
>
> MySQL 8版本的url：
>
> `jdbc:mysql://localhost:3306/ssm?serverTimezone=UTC`
>
> 否则运行测试用例报告如下错误：
>
> `java.sql.SQLException: The server time zone value 'ÖÐ¹ú±ê×¼Ê±¼ä' is unrecognized orrepresents more`

### 2.2 创建maven工程

#### ① 打包方式：jar

#### ② 引入依赖

```xml
<dependencies>
    <!-- Mybatis核心 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
    <!-- junit测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!-- MySQL驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.16</version>
    </dependency>
</dependencies>
```

1. 新建空项目

![image-20230506100039784](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702817.png)

2. 配置maven仓库

![image-20230506100152444](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702818.png)

3. 新建模块

![image-20230506100258926](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702819.png)

4. 配置pom文件

![image-20230506100337714](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702820.png)

### 2.3 创建MyBatis的核心配置文件

> - MyBatis核心配置文件
>   - 核心配置文件
>     - 如何连接数据库
>   - mapper映射文件
>     - SQL语句如何操作数据库
> - 习惯上命名为mybatis-config.xml，这个文件名仅仅只是建议，并非强制要求。将来整合Spring之后，这个配置文件可以省略，所以大家操作时可以直接复制、粘贴。
> - 核心配置文件主要用于配置连接数据库的环境以及MyBatis的全局配置信息
> - 核心配置文件存放的位置是`src/main/resources`目录下

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--设置连接数据库的环境-->
    <environments default="development">
        <environment id="development">
            <!-- 事务管理器 -->
            <transactionManager type="JDBC"/>
            <!-- 数据源 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:13306/ssmserverTimezone=UTC"
                  />
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <mapper resource="mappers/UserMapper.xml"/>
    </mappers>
</configuration>
```

### 2.4 创建mapper接口

> - 中的mapper接口相当于以前的dao。但是区别在于，mapper仅仅是接口，我们不需要提供实现类，MyBatis里的功能可以为接口创建代理实现类。`当我们调用接口中的方法，会对应当前mapper接口的全类名来找到对应的映射文件，然后根据调用方法的方法名找到映射文件里对应的一条sql语句并执行。`
> - 一个mapper接口对应映射文件中的一个sql语句

### 2.5 创建Mybatis的映射文件

![image-20230506105047115](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702821.png)

> **1、映射文件的命名规则：**
>
> 表所对应的实体类的类名+Mapper.xml
>
> 例如：表t_user，映射的实体类为User，所对应的映射文件为UserMapper.xml
>
> 因此一个映射文件对应一个实体类，对应一张表的操作
>
> MyBatis映射文件用于编写SQL，访问以及操作表中的数据
>
> MyBatis映射文件存放的位置是src/main/resources/mappers目录下
>
> **2、 MyBatis中可以面向接口操作数据，要保证两个一致：**
>
> a>mapper接口的全类名和映射文件的命名空间（namespace）保持一致
>
> b>mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性保持一致

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cjj.mybatis.mapper.UserMapper">

    <!--
        mapper接口的映射文件要保证两个一致：
        1. mapper接口的全类名和映射文件的namespace一致
        2. mapper接口中方法的方法名要和映射文件中的sql标签id保持一致
    -->

    <!--int insertUser();-->
    <insert id="insertUser">
        insert into t_user values(null,'admin','123456',23,'男','12345@qq.com')
    </insert>
</mapper>
```

### 2.6 通过Junit测试功能

```java
package com.cjj.mybatis.test;

import com.cjj.mybatis.mapper.UserMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;

public class MybatisTest {
    @Test
    public void testInsert() throws IOException {
        // 获取核心配置文件的输入流
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        // 获取SqlSessionFactoryBuilder对象 --> 执行sql语句的入口
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 获取SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        // 获取sql会话对象SqlSession,是MyBatis提供的操作数据库的对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 获取UserMapper的代理实现类对象
        /*
        *   1. 通过sqlSession.getMapper(Class<T> cls)方法,传入Mapper接口的Class类对象
        *   2. 底层通过代理模式，帮我们创建了该接口的实现类并将其实现类实例对象作为返回值返回
        *   3. 底层如何重写接口方法的呢：通过当前Mapper接口的全类名，找到对应的Mapper.xml映射文件。再通过当			前要调用的方法找到对应映射文件里的sql语句，执行并将结果返回。直接把配置文件映射到实现类的过程给封装起		   来了，程序员是看不到的。
        *   4. 此时userMapper接口对象指向底层代理创建的实例对象
        * */
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // 调用mapper接口中的方法
        int result = userMapper.insertUser();
        System.out.println("结果："+result);
        /*
        *   跳过mapper接口直接执行sql语句(本质也就是重写接口方法的那个过程)
         * */
        int result2 = sqlSession.insert("com.cjj.mybatis.mapper.UserMapper.insertUser");
        System.out.println("结果2："+result2);
        // 提交事务
        sqlSession.commit();
        // 关闭会话
        sqlSession.close();
    }
}

```

> - SqlSession：代表Java程序和**数据库**之间的**会话**。（HttpSession是Java程序和浏览器之间的会话）
>
> - SqlSessionFactory：是“生产”SqlSession的“工厂”。
>
> - 工厂模式：如果创建某一个对象，使用的过程基本固定，那么我们就可以把创建这个对象的相关代码封装到一个“工厂类”中，以后都使用这个工厂类来“生产”我们需要的对象。
> - `SqlSession sqlSession = sqlSessionFactory.openSession(true)`：也可以自动提交事务

### 2.7 加入log4j日志功能

#### ① 加入依赖

```xml
<!-- log4j日志 -->
<dependency>
<groupId>log4j</groupId>
<artifactId>log4j</artifactId>
<version>1.2.17</version>
</dependency>
```

#### ② 加入log4j的配置文件

> - log4j的配置文件名为log4j.xml，存放的位置是src/main/resources目录下
> - 日志的级别
>   - FATAL(致命)>ERROR(错误)>WARN(警告)>INFO(信息)>DEBUG(调试)
>   - 从左到右打印的内容越来越详细

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}%m (%F:%L) \n" />
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug" />
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info" />
    </logger>
    <root>
        <level value="debug" />
        <appender-ref ref="STDOUT" />
    </root>
</log4j:configuration>
```

#### ③ 测试增删改查

> `MybatisTest.java`

```java
package com.cjj.mybatis.test;

import com.cjj.mybatis.mapper.UserMapper;
import com.cjj.mybatis.pojo.User;
import com.cjj.mybatis.utils.SqlSessionUtil;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MybatisTest {
    @Test
    public void testInsert() throws IOException {
        // 获取核心配置文件的输入流
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        // 获取SqlSessionFactoryBuilder对象 --> 执行sql语句的入口
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
        // 获取SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
        // 获取sql会话对象SqlSession,是MyBatis提供的操作数据库的对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
        // 获取UserMapper的代理实现类对象
        /*
        *   1. 通过sqlSession.getMapper(Class<T> cls)方法,传入Mapper接口的Class类对象
        *   2. 底层通过代理模式，帮我们创建了该接口的实现类并将其实现类实例对象作为返回值返回
        *   3. 底层如何重写接口方法的呢：通过当前Mapper接口的全类名，找到对应的Mapper.xml映射文件。再通过当前要调用的方法
        *   找到对应映射文件里的sql语句，执行并将结果返回。直接把配置文件映射到实现类的过程给封装起来了，程序员是看不到的。
        *   4. 此时userMapper接口对象指向底层代理创建的实例对象
        * */
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        // 调用mapper接口中的方法
        int result = userMapper.insertUser();
        System.out.println("结果："+result);
        /*
        *   跳过mapper接口直接执行sql语句(本质也就是重写接口方法的那个过程)
         * */
        int result2 = sqlSession.insert("com.cjj.mybatis.mapper.UserMapper.insertUser");
        System.out.println("结果2："+result2);
        // 提交事务
        sqlSession.commit();
        // 关闭会话
        sqlSession.close();
    }

    @Test
    public void testUpdate(){
        SqlSession sqlSession = SqlSessionUtil.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        userMapper.updateUser();
        sqlSession.close();
    }

    @Test
    public void testDelete(){
        SqlSession sqlSession = SqlSessionUtil.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        userMapper.deleteUser();
        sqlSession.close();
    }

    @Test
    public void testGetUserById(){
        SqlSession sqlSession = SqlSessionUtil.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        User user = userMapper.getUserById();
        System.out.println(user);
    }

    @Test
    public void testGetAllUser(){
        SqlSession sqlSession = SqlSessionUtil.getSqlSession();
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        List<User> userList = userMapper.getAllUser();
        System.out.println(userList);
    }
}
```

> `UserMapper`

```java
package com.cjj.mybatis.mapper;

import com.cjj.mybatis.pojo.User;

import java.util.List;

public interface UserMapper {
    /*
    *  增删改操作返回的值是受影响的行数(int)
    *  */

    /**
     * 添加用户信息
     * @return
     */
    int insertUser();

    /**
     * 修改用户信息
     * @return
     */
    void updateUser();

    /**
     * 删除用户信息
     */
    void deleteUser();

    /**
     * 根据id查询用户信息
     * @return
     */
    User getUserById();

    /**
     * 根据id查询所有用户信息
     */
    List<User> getAllUser();
}
```

> `UserMapper.xml`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cjj.mybatis.mapper.UserMapper">

    <!--
        mapper接口的映射文件要保证两个一致：
        1. mapper接口的全类名和映射文件的namespace一致
        2. mapper接口中方法的方法名要和映射文件中的sql标签id保持一致
    -->

    <!--int insertUser();-->
    <insert id="insertUser">
        insert into t_user values(null,'admin','123456',23,'男','12345@qq.com')
    </insert>
    <!--void updateUser();-->
    <update id="updateUser">
        update t_user set username = 'root',password = '123' where id = 1
    </update>
    <!--void deleteUser();-->
    <delete id="deleteUser">
        delete from t_user where id = 1;
    </delete>
    <!--User getUserById();-->
    <!--
        resultType:设置结果类型，即查询的数据要转换为的java类型
        resultMap:自定义映射，处理多对一或一对多的映射关系
    -->
    <select id="getUserById" resultType="com.cjj.mybatis.pojo.User">
        select * from t_user where id = 2;
    </select>
    <!--List<User> getAllUser();-->
    <select id="getAllUser" resultType="com.cjj.mybatis.pojo.User">
        select * from t_user
    </select>
</mapper>
```

## 3、MyBatis核心配置文件详解

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--
    MyBatis核心配置文件中，标签的顺序：
    properties?,settings?,typeAliases?,typeHandlers?,
    objectFactory?,objectWrapperFactory?,reflectorFactory?,
    plugins?,environments?,databaseIdProvider?,mappers?
-->
<configuration>

    <!-- 引入properties文件，此后就可以在当前配置文件中使用${key}的方式访问value -->
    <properties resource="jdbc.properties"></properties>

    <!-- 设置类型别名 -->
    <typeAliases>
        <!--
            typeAlias：设置某个类型的别名
            属性：
            type：设置需要设置别名的类型
            alias：设置某个类型的别名，若不设置该属性，那么该类型拥有默认的别名，即[类名]且不区分大小写
        -->
        <!--<typeAlias type="com.cjj.mybatis.pojo.User" alias="abc"></typeAlias>-->
        <!--<typeAlias type="com.cjj.mybatis.pojo.User"></typeAlias>-->
        <!-- 通过包设置类型别名,指定包下所有的实体类将全部拥有默认的别名，即类名不区分大小写 -->
        <package name="com.cjj.mybatis.pojo"/>
    </typeAliases>

    <!--设置连接数据库的环境-->
    <!--
        environments：配置多个连接数据库的环境
        属性：
        default：设置默认使用的环境的id
    -->
    <environments default="development">
        <!--
            environment：配置某个具体的环境
            属性：
            id：表示连接数据库的环境的唯一标识，不能重复
        -->
        <environment id="development">
            <!-- 事务管理器 -->
            <!--
                transactionManager：设置事务管理方式
                属性：
                type="JDBC|MANAGED"
                JDBC：表示当前环境中，执行SQL时，使用的是JDBC中原生的事务管理方式，事
                务的提交或回滚需要手动处理
                MANAGED：被管理，例如Spring
            -->
            <transactionManager type="JDBC"/>
            <!-- 数据源 -->
            <!--
                dataSource：配置数据源
                属性：
                type：设置数据源的类型
                type="POOLED|UNPOOLED|JNDI"
                POOLED：表示使用数据库连接池缓存数据库连接
                UNPOOLED：表示不使用数据库连接池
                JNDI：表示使用上下文中的数据源
            -->
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件-->
    <mappers>
        <!--<mapper resource="mappers/UserMapper.xml"/>-->
        <!--
            以包为单位引入映射文件
            要求：
            1、mapper接口所在的包要和映射文件所在的包一致
            2、mapper接口要和映射文件的名字一致
        -->
        <package name="com.cjj.mybatis.mapper"/>
    </mappers>
</configuration>
```

## 4、MyBatis的增删改查

### 4.1 新增

```xml
<!--int insertUser();-->
<insert id="insertUser">
insert into t_user values(null,'admin','123456',23,'男')
</insert>
```

### 4.2 删除

```xml
<!--int deleteUser();-->
<delete id="deleteUser">
delete from t_user where id = 7
</delete>
```

### 4.3 修改

```xml
<!--int updateUser();-->
<update id="updateUser">
update t_user set username='ybc',password='123' where id = 6
</update>
```

### 4.4 查询一个实体类对象

```xml
<!--User getUserById();-->
<select id="getUserById" resultType="com.atguigu.mybatis.bean.User">
select * from t_user where id = 2
</select>
```

### 4.5 查询List集合

```xml
<!--List<User> getUserList();-->
<select id="getUserList" resultType="com.atguigu.mybatis.bean.User">
select * from t_user
</select>
```

## 5、MyBatis获取参数值的两种方式

> - ${}的本质就是`字符串拼接`，#{}的本质就是`占位符赋值`
>- ${}使用字符串拼接的方式拼接sql，若为字符串类型或日期类型的字段进行赋值时，需要手动加单引号但是#{}使用占位符赋值的方式拼接sql，此时为字符串类型或日期类型的字段进行赋值时，可以自动添加单引号。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2533ee2d7f2c431b9c443544afe82fa9.png)

### 5.1 单个字面量类型的参数

> - 字面量：比如`int a = 1`，a是变量，而`1`从字面上看到的是什么就是什么。像字符串、基本数据类型以及其对应的包装类都属于字面量类型。
>
> - 若mapper接口中的方法参数为单个的字面量类型
>
>   此时可以使用`${}和#{}以任意的名称获取参数的值，注意${}需要手动加单引号`

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cjj.mybatis.mapper.UserMapper">

    <!--User getUserByUsername(String username);-->
    <select id="getUserByUsername" resultType="User">
        <!--select * from t_user where username = #{username}-->
        select * from t_user where username = '${username}'
    </select>
</mapper>
```

### 5.2 多个字面量类型的参数

> - 若mapper接口中的方法参数为多个时：此时MyBatis会自动将这些参数放在一个map集合中，以arg0,arg1...为键，以参数为值；以param1,param2...为键，以参数为值；因此只需要通过`${}和#{}`访问map集合的键就可以获取相对应的值，注意${}需要手动加单引号

```xml
    <!--User checkLogin(String username, String password);-->
    <select id="checkLogin" resultType="User">
        <!-- select * from t_user where username = #{param1} and password = #{param2} -->
        select * from t_user where username = '${param1}' and password = '${param2}'
    </select>
```

```java
@Test
public void testCheckLogin(){
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
    User user = userMapper.checkLogin("xtt", "123456");
    System.out.println(user);
}
```

### 5.3 map集合类型的参数

>- 若mapper接口中的方法需要的参数为多个时，此时可以手动创建map集合，将这些数据放在map中只需要通过`${}和#{}`访问map集合`程序员自定义`的键就可以获取相对应的值，注意${}需要手动加单引号

```xml
<!--User checkLoginByMap(Map<String,Object> map);-->
<select id="checkLoginByMap" resultType="User">
     select * from t_user where username = #{username} and password = #{password}
</select>
```

```java
@Test
public void testCheckLoginByMap(){
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
    Map<String,Object> map = new HashMap<>();
    map.put("username","xtt");
    map.put("password","123456");
    User user = userMapper.checkLoginByMap(map);
    System.out.println(user);
}
```

### 5.4 实体类类型的参数

> - 若mapper接口中的方法参数为实体类对象时此时可以使用`${}和#{}`，通过访问实体类对象中的属性名(`和成员变量无关，只和get|set方法有关`)获取属性值，注意${}需要手动加单引号
>
> - 注意
>
>   - Mybatis获取实体类中的属性或给实体类中的属性赋值实际上是看类中的`get`和`set`方法，我们把`get`和`set`方法名中的`get`和`set`去掉，剩下的内容字母变为小写之后就为属性。
>   - 本质上利用反射机制获取方法名然后调用相关方法去掉get|set并将第一个字母变为小写后的字符串就是属性名
>
>   ```java
>   public Integer getId() { // 属性名为#{id}
>           return id;
>       }
>   ```

```XML
<!--int insertUser(User user);-->
<insert id="insertUser">
    insert into t_user values(null,#{username},#{password},#{age},#{gender},#{email})
</insert>
```

### 5.5 使用@param注解标识参数

> - 可以通过@Param注解标识mapper接口中的方法参数。此时，会将这些参数放在map集合中，以@Param注解的value属性值为键，以参数为值；或以param1,param2...为键，以参数为值；只需要通过`${}和#{}`访问map集合的键就可以获取相对应的值，注意`${}`需要手动加单引号
> - 注解底层：像`@param(value = "username")`，底层有一个`Map<String,Object> memberValues`，它是一个 Map 键值对，键是我们注解属性名称，值就是该属性当初被赋上的值`username`。
> - 而此时MyBatis把`@param注解`中的`值`作为`mapper映射文件`sql语句中用接受参数的Map集合中的`键`，而把传入`mapper接口`方法中实参的值作为`mapper映射文件`sql语句中用接受参数的Map集合中的`值`，用于替换`#{}或${}`的值。

```java
    /**
     * 验证登录(使用@param注解)
     * @param username
     * @param password
     * @return
     */
    User checkLoginByParam(@Param("username") String username, @Param("password") String password);
```

## 6、MyBatis的各种查询功能

### 6.1 查询一个实体类对象

```java
/**
 * 根据id查询用户信息 
 * @param id
 * @return
 */
User getUserById(@Param("id") int id);
```

```xml
<!--User getUserById(@Param("id") int id);-->
<select id="getUserById" resultType="user">
	select * from t_user where id = #{id};
</select>
```

### 6.2 查询一个List集合

> - 当查询的数据为多条时，不能使用实体类作为返回值，否则会抛出异常TooManyResultsException；但是若查询的数据只有一条，可以使用实体类或集合作为返回值
>
> - 因为底层判断接口的返回类型是单个`User`，所以实际调用的是如下方法
>
>   ```java
>   sqlSession.selectOne(String s) 
>   ```

```java
/**
 * 获取所有用户信息
 * @return
 */
List<User> getUserList();
```

```xml
<!--List<User> getUserList();-->
<select id="getUserList" resultType="User">
    select * from t_user
</select>
```

### 6.3 查询单个(单行单列)数据

```java
/**
* 查询用户的总记录数
* @return
* 在MyBatis中，对于Java中常用的类型都设置了类型别名
* 例如： java.lang.Integer-->int|integer
* 例如： int-->_int|_integer
* 例如： Map-->map,List-->list
*/
Integer getCount();
```

```xml
<!--Integer getCount();-->
<select id="getCount" resultType="Integer">
	select count(*) from t_user
</select>
```

### 6.4 查询一条数据为Map集合

> - 结果
>
>   ```java
>   /* {password=123456, gender=男, id=3, age=23, email=12345@qq.com, username=xtt} */
>   ```

```java
/**
 * 根据id查询用户信息为map集合
 * @return
 */
Map<String,Object> getUserByIdToMap(@Param("id") int id);
```

```xml
<!--Map<String,Object> getUserByIdToMap();-->
<select id="getUserByIdToMap" resultType="map">
    select * from t_user where id = #{id}
</select>
```

### 6.5 查询多条数据为Map集合

> - `查询的结果无实体类，用map集合方式接受`

#### ① 方式一

```java
/**
* 查询所有用户信息为map集合
* @return
* 将表中的数据以map集合的方式查询，一条数据对应一个map；若有多条数据，就会产生多个map集合，此时可以将这些map放在* 一个list集合中获取
*/
List<Map<String, Object>> getAllUserToMap();
```

```xml
<!--Map<String, Object> getAllUserToMap();-->
<select id="getAllUserToMap" resultType="map">
select * from t_user
</select>
```

#### ② 方式二

```java
/**
* 查询所有用户信息为map集合
* @return
* 将表中的数据以map集合的方式查询，一条数据对应一个map；若有多条数据，就会产生多个map集合，并且最终要以一个map的* 方式返回数据，此时需要通过@MapKey注解设置map集合的键，值是每条数据所对应的map集合
*/
@MapKey("id")
Map<String, Object> getAllUserToMap();
```

```xml
<!--Map<String, Object> getAllUserToMap();-->
<!--
{
    1={password=123456, sex=男, id=1, age=23, username=admin},
    2={password=123456, sex=男, id=2, age=23, username=张三},
    3={password=123456, sex=男, id=3, age=23, username=张三}
}
-->
<select id="getAllUserToMap" resultType="map">
select * from t_user
</select>
```

## 7、特殊SQL的执行

### 7.1 模糊查询

```java
/**
 * 通过用户名模糊查询用户信息
 * @param vague
 * @return
 */
List<User> getUserByLike(@Param("vague") String vague);
```

```xml
<!--List<User> getUserByLike(@Param("vague") String vague);-->
<select id="getUserByLike" resultType="User">
    <!--select * from t_user where username like '%${vague}%'-->
    <!--select * from t_user where username like concat('%',#{vague},'%')-->
    select * from t_user where username like "%"#{vague}"%"
</select>
```

### 7.2 批量删除

```java
/**
 * 批量删除
 * @param ids
 */
void deleteMoreUser(@Param("ids") String ids);
```

```xml
<!--void deleteMoreUser(@Param("ids") String ids)-->
<delete id="deleteMoreUser">
    delete from t_user where id in(${ids})
</delete>
```

### 7.3 动态设置表名

> - 表名不能用`单引号`，所以得用`${tableName}`

```java
/**
 * 动态设置表名，查询用户信息
 * @param tableName
 * @return
 */
List<User> getUserList(@Param("tableName") String tableName);
```

```xml
<!--List<User> getUserList(@Param("tableName") String tableName);-->
<select id="getUserList" resultType="com.cjj.mybatis.pojo.User">
    select * from ${tableName}
</select>
```

### 7.4 添加功能获取自增的主键

> 场景模拟：
>
> t_clazz(clazz_id,clazz_name)
>
> t_student(student_id,student_name,clazz_id)
>
> 1、添加班级信息
>
> 2、获取新添加的班级的id
>
> 3、为班级分配学生，即将某学的班级id修改为新添加的班级的id

```java
/**
 * 添加用户信息
 * @param user
 * @return
 * useGeneratedKeys：设置使用自增的主键
 * keyProperty：因为增删改有统一的返回值:是受影响的行数，因此只能将获取的自增的主键放在传输的参数user对象的某个属性中(比如user.id中)
 */
void insertUser(User user);
```

```xml
<!--void insertUser(User user);-->
<insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
    insert into t_user values (null,#{username},#{password},#{age},#{gender},#{email})
</insert>
```

## 8、自定义映射resultMap

### 8.1 resultMap处理字段和属性的映射关系

> - 若字段名和实体类中的属性名不一致，则可以通过resultMap设置自定义映射
>
> - 若字段名和实体类中的属性名不一致，但是字段名符合数据库的规则（使用_），实体类中的属性
>
>   名符合Java的规则（使用驼峰）此时也可通过以下两种方式处理字段名和实体类中的属性的映射关系
>
>   - a>可以通过为字段起别名的方式，保证和实体类中的属性名保持一致
>   - b>可以在MyBatis的核心配置文件中设置一个全局配置信息mapUnderscoreToCamelCase，可以在查询表中数据时，自动将_类型的字段名转换为驼峰。
>   - 例如：字段名user_name，设置了mapUnderscoreToCamelCase，此时字段名就会转换为userName

```java
/**
 * 根据id查询员工信息
 * @param empId
 * @return
 */
Emp getEmpByEmpId(@Param("empId") Integer empId);
```

```xml
<!--
    resultMap：设置自定义映射
    属性：
    id：表示自定义映射的唯一标识
    type：查询的数据要映射的实体类的类型
    子标签：
    id：设置主键的映射关系
    result：设置普通字段的映射关系
    association：设置多对一的映射关系
    collection：设置一对多的映射关系
    属性：
    property：设置映射关系中实体类中的属性名
    column：设置映射关系中表中的字段名
-->
<resultMap id="empResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
</resultMap>
<!--Emp getEmpByEmpId(@Param("empId") Integer empId);-->
<select id="getEmpByEmpId" resultMap="empResultMap">
    select * from t_emp where emp_id = #{empId}
</select>
```

### 8.2 多对一映射处理

> 场景模拟：
>
> 查询员工信息以及员工所对应的部门信息

#### 8.2.1 级联方式处理映射关系

```java
/**
 * 获取员工以及对应的部门信息(左外连接)
 * @param empId
 * @return
 */
Emp getEmpAndDeptByEmpId(@Param("empId") Integer empId);
```

```xml
<resultMap id="empAndDeptResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
    <result column="dept_id" property="dept.deptId"></result>
    <result column="dept_name" property="dept.deptName"></result>
</resultMap>
<!--Emp getEmpAndDeptByEmpId(@Param("empId") Integer empId);-->
<select id="getEmpAndDeptByEmpId" resultMap="empAndDeptResultMap">
    select
    t_emp.*,t_dept.*
    from t_emp
    LEFT JOIN t_dept
    on t_emp.dept_id = t_dept.dept_id
    where emp_id = #{empId}
</select>
```

#### 8.2.2 使用association处理映射关系

```xml
<resultMap id="empAndDeptResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
    <association property="dept" javaType="Dept">
        <id column="dept_id" property="deptId"></id>
        <result column="dept_name" property="deptName"></result>
    </association>
</resultMap>
<!--Emp getEmpAndDeptByEmpId(@Param("empId") Integer empId);-->
<select id="getEmpAndDeptByEmpId" resultMap="empAndDeptResultMap">
    select
    t_emp.*,t_dept.*
    from t_emp
    LEFT JOIN t_dept
    on t_emp.dept_id = t_dept.dept_id
    where emp_id = #{empId}
</select>
```

#### 8.2.3 分步查询

##### ① 查询员工信息

```java
/**
 * 通过分步查询查询员工以及对应的部门信息(第一步)
 * @param empId
 * @return
 */
Emp getEmpAndDeptByStepOne(@Param("empId") Integer empId);
```

```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
    <!--
		property：需要映射的实体类对象属性
        select：设置分步查询，查询某个属性的值的sql的标识（namespace.sqlId）
        column：将sql以及查询结果中的某个字段设置为分步查询的条件
    -->
    <association property="dept"
                 select="com.cjj.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
                 column="dept_id">
    </association>
</resultMap>
<!--Emp getEmpAndDeptByStepOne(@Param("empId") Integer empId);-->
<select id="getEmpAndDeptByStepOne" resultMap="empAndDeptByStepResultMap">
    select * from t_emp where emp_id = #{empId}
</select>
```

##### ② 根据员工所对应的部门id查询部门信息

```java
/**
 * 通过分步查询查询员工以及对应的部门信息(第二步)
 * @return
 */
Dept getEmpAndDeptByStepTwo(@Param("deptId") Integer deptId);
```

```xml
<!--Dept getEmpAndDeptByStepTwo(@Param("deptId") Integer deptId);-->
<select id="getEmpAndDeptByStepTwo" resultType="com.cjj.mybatis.pojo.Dept">
    select * from t_dept where dept_id = #{deptId}
</select>
```

#### 8.2.4 延迟加载

> - 分步查询的优点：可以实现延迟加载，但是必须在核心配置文件中设置全局配置信息
>
> - `lazyLoadingEnabled`：延迟加载的全局开关。当开启时，所有关联对象都会延迟加载
>
> - `aggressiveLazyLoading`：当开启时，任何方法的调用都会加载该对象的所有属性。否则，每个属性会按需加载。此时就可以实现按需加载，我们获取的数据是什么，就只会执行相应的sql。此时可通过association和collection中的fetchType属性设置当前的分步查询是否使用延迟加载`fetchType="lazy(延迟加载)|eager(立即加载)"`

```xml
<settings>
    <!-- 自动将下划线映射为驼峰 -->
    <setting name="mapUnderscoreToCamelCase" value="true"/>
    <!-- 开启延迟加载 -->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!-- 按需加载 -->
    <setting name="aggressiveLazyLoading" value="false"/>
</settings>
```

```xml
<resultMap id="empAndDeptByStepResultMap" type="Emp">
    <id column="emp_id" property="empId"></id>
    <result column="emp_name" property="empName"></result>
    <result column="age" property="age"></result>
    <result column="gender" property="gender"></result>
    <!--
        fetchType:在开启了延迟加载的环境中，通过该属性设置当前分步查询是否使用延迟加载
        fetchType="eager(立即加载)|lazy(延迟加载)
    -->
    <association property="dept"
                 fetchType="eager"
                 select="com.cjj.mybatis.mapper.DeptMapper.getEmpAndDeptByStepTwo"
                 column="dept_id">
    </association>
</resultMap>
```

### 8.3 一对多映射处理

#### 8.3.1 collection

```java
package com.cjj.mybatis.pojo;

import java.util.List;

public class Dept {

    private Integer deptId;

    private String deptName;
	
    private List<Emp> emps; // 集合

    public Dept() {
    }

    public Dept(Integer deptId, String deptName) {
        this.deptId = deptId;
        this.deptName = deptName;
    }

    public Integer getDeptId() {
        return deptId;
    }

    public void setDeptId(Integer deptId) {
        this.deptId = deptId;
    }

    public String getDeptName() {
        return deptName;
    }

    public void setDeptName(String deptName) {
        this.deptName = deptName;
    }

    public List<Emp> getEmps() {
        return emps;
    }

    public void setEmps(List<Emp> emps) {
        this.emps = emps;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "deptId=" + deptId +
                ", deptName='" + deptName + '\'' +
                ", emps=" + emps +
                '}';
    }
}
```

```java
/**
 * 根据部门id查询部门以及部门中的员工信息
 * @param deptId
 * @return
 */
Dept getDeptAndEmpByDeptId(@Param("deptId") Integer deptId);
```

```xml
<resultMap id="deptAndEmpResultMap" type="Dept">
    <id column="dept_id" property="deptId"></id>
    <result column="dept_name" property="deptName"></result>
    <!--
    	ofType：设置collection标签所处理的集合属性中存储数据的类型
    -->
    <collection property="emps" ofType="Emp">
        <id column="emp_id" property="empId"></id>
        <result column="emp_name" property="empName"></result>
        <result column="age" property="age"></result>
        <result column="gender" property="gender"></result>
    </collection>
</resultMap>
<!--Dept getDeptAndEmpByDeptId(@Param("deptId") Integer deptId);-->
<select id="getDeptAndEmpByDeptId" resultMap="deptAndEmpResultMap">
    select *
    from t_dept
    left join t_emp
    on t_dept.dept_id = t_emp.dept_id
    where t_emp.dept_id = #{deptId}
</select>
```

#### 8.3.2 分步查询

##### ① 查询部门信息

> `DeptMapper.java`

```java
/**
 * 通过分步查询查询部门以及部门中的员工信息的第一步
 * @param deptId
 * @return
 */
Dept getDeptAndEmpByStepOne(@Param("deptId") Integer deptId);
```

```xml
<resultMap id="deptAndEmpByStepResultMap" type="Dept">
    <id column="dept_id" property="deptId"></id>
    <result column="dept_name" property="deptName"></result>
    <collection property="emps"
                select="com.cjj.mybatis.mapper.EmpMapper.getDeptAndDeptByStepTwo"
                column="dept_id">
    </collection>
</resultMap>
<!--Dept getDeptAndEmpByStepOne(@Param("deptId") Integer deptId);-->
<select id="getDeptAndEmpByStepOne" resultMap="deptAndEmpByStepResultMap">
    select * from t_dept where dept_id = #{deptId}
</select>
```

##### ② 根据部门id查询部门中的所有员工

> `EmpMapper.java`

```java
/**
 * 通过分步查询查询部门以及部门中的员工信息的第二步
 * @param deptId
 * @return
 */
List<Emp> getDeptAndDeptByStepTwo(@Param("deptId") Integer deptId);
```

```xml
<!--List<Emp> getDeptAndDeptByStepTwo(@Param("deptId") Integer deptId);-->
<select id="getDeptAndDeptByStepTwo" resultType="com.cjj.mybatis.pojo.Emp">
    select * from t_emp where dept_id = #{deptId}
</select>
```

## 9、动态SQL

>- Mybatis框架的动态SQL技术是一种根据特定条件动态拼装SQL语句的功能，它存在的意义是为了解决拼接SQL语句字符串时的痛点问题。

### 9.1 if

> - if标签可通过test属性的表达式进行判断，若表达式的结果为true，则标签中的内容会执行；反之标签中的内容不会执行

```java
/**
 * 根据条件查询员工信息
 * @param emp
 * @return
 */
List<Emp> getEmpByCondition(Emp emp);
```

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="com.cjj.mybatis.pojo.Emp">
    select * from t_emp where 1 = 1
    <if test="empName != null and empName != '' ">
       and emp_name = #{empName}
    </if>
    <if test="age != null and age != '' ">
        and age = #{age}
    </if>
    <if test="gender != null and gender != '' ">
        and gender = #{gender}
    </if>
</select>
```

### 9.2 where

> - where和if一般结合使用：
>   - a>若where标签中的if条件都不满足，则where标签没有任何功能，即不会添加where关键字
>   - b>若where标签中的if条件满足，则where标签会自动添加where关键字，并将条件最前方多余的and去掉
>
> - 注意：where标签不能去掉条件最后多余的and

```xml
<!--List<Emp> getEmpByCondition(Emp emp);-->
<select id="getEmpByCondition" resultType="com.cjj.mybatis.pojo.Emp">
    select * from t_emp
    <where>
        <if test="empName != null and empName != '' ">
            emp_name = #{empName}
        </if>
        <if test="age != null and age != '' ">
            and age = #{age}
        </if>
        <if test="gender != null and gender != '' ">
            and gender = #{gender}
        </if>
    </where>
</select>
```

### 9.3 trim

> - trim用于去掉或添加标签中的内容，常用属性：
>
>   - `prefix`：在trim标签中的内容的前面添加某些内容
>
>   - `prefixOverrides`：在trim标签中的内容的前面去掉某些内容
>
>   - `suffix`：在trim标签中的内容的后面添加某些内容
>   - `suffixOverrides`：在trim标签中的内容的后面去掉某些内容

```xml
<select id="getEmpByCondition" resultType="com.cjj.mybatis.pojo.Emp">
    select * from t_emp
    <trim prefix="where" suffixOverrides="and">
        <if test="empName != null and empName != '' ">
            emp_name = #{empName}
        </if>
        <if test="age != null and age != '' ">
            and age = #{age}
        </if>
        <if test="gender != null and gender != '' ">
            and gender = #{gender}
        </if>
    </trim>
</select>
```

### 9.4 choose、when、otherwise

> - choose、when、 otherwise相当于`if `| `else if `| `else`

```java
/**
 * 使用Choose查询员工信息
 * @param emp
 * @return
 */
List<Emp> getEmpByChoose(Emp emp);
```

```xml
<!--List<Emp> getEmpByChoose(Emp emp);-->
<select id="getEmpByChoose" resultType="com.cjj.mybatis.pojo.Emp">
    select * from t_emp
    <where>
        <choose>
            <when test="empName != null and empName != ''">
                emp_name = #{empName}
            </when>
            <when test="age != null and age != ''">
                age = #{age}
            </when>
            <when test="gender != null and gender != ''">
                gender = #{gender}
            </when>
        </choose>
    </where>
</select>
```

### 9.5 foreach

> - 标签属性
>   - collection:设置要循环的数组或集合
>   - item:用一个字符串表示数组或集合中的每一个数据
>   - separator:设置每次循环的数据之间的分隔符
>   - open:循环的所有内容以什么开始
>   - close:循环的所有内容以什么结束

#### ① 批量添加

> - 此时方法实参为一个List集合，MyBatis默认会以`list`为键，以参数值`emps`为值放到Map集合中
>   - `[arg0, collection, list]`
> - 我们可以使用@Param指定键名

```java
/**
 * 批量添加员工信息
 * @param emps
 */
void insertMoreEmp(@Param("emps") List<Emp> emps);
```

```xml
<!--void insertMoreEmp(List<Emp> emps);-->
<insert id="insertMoreEmp">
    insert into t_emp values
    <!--
 		collection:属性值为Map集合里的键，最后会被Map集合里的值替换
		此时的键名为@Param的属性值emps
	-->
    <foreach collection="emps" item="emp" separator=",">
        (null,#{emp.empName},#{emp.age},#{emp.gender},null)
    </foreach>
</insert>
```

#### ② 批量删除

> - 此时方法实参为一个数组，MyBatis默认会以array为键名，以参数值`empIds`为键值放到Map集合中
>
> - 我们可以使用`@Param`指定键名

```java
/**
 * 批量删除员工信息
 * @param empIds
 */
void deleteMoreEmp(@Param("empIds") Integer[] empIds);
```

```xml
<!--void deleteMoreEmp(@Param("ids") Integer[] empIds);-->
<delete id="deleteMoreEmp">
    delete from t_emp where emp_id in
    <!--(-->
    <!--    <foreach collection="empIds" item="empId" separator=",">-->
    <!--        #{empId}-->
    <!--    </foreach>-->
    <!--)-->
    <foreach collection="empIds" item="empId" separator="," open="(" close=")">
        #{empId}
    </foreach>
</delete>
```

### 9.6 SQL片段

> - sql片段，可以记录一段公共sql片段，在使用的地方通过include标签进行引入

```xml
<sql id="empColumns">
	eid,ename,age,sex,did
</sql>
select <include refid="empColumns"></include> from t_emp
```

## 10、MyBatis的缓存

### 10.1 MyBatis的一级缓存

> - 一级缓存是SqlSession级别的，通过同一个SqlSession对象查询的数据会被缓存，下次查询相同的数据，就会从缓存中直接获取，不会从数据库重新访问。使一级缓存失效`(清空)`的四种情况：
>
>   - 1) 不同的SqlSession对应不同的一级缓存
>   - 2) 同一个SqlSession但是查询条件不同
>   - 3) 同一个SqlSession两次查询期间执行了任何一次增删改操作
>
>   - 4) 同一个SqlSession两次查询期间手动清空了缓存
>
>     ```java
>     sqlSession.clearCache() // 清空缓存
>     ```

### 10.2 MyBatis的二级缓存

> - 二级缓存是SqlSessionFactory级别，通过同一个SqlSessionFactory创建的SqlSession查询的结果会被缓存；此后若再次执行相同的查询语句，结果就会从缓存中获取。
>
> - 二级缓存开启的条件：
>   - a>在核心配置文件中，设置全局配置属性cacheEnabled="true"，默认为true，不需要设置
>
>   - b>在映射文件中设置标签`<cache/>`
>   - c>二级缓存必须在SqlSession关闭或提交之后有效
>   - d>查询的数据所转换的实体类类型必须`实现序列化的接口`
>
> - 使二级缓存失效的情况：
>   - 两次查询之间执行了任意的增删改，会使一级和二级缓存同时失效

### 10.3 二级缓存的相关配置

> - 在mapper配置文件中添加的cache标签可以设置一些属性：
>
>   - ①eviction属性：缓存回收策略，默认的是 LRU。
>     - LRU（Least Recently Used） – 最近最少使用的：移除最长时间不被使用的对象。
>     - FIFO（First in First out） – 先进先出：按对象进入缓存的顺序来移除它们。
>     - SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。
>     - WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。
>
>   - ②flushInterval属性：刷新间隔，单位毫秒
>     - 默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新
>
>   - ③size属性：引用数目，正整数
>     - 代表缓存最多可以存储多少个对象，太大容易导致内存溢出
>
>   - ④readOnly属性：只读， true/false
>     - true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。
>     - false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是false。

### 10.4 MyBatis缓存查询的顺序

> - 先查询二级缓存，因为二级缓存中可能会有其他程序已经查出来的数据，可以拿来直接使用。
>
> - 如果二级缓存没有命中，再查询一级缓存
>
> - 如果一级缓存也没有命中，则查询数据库
>
> - SqlSession关闭之后，一级缓存中的数据会写入二级缓存

### 10.5 整合第三方缓存EHCache

> - 我们可以使用MyBatis中原生的二级缓存，也可以整合第三方缓存（二级缓存）

#### 10.5.1 添加依赖

```xml
<!-- Mybatis EHCache整合包 -->
<dependency>
    <groupId>org.mybatis.caches</groupId>
    <artifactId>mybatis-ehcache</artifactId>
    <version>1.2.1</version>
</dependency>
<!-- slf4j日志门面的一个具体实现 -->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.3</version>
</dependency>
```

#### 10.5.2 各jar包功能

| jar包名称       | 作用                            |
| --------------- | ------------------------------- |
| mybatis-ehcache | Mybatis和EHCache的整合包        |
| ehcache         | EHCache核心包                   |
| slf4j-api       | SLF4J日志门面包                 |
| logback-classic | 支持SLF4J门面接口的一个具体实现 |

#### 10.5.3 创建EHCache的配置文件ehcache.xml

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
    <!-- 磁盘保存路径 -->
    <diskStore path="G:\code\ehcache"/>
    <defaultCache
            maxElementsInMemory="1000"
            maxElementsOnDisk="10000000"
            eternal="false"
            overflowToDisk="true"
            timeToIdleSeconds="120"
            timeToLiveSeconds="120"
            diskExpiryThreadIntervalSeconds="120"
            memoryStoreEvictionPolicy="LRU">
    </defaultCache>
</ehcache>
```

#### 10.5.4 设置二级缓存的类型

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cjj.mybatis.mapper.CacheMapper">
    
    <cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
    
    <!--Emp getEmpById(@Param("empId") Integer empId);-->
    <select id="getEmpById" resultType="com.cjj.mybatis.pojo.Emp">
        select * from t_emp where emp_id = #{empId}
    </select>
</mapper>
```

#### 10.5.5 加入logback日志

> - 存在SLF4J时，作为简易日志的log4j将失效，此时我们需要借助SLF4J的具体实现logback来打印日志。 创建logback的配置文件logback.xml

## 11、MyBatis的逆向工程

> - 正向工程：先创建Java实体类，由框架负责根据实体类生成数据库表。 Hibernate是支持正向工程的。
>
> - 逆向工程：先创建数据库表，由框架负责根据数据库表，反向生成如下资源：
>   - Java实体类
>   - Mapper接口
>   - Mapper映射文件

### 11.1 创建逆向工程步骤

#### ① 添加依赖和插件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cjj.mybatis</groupId>
    <artifactId>mybatis_mbg</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>
    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.7</version>
        </dependency>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- log4j日志 -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
    </dependencies>
    <!-- 控制Maven在构建过程中相关配置 -->
    <build>
    <!-- 构建过程中用到的插件 -->
    <plugins>
        <!-- 具体插件，逆向工程的操作是以构建过程中插件形式出现的 -->
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.0</version>
            <!-- 插件的依赖 -->
            <dependencies>
                <!-- 逆向工程的核心依赖 -->
                <dependency>
                    <groupId>org.mybatis.generator</groupId>
                    <artifactId>mybatis-generator-core</artifactId>
                    <version>1.3.2</version>
                </dependency>
                <!-- MySQL驱动 -->
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>8.0.16</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
    </build>
</project>
```

#### ② 创建MyBatis核心配置文件

> `mybatis-config.xml`

#### ③ 创建逆向工程配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <!--
        targetRuntime: 执行生成的逆向工程的版本
        MyBatis3Simple: 生成基本的CRUD（清新简洁版）
        MyBatis3: 生成带条件的CRUD（奢华尊享版）
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!-- 数据库的连接信息 -->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:13306/ssm?serverTimezone=UTC"
                        userId="root"
                        password="root">
        </jdbcConnection>
        <!-- javaBean的生成策略-->
        <javaModelGenerator targetPackage="com.cjj.mybatis.pojo"
                            targetProject=".\src\main\java">
            <!-- 是否可以使用子包（使包分层） -->
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.cjj.mybatis.mapper"
                         targetProject=".\src\main\resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>
        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.cjj.mybatis.mapper" 				     	 							  targetProject=".\src\main\java">           
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>
        <!-- 逆向分析的表 -->
        <!-- tableName设置为*号，可以对应所有表，此时不写domainObjectName -->
        <!-- domainObjectName属性指定生成出来的实体类的类名 -->
        <table tableName="t_emp" domainObjectName="Emp"/>
        <table tableName="t_dept" domainObjectName="Dept"/>
    </context>
</generatorConfiguration>
```

#### ④ 执行MBG插件的generate目标

![image-20230511151026386](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702822.png)

#### ⑤ 效果

![image-20230511151054665](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702823.png)

### 11.2 QBC查询

```java
package com.cjj.mybatis.test;

import com.cjj.mybatis.mapper.EmpMapper;
import com.cjj.mybatis.pojo.Emp;
import com.cjj.mybatis.pojo.EmpExample;
import com.cjj.mybatis.utils.SqlSessionUtil;
import org.apache.ibatis.session.SqlSession;
import org.junit.Test;

import java.util.List;

public class MBGTest {

    @Test
    public void testMBG(){
        SqlSession sqlSession = SqlSessionUtil.getSqlSession();
        EmpMapper empMapper = sqlSession.getMapper(EmpMapper.class);
        // 根据主键查询数据
        // Emp emp = empMapper.selectByPrimaryKey(1);
        // System.out.println(emp);
        // 查询所有数据
        // List<Emp> emps = empMapper.selectByExample(null);
        // emps.forEach(System.out::println);
        // 根据条件查询数据
        EmpExample empExample = new EmpExample();
        empExample.createCriteria().andEmpNameEqualTo("张三");
        empExample.or().andGenderEqualTo("男");
        List<Emp> emps1 = empMapper.selectByExample(empExample);
        System.out.println(emps1);
    }
}
```

## 12、分页插件

> - 原生分页代码
>
> limit index,pageSize
>
> pageSize：每页显示的条数
>
> pageNum：当前页的页码
>
> index：当前页的起始索引，index=(pageNum-1)*pageSize
>
> count：总记录数
>
> totalPage：总页数
>
> totalPage = count / pageSize;
>
> if(count % pageSize != 0){
>
> totalPage += 1;
>
> }
>
> pageSize=4，pageNum=1，index=0 limit 0,4
>
> pageSize=4，pageNum=3，index=8 limit 8,4
>
> pageSize=4，pageNum=6，index=20 limit 8,4

### 12.1 分页插件的使用步骤

#### ① 增加依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.2.0</version>
</dependency>
```

#### ② 配置分页插件

> - 在MyBatis核心配置文件中配置分页插件

```xml
<!-- 设置插件 -->
<plugins>
    <!-- 配置分页插件 -->
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

### 12.2 分页插件的使用

```java
@Test
public void testPage(){
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    // 查询开始之前开启分页功能
    Page<Object> page = PageHelper.startPage(1, 4);
    List<Emp> emps = mapper.selectByExample(null);
    System.out.println(emps);
    emps.forEach(System.out::println);
    System.out.println(page);
}
```

a> 在查询功能之前使用`PageHelper.startPage(int pageNum, int pageSize)`开启分页功能

> pageNum：当前页的页码
>
> pageSize：每页显示的条数

b> 在查询获取list集合之后，使用`PageInfo<T> pageInfo = new PageInfo<>(List<T> list, int navigatePages)`获取分页相关数据

```java
@Test
public void testPage(){
    SqlSession sqlSession = SqlSessionUtil.getSqlSession();
    EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
    // 查询开始之前开启分页功能
    Page<Object> page = PageHelper.startPage(6, 4);
    List<Emp> emps = mapper.selectByExample(null);
    PageInfo<Emp> empPageInfo = new PageInfo<>(emps, 5);
    System.out.println(emps);
    emps.forEach(System.out::println);
    System.out.println(empPageInfo);
}
```

> PageInfo{
>
> pageNum=6, pageSize=4, size=4, startRow=21, endRow=24, total=34, pages=9,
>
> list=Page{count=true, pageNum=6, pageSize=4, startRow=20, endRow=24, total=34, pages=9, 
>
> reasonable=false, pageSizeZero=false},
>
> prePage=5, nextPage=7, isFirstPage=false, isLastPage=false, hasPreviousPage=true, hasNextPage=true
>
>  navigatePages=5, navigateFirstPage=4, navigateLastPage=8,
>
> navigatepageNums=[4, 5, 6, 7, 8]
>
> }
>
> pageNum：当前页的页码
>
> pageSize：每页显示的条数
>
> size：当前页显示的真实条数
>
> total：总记录数
>
> pages：总页数
>
> prePage：上一页的页码
>
> nextPage：下一页的页码
>
> isFirstPage/isLastPage：是否为第一页/最后一页
>
> hasPreviousPage/hasNextPage：是否存在上一页/下一页
>
> navigatePages：导航分页的页码数
>
> navigatepageNums：导航分页的页码，[1,2,3,4,5]

# 二、Spring

## 1、Spring简介

### 1.1 Spring概述

官网地址：https://spring.io/

> - Spring 是最受欢迎的企业级 Java 应用程序开发框架，数以百万的来自世界各地的开发人员使用
>
> - Spring 框架来创建性能好、易于测试、可重用的代码。
>
> - Spring 框架是一个开源的 Java 平台，它最初是由 Rod Johnson 编写的，并且于 2003 年 6 月首次在 Apache 2.0 许可下发布。
>
> - Spring 是轻量级的框架，其基础版本只有 2 MB 左右的大小。
>
> - Spring 框架的核心特性是可以用于开发任何 Java 应用程序，但是在 Java EE 平台上构建 web 应用程序是需要扩展的。 Spring 框架的目标是使 J2EE 开发变得更容易使用，通过启用基于 POJO编程模型来促进良好的编程实践。

### 1.2 Spring家族

项目列表：https://spring.io/projects

### 1.3 Spring Framework

Spring 基础框架，可以视为 Spring 基础设施，基本上任何其他 Spring 项目都是以 Spring Framework为基础的。

#### 1.3.1 Spring Framework特性

> - `非侵入式`：使用 Spring Framework 开发应用程序时，Spring 对应用程序本身的结构影响非常小。对领域模型可以做到零污染；对功能性组件也只需要使用几个简单的注解进行标记，完全不会破坏原有结构，反而能将组件结构进一步简化。这就使得基于 Spring Framework 开发应用程序时结构清晰、简洁优雅。
> - `控制反转`：IOC——Inversion of Control，翻转资源获取方向。把自己创建资源、向环境索取资源变成环境将资源准备好，我们享受资源注入。
>   - 把我们当前对象的控制权，反转给了程序本身，也就是由之前程序员自己`new`对象的方式变成交给spring管理。我们从主动获取对象变成了被动去接受spring框架的一个注入，其实就是框架为我们提供这个对象。
>   - Spring里的对象配置在XML文件中，我们配置文件配置什么对象，Spring就为我们提供什么对象。降低了程序的耦合性和对象与对象之间的依赖关系。
> - `面向切面编程`：AOP——Aspect Oriented Programming，在不修改源代码的基础上增强代码功能。
>   - 面向切面：对面向对象的一个补充。面向对象只能`纵向`通过继承来增强方法，而面向切面直接横向通过通知增强方法。
>   - 比如：事务管理它不是一段连续执行的代码，此时对其用面向对象的方式进行封装是实现不了的。我们就需要使用面向切面（横向抽取），可以把一些事务管理的代码横向抽取出来，到一个切面中。不改变源代码的基础上，把我们当前实现事务管理的这个切面`作用于`我们当前需要被事务管理的方法中。
> - `容器`：Spring IOC 是一个容器，因为它包含并且管理组件对象的生命周期。组件享受到了容器化的管理，替程序员屏蔽了组件创建过程中的大量细节，极大的降低了使用门槛，大幅度提高了开发效率。
> - `底层`：工厂模式，把创建对象的完整过程进行了屏蔽。我们只需使用，无需关注细节。
> - `组件化`：Spring 实现了使用简单的组件配置组合成一个复杂的应用。在 Spring 中可以使用 XML和 Java 注解组合这些对象。这使得我们可以基于一个个功能明确、边界清晰的组件有条不紊的搭建超大型复杂应用系统。
> - `声明式`：很多以前需要编写代码才能实现的功能，现在只需要声明需求即可由框架代为实现。
> - `一站式`：在 IOC 和 AOP 的基础上可以整合各种企业应用的开源框架和优秀的第三方类库。而且Spring 旗下的项目已经覆盖了广泛领域，很多方面的功能性需求可以在 Spring Framework 的基础上全部使用 Spring 来实现。

#### 1.3.2 Spring Framework五大功能模块

| 功能模块                | 功能介绍                                                  |
| ----------------------- | --------------------------------------------------------- |
| Core Container          | 核心容器，在 Spring 环境下使用任何功能都必须基于 IOC 容器 |
| AOP&Aspects             | 面向切面编程                                              |
| Testing                 | 提供了对 junit 或 TestNG 测试框架的整合                   |
| Data Access/Integration | 提供了对数据访问/集成的功能                               |
| Spring MVC              | 提供了面向Web应用程序的集成功能                           |

## 2、IOC

### 2.1 IOC容器

#### 2.1.1 IOC思想

IOC：Inversion of Control **反转控制**

##### ① 获取资源的传统方式

自己做饭：买菜、洗菜、择菜、改刀、炒菜，全过程参与，费时费力，必须清楚了解资源创建整个过程中的全部细节且熟练掌握。

在应用程序中的组件需要获取资源时，传统的方式是组件**主动**的从容器中获取所需要的资源，在这样的模式下开发人员往往需要知道在具体容器中特定资源的获取方式，增加了学习成本，同时降低了开发效率。

##### ② 反转控制方式获取资源

点外卖：下单、等、吃，省时省力，不必关心资源创建过程的所有细节。

制的思想完全颠覆了应用程序组件获取资源的传统方式：反转了资源的获取方向——改由容器主动的将资源推送给需要的组件，开发人员不需要知道容器是如何创建资源对象的，只需要提供接收资源的方式即可，极大的降低了学习成本，提高了开发的效率。这种行为也称为查找的**被动**形式。

##### ③ DI

> - 依赖哪个对象，Spring就给哪个对象赋值
>   - 比如`Controller层`依赖于`Service`层的`service`对象

**DI**：Dependency Injection，翻译过来是**依赖注入**。

DI 是 IOC 的另一种表述方式：即组件以一些预先定义好的方式（例如：setter 方法）接受来自于容器的资源注入。相对于IOC而言，这种表述更直接。

所以结论是：IOC 就是一种反转控制的思想， 而 DI 是对 IOC 的一种具体实现。

#### 2.1.2 IOC容器在Spring中的实现

Spring 的 IOC 容器就是 IOC 思想的一个落地的产品实现。IOC 容器中管理的组件也叫做 bean。在创建bean 之前，首先需要创建 IOC 容器。Spring 提供了 IOC 容器的两种实现方式：

##### ① BeanFactory

这是 IOC 容器的基本实现，是 Spring 内部使用的接口。面向 Spring 本身，不提供给开发人员使用。

##### ② ApplicationContext

BeanFactory 的子接口，提供了更多高级特性。面向 Spring 的使用者，几乎所有场合都使用ApplicationContext 而不是底层的 BeanFactory。

##### ③ ApplicationContext主要实现类

![image-20230512143355400](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702824.png)

| 类型名                          | 简介                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| ClassPathXmlApplicationContext  | 通过读取类路径下的 XML 格式的配置文件创建 IOC 容器对象       |
| FileSystemXmlApplicationContext | 通过文件系统路径读取 XML 格式的配置文件创建 IOC 容器对象     |
| ConfigurableApplicationContext  | ApplicationContext 的子接口，包含一些扩展方法refresh() 和 close() ，让 ApplicationContext 具有启动、关闭和刷新上下文的能力。 |
| WebApplicationContext           | 专门为 Web 应用准备，基于 Web 环境创建 IOC 容器对象，并将对象引入存入 ServletContext 域中。 |

> - 以后的java程序打成jar包，不一定在原电脑运行，在其他电脑运行时此时如果用`FileSystemXmlApplicationContext`创建IOC容器，可能会出现路径错误

### 2.2 基于 XML管理bean

#### 2.2.1 实验一：入门案例

##### ① 创建Maven Module

##### ② 引入依赖

> - spring-context
>   - context：上下文
>   - 依赖具有传递性，spring-context所依赖的jar包也会自动导入maven工程

```xml
<dependencies>
    <!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.1</version>
    </dependency>
    <!-- junit测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

![image-20230512144022946](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702825.png)

##### ③ 创建HelloWorld类

```java
package com.cjj.spring.pojo;

public class HelloWorld {

    public void sayHello(){
        System.out.println("Hello,Spring");
    }

}

```

##### ④ 创建Spirng的配置文件

> - `applicationContext.xml`

![image-20230512150051744](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702826.png)

![image-20230512150117498](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702827.png)

##### ⑤ 在Spring的配置文件中配置bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--
    约束：规定用户能够定义什么标签等
 -->
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        配置HelloWorld所对应的bean，即将HelloWorld的对象交给Spring的IOC容器管理
        通过bean标签配置IOC容器所管理的bean
        属性：
        id：设置bean的唯一标识
        class：设置bean所对应类型的全类名
    -->
    <bean id="helloworld" class="com.cjj.spring.pojo.HelloWorld"></bean>

</beans>
```

##### ⑥ 创建测试类测试

> - resouce目录和java目录里的内容最终会被加载到同一路径下（类路径）

```java
@Test
public void test(){
    // 获取IOC容器
    ApplicationContext ioc = new ClassPathXmlApplicationContext("applicationContext.xml");
    // 获取IOC容器中的bean
    /*
    *   由于是用id获取bean,并不知道bean的类型,所以返回的是一个Object对象。
    *   需要我们手动进行类型的强制转换
    * */
    HelloWorld helloworld = (HelloWorld) ioc.getBean("helloworld");
    helloworld.sayHello();
}
```

##### ⑦ 思路

> - ioc创建对象的方式
>   - 在解析spring的xml配置文件时，获取bean标签上class的属性值，也就知道了当前bean标签所管理对象的类型的`全类名`。但是bean标签的`class`属性值是不确定的，不同bean标签可以配置不同类型的对象。因此想要获取对象则需通过IOC容器需要通过`反射`的方式：`Class.forName()`方法获取class类对象，通过class对象的`new Instance()`方法创建实例对象。而反射创建对象默认是通过无参构造器来创建对象的。

![image-20230513211148178](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702828.png)

##### ⑧ 注意

Spring 底层默认通过反射技术调用组件类的无参构造器来创建组件对象，这一点需要注意。如果在需要无参构造器时，没有无参构造器，则会抛出下面的异常：

```java
org.springframework.beans.factory.BeanCreationException: Error creating bean with name
'helloworld' defined in class path resource [applicationContext.xml]: Instantiation of bean
failed; nested exception is org.springframework.beans.BeanInstantiationException: Failed
to instantiate [com.atguigu.spring.bean.HelloWorld]: No default constructor found; nested
exception is java.lang.NoSuchMethodException: com.atguigu.spring.bean.HelloWorld.<init>
()
```

#### 2.2.2 实验二：获取bean

##### ① 方式一：根据id获取

```java
@Test
public void testIOC(){
    // 获取IOC容器
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc.xml");
    // 获取bean对象
    Student student = (Student) ioc.getBean("studentOne"); 
    System.out.println(student);
}
```

##### ② 方式二：根据bean的类型获取

```java
@Test
public void testIOC(){
    // 获取IOC容器
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc.xml");
    // 获取bean对象
    Student student = ioc.getBean(Student.class);
    System.out.println(student);
}
```

##### ③ 方式三：根据id和bean类型获取

```java
@Test
public void testIOC(){
    // 获取IOC容器
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc.xml");
    Student student = ioc.getBean("studentOne", Student.class);
    System.out.println(student);
}
```

##### ④ 注意

当根据类型获取bean时，要求IOC容器中指定类型的bean有且只能有一个

当IOC容器中一共配置了两个：

```xml
<bean id="helloworldOne" class="com.atguigu.spring.bean.HelloWorld"></bean>
<bean id="helloworldTwo" class="com.atguigu.spring.bean.HelloWorld"></bean>
```

根据类型获取时会抛出异常：`NoUniqueBeanDefinitionException`

```java
org.springframework.beans.factory.NoUniqueBeanDefinitionException: No qualifying bean
of type 'com.atguigu.spring.bean.HelloWorld' available: expected single matching bean but
found 2: helloworldOne,helloworldTwo
```

##### ⑤ 扩展

如果组件类实现了接口，根据接口类型可以获取 bean 吗？

> - 可以，前提是bean唯一
> - 我的理解：spring获取了接口的class对象，通过反射机制在运行时获取了接口的实现类信息，因为实现类所在的bean是唯一的，所以能够根据类名找到唯一的bean，从而IOC容器转为通过`方式二：根据bean的类型`来获取bean对象。

```java
@Test
public void testIOC(){
    // 获取IOC容器
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc.xml");
    // 获取bean对象
    Person person = ioc.getBean(Person.class);
    System.out.println(person);
}
```

如果一个接口有多个实现类，这些实现类都配置了 bean，根据接口类型可以获取 bean 吗？

> - 不行，因为bean不唯一

##### ⑥ 结论

根据类型来获取bean时，在满足bean唯一性的前提下，其实只是看：『对象 **instanceof** 指定的类型』的返回结果，只要返回的是true就可以认定为和类型匹配，能够获取到。

> - 即通过bean的类型、bean所继承的类的类型、bean所实现的接口的类型都可以获取该bean
> - `ioc.getBean(bean的类型|bean所继承的类的类型|bean所实现的接口的类型)`;

```java
@Test
public void testIOC(){
    // 获取IOC容器
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc.xml");
    // 获取bean对象
    Person person = 
}
```

#### 2.2.3 实验三：依赖注入之setter注入

> - 依赖注入
>   - 简单来说，就是通过set方法给类中的属性赋值。实现方式就是通过在配置文件的bean标签里进行属性赋值。
>   - 属性：set方法

##### ① 创建学生类Student  

```java
package com.cjj.spring.pojo;

public class Student implements Person{

    private Integer sid;

    private String name;

    private Integer age;

    private String gender;

    public Student(Integer sid, String name, Integer age, String gender) {
        this.sid = sid;
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    public Student() {

    }

    public Integer getSid() {
        return sid;
    }

    public void setSid(Integer sid) {
        this.sid = sid;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    @Override
    public String toString() {
        return "Student{" +
                "sid=" + sid +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                '}';
    }
}
```

##### ② 配置bean时为属性赋值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- property标签：通过组件类的setXxx()方法给组件对象设置属性 -->
    <!-- name属性：指定属性名（这个属性名是getXxx()、setXxx()方法定义的，和成员变量无关）-->
    <!-- value属性：指定属性值 -->
    <bean id="studentTwo" class="com.cjj.spring.pojo.Student">
        <property name="sid" value="1001"></property>
        <property name="name" value="张三"></property>
        <property name="age" value="23"></property>
        <property name="gender" value="男"></property>
    </bean>
</beans>
```

##### ③ 测试

```java
@Test
public void testDI(){
    // 获取IOC容器
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc.xml");
    // 获取bean
    Student student = ioc.getBean("studentTwo", Student.class);
    // Student{sid=1001, name='张三', age=23, gender='男'}
    System.out.println(student);
}
```

#### 2.2.4 实验四：依赖注入之构造器注入

##### ① 在Student类中添加有参构造

```java
public Student(Integer id, String name, Integer age, String sex) {
    this.id = id;
    this.name = name;
    this.age = age;
    this.sex = sex;
}
```

##### ② 配置bean

> - 注意：
>   - constructor-arg标签还有两个属性可以进一步描述构造器参数：
>
>   - index属性：指定参数所在位置的索引（从0开始）
>
>   - name属性：指定参数名
>
>     - ```xml
>       <constructor-arg value="23" name="age"></constructor-arg>
>       ```

```xml
<bean id="studentTwo" class="com.atguigu.spring.bean.Student">
    <constructor-arg value="1002"></constructor-arg>
    <constructor-arg value="李四"></constructor-arg>
    <constructor-arg value="33"></constructor-arg>
    <constructor-arg value="女"></constructor-arg>
</bean>
```

##### ③ 测试

```java
@Test
public void testDIBySet(){
    // 获取IOC容器
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc.xml");
    // 获取bean
    Student student = ioc.getBean("studentTwo", Student.class);
    System.out.println(student);
}
```

#### 2.2.5 实验五：特殊值处理

##### ① 字面量赋值

> 什么是字面量？
>
> int a = 10;
>
> 声明一个变量a，初始化为10，此时a就不代表字母a了，而是作为一个变量的名字。当我们引用a的时候，我们实际上拿到的值是10。
>
> 而如果a是带引号的：'a'，那么它现在不是一个变量，它就是代表a这个字母本身，这就是字面量。所以字面量没有引申含义，就是我们看到的这个数据本身。

```xml
<!-- 使用value属性给bean的属性赋值时，Spring会把value属性的值看做字面量 -->
<property name="name" value="张三"/>
```

##### ② null值

```xml
<bean id="studentFour" class="com.cjj.spring.pojo.Student">
    <property name="sid" value="1001"></property>
    <property name="name">
        <value><![CDATA[<王五>]]></value>
    </property>
    <property name="age" value="23"></property>
    <!-- 属性设置为null值 -->
    <property name="gender" >
        <null></null>
    </property>
</bean>
```

> - 注意：
>
> ```xml
> <property name="name" value="null"></property>
> ```
>
> ​	以上写法，为name所赋的值是字符串null

##### ③ xml实体

```xml
<!-- 小于号在XML文档中用来定义标签的开始，不能随便使用 -->
<!-- 解决方案一：使用XML实体来代替 -->
<property name="expression" value="a &lt; b"/>
```

##### ④ CDATA节

```xml
<property name="expression">
    <!-- 解决方案二：使用CDATA节 -->
    <!-- CDATA中的C代表Character，是文本、字符的含义，CDATA就表示纯文本数据 -->
    <!-- XML解析器看到CDATA节就知道这里是纯文本，就不会当作XML标签或属性来解析 -->
    <!-- 所以CDATA节中写什么符号都随意 -->
    <value><![CDATA[a < b]]></value>
</property
```

#### 2.2.6 实验六：为类类型属性赋值

##### ① 创建班级类Clazz

```java
package com.cjj.spring.pojo;

public class Clazz {

    private Integer cid;

    private String cname;

    public Clazz() {
    }

    public Clazz(Integer cid, String cname) {
        this.cid = cid;
        this.cname = cname;
    }

    public Integer getCid() {
        return cid;
    }

    public void setCid(Integer cid) {
        this.cid = cid;
    }

    public String getCname() {
        return cname;
    }

    public void setCname(String cname) {
        this.cname = cname;
    }

    @Override
    public String toString() {
        return "Clazz{" +
                "cid=" + cid +
                ", cname='" + cname + '\'' +
                '}';
    }
}
```

##### ② 修改Student类

> - 学生和班级是多对一的关系，在多的这边添加Clazz实体类对象属性。

```java
private Clazz clazz;

public Clazz getClazz() {
    return clazz;
}
public void setClazz(Clazz clazz) {
    this.clazz = clazz;
}
```

##### ③ 方式一：引用外部已声明的bean

> - 配置Clazz类型的bean

```xml
<bean id="classOne" class="com.cjj.spring.pojo.Clazz">
    <property name="cid" value="201"></property>
    <property name="cname" value="计科201"></property>
</bean>
```

> - 为Student中的clazz属性赋值

```xml
<bean id="studentFive" class="com.cjj.spring.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="name" value="赵六"></property>
    <property name="age" value="26"></property>
    <property name="gender" value="男"></property>
    <!-- ref属性：引用IOC容器中某个bean的id，将所对应的bean为属性赋值 -->
    <property name="clazz" ref="classOne"></property>
 </bean>
```

> - 错误演示
>
> - 如果错把ref属性写成了value属性，会抛出异常： `Caused by: java.lang.IllegalStateException:Cannot convert value of type 'java.lang.String' to required type'com.atguigu.spring.bean.Clazz' for property 'clazz': no matching editors or conversionstrategy found`
>
>   意思是不能把String类型转换成我们要的Clazz类型，说明我们使用value属性时，Spring只把这个属性看做一个普通的字符串，不会认为这是一个bean的id，更不会根据它去找到bean来赋值

```xml
<bean id="studentFive" class="com.cjj.spring.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="name" value="赵六"></property>
    <property name="age" value="26"></property>
    <property name="gender" value="男"></property>
    <property name="clazz" value="classOne"></property>
 </bean>
```

##### ④ 方式二：内部bean

```xml
<bean id="studentFour" class="com.atguigu.spring.bean.Student">
    <property name="id" value="1004"></property>
    <property name="name" value="赵六"></property>
    <property name="age" value="26"></property>
    <property name="sex" value="女"></property>
    <property name="clazz">
        <!-- 在一个bean中再声明一个bean就是内部bean -->
        <!-- 内部bean只能用于给属性赋值，不能在外部通过IOC容器获取，因此可以省略id属性 -->
        <bean id="clazzInner" class="com.atguigu.spring.bean.Clazz">
            <property name="clazzId" value="2222"></property>
            <property name="clazzName" value="远大前程班"></property>
        </bean>
    </property>
</bean>
```

##### ⑤ 方式三：级联属性赋值

```xml
<bean id="studentFour" class="com.atguigu.spring.bean.Student">
    <property name="id" value="1004"></property>
    <property name="name" value="赵六"></property>
    <property name="age" value="26"></property>
    <property name="sex" value="女"></property>
    <!-- 一定先引用某个bean为属性赋值，才可以使用级联方式更新属性 -->
    <property name="clazz" ref="clazzOne"></property>
    <property name="clazz.clazzId" value="3333"></property>
    <property name="clazz.clazzName" value="最强王者班"></property>
</bean>
```

#### 2.2.7 实验七：为数组类型属性赋值

##### ① 修改Student类

> - 添加以下代码

```java
private String[] hobbies;

public String[] getHobbies() {
	return hobbies;
}

public void setHobbies(String[] hobbies) {
	this.hobbies = hobbies;
}
```

##### ② 配置bean

```xml
<bean id="studentFive" class="com.cjj.spring.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="name" value="赵六"></property>
    <property name="age" value="26"></property>
    <property name="gender" value="男"></property>
    <!-- ref属性：引用IOC容器中某个bean的id，将所对应的bean为属性赋值 -->
    <property name="clazz" ref="clazzOne"></property>
    <property name="hobby">
        <array>
            <value>唱跳</value>
            <value>rap</value>
            <value>篮球</value>
        </array>
    </property>
 </bean>
```

#### 2.2.8 实验八：为集合类型属性赋值

##### ① 为List集合类型属性赋值

> 在Clazz类中添加以下代码：

```java
private List<Student> students;

public List<Student> getStudents() {
	return students;
}

public void setStudents(List<Student> students) {
	this.students = students;
}
```

> 配置bean
>
> - 若为Set集合类型属性赋值，只需要将其中的list标签改为set标签即可

```xml
<bean id="classOne" class="com.cjj.spring.pojo.Clazz">
    <property name="cid" value="201"></property>
    <property name="cname" value="计科201"></property>
    <property name="students">
        <list>
            <ref bean="studentOne"></ref>
            <ref bean="studentTwo"></ref>
            <ref bean="studentThree"></ref>
        </list>
    </property>
</bean>
```

##### ② 为Map集合类型属性赋值

> 创建Teacher类

```java
package com.cjj.spring.pojo;

public class Teacher {

    private Integer tid;

    private String tname;

    public Teacher() {
    }

    public Teacher(Integer tid, String tname) {
        this.tid = tid;
        this.tname = tname;
    }

    public Integer getTid() {
        return tid;
    }

    public void setTid(Integer tid) {
        this.tid = tid;
    }

    public String getTname() {
        return tname;
    }

    public void setTname(String tname) {
        this.tname = tname;
    }

    @Override
    public String toString() {
        return "Teacher{" +
                "tid=" + tid +
                ", tname='" + tname + '\'' +
                '}';
    }
}
```

> 在Student类添加如下代码

```java
private Map<String, Teacher> teacherMap;

public Map<String, Teacher> getTeacherMap() {
	return teacherMap;
}

public void setTeacherMap(Map<String, Teacher> teacherMap) {
	this.teacherMap = teacherMap;
}
```

> 配置bean

```xml
<bean id="teacherOne" class="com.cjj.spring.pojo.Teacher">
    <property name="tid" value="10001"></property>
    <property name="tname" value="大白"></property>
</bean>

<bean id="teacherTwo" class="com.cjj.spring.pojo.Teacher">
    <property name="tid" value="10002"></property>
    <property name="tname" value="小白"></property>
</bean>

<bean id="studentFive" class="com.cjj.spring.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="name" value="赵六"></property>
    <property name="age" value="26"></property>
    <property name="gender" value="男"></property>
    <!--<property name="clazz" ref="classOne"></property>-->
    <property name="clazz" >
        <!-- 内部bean，只能在当前bean的内部使用，不能直接通过IOC容器获取 -->
        <bean id="clazzInner" class="com.cjj.spring.pojo.Clazz">
            <property name="cid" value="202"></property>
            <property name="cname" value="计科202"></property>
        </bean>
    </property>
    <property name="hobby">
        <array>
            <value>唱跳</value>
            <value>rap</value>
            <value>篮球</value>
        </array>
    </property>
    <property name="teacherMap">
        <map>
            <entry key="10001" value-ref="teacherOne"></entry>
            <entry key="10002" value-ref="teacherTwo"></entry>
        </map>
    </property>
 </bean>
```

##### ③ 引用集合类型的bean

> 使用`util:list`、`util:map`标签必须引入相应的命名空间，可以通过idea的提示功能选择

```xml
<!-- 配置一个集合类型的bean，需要使用util的约束标签 -->
<util:list id="studentList">
    <ref bean="studentOne"></ref>
    <ref bean="studentTwo"></ref>
    <ref bean="studentThree"></ref>
</util:list>

<util:map id="teacherMap">
    <entry key="10001" value-ref="teacherOne"></entry>
    <entry key="10002" value-ref="teacherTwo"></entry>
</util:map>

<bean id="studentFive" class="com.cjj.spring.pojo.Student">
    <property name="sid" value="1004"></property>
    <property name="name" value="赵六"></property>
    <property name="age" value="26"></property>
    <property name="gender" value="男"></property>
    <!--<property name="clazz" ref="classOne"></property>-->
    <property name="clazz" >
        <!-- 内部bean，只能在当前bean的内部使用，不能直接通过IOC容器获取 -->
        <bean id="clazzInner" class="com.cjj.spring.pojo.Clazz">
            <property name="cid" value="202"></property>
            <property name="cname" value="计科202"></property>
        </bean>
    </property>
    <property name="hobby">
        <array>
            <value>唱跳</value>
            <value>rap</value>
            <value>篮球</value>
        </array>
    </property>
    <property name="teacherMap" ref="teacherMap"></property>
 </bean>
```

#### 2.2.9 实验九：p命名空间

> 引入p命名空间后，可以通过以下方式为bean的各个属性赋值

```xml
<bean id="studentSix" class="com.cjj.spring.pojo.Student"
      p:sid="1005" p:name="小红" p:teacherMap-ref="teacherMap">
</bean>
```

#### 2.2.10 实验十：引入外部属性文件

##### ① 加入依赖

```xml
<!-- MySQL驱动 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.16</version>
</dependency>
<!-- 数据源 -->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.0.31</version>
</dependency>
```

##### ② 创建外部属性文件	

![image-20230514231311145](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702829.png)

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:13306/ssm?serverTimezone=UTC
jdbc.username=root
jdbc.password=root
```

##### ③ 引入属性文件

```xml
<!-- 引入外部属性文件 -->
<context:property-placeholder location="classpath:jdbc.properties"/>
```

##### ④ 配置bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!--引入jdbc.properties文件,之后通过${key}的方式访问value-->
    <!--<context:property-placeholder location="jdbc.properties"></context:property-placeholder>-->
    <context:property-placeholder location="jdbc.properties"></context:property-placeholder>

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
</beans>
```

##### ⑤ 测试

```java
@Test
public void testDataSource() throws SQLException {
    ClassPathXmlApplicationContext ioc = new ClassPathXmlApplicationContext("springdatasource.xml");
    DruidDataSource dataSource = ioc.getBean(DruidDataSource.class);
    System.out.println(dataSource.getConnection());
}
```

#### 2.2.11 实验十一：bean的作用域

##### ① 概念

在Spring中可以通过配置bean标签的scope属性来指定bean的作用域范围，各取值含义参加下表：

| 取值              | 含义                                    | 创建对象的时机  |
| ----------------- | --------------------------------------- | --------------- |
| singleton（默认） | 在IOC容器中，这个bean的对象始终为单实例 | IOC容器初始化时 |
| prototype         | 这个bean在IOC容器中有多个实例           | 获取bean时      |

如果是在WebApplicationContext环境下还会有另外两个作用域（但不常用）：

| 取值    | 含义                 |
| ------- | -------------------- |
| request | 在一个请求范围内有效 |
| session | 在一个会话范围内有效 |

##### ② 创建类User

```java
package com.cjj.spring.pojo;

public class User {
    private Integer id;
    private String username;
    private String password;
    private Integer age;

    public User() {
    }

    public User(Integer id, String username, String password, Integer age) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.age = age;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", age=" + age +
                '}';
    }
}
```

##### ③ 配置bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- scope属性：取值singleton（默认值），bean在IOC容器中只有一个实例，IOC容器初始化时创建对象 -->
    <!-- scope属性：取值prototype，bean在IOC容器中可以有多个实例，getBean()时创建对象 -->
    <bean id="student" class="com.cjj.spring.pojo.User"></bean>

</beans>
```

##### ④ 测试

```java
@Test
public void testScope(){
    ApplicationContext ioc = new ClassPathXmlApplicationContext("spring-scope.xml");
    User user1 = ioc.getBean(User.class);
    User user2 = ioc.getBean(User.class);
    System.out.println(user1==user2);
}
```

#### 2.2.12 实验十二：bean的生命周期

##### ① 具体生命周期过程

> - 在单例作用域的bean的情况下，获取ioc容器时即可完成如下三个生命周期步骤。原因是，单例模式下用的都是同一个对象，所以获取IOC容器的时候就完成bean的初始化，getBean的时候直接获取该对象的引用即可。
>   - 实例化
>   - 依赖注入
>   - 初始化

- bean对象创建（调用无参构造器）
- 给bean对象设置属性
- bean对象初始化之前操作（由bean的后置处理器负责）
  - `postProcessBeforeInitialization()`
- bean对象初始化（需在配置bean时指定初始化方法）
- bean对象初始化之后操作（由bean的后置处理器负责）
  - `postProcessAfterInitialization()`
- bean对象就绪可以使用
- bean对象销毁（需在配置bean时指定销毁方法）
- IOC容器关闭

##### ② 修改类User

> 注意其中的initMethod()和destroyMethod()，可以通过配置bean指定为初始化和销毁的方法

```java
package com.cjj.spring.pojo;

public class User {
    private Integer id;
    private String username;
    private String password;
    private Integer age;
	
    public User() {
        System.out.println("生命周期1：实例化");
    }

    public User(Integer id, String username, String password, Integer age) {
        this.id = id;
        this.username = username;
        this.password = password;
        this.age = age;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        System.out.println("生命周期2：setId()依赖注入");
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public void initMethod(){
        System.out.println("生命周期3：initMethod()初始化");
    }

    public void destroyMethod(){
        System.out.println("生命周期4：destroyMethod()销毁");
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                ", age=" + age +
                '}';
    }
}
```

##### ③ 配置bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!-- 使用init-method属性指定初始化方法 -->
    <!-- 使用destroy-method属性指定销毁方法 -->
    <bean id="user" class="com.cjj.spring.pojo.User" scope="" init-method="initMethod" destroy-method="destroyMethod">
        <property name="id" value="1"></property>
        <property name="username" value="admin"></property>
        <property name="password" value="123"></property>
        <property name="age" value="12"></property>
    </bean>

</beans>
```

##### ④ 测试

```java
@Test
public void test(){
    /*
    *   ConfigurableApplicationContext是ApplicationContext的子接口
    *   其中扩展了刷新和关闭容器的方法
    * */
    ConfigurableApplicationContext ioc = new ClassPathXmlApplicationContext("spring-lifecycle.xml");
    User user = ioc.getBean(User.class);
    System.out.println(user);
    ioc.close();
}
```

##### ⑤ bean的后置处理器

bean的后置处理器会在生命周期的初始化前后添加额外的操作，需要实现BeanPostProcessor接口，且配置到IOC容器中，需要注意的是，bean后置处理器不是单独针对某一个bean生效，而是针对IOC容器中`所有bean`都会执行

创建bean的后置处理器：

```java
package com.cjj.spring.process;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;

public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        // 此方法在bean的生命周期初始化之前执行
        System.out.println("MyBeanPostProcessor-->后置处理器postProcessBeforeInitialization");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        // 此方法在bean的生命周期初始化之后执行
        System.out.println("MyBeanPostProcessor-->后置处理器postProcessAfterInitialization");
        return bean;
    }
}
```

在IOC容器中配置后置处理器：

```xml
<!-- bean的后置处理器要放入IOC容器才能生效,此时对所有bean都添加了后置处理器 -->
<bean id="myBeanProcessor" class="com.cjj.spring.process.MyBeanPostProcessor"></bean>
```

#### 2.2.13 实验十三：FactoryBean

> - beanFactory：bean工厂，是IOC的基本实现，作用是用来帮我们`管理bean`的。
> - FactoryBean：工厂bean，专门作为一个Bean，交给IOC容器来管理的。

##### ① 简介

> - 普通工厂创建对象流程
>   - 配置到IOC容器中
>   - 获取工厂bean对象
>   - 调用工厂类中生产对象的方法获取接收对象
> - FactoryBean：少了一个调用方法的步骤。将工厂Bean加入到IOC容器，就可以直接获取工厂提供的bean，不用再获取factory
>   - 配置到IOC容器中
>   - 直接获取该工厂方法所提供的对象

FactoryBean是Spring提供的一种整合第三方框架的常用机制。和普通的bean不同，配置一个FactoryBean类型的bean，在获取bean的时候得到的并不是class属性中配置的这个类的对象，而是getObject()方法的返回值。通过这种机制，Spring可以帮我们把复杂组件创建的详细过程和繁琐细节都屏蔽起来，只把最简洁的使用界面展示给我们。

将来我们整合Mybatis时，Spring就是通过FactoryBean机制来帮我们创建SqlSessionFactory对象的。

##### ② 创建类UserFactoryBean

```java
package com.cjj.spring.factory;

import com.cjj.spring.pojo.User;
import org.springframework.beans.factory.FactoryBean;
// 实现FactoryBean接口
public class UserFactoryBean implements FactoryBean<User> {

    @Override
    public User getObject() throws Exception {
        return new User();
    }

    @Override
    public Class<?> getObjectType() {
        return User.class;
    }
}
```

##### ③ 配置bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
        配置bean的class类型是UserFactoryBean,但真正交给IOC容器管理的是其工厂bean中
        getObject()方法所返回的对象
    -->
    <bean class="com.cjj.spring.factory.UserFactoryBean"></bean>

</beans>
```

##### ④ 测试

```java
@Test
public void testFactoryBean(){
    ApplicationContext ioc = new ClassPathXmlApplicationContext("spring-factory.xml");
    // 直接根据User.class获取对象
    User user = ioc.getBean(User.class);
    System.out.println(user);
}
```

#### 2.2.14 实验十四：基于xml的自动装配

> - 自动装配
>   - 根据指定的策略，在IOC容器中匹配某一个bean，自动为指定的bean中所依赖的类类型或接口类型属性赋值

##### ① 场景模拟

> 创建类UserController

```java
package com.cjj.spring.controller;

import com.cjj.spring.service.UserService;

public class UserController {

    private UserService userService;

    public UserService getUserService() {
        return userService;
    }

    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    public void saveUser(){
        userService.saveUser();
    }
}
```

> 创建接口UserService

```java
package com.cjj.spring.service;

public interface UserService {

    /**
     * 保存用户信息
     */
    void saveUser();
}
```

> 创建类UserServiceImpl实现接口UserService

```java
package com.cjj.spring.service.impl;

import com.cjj.spring.dao.UserDao;
import com.cjj.spring.service.UserService;

public class UserServiceImpl implements UserService {

    private UserDao userDao;

    public UserDao getUserDao() {
        return userDao;
    }

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void saveUser() {
        userDao.saveUser();
    }
}
```

> 创建接口UserDao

```java
package com.cjj.spring.dao;

public interface UserDao {

    /**
     * 保存用户信息
     */
    void saveUser();
}
```

> 创建类UserDaoImpl实现接口UserDao

```java
package com.cjj.spring.dao.impl;

import com.cjj.spring.dao.UserDao;

public class UserDaoImpl implements UserDao {

    @Override
    public void saveUser() {
        System.out.println("保存成功");
    }
}
```

##### ② 配置bean

> 

1. 通过setter注入手动配置bean属性之间的依赖关系

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
    <bean id="userController" class="com.cjj.spring.controller.UserController">
        <!--userService属性引用userService的bean对象来注入-->
        <property name="userService" ref="userService"></property>
    </bean>

    <bean id="userService" class="com.cjj.spring.service.impl.UserServiceImpl">
        <!--userDao属性引用userDao的bean对象来注入-->
        <property name="userDao" ref="userDao"></property>
    </bean>

    <bean id="userDao" class="com.cjj.spring.dao.impl.UserDaoImpl"></bean>
    
</beans>
```

2. 通过bean标签的`autowire="byType"`自动装配属性

> 使用bean标签的autowire属性设置自动装配效果
>
> - 自动装配方式：byType
>
> - byType：根据类型匹配IOC容器中的某个兼容类型的bean，为属性自动赋值。比如属性值为接口类型，则寻找该接口实现类类型唯一匹配的bean，并注入。
>
> - 若在IOC中，没有任何一个兼容类型的bean能够为属性赋值，则该属性不装配，即值为默认值null
>
> - 若在IOC中，有多个兼容类型的bean能够为属性赋值，则抛出异常`NoUniqueBeanDefinitionException`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userController" class="com.cjj.spring.controller.UserController" autowire="byType"></bean>

    <bean class="com.cjj.spring.service.impl.UserServiceImpl" autowire="byType"></bean>

    <bean class="com.cjj.spring.dao.impl.UserDaoImpl"></bean>

</beans>
```

3. 通过bean标签的`autowire="byName"`自动装配属性

> - 自动装配方式：byName
>
> - byName：将自动装配的属性的属性名，作为bean的id在IOC容器中匹配相对应的bean进行赋值

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userController" class="com.cjj.spring.controller.UserController" autowire="byName"></bean>

    <bean id="userService" class="com.cjj.spring.service.impl.UserServiceImpl" autowire="byName"></bean>

    <bean id="userDao" class="com.cjj.spring.dao.impl.UserDaoImpl"></bean>

</beans>
```

### 2.3 基于注解管理bean

#### 2.3.1 实验一：标记与扫描

##### ① 注解

和 XML 配置文件一样，注解本身并不能执行，注解本身仅仅只是做一个标记，具体的功能是框架检测
到注解标记的位置，然后针对这个位置按照注解标记的功能来执行具体操作。

本质上：所有一切的操作都是Java代码来完成的，XML和注解只是告诉框架中的Java代码如何执行。

举例：元旦联欢会要布置教室，蓝色的地方贴上元旦快乐四个字，红色的地方贴上拉花，黄色的地方贴
上气球。

![image-20230515172050397](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702830.png)

班长做了所有标记，同学们来完成具体工作。墙上的标记相当于我们在代码中使用的注解，后面同学们
做的工作，相当于框架的具体操作。

##### ② 扫描

Spring 为了知道程序员在哪些地方标记了什么注解，就需要通过扫描的方式，来进行检测。然后根据注
解进行后续操作。

##### ③ 新建Maven Module

```xml
<dependencies>
    <!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.3.1</version>
    </dependency>
    <!-- junit测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

##### ④ 创建spring配置文件

![image-20230515193605263](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702831.png)

##### ⑤ 标识组件的常用注解

> - `@Component`：将类标识为普通组件 
>
> - `@Controller`：将类标识为控制层组件
>
> - `@Service`：将类标识为业务层组件
>
> - `@Repository`：将类标识为持久层组件

问：以上四个注解有什么关系和区别？

![image-20230515194930165](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702832.png)

通过查看源码我们得知，@Controller、@Service、@Repository这三个注解只是在@Component注解
的基础上起了三个新的名字。

对于Spring使用IOC容器管理这些组件来说没有区别。所以@Controller、@Service、@Repository这
三个注解只是给开发人员看的，让我们能够便于分辨组件的作用。

注意：虽然它们本质上一样，但是为了代码的可读性，为了程序结构严谨我们肯定不能随便胡乱标记。

##### ⑥ 创建组件

创建控制层组件

```java
@Controller
public class UserController {
    
}
```

创建接口UserService

```java
public interface UserService {
    
}
```

创建业务层组件UserServiceImpl

```java
@Service
public class UserServiceImpl implements UserService {
    
}
```

创建接口UserDao

```java
public interface UserDao {
    
}
```

创建持久层组件UserDaoImpl

```java
@Repository
public class UserDaoImpl implements UserDao {
    
}
```

#####  ⑦ 扫描组件

> - 被注解标识且被扫描到的类，将作为组件也就是IOC容器里的bean被管理。相当于内部创建了一个`<bean/>`。
> - 当整合SpringMVC时，它需要扫描包下的控制层，所以此时Spring的配置文件里就要排除扫描此包。

情况一：基本的扫描方式，把指定路径下的所有类进行扫描。

```xml
<!--扫描组件-->
<context:component-scan base-package="com.cjj.spring"></context:component-scan>
```

情况二：指定要排除的组件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
	<!-- context:exclude-filter标签：指定排除规则 -->
    <!--
        type：设置排除或包含的依据
        type="annotation"，根据注解排除，expression中设置要排除的注解的全类名
        type="assignable"，根据类型排除，expression中设置要排除的类型的全类名
    -->
    <!--扫描组件-->
    <context:component-scan base-package="com.cjj.spring">
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        
        <context:exclude-filter type="assignable" expression="com.cjj.spring.controller.UserController"/>
    </context:component-scan>

</beans>
```

情况三：仅扫描指定组件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">
    <!-- context:include-filter标签：指定在原有扫描规则的基础上追加的规则 -->
    <!-- use-default-filters属性：取值false表示关闭默认扫描规则 -->
    <!-- 此时必须设置use-default-filters="false"，因为默认规则即扫描指定包下所有类 -->
    <!--
        type：设置排除或包含的依据
        type="annotation"，根据注解排除，expression中设置要排除的注解的全类名
        type="assignable"，根据类型排除，expression中设置要排除的类型的全类名
    -->
    <!--扫描组件-->
    <context:component-scan base-package="com.cjj.spring" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

</beans>
```

##### ⑧ 测试

```java
@Test
public void test(){
    ApplicationContext ioc = new ClassPathXmlApplicationContext("spring-ioc-annotation.xml");
    UserController userController = ioc.getBean(UserController.class);
    System.out.println(userController);
    UserService userService = ioc.getBean(UserService.class);
    System.out.println(userService);
    UserDao userDao = ioc.getBean(UserDao.class);
    System.out.println(userDao);
}
```

##### ⑨ 组件所对应的bean的id

在我们使用XML方式管理bean的时候，每个bean都有一个唯一标识，便于在其他地方引用。现在使用注解并被扫描后，IOC容器为被扫描的注解所在的类生成了一个bean组件，这个组件仍然应该有一个唯一标识id。

> 默认情况
>
> 类名首字母小写就是bean的id。例如：UserController类对应的bean的id就是userController。
>
> 自定义bean的id可通过标识组件的注解的value属性设置自定义的bean的id
>
> ```java
> @Service("userService") //默认为userServiceImpl 
> 
> public class UserServiceImpl implements UserService {
>     
> }
> ```

#### 2.3.2 实验二：基于注解的自动装配

##### ① 场景模拟

> - 参考基于xml的自动装配
>   - 在UserController中声明UserService对象
>   -  在UserServiceImpl中声明UserDao对象

##### ② @Autowired注解

在成员变量上直接标记@Autowired注解即可完成自动装配，不需要提供setXxx()方法。以后我们在项目中的正式用法就是这样。

```java
@Controller
public class UserController {
    
    @Autowired
    private UserService userService;
    public void saveUser(){
        userService.saveUser();
    }
}
```

```java
public interface UserService {

    /**
     * 保存用户信息
     */
    void saveUser();
}
```

```java
@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserDao userDao;

    public UserDao getUserDao() {
        return userDao;
    }

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public void saveUser() {
        userDao.saveUser();
    }
}
```

```java
public interface UserDao {

    /**
     * 保存用户信息
     */
    void saveUser();
}
```

```java
@Repository
public class UserDaoImpl implements UserDao {

    @Override
    public void saveUser() {
        System.out.println("保存成功");
    }

}
```

##### ③ @Autowired注解其他细节

> `@Autowired`注解可以标记在构造器和set方法上

```java
@Controller
public class UserController {
    
    private UserService userService;
    
    @Autowired
    public UserController(UserService userService) {
        this.userService = userService;
    }

    public void saveUser(){
        userService.saveUser();
    }
}
```

```java
@Controller
public class UserController {

    private UserService userService;

    public UserController(UserService userService) {
        this.userService = userService;
    }

    @Autowired
    public void setUserService(UserService userService) {
        this.userService = userService;
    }

    public void saveUser(){
        userService.saveUser();
    }
}
```

##### ④ @Autowired工作流程

![image-20230516102942281](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702833.png)

- 首先根据所需要的组件类型到IOC容器中查找
  - 能够找到唯一的bean：直接执行装配
  - 如果完全找不到匹配这个类型的bean：装配失败
  - 和所需类型匹配的bean不止一个
    - 没有@Qualifier注解：根据@Autowired标记位置成员变量的变量名作为bean的id进行匹配
      - 能够找到：执行装配
      - 找不到：装配失败
    - 使用@Qualifier注解：根据@Qualifier注解中指定的名称作为bean的id进行匹配
      - 能够找到：执行装配
      - 找不到：装配失败

```java
@Controller
public class UserController {
    
    @Autowired
    @Qualifier("userServiceImpl")
    private UserService userService;
    
	public void saveUser(){
	userService.saveUser();
	}
   
}
```

>@Autowired中有属性required，默认值为true，因此在自动装配无法找到相应的bean时，会装配失败
>
>可以将属性required的值设置为true，则表示能装就装，装不上就不装，此时自动装配的属性为默认值
>
>但是实际开发时，基本上所有需要装配组件的地方都是必须装配的，用不上这个属性。

## 3、AOP

> - AOP：在不修改源码的情况下，进一步增强功能
> - 面向对象是纵向继承机制，只能封装一段连续执行的代码。

### 3.1 场景模拟

#### 3.1.1 声明接口

声明计算器接口Calculator，包含加减乘除的抽象方法

```java
package com.cjj.spring.proxy;

public interface Calculator {
    int add(int i, int j);

    int sub(int i, int j);

    int mul(int i, int j);

    int div(int i, int j);
}

```

#### 3.1.2 创建实现类

![image-20230517102518817](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702834.png)

```java
package com.cjj.spring.proxy;

public class CalculatorImpl implements Calculator {
    @Override
    public int add(int i, int j) {
        int result = i + j;
        System.out.println("方法内部 result = " + result);
        return result;
    }

    @Override
    public int sub(int i, int j) {
        int result = i - j;
        System.out.println("方法内部 result = " + result);
        return result;
    }

    @Override
    public int mul(int i, int j) {
        int result = i * j;
        System.out.println("方法内部 result = " + result);
        return result;
    }

    @Override
    public int div(int i, int j) {
        int result = i / j;
        System.out.println("方法内部 result = " + result);
        return result;
    }
}
```

#### 3.1.3 创建带日志功能的实现类

![image-20230517102612702](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702835.png)

> 

```java
package com.cjj.spring.proxy;

public class CalculatorLogImpl implements Calculator {
    @Override
    public int add(int i, int j) {
        System.out.println("[日志] add 方法开始了，参数是：" + i + "," + j);
        int result = i + j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] add 方法结束了，结果是：" + result);
        return result;
    }

    @Override
    public int sub(int i, int j) {
        System.out.println("[日志] sub 方法开始了，参数是：" + i + "," + j);
        int result = i - j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] sub 方法结束了，结果是：" + result);
        return result;
    }

    @Override
    public int mul(int i, int j) {
        System.out.println("[日志] mul 方法开始了，参数是：" + i + "," + j);
        int result = i * j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] mul 方法结束了，结果是：" + result);
        return result;
    }

    @Override
    public int div(int i, int j) {
        System.out.println("[日志] div 方法开始了，参数是：" + i + "," + j);
        int result = i / j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] div 方法结束了，结果是：" + result);
        return result;
    }
}
```

#### 3.1.4 提出问题

##### ① 现有代码缺陷

针对带日志功能的实现类，我们发现有如下缺陷：

- 对核心业务功能有干扰，导致程序员在开发核心业务功能时分散了精力
- 附加功能分散在各个业务功能方法中，不利于统一维护

##### ② 解决思路

解决这两个问题，核心就是：`解耦`。我们需要把附加功能从业务功能代码中抽取出来。

##### ③ 困难

解决问题的困难：要抽取的代码在方法内部，靠以前`把子类中的重复代码抽取到父类`的方式没法解决。所以需要引入新的技术。

- 你子类调用父类的方法，只能在调用的位置执行，无法实现当前日志的功能：在核心业务代码前后输出日志。

### 3.2 代理模式

#### 3.2.1 概念

##### ① 介绍

二十三种设计模式中的一种，属于结构型模式。它的作用就是通过提供一个代理类，让我们在调用目标方法的时候，不再是直接对目标方法进行调用，而是通过代理类`间接`调用。让不属于目标方法核心逻辑的代码从目标方法中剥离出来——`解耦`。调用目标方法时先调用代理对象的方法，减少对目标方法的调用和打扰，同时让附加功能能够集中在一起也有利于统一维护。

![image-20230517105106226](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702836.png)

> 使用代理后

![image-20230517105155697](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702837.png)

##### ② 生活中的代理

- 广告商找大明星拍广告需要经过经纪人

- 合作伙伴找大老板谈合作要约见面时间需要经过秘书

- 房产中介是买卖双方的代理

##### ③ 相关术语

- 代理：将非核心逻辑剥离出来以后，封装这些非核心逻辑的类、对象、方法。

- 目标：被代理“套用”了非核心逻辑代码的类、对象、方法。

#### 3.2.2 静态代理

创建静态代理类：

```java
package com.cjj.spring.proxy;

public class CalculatorStaticProxy implements Calculator {

    // 目标对象
    private CalculatorImpl target;

    public CalculatorStaticProxy(CalculatorImpl target) {
        this.target = target;
    }

    @Override
    public int add(int i, int j) {
        // 附加功能由代理类中的代理方法来实现
        System.out.println("[日志] add 方法开始了，参数是：" + i + "," + j);
        // 通过目标对象来实现核心业务逻辑
        int addResult = target.add(i, j);
        System.out.println("[日志] add 方法结束了，结果是：" + addResult);
        return addResult;
    }

    @Override
    public int sub(int i, int j) {
        // 附加功能由代理类中的代理方法来实现
        System.out.println("[日志] sub 方法开始了，参数是：" + i + "," + j);
        // 通过目标对象来实现核心业务逻辑
        int subResult = target.sub(i, j);
        System.out.println("[日志] sub 方法结束了，结果是：" + subResult);
        return subResult;
    }

    @Override
    public int mul(int i, int j) {
        // 附加功能由代理类中的代理方法来实现
        System.out.println("[日志] mul 方法开始了，参数是：" + i + "," + j);
        // 通过目标对象来实现核心业务逻辑
        int mulResult = target.add(i, j);
        System.out.println("[日志] mul 方法结束了，结果是：" + mulResult);
        return mulResult;
    }

    @Override
    public int div(int i, int j) {
        // 附加功能由代理类中的代理方法来实现
        System.out.println("[日志] div 方法开始了，参数是：" + i + "," + j);
        // 通过目标对象来实现核心业务逻辑
        int divResult = target.add(i, j);
        System.out.println("[日志] div 方法结束了，结果是：" + divResult);
        return divResult;
    }
}
```

> - 静态代理确实实现了解耦，但是由于代码都写死了，完全不具备任何的灵活性。就拿日志功能来说，将来其他地方也需要附加日志，那还得再声明更多个静态代理类，那就产生了大量重复的代码，日志功能还是分散的，没有统一管理。
>
> - 提出进一步的需求：将日志功能集中到一个代理类中，将来有任何日志需求，都通过这一个代理类来实现。这就需要使用`动态代理技术`了。

#### 3.2.3 动态代理

> - jvm通过jdk提供给我们的API，在代码``运行的过程`中动态的帮我们创建每个目标类对应的动态代理类。
>   - 动态：动态生成每一个目标类所对应的代理类
> - 类加载器
>   - 根类加载器
>     - C写的核心类库
>   - 扩展类加载器
>     - 扩展类库
>   - 应用类加载器
>     - 自己写的类
> - jdk动态代理
>   - 要求必须有接口，最终生成的代理类和目标类实现相同的接口，咋com.sun.proxy包下，类名为$proxy2
> - cglib动态代理
>   - 最终生成的代理类会继承目标类，并且和目标类在相同的包下

> - 是的，proxy在这里是一个接口类型的变量，指向实现该接口的实现类。在这种情况下，虽然factory返回的是一个代理对象，但它已经实现了Calculator接口，因此我们可以将该代理对象转换为Calculator类型。当我们通过代理对象调用Calculator接口中定义的add()方法时，实际上是调用代理对象内部的invoke()方法，该方法执行了我们在ProxyFactory中指定的增强行为，并最终调用了实际的Calculator实现类的add()方法
>
> - 当你获取ProxyFactory类的类加载器时，Java虚拟机会使用ClassLoader加载ProxyFactory类。这个ClassLoader，并不是直接将字节码文件加载到内存中，而是会先到Bootstrap ClassLoader（引导类加载器）中查找指定的类是否已经被加载了。如果没有被加载，则会使用该ClassLoader去加载字节码文件，并返回一个对应的Class对象。这个Class对象持有了被加载类的元信息，在内存中表示该类，可以通过它来创建代理对象和调用其方法。
>
>   在调用Proxy.newProxyInstance()方法时，类加载器会载入目标对象的接口信息，并生成代理类的字节码。在生成代理类对象时，会使用该ClassLoader实例来进行加载操作。因此，该ClassLoader确保了代理对象是在正确的ClassLoader中进行加载的，因为在JVM中，每个ClassLoader有其自己的命名空间，它能看到的类只有在与它在同一个ClassLoader中的类，因此需要确保动态生成的代理类是在与目标对象在同一个ClassLoader中。
>

![image-20230522084744234](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702838.png)

生成代理对象的工厂类：

> - 新生成的代理类字节码文件都是由 ProxyFactory 类的类加载器加载到内存中，并且在内存中动态生成代理对象。在我们调用 Proxy.newProxyInstance(classLoader,interfaces,h) 方法时，JVM 会自动为我们在内存中生成一个新的代理类字节码，并将其动态加载到类加载器中。换而言之，这个代理类是运行时生成的，并且完全存储在内存中。因此，我们无法直接查看代理类的字节码文件，但可以通过反射 API 来获取其类名称、包名，以及方法等相关信息。需要注意的是，代理类的生成和加载会产生一定的性能开销，因此在实际应用中需要根据具体情况进行优化。

```java
package com.cjj.spring.proxy;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;
import java.util.Arrays;

public class ProxyFactory {

    private Object target; // 目标对象

    public ProxyFactory(Object target){
        this.target = target;
    }

    public Object getProxy(){
        /**
         * ClassLoader loader：指定加载来动态生成代理类（ProxyFactory）的这个类的类加载器,
         * Class<?>[] interfaces
         * InvocationHandler h)
         */
        ClassLoader classLoader = this.getClass().getClassLoader();
        Class<?>[] interfaces = this.target.getClass().getInterfaces();
        InvocationHandler h = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                System.out.println("日志，方法："+method.getName()+",参数："+ Arrays.toString(args));
                // proxy表示代理对象，method表示要执行的方法，args表示要执行的方法的参数列表。
                Object result = method.invoke(target, args); // 调用目标对象实现的功能方法
                return result;
            }
        };
        return Proxy.newProxyInstance(classLoader,interfaces,h);
    }

}
```

#### 3.2.4 测试

```java
@Test
public void testDynamicProxy() {
    ProxyFactory factory = new ProxyFactory(new CalculatorImpl());
    // 实现了某接口的类 可以强制转换成该接口类型的变量
    Calculator proxy = (Calculator) factory.getProxy();
    System.out.println(proxy.getClass());
    proxy.add(1, 0);
}
```

### 3.3 AOP概念及相关术语

#### 3.3.1 概述

AOP（Aspect Oriented Programming）是一种设计思想，是软件设计领域中的面向切面编程，它是面向对象编程的一种补充和完善，它以通过预编译方式和`运行期`动态代理方式实现在不修改源代码的情况下给程序动态统一添加额外功能的一种技术。

#### 3.3.2 相关术语

##### ① 横切关注点

从核心业务当中抽取出来的非核心业务代码，如上述的日志功能。在同一个项目中，我们可以使用多个横切关注点对相关方法进行多个不同方面的增强。

这个概念不是语法层面天然存在的，而是根据附加功能的逻辑上的需要：有十个附加功能，就有十个横切关注点。

![image-20230523084130726](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702839.png)

##### ② 通知

把横切关注点封装到我们的一个类中，这个类就叫做`切面`。而一个横切关注点在切面中就叫一个`通知方法`

- 前置通知：在被代理的目标方法**前**执行
- 返回通知：在被代理的目标方法**成功结束**后执行（寿终正寝），return之后。
- 异常通知：在被代理的目标方法**异常结束**后执行（死于非命），目标对象出现异常时执行的内容。
- 后置通知：在被代理的目标方法**最终结束**后执行（盖棺定论），目标对象finally中执行的内容。
- 环绕通知：使用try...catch...finally结构围绕整个被代理的目标方法，包括上面四种通知对应的所有位置  

![image-20230523084408310](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702840.png)

##### ③ 切面

封装通知方法的类

![image-20230523090702809](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702841.png)

##### ④  目标

被代理的目标对象

##### ⑤ 代理

向目标对象应用通知之后创建的代理对象。我们并不需要手动的去创建代理对象和代理工厂，是由AOP封装帮我们实现的。

##### ⑥ 连接点

抽取横切关注点的位置。

这也是一个纯逻辑概念，指的就是一个位置，不是语法定义的。

把方法排成一排，每一个横切位置看成x轴方向，把方法从上到下执行的顺序看成y轴，x轴和y轴的交叉点就是连接点。

![image-20230523091522438](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702842.png)

##### ⑦ 切入点

代码实现，定位连接点的方式。

每个类的方法中都包含多个连接点，所以连接点是类中客观存在的事物（从逻辑上来说）。

如果把连接点看作数据库中的记录，那么切入点就是查询记录的 SQL 语句。

Spring 的 AOP 技术可以通过切入点定位到特定的连接点。切点通过 org.springframework.aop.Pointcut 接口进行描述，它使用类和方法作为连接点的查询条件。

#### 3.3.3 作用

- 简化代码：把方法中固定位置的重复的代码**抽取**出来，让被抽取的方法更专注于自己的核心功能，

  提高内聚性。

- 代码增强：把特定的功能封装到切面类中，看哪里有需要，就往上套，被**套用**了切面逻辑的方法就

  被切面给增强了。

### 3.4 基于注解的AOP

#### 3.4.1 技术说明

> - AspectJ：AOP是一种思想，而AspectJ是AOP的一种具体实现

![image-20230523092638086](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702843.png)

- 动态代理（InvocationHandler）：JDK原生的实现方式，需要被代理的目标类必须实现接口。因为这个技术要求**代理对象和目标对象实现同样的接口**（兄弟两个拜把子模式）。
- cglib：通过**继承被代理的目标类**（认干爹模式）实现代理，所以不需要目标类实现接口。
- AspectJ：本质上是静态代理，并不会动态的生成代理类，**将代理逻辑“织入”被代理的目标类编译得到的字节码文件**，所以最终效果是动态的。weaver就是织入器。Spring只是借用了AspectJ中的注解。

#### 3.4.2 准备工作

##### ① 添加依赖

在IOC所需依赖基础上再加入下面依赖即可：

```xml
<!-- spring-aspects会帮我们传递过来aspectjweaver -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.3.1</version>
</dependency>
```

##### ② 准备被代理的目标资源

接口：

```java
package com.cjj.spring.aop.annotation;

public interface Calculator {
    int add(int i, int j);

    int sub(int i, int j);

    int mul(int i, int j);

    int div(int i, int j);
}
```

实现类：

```java
package com.cjj.spring.proxy;

public class CalculatorLogImpl implements Calculator {
    @Override
    public int add(int i, int j) {
        System.out.println("[日志] add 方法开始了，参数是：" + i + "," + j);
        int result = i + j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] add 方法结束了，结果是：" + result);
        return result;
    }

    @Override
    public int sub(int i, int j) {
        System.out.println("[日志] sub 方法开始了，参数是：" + i + "," + j);
        int result = i - j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] sub 方法结束了，结果是：" + result);
        return result;
    }

    @Override
    public int mul(int i, int j) {
        System.out.println("[日志] mul 方法开始了，参数是：" + i + "," + j);
        int result = i * j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] mul 方法结束了，结果是：" + result);
        return result;
    }

    @Override
    public int div(int i, int j) {
        System.out.println("[日志] div 方法开始了，参数是：" + i + "," + j);
        int result = i / j;
        System.out.println("方法内部 result = " + result);
        System.out.println("[日志] div 方法结束了，结果是：" + result);
        return result;
    }
}
```

#### 3.4.3 创建切面类并配置

> - 切面类和实现类都基于ioc，所以需要交给ioc容器管理

```java
package com.cjj.spring.aop.annotation;

import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.Signature;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

import java.util.Arrays;

@Component
@Aspect // 将当前组件表示为切面
public class LoggerAspect {

    @Pointcut("execution(* com.cjj.spring.aop.annotation.CalculatorImpl.*(..))")
    public void pointCut(){}

    // @Before("execution(public int com.cjj.spring.aop.annotation.CalculatorImpl.add(int,int))")
    // @Before("execution(* com.cjj.spring.aop.annotation.CalculatorImpl.*(..))")
    @Before("pointCut()")
    public void beforAdviceMethod(JoinPoint joinPoint){
        // 获取连接点所对应方法的签名信息
        Signature signature = joinPoint.getSignature();
        // 获取连接点所对应方法的参数
        Object[] args = joinPoint.getArgs();
        System.out.println("LoggerAspect,前置通知,方法："+signature.getName()+",参数："+ Arrays.toString(args));
    }

    // 后置通知e
    @After("pointCut()")
    public void afterAdviceMethod(JoinPoint joinPoint){
        // 获取连接点所对应方法的签名信息
        Signature signature = joinPoint.getSignature();
        // 获取连接点所对应方法的参数
        Object[] args = joinPoint.getArgs();
        System.out.println("LoggerAspect,后置通知,方法："+signature.getName()+",参数："+ Arrays.toString(args));
    }

    // 返回通知
    @AfterReturning(value = "pointCut()",returning = "result")
    public void afterReturningAdviceMethod(JoinPoint joinPoint, Object result){
        // 获取连接点所对应方法的签名信息
        Signature signature = joinPoint.getSignature();
        System.out.println("LoggerAspect,返回通知,方法："+signature.getName()+",结果："+result);
    }

}
```

在Spring的配置文件中配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <context:component-scan base-package="com.cjj.spring.aop.annotation"></context:component-scan>

    <!--
        1、将目标对象和切面交给IOC容器管理（注解+扫描）
        2、开启AspectJ的自动代理，为目标对象自动生成代理
        3、将切面类通过注解@Aspect标识
    -->
    <!--开启基于注解的AOP-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>

</beans>
```

#### 3.4.4 各种通知

- 前置通知：使用@Before注解标识，在被代理的目标方法**前**执行
- 返回通知：使用@AfterReturning注解标识，在被代理的目标方法**成功结束**后执行（**寿终正寝**）
- 异常通知：使用@AfterThrowing注解标识，在被代理的目标方法**异常结束**后执行（**死于非命**）
- 后置通知：使用@After注解标识，在被代理的目标方法**最终结束**后执行（**盖棺定论**）
- 环绕通知：使用@Around注解标识，使用try...catch...finally结构围绕**整个**被代理的目标方法，包括上面四种通知对应的所有位置

> 各种通知的执行顺序：
>
> - Spring版本5.3.x以前：
>
>   - 前置通知
>
>   - 目标操作
>
>   - 后置通知
>   - 返回通知或异常通知
>
> - Spring版本5.3.x以后：
>   - 前置通知
>   - 目标操作
>   - 返回通知或异常通知
>
>   - 后置通知

#### 3.4.5 切入点表达式语法

##### ① 作用

![image-20230528225058721](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702844.png)

##### ② 语法细节

![image-20230528225128193](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702845.png)

![image-20230528225147209](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702846.png)

#### 3.4.5 重用切入点表达式

##### ① 声明

```java
@Pointcut("execution(* com.atguigu.aop.annotation.*.*(..))")
public void pointCut(){}
```

##### ② 在同一个切面中使用

```java
@Before("pointCut()")
public void beforeMethod(JoinPoint joinPoint){
    String methodName = joinPoint.getSignature().getName();
    String args = Arrays.toString(joinPoint.getArgs());
    System.out.println("Logger-->前置通知，方法名："+methodName+"，参数："+args);
}
```

##### ③ 在不同切面中使用

```java
@Before("com.atguigu.aop.CommonPointCut.pointCut()")
public void beforeMethod(JoinPoint joinPoint){
    String methodName = joinPoint.getSignature().getName();
    String args = Arrays.toString(joinPoint.getArgs());
    System.out.println("Logger-->前置通知，方法名："+methodName+"，参数："+args);
}
```

#### 3.4.7 获取通知的相关信息

##### ① 获取连接点信息

获取连接点信息可以在通知方法的参数位置设置JoinPoint类型的形参

```java
@Before("execution(public int com.atguigu.aop.annotation.CalculatorImpl.*(..))")
public void beforeMethod(JoinPoint joinPoint){
    //获取连接点的签名信息
    String methodName = joinPoint.getSignature().getName();
    //获取目标方法到的实参信息
    String args = Arrays.toString(joinPoint.getArgs());
    System.out.println("Logger-->前置通知，方法名："+methodName+"，参数："+args);
}
```

##### ② 获取目标方法的返回值

@AfterReturning中的属性returning，用来将通知方法的某个形参，接收目标方法的返回值

```java
@AfterReturning(value = "execution(* com.atguigu.aop.annotation.CalculatorImpl.*(..))", returning = "result")
    public void afterReturningMethod(JoinPoint joinPoint, Object result){
    String methodName = joinPoint.getSignature().getName();
    System.out.println("Logger-->返回通知，方法名："+methodName+"，结果："+result);
}
```

##### ③ 获取目标方法的异常

@AfterThrowing中的属性throwing，用来将通知方法的某个形参，接收目标方法的异常

```java
@AfterThrowing(value = "execution(* com.atguigu.aop.annotation.CalculatorImpl.*
(..))", throwing = "ex")
    public void afterThrowingMethod(JoinPoint joinPoint, Throwable ex){
    String methodName = joinPoint.getSignature().getName();
    System.out.println("Logger-->异常通知，方法名："+methodName+"，异常："+ex);
}
```

#### 3.4.8 环绕通知

```java
@Around("execution(* com.cjj.spring.aop.annotation.CalculatorImpl.*(..))")
public Object aroundMethod(ProceedingJoinPoint joinPoint) {
    String methodName = joinPoint.getSignature().getName();
    String args = Arrays.toString(joinPoint.getArgs());
    Object result = null;
    try {
        System.out.println("环绕通知-->目标对象方法执行之前");
        //目标方法的执行，目标方法的返回值一定要返回给外界调用者
        result = joinPoint.proceed();
        System.out.println("环绕通知-->目标对象方法返回值之后");
    } catch (Throwable throwable) {
        throwable.printStackTrace();
        System.out.println("环绕通知-->目标对象方法出现异常时");
    } finally {
        System.out.println("环绕通知-->目标对象方法执行完毕");
    }
    return result;
}
```

#### 3.4.9 切面的优先级

相同目标方法上同时存在多个切面时，切面的优先级控制切面的内外嵌套顺序。

- 优先级高的切面：外面
- 优先级低的切面：里面

使用@Order注解可以控制切面的优先级：

- @Order(较小的数)：优先级高
- @Order(较大的数)：优先级低  

![image-20230529234606925](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702847.png)

### 3.5 基于XML的AOP(了解)

## 4、声明式事务

> - AOP的重要应用

### 4.1 JdbcTemplate

> - spring-jdbc.jar包为我们提供的一个类，来执行CRUD的SQL语句

#### 4.1.1 简介

Spring 框架对 JDBC 进行封装，使用 JdbcTemplate 方便实现对数据库操作

#### 4.1.2 准备工作

##### ① 导入依赖

> - orm:object-relational mapping 对象关系映射
>
> - tx:操作事务相关jar包
> - spring-test
>   - spring整合junit，让我们的测试类在我们spring的测试环境来执行。这时我们不要每一回都手动获取IOC容器了，可以直接通过依赖注入的方式来获取IOC容器中的某个bean，直接使用即可。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cjj.spring</groupId>
    <artifactId>spring-transaction</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- Spring 持久化层支持jar包 -->
        <!-- Spring 在执行持久化层操作、与持久化层技术进行整合过程中，需要使用orm、jdbc、tx三个
        jar包 -->
        <!-- 导入 orm 包就可以通过 Maven 的依赖传递性把其他两个也导入 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- Spring 测试相关 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- MySQL驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
        <!-- 数据源 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.31</version>
        </dependency>
    </dependencies>
</project>
```

##### ② 创建jdbc.properties



##### ③ 配置spring的xml配置文件

> - `classpath:jdbc.properties `：在web工程里需要制定配置文件的类路径

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--引入properties文件-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

    <!-- 基于IOC管理数据源,连接数据库 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <bean class="org.springframework.jdbc.core.JdbcTemplate">
        <!-- 引入数据源，操作数据库 -->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

</beans>
```

#### 4.1.3 测试

##### ① 在测试类装配JdbcTemplate

```java
package com.cjj.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

// 指定当前测试类在Spring的测试环境中执行,通过依赖注入的方式直接获取IOC容器中的bean
@RunWith(SpringJUnit4ClassRunner.class)
// 设置Spring测试环境的配置文件
@ContextConfiguration("classpath:spring-jdbc.xml")
public class JdbcTemplateTest {

    @Autowired
    private JdbcTemplate jdbcTemplate;

}

```

##### ② 测试功能

```java
package com.cjj.test;

import com.cjj.spring.pojo.User;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.util.List;

// 指定当前测试类在Spring的测试环境中执行,通过依赖注入的方式直接获取IOC容器中的bean
@RunWith(SpringJUnit4ClassRunner.class)
// 设置Spring测试环境的配置文件
@ContextConfiguration("classpath:spring-jdbc.xml")
public class JdbcTemplateTest {

    @Autowired
    private JdbcTemplate jdbcTemplate;
	
    // 插入数据
    @Test
    public void testInsert(){
        String sql = "insert into t_user values(null,?,?,?,?,?)";
        jdbcTemplate.update(sql,"root","123",23,"女","1234@qq.com");
    }
	
    // 根据id查询
    @Test
    public void testGetById(){
        String sql = "select * from t_user where id = ?";
        User user = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(User.class), 1);
        System.out.println(user);
    }

    // 查询为一个List集合
    @Test
    public void testGetAllUser(){
        String sql = "select * from t_user";
        List<User> list = jdbcTemplate.query(sql, new BeanPropertyRowMapper<>(User.class));
        System.out.println(list);
    }

    // 查询单行单列
    @Test
    public void testGetCount(){
        String sql = "select count(*) from t_user";
        Integer count = jdbcTemplate.queryForObject(sql, Integer.class);
        System.out.println(count);
    }

}
```

### 4.2 声明式事务概念

#### 4.2.1 编程式事务

事务功能的相关操作全部通过自己编写代码来实现：

```java
Connection conn = ...;
try {
    
	// 开启事务：关闭事务的自动提交
    conn.setAutoCommit(false);
    
	// 核心操作
    
	// 提交事务
    conn.commit();
    
} catch (Exception e) {
    
	// 回滚事务
    conn.rollBack();
    
} finally { 
    
    // 释放数据库连接
	conn.close();
}
```

编程式的实现方式存在缺陷：

- 细节没有被屏蔽：具体操作过程中，所有细节都需要程序员自己来完成，比较繁琐。

- 代码复用性不高：如果没有有效抽取出来，每次实现功能都需要自己编写代码，代码就没有得到复用。

#### 4.2.2 声明式

> - 不需要程序员手写切面和通知，只需要把我们当前事务管理的切面中的通知直接定位到我们的连接点上。
> - 我们通过框架设置事务中的一些属性

既然事务控制的代码有规律可循，代码的结构基本是确定的，所以框架就可以将固定模式的代码抽取出来，进行相关的封装。

封装起来后，我们只需要在配置文件中进行简单的配置即可完成操作。

- 好处1：提高开发效率 
- 好处2：消除了冗余的代码
- 好处3：框架会综合考虑相关领域中在实际开发环境下有可能遇到的各种问题，进行了健壮性、性能等各个方面的优化

所以，我们可以总结下面两个概念：

- **编程式**：**自己写代码**实现功能

- **声明式**：通过**配置**让**框架**实现功能

### 4.3 基于注解的声明式事务

> - 在Mysql默认情况下，我们当前的一个sql语句独占一个事务，并且自动提交。
> - 所以我们需要事先关闭事务的自动提交，因为如果不关闭，每个sql就独占一个事务。

#### 4.3.1 准备工作

##### ① 加入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cjj.spring</groupId>
    <artifactId>spring-transaction</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- 基于Maven依赖传递性，导入spring-context依赖即可导入当前所需所有jar包 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- Spring 持久化层支持jar包 -->
        <!-- Spring 在执行持久化层操作、与持久化层技术进行整合过程中，需要使用orm、jdbc、tx三个
        jar包 -->
        <!-- 导入 orm 包就可以通过 Maven 的依赖传递性把其他两个也导入 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- Spring 测试相关 -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- junit测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!-- MySQL驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.16</version>
        </dependency>
        <!-- 数据源 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.31</version>
        </dependency>
    </dependencies>

</project>
```

##### ② 创建jdbc.properties

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:13306/ssm?serverTimezone=UTC
jdbc.username=root
jdbc.password=root
```

##### ③ 配置Spring配置文件

> - 声明式事务需要在jdbcTemplate执行sql语句的过程中来测试

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 引入外部文件 -->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>

    <!-- 引入数据源，连接数据库 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
    <!-- 创建JdbcTemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

</beans>
```

##### ④ 创建表

```sql
CREATE TABLE `t_book` (
`book_id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
`book_name` varchar(20) DEFAULT NULL COMMENT '图书名称',
`price` int(11) DEFAULT NULL COMMENT '价格',
`stock` int(10) unsigned DEFAULT NULL COMMENT '库存（无符号）',
PRIMARY KEY (`book_id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
insert into `t_book`(`book_id`,`book_name`,`price`,`stock`) values (1,'斗破苍
穹',80,100),(2,'斗罗大陆',50,100);
CREATE TABLE `t_user` (
`user_id` int(11) NOT NULL AUTO_INCREMENT COMMENT '主键',
`username` varchar(20) DEFAULT NULL COMMENT '用户名',
`balance` int(10) unsigned DEFAULT NULL COMMENT '余额（无符号）',
PRIMARY KEY (`user_id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;
insert into `t_user`(`user_id`,`username`,`balance`) values (1,'admin',50);
```

⑤ 创建组件

创建BookController：

```java
@Controller
public class BookController {

    @Autowired
    private BookService bookService;

    public void buyBook(Integer userId, Integer bookId){
        bookService.buyBook(userId,bookId);
    }

}
```

创建接口BookService：

```java
public interface BookService {
    void buyBook(Integer userId, Integer bookId);
}
```

创建实现类BookServiceImpl：

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;

    @Override
    public void buyBook(Integer userId, Integer bookId) {
        //查询图书的价格
        Integer price = bookDao.getPriceByBookId(bookId);
        //更新图书的库存
        bookDao.updateStock(bookId);
        //更新用户的余额
        bookDao.updateBalance(userId, price);
    }
}
```

创建接口BookDao：

```java
public interface BookDao {
    Integer getPriceByBookId(Integer bookId);

    void updateStock(Integer bookId);

    void updateBalance(Integer userId, Integer price);
}
```

创建实现类BookDaoImpl：

```java
@Repository
public class BookDaoImpl implements BookDao {
    @Autowired
    private JdbcTemplate jdbcTemplate;

    @Override
    public Integer getPriceByBookId(Integer bookId) {
        String sql = "select price from t_book where book_id = ?";
        return jdbcTemplate.queryForObject(sql, Integer.class, bookId);
    }
    @Override
    public void updateStock(Integer bookId) {
        String sql = "update t_book set stock = stock - 1 where book_id = ?";
        jdbcTemplate.update(sql, bookId);
    }
    @Override
    public void updateBalance(Integer userId, Integer price) {
        String sql = "update t_user set balance = balance - ? where user_id = ?";
        jdbcTemplate.update(sql, price, userId);
    }
}
```

#### 4.3.2 测试无事务情况

##### ① 创建测试类

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:tx-annotation.xml")
public class TxByAnnotationTest {

    @Autowired
    private BookController bookController;

    @Test
    public void testBuyBook(){
        bookController.buyBook(1,1);
    }

}
```

##### ② 模拟场景

用户购买图书，先查询图书的价格，再更新图书的库存和用户的余额

假设用户id为1的用户，购买id为1的图书

用户余额为50，而图书价格为80

购买图书之后，用户的余额为-30，数据库中余额字段设置了无符号，因此无法将-30插入到余额字段

此时执行sql语句会抛出SQLException

##### ③ 观察结果

因为没有添加事务，图书的库存更新了，但是用户的余额没有更新

显然这样的结果是错误的，购买图书是一个完整的功能，更新库存和更新余额要么都成功要么都失败

#### 4.3.3 加入事务

##### ① 添加事务配置

在Spring的配置文件中添加配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

    <!-- 配置事务管理器：spring帮我们管理的切面，环绕通知 -->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <!-- 所有事务相关的操作都是基于connection连接对象来的，所以要依赖于数据源对象 -->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!--
         开启事务的注解驱动,把切面里的通知作用到连接点上
         使用@Transactional注解所标识的方法或类中所有的方法使用事务进行管理
         transaction-manager属性的默认值是transactionManager
         如果事务管理器bean的id正好就是这个默认值，则可以省略这个属性
     -->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>

</beans>
```

注意：导入的名称空间需要 **tx** **结尾**的那个。

![image-20230531230705887](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702848.png)

##### ② 添加事务注解

因为service层表示业务逻辑层，一个方法表示一个完成的功能，因此处理事务一般在service层处理在BookServiceImpl的buybook()添加注解@Transactional

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;

    @Override
    @Transactional
    public void buyBook(Integer userId, Integer bookId) {
        //查询图书的价格
        Integer price = bookDao.getPriceByBookId(bookId);
        //更新图书的库存
        bookDao.updateStock(bookId);
        //更新用户的余额
        bookDao.updateBalance(userId, price);
    }
}
```

##### ③ 观察结果

由于使用了Spring的声明式事务，更新库存和更新余额都没有执行

#### 4.3.4 @Transactional注解标识的位置

`@Transactional`标识在方法上，则只会影响该方法

`@Transactional`标识的类上，则会影响类中所有的方法

#### 4.3.5 事务属性：只读

##### ① 介绍

> - 当事务只涉及查询操作时可以设置事务的只读，告诉数据库我们当前的事务当中不涉及任何写操作。从数据库层面可以优化操作，比如不加锁，保证数据的一致性
> - 如果一个事务有增删改操作，则不能设置为只读。如`buyBook()`方法，不光有查询还有修改操作。

对一个查询操作来说，如果我们把它设置成只读，就能够明确告诉数据库，这个操作不涉及写操作。这样数据库就能够针对查询操作来进行优化。

##### ② 使用方式

```java
@Transactional(readOnly = true)
public void buyBook(Integer bookId, Integer userId) {
    //查询图书的价格
    Integer price = bookDao.getPriceByBookId(bookId);
    //更新图书的库存
    bookDao.updateStock(bookId);
    //更新用户的余额
    bookDao.updateBalance(userId, price);
    //System.out.println(1/0);
}
```

##### ③ 注意

对增删改操作设置只读会抛出下面异常：

`Caused by: java.sql.SQLException: Connection is read-only. Queries leading to data modificationare not allowed`

#### 4.3.6 事务属性：超时

##### ① 介绍

事务在执行过程中，有可能因为遇到某些问题，导致程序卡住，从而长时间占用数据库资源。而长时间占用资源，大概率是因为程序运行出现了问题（可能是Java程序或MySQL数据库或网络连接等等）。

此时这个很可能出问题的程序应该被回滚，撤销它已做的操作，事务结束，把资源让出来，让其他正常程序可以执行。

概括来说就是一句话：超时回滚，释放资源。

##### ② 使用方式

> - @Transactional(timeout = -1)：默认值为-1，表示一直等，直到事务执行完或不报错位置。

```java
@Transactional(timeout = 3) // 超时3秒则当前事务强制回滚
public void buyBook(Integer bookId, Integer userId) {
    try {
        // 休眠5秒
    	TimeUnit.SECONDS.sleep(5);
    } catch (InterruptedException e) {
    	e.printStackTrace();
    }
    //查询图书的价格
    Integer price = bookDao.getPriceByBookId(bookId);
    //更新图书的库存
    bookDao.updateStock(bookId);
    //更新用户的余额
    bookDao.updateBalance(userId, price);
    //System.out.println(1/0);
}
```

##### ③ 观察结果

> - 抛出事务超时异常，并强制回滚

`org.springframework.transaction.**TransactionTimedOutException**: Transaction timed out:deadline was Fri Jun 04 16:25:39 CST 2022`

#### 4.3.7 事务属性：回滚策略

> - 默认的回滚策略，任何的运行时异常都会回滚

##### ① 介绍

声明式事务默认只针对运行时异常回滚，编译时异常不回滚。

可以通过@Transactional中相关属性设置回滚策略

- rollbackFor属性：需要设置一个Class类型的对象，因为什么而回滚
- rollbackForClassName属性：需要设置一个字符串类型的全类名
- noRollbackFor属性：需要设置一个Class类型的对象
- rollbackFor属性：需要设置一个字符串类型的全类名

##### ② 使用方式

```java
@Transactional(noRollbackFor = ArithmeticException.class)
    //@Transactional(noRollbackForClassName = "java.lang.ArithmeticException")
    public void buyBook(Integer bookId, Integer userId) {
    //查询图书的价格
    Integer price = bookDao.getPriceByBookId(bookId);
    //更新图书的库存
    bookDao.updateStock(bookId);
    //更新用户的余额
    bookDao.updateBalance(userId, price);
    System.out.println(1/0);
}
```

##### ③ 观察结果

虽然购买图书功能中出现了数学运算异常（ArithmeticException），但是我们设置的回滚策略是，当出现ArithmeticException不发生回滚，因此购买图书的操作正常执行

#### 4.3.8 事务属性：事务隔离级别

> - 事务的四大特性
>   - 原子性
>   - 一致性
>   - 隔离性
>   - 持久性
>     - 和隔离级别有关
> - Mysql数据库隔离级别默认为：可重复读

##### ① 介绍

数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题。一个事务与其他事务隔离的程度称为隔离级别。SQL标准中规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性就越好，但并发性越弱。

隔离级别一共有四种：

- 读未提交：READ UNCOMMITTED 

  允许Transaction01读取Transaction02未提交的修改。

  > 有两个并发的事务A和B，A的隔离级别设置为读未提交，B没有设置隔离级别即默认的可重复读。
  >
  > A独立提交的话，在A事务就可以读到B中没有提交的数据。
  >
  > 比如B中添加了一条id = 10的数据，A就可以读到B中添加的这条未被提交的数据。
  >
  > 如果B没有提交事务而回滚了事务，A之前读的B中的这条数据就没有意义了，也就是`脏读`

- 读已提交：READ COMMITTED、

  要求Transaction01只能读取Transaction02已提交的修改。

  >有两个并发的事务A和B，A的隔离级别设置为读已提交，B没有设置隔离级别即默认的可重复读。A只能读B中已提交的数据。
  >
  >假如B事务添加一条用户信息，当前的A只能读取B提交之后的数据，提交事务前是读不到的信息的。
  >
  >此时出现的问题就是，这一次读取的和上一次读取的数据不一样。也就是B没有提交事务读取出来的，和B提交事务后读取出来的数据不一致，也就是`不可重复读`。
  >
  >

- 可重复读：REPEATABLE READ

  确保Transaction01可以多次从一个字段中读取到相同的值，即Transaction01执行期间禁止其它

  > 有两个并发的事务A和B，A和B的隔离级别设置为可重复读。
  >
  > 此时A事务需要读取id = 1的这条数据，那么就给给这条数据加锁，B事务无法操作这条数据。

- 事务对这个字段进行更新。

  串行化：SERIALIZABLE

  确保Transaction01可以多次从一个表中读取到相同的行，在Transaction01执行期间，禁止其它事务对这个表进行添加、更新、删除操作。可以避免任何并发问题，但性能十分低下。

各个隔离级别解决并发问题的能力见下表：

![image-20230601124704005](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702849.png)

##### ② 使用方式

```java
@Transactional(isolation = Isolation.DEFAULT)//使用数据库默认的隔离级别
@Transactional(isolation = Isolation.READ_UNCOMMITTED)//读未提交
@Transactional(isolation = Isolation.READ_COMMITTED)//读已提交
@Transactional(isolation = Isolation.REPEATABLE_READ)//可重复读
@Transactional(isolation = Isolation.SERIALIZABLE)//串行化
```

#### 4.3.9 事务属性：事务传播行为

> 

##### ① 介绍

当事务方法被另一个事务方法调用时，必须指定事务应该如何传播。例如：方法可能继续在现有事务中运行，也可能开启一个新事务，并在自己的事务中运行。

##### ② 测试

创建接口CheckoutService：

```java
public interface CheckoutService {
	void checkout(Integer[] bookIds, Integer userId);
}
```

创建实现类CheckoutServiceImpl：

```java
@Service
public class CheckoutServiceImpl implements CheckoutService {
    @Autowired
    private BookService bookService;
    @Override
    @Transactional
    //一次购买多本图书
    public void checkout(Integer[] bookIds, Integer userId) {
        for (Integer bookId : bookIds) {
            bookService.buyBook(bookId, userId);
    	}
	}
}
```

在BookController中添加方法：

```java
@Autowired
private CheckoutService checkoutService;
public void checkout(Integer[] bookIds, Integer userId){
    checkoutService.checkout(bookIds, userId);
}
```

在数据库中将用户的余额修改为100元

##### ③ 观察结果

可以通过@Transactional中的propagation属性设置事务传播行为

修改BookServiceImpl中buyBook()上，注解@Transactional的propagation属性

@Transactional(propagation = Propagation.REQUIRED)，默认情况，表示如果当前线程上有已经开启的事务可用，那么就在这个事务中运行。经过观察，购买图书的方法buyBook()在checkout()中被调用，checkout()上有事务注解，因此在此事务中执行。所购买的两本图书的价格为80和50，而用户的余额为100，因此在购买第二本图书时余额不足失败，导致整个checkout()回滚，即只要有一本书买不了，就都买不了

@Transactional(propagation = Propagation.REQUIRES_NEW)，表示不管当前线程上是否有已经开启的事务，都要开启新事务。同样的场景，每次购买图书都是在buyBook()的事务中执行，因此第一本图书购买成功，事务结束，第二本图书购买失败，只在第二次的buyBook()中回滚，购买第一本图书不受影响，即能买几本就买几本

### 4.4 基于XML的声明式事务

# 三、SpringMVC

## 1、SpringMVC简介

### 1.1 什么是MCV

MVC是一种软件架构的思想，将软件按照模型、视图、控制器来划分

M：Model，模型层，指工程中的JavaBean，作用是处理数据

JavaBean分为两类：

- 一类称为实体类Bean：专门存储业务数据的，如 Student、User 等

- 一类称为业务处理 Bean：指 Service 或 Dao 对象，专门用于处理业务逻辑和数据访问。

V：View，视图层，指工程中的html或jsp等页面，作用是与用户进行交互，展示数据

C：Controller，控制层，指工程中的servlet，作用是接收请求和响应浏览器

MVC的工作流程： 用户通过视图层发送请求到服务器，在服务器中请求被Controller接收，Controller调用相应的Model层处理请求，处理完毕将结果返回到Controller，Controller再根据请求处理的结果找到相应的View视图，渲染数据后最终响应给浏览器

### 1.2 什么是SpringMVC

> - 三层架构
>   - 表述层
>     - 前台页面
>     - 后台servlet
>   - 业务层
>   - 持久层
> - 封装了Servlet

SpringMVC是Spring的一个后续产品，是Spring的一个子项目

SpringMVC 是 Spring 为表述层开发提供的一整套完备的解决方案。在表述层框架历经 Strust、WebWork、Strust2 等诸多产品的历代更迭之后，目前业界普遍选择了 SpringMVC 作为 Java EE 项目表述层开发的**首选方案**。

> 注：三层架构分为表述层（或表示层）、业务逻辑层、数据访问层，表述层表示前台页面和后台servlet

### 1.3 SpringMVC的特点

- **Spring** **家族原生产品**，与 IOC 容器等基础设施无缝对接

- **基于原生的**Servlet**，通过了功能强大的**前端控制器**DispatcherServlet**，对请求和响应进行统一

  处理

- 表述层各细分领域需要解决的问题**全方位覆盖**，提供**全面解决方案**

- **代码清新简洁**，大幅度提升开发效率
- 内部组件化程度高，可插拔式组件**即插即用**，想要什么功能配置相应组件即可

- **性能卓著**，尤其适合现代大型、超大型互联网项目要求

## 2、入门案例

### 2.1 开发环境

IDE：idea 

构建工具：maven3.6.2

服务器：tomcat8.5

Spring版本：5.3.1

### 2.2 创建maven工程

#### ① 添加web模块

> - 在pom.xml文件中设置打包方式为：war
>
> - 在项目结构中添加web.xml配置文件
>
>   ![image-20230601131355461](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702850.png)
>
> - 设置路径：`G:\代码\IDEA\workspace\SSM\spring_mvc_helloworld\src\main\webapp\WEB-INF\web.xml`
>
>   ![image-20230601131443627](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702851.png)

#### ② 打包方式：war

#### ③ 引入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.cjj</groupId>
    <artifactId>spring_mvc_helloworld</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <properties>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- SpringMVC -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>5.3.1</version>
        </dependency>
        <!-- 日志 -->
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>
        <!-- ServletAPI -->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
            <scope>provided</scope>
        </dependency>
        <!-- Spring5和Thymeleaf整合包 -->
        <dependency>
            <groupId>org.thymeleaf</groupId>
            <artifactId>thymeleaf-spring5</artifactId>
            <version>3.0.12.RELEASE</version>
        </dependency>
    </dependencies>

</project>
```

注：由于 Maven 的传递性，我们不必将所有需要的包全部配置依赖，而是配置最顶端的依赖，其他靠传递性导入。

![image-20230601131937316](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702852.png)

### 2.3 配置web.xml

> - 需要复习javaweb2022版 servlet

注册SpringMVC的前端控制器DispatcherServlet

#### ① 默认配置方式

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--配置SpringMVC的前端控制器DispatcherServlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--设置springmvc配置文件的位置和名称-->
        <init-param>
            <!--指定springmvc配置文件路径-->
            <param-name>contextConfigLocation</param-name>
            <!--类路径下的-->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!--将DispatcherServlet初始化提前到服务器启动时,不然第一次访问页面需要转圈圈-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <!--default:处理静态资源-->
        <servlet-name>SpringMVC</servlet-name>
        <!--前端控制器请求的路径模型-->
        <!--当浏览器发送的请求符合ur-pattern时,那么当前请求就会被前端控制器所处理-->
        <url-pattern>/</url-pattern> <!--jsp在tomcat的xml文件中，有专门的处理-->
    </servlet-mapping>
    
</web-app>
```

#### ② 扩展配置方式

可通过init-param标签设置SpringMVC配置文件的位置和名称，通过load-on-startup标签设置SpringMVC前端控制器DispatcherServlet的初始化时间

```xml
<!-- 配置SpringMVC的前端控制器，对浏览器发送的请求统一进行处理 -->
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servletclass>
    <!-- 通过初始化参数指定SpringMVC配置文件的位置和名称 -->
        <init-param>
        <!-- contextConfigLocation为固定值 -->
        <param-name>contextConfigLocation</param-name>
        <!-- 使用classpath:表示从类路径查找配置文件，例如maven工程中的
        src/main/resources -->
        <param-value>classpath:springMVC.xml</param-value>
    </init-param>
    <!--
    作为框架的核心组件，在启动过程中有大量的初始化操作要做
    而这些操作放在第一次请求时才执行会严重影响访问速度
    因此需要通过此标签将启动控制DispatcherServlet的初始化时间提前到服务器启动时
    -->
    <load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--
    设置springMVC的核心控制器所能处理的请求的请求路径
    /所匹配的请求可以是/login或.html或.js或.css方式的请求路径
    但是/不能匹配.jsp请求路径的请求
    -->
    <url-pattern>/</url-pattern>
</servlet-mapping>
```

> <url-pattern>标签中使用/和/*的区别：
>
> /所匹配的请求可以是/login或.html或.js或.css方式的请求路径，但是/不能匹配.jsp请求路径的请求因此就可以避免在访问jsp页面时，该请求被DispatcherServlet处理，从而找不到相应的页面
>
> /*则能够匹配所有请求，例如在使用过滤器时，若需要对所有请求进行过滤，就需要使用/*的写法
>

### 2.4 创建请求控制器

> - 我们SpringMVC封装的是Servlet，我们不需要手动创建servlet，当前所有的请求经过web.xml的配置都被我们的DispatcherServlet来处理了。它处理的是共性的问题：获取请求参数，往域对象中共享数据，页面跳转，转发和重定向。当前请求具体如何处理则需要写一个方法。

由于`前端控制器`对浏览器发送的请求进行了统一的处理，但是具体的请求有不同的处理过程，因此需要创建处理具体请求的类，即`请求控制器`

请求控制器中每一个处理请求的方法成为控制器方法

因为SpringMVC的控制器由一个POJO（普通的Java类）担任，因此需要通过@Controller注解将其标识为一个控制层组件，交给Spring的IoC容器管理，此时SpringMVC才能够识别控制器的存在

```java
// 当前控制层中的方法，可以通过SpringMVC为我们提供的方式将它设置为处理请求的方法
@Controller
public class HelloController {
    
}
```

### 2.5 创建SpringMVC的配置文件

> - 不用手动加载配置文件，在DispatcherServlet初始化的时候就会自动加载SpringMVC的配置文件
> - 所以SpringMCV配置文件有一个固定的名字和一个固定的位置
>   - 默认位置：WEB-INF包下
>   - 默认名称：`<servlet-name>`的值加上`-servlet.xml`，也就是`SpringMVC-servlet.xml`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--扫描组件-->
    <context:component-scan base-package="com.cjj.controller"></context:component-scan>

    <!-- 配置Thymeleaf视图解析器 -->
    <!--可以使用SpringMVC为我们提供的方式，来进行视图渲染，并实现页面跳转-->
    <!--通过源码：由SpringMVC中DispatcherServlet内部帮我们加载的-->
    <bean id="viewResolver"
          class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <!--优先级-->
        <property name="order" value="1"/>
        <!--编码-->
        <property name="characterEncoding" value="UTF-8"/>
        <!--模板引擎-->
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <!--模板解析器-->
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>
                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
</beans>
```

### 2.6 测试Helloworld

配置tomcat

![image-20230601213825939](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702853.png)

![image-20230601213923335](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702854.png)

> 将工程部署到tomcat上

![image-20230601214055664](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702855.png)

![image-20230601214119648](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702856.png)

> 通过应用的上下文路径访问web服务器上的不同工程

![image-20230601214233208](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702857.png)

> 重新部署：将tomcat服务器上的工程删除再重新添加上
>
> 更新类和资源：鼠标焦点移出idea时

![image-20230601214712816](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702858.png)

![image-20230601214438067](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702859.png)

#### ① 实现对首页的访问

> - 浏览器无法直接访问WEB-INF包下的资源，出于安全考虑，只能服务器/转发来进行访问

在请求控制器中创建处理请求的方法

```java
@Controller
public class HelloController {

    /*
        将浏览器发送的请求映射到当前方法的执行
        属性值value：`/`一般表示绝对路径，`/`被服务器解析为:localhost:8080/上下文路径
        请求路径和@RequestMapping的value属性值解析的路径一样，则该方法是处理请求的方法
     */

    @RequestMapping("/")
    public String protal(){
        // 将逻辑视图返回：要跳转页面的物理视图去掉前缀和后缀，也就是index。
        // 返回的逻辑视图会被配置文件中的视图解析器来解析，把返回的逻辑视图拼接好前缀和后缀，就能匹配到完整的物理视图(路径)
        // 最后通过Thymeleaf视图解析器的渲染来跳转页面
        return "index";
    }

}
```

#### ② 通过超链接跳转到指定页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http//www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>首页</title>
</head>
<body>
    <h1>index.html</h1>
    <a th:href="@{/hello}">测试SpringMVC</a>
    <a href="/hello">测试绝对路径</a>

</body>
</html>
```

在请求控制器中创建处理请求的方法

```java
@RequestMapping("/hello")
public String hello(){
    return "success";
}
```

### 2.7 总结

浏览器发送请求，若请求地址符合前端控制器的url-pattern，该请求就会被前端控制器DispatcherServlet处理。前端控制器会读取SpringMVC的核心配置文件，通过扫描组件找到控制器，将请求地址和控制器中@RequestMapping注解的value属性值进行匹配，若匹配成功，该注解所标识的控制器方法就是处理请求的方法。处理请求的方法需要返回一个字符串类型的视图名称，该视图名称会被视图解析器解析，加上前缀和后缀组成视图的路径，通过Thymeleaf对视图进行渲染，最终转发到视图所对应页面。

## 3、@RequestMapping注解

#### 3.1 @RequestMapping注解的功能

从注解名称上我们可以看到，@RequestMapping注解的作用就是将请求和处理请求的控制器方法关联起来，建立映射关系。

SpringMVC 接收到指定的请求，就会来找到在映射关系中对应的控制器方法来处理这个请求。

#### 3.2 @RequestMapping注解的位置

> - 请求的路径要与类和方法上路径拼接之后的路径匹配才行。

@RequestMapping标识一个类：设置映射请求的请求路径的初始信息

@RequestMapping标识一个方法：设置映射请求请求路径的具体信息

```java
@Controller
@RequestMapping("/cjj")
public class TestRequsetMappingController {

    // 此时控制器方法所匹配请求的请求路径为/test/hello
    @RequestMapping("/test")
    public String test(){
        return "success";
    }

}
```

#### 3.3 @RequestMapping注解的value属性

@RequestMapping注解的value属性通过请求的请求地址匹配请求映射

@RequestMapping注解的value属性是一个`字符串类型的数组`，表示该请求映射能够匹配多个请求地址所对应的请求

@RequestMapping注解的value属性必须设置，至少通过请求地址匹配请求映射

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http//www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <h1>index.html</h1>
  <a th:href="@{/test}">测试</a>
  <a th:href="@{/test1}">测试@RequestMapping的value</a>
</body>
</html>
```

```java
@Controller
public class TestRequsetMappingController {

    @RequestMapping({"/test","/test1"})
    public String test(){
        return "success";
    }

}
```

#### 3.4 @RequestMapping注解的method属性

> - 也就是说，请求控制器的方法的执行必须要同时满足value和method的属性的条件。
> - `地址栏直接粘贴地址访问是GET请求`

@RequestMapping注解的method属性通过请求的请求方式（get或post）匹配请求映射

@RequestMapping注解的method属性是一个RequestMethod类型的`数组`，表示该请求映射能够匹配多种请求方式的请求

若当前请求的请求地址满足请求映射的value属性，但是请求方式不满足method属性，则浏览器报错

405：Request method 'POST' not supported

```html
<a th:href="@{/test}">测试@RequestMapping的value属性-->/test</a><br>
<form th:action="@{/test}" method="post">
	<input type="submit">
</form>
```

```java
@Controller
public class TestRequsetMappingController {

    @RequestMapping(value = {"/test","/test1"},method = RequestMethod.GET)
    public String test(){
        return "success";
    }

}
```

> 注：
>
> - 1、对于处理指定请求方式的控制器方法，SpringMVC中提供了@RequestMapping的派生注解
>
>   处理get请求的映射-->@GetMapping
>
>   处理post请求的映射-->@PostMapping
>
>   处理put请求的映射-->@PutMapping
>
>   处理delete请求的映射-->@DeleteMapping
>
> - 2、常用的请求方式有get，post，put，delete
>
>   但是目前浏览器只支持get和post，若在form表单提交时，为method设置了其他请求方式的字符串（put或delete），则按照默认的请求方式get处理若要发送put和delete请求，则需要通过spring提供的过滤器HiddenHttpMethodFilter，在RESTful部分会讲到

#### 3.5 @RequestMapping注解的params属性（了解）

> - Get请求
>   - 在提交表单时，会把请求参数拼接在请求地址后

@RequestMapping注解的params属性通过请求的请求参数匹配请求映射

@RequestMapping注解的params属性是一个字符串类型的数组，可以通过四种表达式设置请求参数和请求映射的匹配关系

"param"：要求请求映射所匹配的请求必须携带param请求参数

"!param"：要求请求映射所匹配的请求必须不能携带param请求参数

"param=value"：要求请求映射所匹配的请求必须携带param请求参数且param=value

"param!=value"：要求请求映射所匹配的请求必须携带param请求参数但是param!=value

```html
<a th:href="@{/test(username='admin',password=123456)">测试@RequestMapping的
params属性-->/test</a><br>
```

```java
@RequestMapping(
        value = {"/testRequestMapping", "/test"}
        ,method = {RequestMethod.GET, RequestMethod.POST}
        ,params = {"username","password!=123456"}
)
public String testRequestMapping(){
    return "success";
}
```



#### 3.6 @RequestMapping注解的headers属性（了解）

@RequestMapping注解的headers属性通过请求的请求头信息匹配请求映射

@RequestMapping注解的headers属性是一个字符串类型的数组，可以通过四种表达式设置请求头信息和请求映射的匹配关系

"header"：要求请求映射所匹配的请求必须携带header请求头信息

"!header"：要求请求映射所匹配的请求必须不能携带header请求头信息

"header=value"：要求请求映射所匹配的请求必须携带header请求头信息且header=value

"header!=value"：要求请求映射所匹配的请求必须携带header请求头信息且header!=value

若当前请求满足@RequestMapping注解的value和method属性，但是不满足headers属性，此时页面显示404错误，即资源未找到

#### 3.7 SpringMVC支持ant风格的路径

`？`：表示任意的单个字符

`*`：表示任意的0个或多个字符

`**`：表示任意层数的任意目录

注意：在使用`**`时，只能使用`/**/xxx`的方式

#### 3.8 SpringMVC支持路径中的占位符（重点）

原始方式：/deleteUser?id=1

rest方式：/user/delete/1

SpringMVC路径中的占位符常用于RESTful风格中，当请求路径中将某些数据通过路径的方式传输到服务器中，就可以在相应的@RequestMapping注解的value属性中通过占位符{xxx}表示传输的数据，在通过@PathVariable注解，将占位符所表示的数据赋值给控制器方法的形参

```html
<a th:href="@{/testRest/1/admin}">测试路径中的占位符-->/testRest</a><br>
```

```java
@RequestMapping("/testRest/{id}/{username}")
public String testRest(@PathVariable("id") String id, @PathVariable("username")
String username){
    System.out.println("id:"+id+",username:"+username);
    return "success";
}
//最终输出的内容为-->id:1,username:admin
```

## 4、SpringMVC获取请求参数

### 4.1 通过ServletAPI获取

> - 当浏览器发送请求被`DispatcherServlet`控制器处理之后，它会拿着当前的请求信息跟控制层`@RequestMapping`中的`value`值进行匹配，匹配成功即帮我们调用当前的方法。调用方法时，也会判断当前方法参数的类型。如果参数类型为`HttpServletRequest`，则自动帮我们赋值，有关请求体的信息。

将HttpServletRequest作为控制器方法的形参，此时HttpServletRequest类型的参数表示封装了当前请求的请求报文的对象

```java
@Controller
public class TestParamController {

    @RequestMapping("/param/servletAPI")
    public String getParamByServletAPI(HttpServletRequest request){
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        System.out.println("username:" + username + ",password:" + password);
        return "success";
    }

}
```

### 4.2 通过控制器方法的形参获取请求参数

在控制器方法的形参位置，设置和请求参数同名的形参，当浏览器发送请求，匹配到请求映射时，在DispatcherServlet中就会将请求参数赋值给相应的形参

```html
<a th:href="@{/param(username='admin',password=123456)}">测试获取请求参数</a><br>

<form th:action="@{/param}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit" value="登录">
</form>
```

> 注：
>
> 若请求所传输的请求参数中有多个同名的请求参数，此时可以在控制器方法的形参中设置字符串
>
> 数组或者字符串类型的形参接收此请求参数
>
> 若使用字符串数组类型的形参，此参数的数组中包含了每一个数据
>
> 若使用字符串类型的形参，此参数的值为每个数据中间使用逗号拼接的结果

### 4.3 @RequestParam

`@RequestParam`是将请求参数和控制器方法的形参创建映射关系

```java
@RequestMapping("/param")
public String getParam(@RequestParam("userName") String username, String password){
    System.out.println("username:" + username + ",password:" + password);
    return "success";
}
```

`@RequestParam`注解一共有三个属性：

`value：`指定一个请求参数中的名字与当前形参进行绑定

`required`：设置是否必须传输此请求参数，默认值为true

若设置为true时，则当前请求必须传输value所指定的请求参数，若没有传输该请求参数，且没有设置defaultValue属性，则页面报错400：Required String parameter 'xxx' is not present；若设置为false，则当前请求不是必须传输value所指定的请求参数，若没有传输，则注解所标识的形参的值为null

`defaultValue`：不管required属性值为true或false，当value所指定的请求参数没有传输或传输的值为""时，则使用默认值为形参赋值

### 4.4 @RequestHeader

> - cookie里面保存了浏览器的一些信息，这些信息在服务器需要用到的时候会发送给服务器[比如sessionId]

@RequestHeader是将请求头信息和控制器方法的形参创建映射关系

@RequestHeader注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

### 4.5 @CookieValue

@CookieValue是将cookie数据和控制器方法的形参创建映射关系

@CookieValue注解一共有三个属性：value、required、defaultValue，用法同@RequestParam

```java
@Controller
public class TestParamController {

    @RequestMapping("/param/servletAPI")
    public String getParamByServletAPI(HttpServletRequest request){
        // 获取浏览器cookie信息，存在服务器上
        HttpSession session = request.getSession();
        String userName = request.getParameter("useName");
        String password = request.getParameter("password");
        System.out.println("username:" + userName + ",password:" + password);
        return "success";
    }

    @RequestMapping("/param")
    public String getParam(
            @RequestParam("userName") String username,
            String password,
            @RequestHeader("referer") String referer,
            @CookieValue("JSESSIONID") String jsessionId
    )
    {
        System.out.println("username:" + username + ",password:" + password);
        System.out.println("referer："+referer);
        System.out.println("jsessionId："+jsessionId);
        return "success";
    }
}
```

### 4.6 通过POJO获取请求参数

可以在控制器方法的形参位置设置一个实体类类型的形参，此时若浏览器传输的请求参数的参数名和实体类中的属性名一致，那么请求参数就会为此属性赋值

```html
<form th:action="@{/param/pojo}" method="post">
    用户名：<input type="text" name="username"><br>
    密码：<input type="password" name="password"><br>
    <input type="submit" value="登录">
</form>
```

```java
@RequestMapping("/param/pojo")
public String getParamByPojo(User user){
    System.out.println(user);
    return "success";
}
// User{id=null, username='18175013340', password='admin'}
```

### 4.7 解决获取请求参数的乱码问题

> - 设置编码之前，不能获取任何的请求参数，不然就无效了
> - Tomcat7版本：默认POST和GET请求都会中文乱码。
>   - GET乱码解决方式：找到tomcat文件夹中server.xml文件，在port="8080"那一行添加`URIEncoding="UTF-8"`
>   - POST乱码解决方式：手动设置编码
> - Tomcat8版本：GET没乱码，只有POST有乱码
> - 注：`SpringMVC中处理编码的过滤器一定要配置到其他过滤器之前，否则无效`

解决获取请求参数的乱码问题，可以使用SpringMVC提供的编码过滤器CharacterEncodingFilter，但是必须在web.xml中进行注册

```xml
<!--配置Spring的编码过滤器-->
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilte
    <!--框架有默认的编码方式，我们需要手动设置编码-->
    <!--默认只设置请求的编码，也就是request.characterEncoding("UTF-8")-->
    <init-param>
        <param-name>encoding</param-name>
        <param-value>UTF-8</param-value>
    </init-param>
    <!--同时设置请求和响应的编码-->
    <init-param>
        <param-name>forceEncoding</param-name>
        <param-value>true</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

## 5、域对象共享数据

> - 处理请求的过程
>   - 获取请求参数
>   - 用Service处理业务逻辑
>   - 往域对象共享数据
>   - 实现页面渲染和跳转

### 5.1 使用ServletAPI向request域对象共享数据

```java
@RequestMapping("/testServletAPI")
public String testServletAPI(HttpServletRequest request){
    request.setAttribute("testScope", "hello,servletAPI");
    return "success";
}
```

### 5.2 使用ModelAndView向request域对象共享数据

> - ModelAndView对象：底层最终都会把我们往域中共享的数据和视图封装为一个这个对象。
>   - 后面可以DEBUG源码查看与验证，P139
> - Model：模型
>   - 往域对象存储数据
> - View：设置视图

```java
@RequestMapping("/test/mav")
public ModelAndView testMAV(){
    /**
     * Model:向请求域中共享数据
     * View:设置逻辑视图实现页面跳转
     */
    ModelAndView mav = new ModelAndView();
    // 向请求域中共享数据
    mav.addObject("testRequestScope","hello,ModeAndView");
    // 设置逻辑视图
    mav.setViewName("success");
    return mav;
}
```

```html
<p th:text="${testRequestScope}"></p>
```

### 5.3 使用Model向request域对象共享数据

```java
@RequestMapping("/test/model")
public String testModel(Model model){
    model.addAttribute("testRequestScope","hello,Model");
    return "success";
}
```

### 5.4 使用map向request域对象共享数据

```java
@RequestMapping("/test/modelMap")
public String testModelMap(ModelMap modelMap){
    modelMap.addAttribute("testRequestScope","hello,modelMap");
    return "success";
}
```

### 5.5 使用ModelMap向request域对象共享数据

```java
@RequestMapping("/test/map")
public String testMap(Map<String,Object> map){
    map.put("testRequstScope","hello,map");
    return "success";
}
```

### 5.6、Model、ModelMap、Map的关系

Model、ModelMap、Map类型的参数其实本质上都是 BindingAwareModelMap 类型的

```java
public interface Model{}
public class ModelMap extends LinkedHashMap<String, Object> {}
public class ExtendedModelMap extends ModelMap implements Model {}
public class BindingAwareModelMap extends ExtendedModelMap {}
```

### 5.7 向session域共享数据

> - 一次会话：浏览器开启到关闭的过程
> - 应用域：服务器运行的整个过程中

```html
<p th:text="${session.testSessionScope}"></p>
<p th:text="${application.testApplicationScope}"></p>
body>
```

```java
@RequestMapping("/test/session")
public String testSession(HttpSession session){
    session.setAttribute("testSessionScope","hello,session");
    return "success";
}6、SpringMVC的视图
```

### 5.8 向application域共享数据

```java
@RequestMapping("/test/application")
public String testApplication(HttpSession session){
    ServletContext servletContext = session.getServletContext();
    servletContext.setAttribute("testApplicationScope","hello,application");
    return "success";
}
```

## 6、SpringMVC的视图

SpringMVC中的视图是View接口，视图的作用渲染数据，将模型Model中的数据展示给用户SpringMVC视图的种类很多，默认有转发视图和重定向视图

当工程引入jstl的依赖，转发视图会自动转换为JstlView

若使用的视图技术为Thymeleaf，在SpringMVC的配置文件中配置了Thymeleaf的视图解析器，由此视图解析器解析之后所得到的是ThymeleafView

### 6.1 ThymeleafView

当控制器方法中所设置的视图名称没有任何前缀时，此时的视图名称会被SpringMVC配置文件中所配置的视图解析器解析，视图名称拼接视图前缀和视图后缀所得到的最终路径，会通过转发的方式实现跳转

```java
@RequestMapping("/testHello")
public String testHello(){
	return "hello";
}
```

![image-20230606104053291](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702860.png)

### 6.2 转发视图

SpringMVC中默认的转发视图是InternalResourceView

SpringMVC中创建转发视图的情况：

当控制器方法中所设置的视图名称以"forward:"为前缀时，创建InternalResourceView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"forward:"去掉，剩余部分作为最终路径通过转发的方式实现跳转

例如"forward:/"，"forward:/employee"

```java
@RequestMapping("/testForward")
public String testForward(){
	return "forward:/testHello";
}
```

![image-20230606104140569](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702861.png)

### 6.3 重定向视图

SpringMVC中默认的重定向视图是RedirectView

当控制器方法中所设置的视图名称以"redirect:"为前缀时，创建RedirectView视图，此时的视图名称不会被SpringMVC配置文件中所配置的视图解析器解析，而是会将前缀"redirect:"去掉，剩余部分作为最终路径通过重定向的方式实现跳转

例如"redirect:/"，"redirect:/employee"

```java
@RequestMapping("/testRedirect")
public String testRedirect(){
	return "redirect:/testHello";
}
```

![image-20230606104234189](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702862.png)

> 注：
>
> 重定向视图在解析时，会先将redirect:前缀去掉，然后会判断剩余部分是否以/开头，若是则会自
>
> 动拼接上下文路径

### 6.4 视图控制器view-controller

当控制器方法中，不做处理仅仅用来实现页面跳转，即只需要设置视图名称时，可以将处理器方法使用view-controller标签进行表示

```xml
<!-- 开启 注解驱动-->
<mvc:annotation-driven></mvc:annotation-driven>
<!--
path：设置处理的请求地址
view-name：设置请求地址所对应的视图名称
-->
<mvc:view-controller path="/testView" view-name="success"></mvc:view-controller>
```

> 注：
>
> 当SpringMVC中设置任何一个view-controller时，其他控制器中的请求映射将全部失效，此时需要在SpringMVC的核心配置文件中设置开启mvc注解驱动的标签：`<mvc:annotation-driven />`

## 7、RESTful

### 7.1 RESTful简介

> - 一种风格：把服务器上所有东西都看做一种资源

REST：**Re**presentational **S**tate **T**ransfer，表现层资源状态转移。

#### ① 资源

资源是一种看待服务器的方式，即，将服务器看作是由很多离散的资源组成。每个资源是服务器上一个可命名的抽象概念。因为资源是一个抽象的概念，所以它不仅仅能代表服务器文件系统中的一个文件、数据库中的一张表等等具体的东西，可以将资源设计的要多抽象有多抽象，只要想象力允许而且客户端应用开发者能够理解。与面向对象设计类似，资源是以名词为核心来组织的，首先关注的是名词。一个资源可以由一个或多个URI来标识。URI既是资源的名称，也是资源在Web上的地址。对某个资源感兴趣的客户端应用，可以通过资源的URI与其进行交互。

#### ② 资源的表述

资源的表述是一段对于资源在某个特定时刻的状态的描述。可以在客户端-服务器端之间转移（交换）。资源的表述可以有多种格式，例如HTML/XML/JSON/纯文本/图片/视频/音频等等。资源的表述格式可以通过协商机制来确定。请求-响应方向的表述通常使用不同的格式。

#### **③ 状态转移**

状态转移说的是：在客户端和服务器端之间转移（transfer）代表资源状态的表述。通过转移和操作资源的表述，来间接实现操作资源的目的

### 7.2 RESTful的实现

> - GET：查询、获取资源
> - POST：新建资源
> - PUT：修改资源
> - DELETE：删除资源

具体说，就是 HTTP 协议里面，四个表示操作方式的动词：GET、POST、PUT、DELETE。

它们分别对应四种基本操作：GET 用来获取资源，POST 用来新建资源，PUT 用来更新资源，DELETE用来删除资源。

REST 风格提倡 URL 地址使用统一的风格设计，从前到后各个单词使用斜杠分开，不使用问号键值对方式携带请求参数，而是将要发送给服务器的数据作为 URL 地址的一部分，以保证整体风格的一致性。

![image-20230608152912507](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702863.png)

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http//www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
  <h1>index.html</h1>
    
    <a th:href="@{/user}">查询所有用户信息</a>
  <a th:href="@{/user/1}">查询id=1用户信息</a>
    
<form th:action="@{/user}" method="post">
    <input type="submit" value="点击添加用户信息">
</form>
    
  <form th:action="@{/user}" method="post">
      <input type="hidden" name="_method" value="put">
      <input type="submit" value="点击修改用户信息">
  </form>

  <form th:action="@{/user/1}" method="post">
      <input type="hidden" name="_method" value="delete">
      <input type="submit" value="点击删除用户信息">
  </form>
</body>
</html>
```

```java
package com.cjj.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;

@Controller
public class TestRestController {

    @RequestMapping(value = "/user",method = RequestMethod.GET)
    public String getAllUser(){
        System.out.println("查询所有用户信息：/user-->get");
        return "success";
    }

    @RequestMapping(value = "/user/{id}",method = RequestMethod.GET)
    public String getUserById(@PathVariable Integer id){
        System.out.println("根据id查询用户信息：/user/"+ id +"-->get");
        return "success";
    }

    @RequestMapping(value = "/user",method = RequestMethod.POST)
    public String saveUser(){
        System.out.println("保存用户信息：/user-->post");
        return "success";
    }

    @RequestMapping(value = "/user",method = RequestMethod.PUT)
    public String updateUser(){
        System.out.println("修改用户信息：/user-->put");
        return "success";
    }

    @RequestMapping(value = "/user/{id}",method = RequestMethod.DELETE)
    public String deleteUser(@PathVariable Integer id){
        System.out.println("删除用户信息：/user/"+id+"-->delete");
        return "success";
    }
}
```

### 7.3 HiddenHttpMethodFilter

> - 在web.xml中注册**HiddenHttpMethodFilter**

```xml
!--设置处理请求方式的过滤器-->
<filter>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>HiddenHttpMethodFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

由于浏览器只支持发送get和post方式的请求，那么该如何发送put和delete请求呢？

SpringMVC 提供了 **HiddenHttpMethodFilter** 帮助我们**将** **POST** **请求转换为** **DELETE** **或** **PUT** **请求**

**HiddenHttpMethodFilter** 处理put和delete请求的条件：

a>当前请求的请求方式必须为post

b>当前请求必须传输请求参数_method

满足以上条件，**HiddenHttpMethodFilter** 过滤器就会将当前请求的请求方式转换为请求参数method的值，因此请求参数_method的值才是最终的请求方式

> 目前为止，SpringMVC中提供了两个过滤器：`CharacterEncodingFilter`和`HiddenHttpMethodFilter`在web.xml中注册时，必须先注册CharacterEncodingFilter，再注册HiddenHttpMethodFilter
>
> 原因：
>
> - 在 CharacterEncodingFilter 中通过 request.setCharacterEncoding(encoding) 方法设置字符集的
> - request.setCharacterEncoding(encoding) 方法要求前面不能有任何获取请求参数的操作
>
> - 而 HiddenHttpMethodFilter 恰恰有一个获取请求方式的操作：
>
>   - ```java
>     String paramValue = request.getParameter(this.methodParam);
>     ```

### 7.4 **HiddenHttpMethodFilter** 源码解析

> 暂时跳过

## 8、RESTful案例

> - Tomcat的web.xml文件里的默认的Servlet本来是处理静态资源的，但是tomcat里的url-pattern被我们自己工程中写的web.xml覆盖了，变成由DispatcherServlet处理url里的请求，但是无法处理静态资源。
>
> - 所以我们需要在springmvc.xml里配置一个标签
>
>   - ```xml
>     <mvc:default-servlet-handler></mvc:default-servlet-handler>
>     ```

### 8.1 准备工作

和传统 CRUD 一样，实现对员工信息的增删改查。

- 搭建环境

- 准备实体类

```java
package com.cjj.pojo;

public class Employee {
    private Integer id;
    private String lastName;
    private String email;
    //1 male, 0 female
    private Integer gender;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Integer getGender() {
        return gender;
    }

    public void setGender(Integer gender) {
        this.gender = gender;
    }

    public Employee(Integer id, String lastName, String email, Integer
            gender) {
        super();
        this.id = id;
        this.lastName = lastName;
        this.email = email;
        this.gender = gender;
    }

    public Employee() {
    }
}
```

- 准备dao模拟数据

```java
@Repository
public class EmployeeDao {

    private static Map<Integer, Employee> employees = null;

    static {
        employees = new HashMap<Integer, Employee>();
        employees.put(1001, new Employee(1001, "E-AA", "aa@163.com", 1));
        employees.put(1002, new Employee(1002, "E-BB", "bb@163.com", 1));
        employees.put(1003, new Employee(1003, "E-CC", "cc@163.com", 0));
        employees.put(1004, new Employee(1004, "E-DD", "dd@163.com", 0));
        employees.put(1005, new Employee(1005, "E-EE", "ee@163.com", 1));
    }

    private static Integer initId = 1006;

    public void save(Employee employee) {
        if (employee.getId() == null) {
            employee.setId(initId++);
        }
        employees.put(employee.getId(), employee);
    }

    public Collection<Employee> getAll() {
        return employees.values();
    }

    public Employee get(Integer id) {
        return employees.get(id);
    }

    public void delete(Integer id) {
        employees.remove(id);
    }
}
```

### 8.2 功能清单

![image-20230609084531840](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702864.png)

### 8.3 具体功能：访问首页

#### ① 配置view-controller

```xml
<mvc:view-controller path="/" view-name="index"/>
```

#### ② 创建页面

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8" >
    <title>Title</title>
</head>
<body>
    <h1>首页</h1>
    <a th:href="@{/employee}">访问员工信息</a>
    </body>
</html>
```

### 8.4 具体功能：查询所有员工数据

####  ① 控制器方法

```java
package com.cjj.controller;

import com.cjj.dao.EmployeeDao;
import com.cjj.pojo.Employee;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.RequestMapping;

import java.util.Collection;

@Controller
public class EmployeeController {

    @Autowired
    private EmployeeDao employeeDao;

    @RequestMapping("/employee")
    public String getAllEmployee(Model model){
        // 获取所有员工信息
        Collection<Employee> allEmployee = employeeDao.getAll();
        // 向请求域中共享数据
        model.addAttribute("allEmployee",allEmployee);
        return "employee_list";
    }


}
```

#### ② 创建employee_list.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http//www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>employee list</title>
</head>
<body>
    <table>
        <tr>
            <th colspan="5">employee list</th>
        </tr>
        <tr>
            <th>id</th>
            <th>lastName</th>
            <th>email</th>
            <th>gender</th>
            <th>options</th>
        </tr>
        <tr th:each="employee : ${allEmployee}">
            <td th:text="${employee.id}"></td>
            <td th:text="${employee.lastName}"></td>
            <td th:text="${employee.email}"></td>
            <td th:text="${employee.gender}"></td>
            <td>
                <a href="">delete</a>
                <a href="">update</a>
            </td>
        </tr>
    </table>
</body>
</html>
```

### 8.5 具体功能：删除

#### ① 创建处理delete请求方式的表单

```html
<!-- 作用：通过超链接控制表单的提交，将post请求转换为delete请求 -->
<form id="delete_form" method="post">
    <!-- HiddenHttpMethodFilter要求：必须传输_method请求参数，并且值为最终的请求方式 -->
    <input type="hidden" name="_method" value="delete"/>
</form>
```

#### ② 删除超链接绑定点击事件

引入vue.js

```html
<script type="text/javascript" th:src="@{/static/js/vue.js}"></script>
```

删除超链接

```html
<a class="deleteA" @click="deleteEmployee"th:href="@{'/employee/'+${employee.id}}">delete</a>
```

通过vue处理点击事件

```vue
<script type="text/javascript">
    var vue = new Vue({
        el: "#dataTable",
        methods: {
            //event表示当前事件
            deleteEmployee: function (event) {
                //通过id获取表单标签
                var delete_form = document.getElementById("delete_form");
                //将触发事件的超链接的href属性为表单的action属性赋值
                delete_form.action = event.target.href;
                //提交表单
                delete_form.submit();
                //阻止超链接的默认跳转行为
                event.preventDefault();
            }
        }
    });
</script>
```

#### ③ 控制器方法

```java
@RequestMapping(value = "/employee/{id}", method = RequestMethod.DELETE)
public String deleteEmployee(@PathVariable("id") Integer id){
    employeeDao.delete(id);
    return "redirect:/employee";
}
```

### 8.6 具体功能：跳转到添加数据页面

#### ① 配置view-controller

```xml
<mvc:view-controller path="/toAdd" view-name="employee_add"></mvc:view-controller>
```

#### ② 创建employee_add.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Add Employee</title>
</head>
<body>
     <form th:action="@{/employee}" method="post">
        lastName:<input type="text" name="lastName"><br>
        email:<input type="text" name="email"><br>
        gender:<input type="radio" name="gender" value="1">male
        <input type="radio" name="gender" value="0">female<br>
        <input type="submit" value="add"><br>
    </form>
</body>
</html>
```

### 8.7 具体功能：执行保存

#### ① 控制器方法

```java
@RequestMapping(value = "/employee", method = RequestMethod.POST)
public String addEmployee(Employee employee){
    employeeDao.save(employee);
    return "redirect:/employee";
}
```

### 8.8 具体功能：跳转到更新数据页面

#### ① 修改超链接

```html
<a th:href="@{'/employee/'+${employee.id}}">update</a>
```

#### ② 控制器方法

```java
@RequestMapping(value = "/employee/{id}", method = RequestMethod.GET)
public String getEmployeeById(@PathVariable("id") Integer id, Model model){
    Employee employee = employeeDao.get(id);
    model.addAttribute("employee", employee);
    return "employee_update";
}
```

#### ③ 创建employee_update.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Update Employee</title>
</head>
<body>
<form th:action="@{/employee}" method="post">
    <input type="hidden" name="_method" value="put">
    <input type="hidden" name="id" th:value="${employee.id}">
    lastName:<input type="text" name="lastName" th:value="${employee.lastName}">
    <br>
    email:<input type="text" name="email" th:value="${employee.email}"><br>
    <!--
    	th:field="${employee.gender}"可用于单选框或复选框的回显
		若单选框的value和employee.gender的值一致，则添加checked="checked"属性
	-->
    gender:<input type="radio" name="gender" value="1"
    th:field="${employee.gender}">male
    <input type="radio" name="gender" value="0"
    th:field="${employee.gender}">female<br>
    <input type="submit" value="update"><br>
</form>
</body>
</html>
```

### 8.9 具体功能：执行更新

#### ① 控制器方法

```java
@RequestMapping(value = "/employee", method = RequestMethod.PUT)
public String updateEmployee(Employee employee){
    employeeDao.save(employee);
    return "redirect:/employee";
}
```

## 9、SpringMVC处理ajax请求

### 9.1 @RequestBody

@RequestBody可以获取请求体信息，使用@RequestBody注解标识控制器方法的形参，当前请求的请求体就会为当前注解所标识的形参赋值

```vue
<div id="app">
    <h1>index.html</h1>
    <input type="button" value="测试springmvc处理ajax" @click="testAjax()">
    <input type="button" value="测试springmvc处理ajax" @click="testRequestBody()">
</div>
<script type="text/javascript">
    var vue = new Vue({
        el:"#app",
        methods:{
            testAjax(){
                alert("test")
                axios.post(
                    "/SpringMVC/test/ajax?id=1001",
                    {username:"admin",password:"123456"}
                ).then(response=>{
                    console.log(response.data)
                });
            }
    })
</script>
```

```java
@RequestMapping("/test/RequestBody")
public String testRequestBody(@RequestBody String requestBody){
    System.out.println("requestBody:"+requestBody);
    return "success";
}
```

输出结果：

requestBody:username=admin&password=123456

### 9.2 @RequestBody获取json格式的请求参数

> 在使用了axios发送ajax请求之后，浏览器发送到服务器的请求参数有两种格式：
>
> 1、name=value&name=value...，此时的请求参数可以通过`request.getParameter()`获取，对应SpringMVC中，可以直接通过控制器方法的形参获取此类请求参数
>
> 2、{key:value,key:value,...}，此时无法通过request.getParameter()获取，之前我们使用操作json的相关jar包gson或jackson处理此类请求参数，可以将其转换为指定的实体类对象或map集合。在SpringMVC中，直接使用@RequestBody注解标识控制器方法的形参即可将此类请求参数转换为java对象

使用@RequestBody获取json格式的请求参数的条件：

1、导入jackson的依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

2、SpringMVC的配置文件中设置开启mvc的注解驱动

```xml
<!--开启mvc的注解驱动-->
<mvc:annotation-driven />
```

3、在控制器方法的形参位置，设置json格式的请求参数要转换成的java类型（实体类或map）的参数，并使用@RequestBody注解标识

```vue
<script type="text/javascript">
    var vue = new Vue({
        el:"#app",
        methods:{
            testRequestBody(){
                axios.post(
                    "/SpringMVC/test/RequestBody/json",
                    {username:"admin",password:"123456",age:"23",gender:"男"}
                ).then(response=>{
                });
            }
        }
    })
</script>
```

```java
@RequestMapping("/test/RequestBody/json")
public void testRequestBody(@RequestBody User user,Integer id, HttpServletResponse response) throws IOException {
    System.out.println(user);
    response.getWriter().write("hello,ajax");
}
```

```java
// 将json数据转成map集合
@RequestMapping("/test/RequestBody/json")
public void testRequestBody(@RequestBody Map<String,Object> map, Integer id, HttpServletResponse response) throws IOException {
    System.out.println(map);
    response.getWriter().write("hello,ajax");
}
```

### 9.3 @ResponseBody

@ResponseBody用于标识一个控制器方法，可以将该方法的返回值直接作为响应报文的响应体响应到浏览器，

使用前置条件同上。

```java
@RequestMapping("/testResponseBody")
public String testResponseBody(){
    //此时会跳转到逻辑视图success所对应的页面
    return "success";
}
@RequestMapping("/testResponseBody")
@ResponseBody
public String testResponseBody(){
    //此时响应浏览器数据success
    return "success";
}
```

### 9.4 @ResponseBody响应浏览器JSON数据

服务器处理ajax请求之后，大多数情况都需要向浏览器响应一个java对象，此时必须将java对象转换为json字符串才可以响应到浏览器，之前我们使用操作json数据的jar包gson或jackson将java对象转换为json字符串。在SpringMVC中，我们可以直接使用@ResponseBody注解实现此功能

@ResponseBody响应浏览器json数据的条件：

1、导入jackson的依赖

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.12.1</version>
</dependency>
```

2、SpringMVC的配置文件中设置开启mvc的注解驱动

```xml
<!--开启mvc的注解驱动-->
<mvc:annotation-driven />
```

3、使用@ResponseBody注解标识控制器方法，在方法中，将需要转换为json字符串并响应到浏览器的java对象作为控制器方法的返回值，此时SpringMVC就可以将此对象直接转换为json字符串并响应到浏览器

```vue
<input type="button" value="测试@ResponseBody响应浏览器json格式的数据"
@click="testResponseBody()"><br>
<script type="text/javascript" th:src="@{/js/vue.js}"></script>
<script type="text/javascript" th:src="@{/js/axios.min.js}"></script>
<script type="text/javascript">
var vue = new Vue({
    el:"#app",
    methods:{
    testResponseBody(){
        axios.post("/SpringMVC/test/ResponseBody/json").then(response=>{
        console.log(response.data);
    });
    }
   }
});
</script>
```

```java
//响应浏览器list集合
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public List<User> testResponseBody(){
    User user1 = new User(1001,"admin1","123456",23,"男");
    User user2 = new User(1002,"admin2","123456",23,"男");
    User user3 = new User(1003,"admin3","123456",23,"男");
    List<User> list = Arrays.asList(user1, user2, user3);
    return list;
}
//响应浏览器map集合
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public Map<String, Object> testResponseBody(){
    User user1 = new User(1001,"admin1","123456",23,"男");
    User user2 = new User(1002,"admin2","123456",23,"男");
    User user3 = new User(1003,"admin3","123456",23,"男");
    Map<String, Object> map = new HashMap<>();
    map.put("1001", user1);
    map.put("1002", user2);
    map.put("1003", user3);
    return map;
}
//响应浏览器实体类对象
@RequestMapping("/test/ResponseBody/json")
@ResponseBody
public User testResponseBody(){
    return user;
}
```

### 9.5 @RestController注解

`@RestController`注解是springMVC提供的一个复合注解，标识在控制器的类上，就相当于为类添加了@Controller注解，并且为其中的每个方法添加了@ResponseBody注解

## 10、文件上传和下载

### 10.1 文件下载

`ResponseEntity`用于控制器方法的返回值类型，该控制器方法的返回值就是响应到浏览器的响应报文

使用ResponseEntity实现下载文件的功能

> - `File.separator`
>
>   ​	该方法能够自适配系统来返回一个文件分隔符
>
> - `byte[] bytes = new byte[is.available()];`
>
>   - is.available()方法会获取当前字节输入流所对应的文件所占字节数

```java
@Controller
public class FileUpAndDownController {

    @RequestMapping("/testDown")
    /**
    	设置session参数：因为我们要实现文件下载功能，也就需要进行文件的复制。
    	所以需要知道当前下载文件所在服务器的路径，就可以通过session获取servletContext对象
    	调用其getRealPath()获取某个资源在服务器上的路径
    */
    public ResponseEntity<byte[]> testResponseEntity(HttpSession session) throws
            IOException {
        //获取ServletContext对象
        ServletContext servletContext = session.getServletContext();
        //获取服务器中文件的真实路径
        String realPath = servletContext.getRealPath("/static/img/1.jpg");
        //创建输入流
        InputStream is = new FileInputStream(realPath);
        //创建字节数组
        byte[] bytes = new byte[is.available()];
        //将流读到字节数组中
        is.read(bytes);
        //创建HttpHeaders对象设置响应头信息
        MultiValueMap<String, String> headers = new HttpHeaders();
        //设置要下载方式以及下载文件的名字
        headers.add("Content-Disposition", "attachment;filename=1.jpg"); //以附件形式下载
        //设置响应状态码
        HttpStatus statusCode = HttpStatus.OK;
        //创建ResponseEntity对象
        ResponseEntity<byte[]> responseEntity = new ResponseEntity<>(bytes, headers, statusCode);
        //关闭输入流
        is.close();
        return responseEntity;
    }
}
```

### 10.2 文件上传

文件上传要求form表单的请求方式必须为post，并且添加属性`enctype="multipart/form-data"SpringMVC`中将上传的文件封装到MultipartFile对象中，通过此对象可以获取文件相关信息

上传步骤：

①添加依赖：

```xml
<!-- https://mvnrepository.com/artifact/commons-fileupload/commons-fileupload -->
<dependency>
    <groupId>commons-fileupload</groupId>
    <artifactId>commons-fileupload</artifactId>
    <version>1.3.1</version>
</dependency>
```

②在SpringMVC的配置文件中添加配置：

```xml
<!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
<bean id="multipartResolver"
class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
</bean>
```

③控制器方法：

```java
// photo:名称和input中的name一致
@RequestMapping("/testUp")
public String testUp(MultipartFile photo, HttpSession session) throws IOException {
    //获取上传的文件的文件名
    String fileName = photo.getOriginalFilename();
    //处理文件重名问题
    String hzName = fileName.substring(fileName.lastIndexOf("."));
    fileName = UUID.randomUUID().toString() + hzName;
    //获取服务器中photo目录的路径
    ServletContext servletContext = session.getServletContext();
    String photoPath = servletContext.getRealPath("photo");
    File file = new File(photoPath);
    if (!file.exists()) {
        file.mkdir();
    }
    String finalPath = photoPath + File.separator + fileName;
    //实现上传功能
    photo.transferTo(new File(finalPath));
    return "success";
}
```

## 11、拦截器

> - 过滤器：在浏览器和目标资源之间过滤。也就是浏览器和DispartcherSeverlet之间。
> - 拦截器：在DispartcherSeverlet调用控制器方法执行前后

### 11.1 拦截器的配置

SpringMVC中的拦截器用于拦截控制器方法的执行

SpringMVC中的拦截器需要实现HandlerInterceptor

SpringMVC的拦截器必须在SpringMVC的配置文件中进行配置：

```xml
<mvc:interceptors>
    <!--<bean class="com.cjj.interceptor.FirstInterceptor"></bean>-->
    <!--<ref bean="firstInterceptor"></ref>-->
    <!-- 以上两种配置方式都是对DispatcherServlet所处理的所有的请求进行拦截 -->
    <mvc:interceptor>
        <!--
           /*:拦截只有一层目录下的请求，比如http://localhost:8080/SpringMVC/test
           /**:拦截所有
        -->
        <mvc:mapping path="/**"/>
        <!--配置需要排除拦截的请求的请求路径-->
        <mvc:exclude-mapping path="/abc"/>
        <!--配置拦截器-->
        <ref bean="firstInterceptor"></ref>
    </mvc:interceptor>
</mvc:interceptors>
```

拦截器：

```java
@Component
public class FirstInterceptor implements HandlerInterceptor {
    /**
     * 控制器方法执行之前执行
     * @param request
     * @param response
     * @param handler
     * @return false:拦截|true:放行
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("FirstInterceptor-->preHandle");
        return HandlerInterceptor.super.preHandle(request, response, handler);
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("FirstInterceptor-->postHandle");
        HandlerInterceptor.super.postHandle(request, response, handler, modelAndView);
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("FirstInterceptor-->afterCompletion");
        HandlerInterceptor.super.afterCompletion(request, response, handler, ex);
    }
}
```

### 11.2 拦截器的三个抽象方法

SpringMVC中的拦截器有三个抽象方法：

`preHandle`：控制器方法执行之前执行preHandle()，其boolean类型的返回值表示是否拦截或放行，返回true为放行，即调用控制器方法；返回false表示拦截，即不调用控制器方法

`postHandle`：控制器方法执行之后执行postHandle()

`afterCompletion`：处理完视图和模型数据，渲染视图完毕之后执行afterCompletion()

### 11.3 多个拦截器的执行顺序

①若每个拦截器的preHandle()都返回true

此时多个拦截器的执行顺序和拦截器在SpringMVC的配置文件的配置顺序有关：preHandle()会按照配置的顺序执行，而postHandle()和afterCompletion()会按照配置的反序执行

```xml
<mvc:interceptors>
    <!--<bean class="com.cjj.interceptor.FirstInterceptor"></bean>-->
    <!--<ref bean="firstInterceptor"></ref>-->
    <mvc:interceptor>
        <!--
           /*:拦截只有一层目录下的请求，比如http://localhost:8080/SpringMVC/test
           /**:拦截所有
        -->
        <mvc:mapping path="/**"/>
        <!--配置需要排除拦截的请求的请求路径-->
        <mvc:exclude-mapping path="/abc"/>
        <!--配置拦截器-->
        <ref bean="firstInterceptor"></ref>
    </mvc:interceptor>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <mvc:exclude-mapping path="/abc"/>
        <ref bean="secondInterceptor"></ref>
    </mvc:interceptor>
</mvc:interceptors>
```

![image-20230628120507863](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702865.png)

②若某个拦截器的preHandle()返回了false

preHandle()返回false和它之前的拦截器的preHandle()都会执行，postHandle()都不执行，返回false的拦截器之前的拦截器的afterCompletion()会执行

![image-20230628121035529](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702866.png)

## 12、异常处理器

> - SpringMVC默认使用`DefaultHandlerExceptionResolove`默认异常解析器。

### 12.1 基于配置的异常处理

SpringMVC提供了一个处理控制器方法执行过程中所出现的异常的接口（控制器异常处理解析器）：`HandlerExceptionResolverHandler`

ExceptionResolver接口的实现类有：DefaultHandlerExceptionResolver和SimpleMappingExceptionResolver

SpringMVC提供了自定义的异常处理器SimpleMappingExceptionResolver，使用方式：

```xml
<bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
    <property name="exceptionMappings">
        <props>
            <!--key设置要处理的异常，value设置出现该异常时要跳转的页面对应的逻辑视图-->
            <prop key="java.lang.ArithmeticException">error</prop>
        </props>
    </property>
    <!--内部操作:把我们当前异常信息共享到请求域中-->
    <!--
        属性名：ex
        属性值：上述prop定义的异常信息
    -->
    <property name="exceptionAttribute" value="ex"></property>
</bean>
```

### 12.2 基于注解的异常处理

```java
@Target({ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface ExceptionHandler {
    // 泛型：? 表示是Throwable本身或者是它的子类
    Class<? extends Throwable>[] value() default {};
}
```

```java
//@ControllerAdvice将当前类标识为异常处理的组件
@ControllerAdvice
public class ExceptionController {
    //@ExceptionHandler用于设置所标识方法处理的异常
    @ExceptionHandler(ArithmeticException.class)
    //ex表示当前请求处理中出现的异常对象
    public String handleArithmeticException(Exception ex, Model model) {
        model.addAttribute("ex", ex);
        return "error";
    }
}
```

## 13、`☆`注解配置SpringMVC

`使用配置类和注解代替web.xml和SpringMVC配置文件的功能`

web.xml

> - 当没有开启注解驱动，配置了前端控制器，依然可以使用@RequestMapping来进行请求映射
>   - 当DispatcherServlet配置了作为前端控制器时，它会拦截所有的请求并将其分发给相应的处理器进行处理。这样，即使没有开启注解驱动，DispatcherServlet仍然能够使用注解进行请求映射。
>   - 当请求到达DispatcherServlet时，它会根据配置文件中的`<context:component-scan>`标签扫描指定包下的注解，找到带有`@Controller`注解的类，并将其初始化为处理器对请求进行处理。而`@RequestMapping`是`@Controller`注解的一部分，它被用来配置请求的映射路径。
>   - 所以，即使没有开启注解驱动，只要配置了DispatcherServlet作为前端控制器，并在控制器类上使用了`@Controller`注解和`@RequestMapping`注解，DispatcherServlet仍然可以识别并使用这些注解来处理请求。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">

    <!--配置SpringMVC的前端控制器DispatcherServlet-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--设置springmvc配置文件的位置和名称-->
        <init-param>
            <!--指定springmvc配置文件路径-->
            <param-name>contextConfigLocation</param-name>
            <!--类路径下的-->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!--将DispatcherServlet初始化提前到服务器启动时,不然第一次访问页面需要转圈圈-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <!--default:处理静态资源-->
        <servlet-name>SpringMVC</servlet-name>
        <!--前端控制器请求的路径模型-->
        <!--当浏览器发送的请求符合url-pattern时,那么当前请求就会被前端控制器所处理-->
        <url-pattern>/</url-pattern> <!--jsp在tomcat的xml文件中，有专门的处理-->
    </servlet-mapping>
    
</web-app>
```

SpringMVC.xml

> - 开启注解驱动
>   - 在Spring MVC中设置`<mvc:view-controller>`时，其他控制器中的请求映射将会被忽略，这是因为View Controller的目的是将请求直接映射到一个预定义的视图，而不需要经过控制器的处理。
>   - 当你配置了View Controller后，Spring MVC会优先匹配这些预定义的请求映射，而不再执行其他控制器中的请求映射。这就导致其他控制器中的请求映射将会失效。
>   - 为了解决这个问题，你可以在Spring MVC的核心配置文件中添加`<mvc:annotation-driven />`标签来开启注解驱动。这样做的目的是告诉Spring MVC，使用注解来处理请求映射，并优先使用注解配置的控制器处理请求。
>   - 使用`<mvc:annotation-driven />`标签开启注解驱动后，注解配置的控制器将恢复

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--扫描组件-->
    <context:component-scan base-package="com.cjj.controller"></context:component-scan>

    <!-- 配置Thymeleaf视图解析器 -->
    <!--可以使用SpringMVC为我们提供的方式，来进行视图渲染，并实现页面跳转-->
    <!--通过源码：由SpringMVC中DispatcherServlet内部帮我们加载的-->
    <bean id="viewResolver"
          class="org.thymeleaf.spring5.view.ThymeleafViewResolver">
        <!--优先级-->
        <property name="order" value="1"/>
        <!--编码-->
        <property name="characterEncoding" value="UTF-8"/>
        <!--模板引擎-->
        <property name="templateEngine">
            <bean class="org.thymeleaf.spring5.SpringTemplateEngine">
                <!--模板解析器-->
                <property name="templateResolver">
                    <bean class="org.thymeleaf.spring5.templateresolver.SpringResourceTemplateResolver">
                        <!-- 视图前缀 -->
                        <property name="prefix" value="/WEB-INF/templates/"/>
                        <!-- 视图后缀 -->
                        <property name="suffix" value=".html"/>
                        <property name="templateMode" value="HTML5"/>
                        <property name="characterEncoding" value="UTF-8" />
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
    
    <!--配置默认的servlet处理静态资源-->
    <mvc:default-servlet-handler></mvc:default-servlet-handler>
    
    <!-- 开启 注解驱动-->
    <mvc:annotation-driven></mvc:annotation-driven>
    <!--
    path：设置处理的请求地址
    view-name：设置请求地址所对应的视图名称
    -->
    <mvc:view-controller path="/testView" view-name="success"></mvc:view-controller>
    
    <!--必须通过文件解析器的解析才能将文件转换为MultipartFile对象-->
    <bean id="multipartResolver"
    class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    </bean>
    
    <!-- 配置拦截器 -->
    <mvc:interceptors>
        <!--<bean class="com.cjj.interceptor.FirstInterceptor"></bean>-->
        <!--<ref bean="firstInterceptor"></ref>-->
        <!-- 以上两种配置方式都是对DispatcherServlet所处理的所有的请求进行拦截 -->
        <mvc:interceptor>
            <!--
               /*:拦截只有一层目录下的请求，比如http://localhost:8080/SpringMVC/test
               /**:拦截所有
            -->
            <mvc:mapping path="/**"/>
            <!--配置需要排除拦截的请求的请求路径-->
            <mvc:exclude-mapping path="/abc"/>
            <!--配置拦截器-->
            <ref bean="firstInterceptor"></ref>
        </mvc:interceptor>
	</mvc:interceptors>
    
    <!-- 配置异常处理器 -->
    <bean class="org.springframework.web.servlet.handler.SimpleMappingExceptionResolver">
        <property name="exceptionMappings">
            <props>
                <!--key设置要处理的异常，value设置出现该异常时要跳转的页面对应的逻辑视图-->
                <prop key="java.lang.ArithmeticException">error</prop>
            </props>
        </property>
        <!--内部操作:把我们当前异常信息共享到请求域中-->
        <!--
            属性名：ex
            属性值：上述prop定义的异常信息
        -->
        <property name="exceptionAttribute" value="ex"></property>
    </bean>
</beans>
```

### 13.1 创建初始化类，代替web.xml

> - Servlet容器：Tomcat

在Servlet3.0环境中，容器会在类路径中查找实现javax.servlet.ServletContainerInitializer接口的类，如果找到的话就用它来配置Servlet容器。

 Spring提供了这个接口的实现，名为SpringServletContainerInitializer，这个类反过来又会查找实现WebApplicationInitializer的类并将配置的任务交给它们来完成。

Spring3.2引入了一个便利的WebApplicationInitializer基础实现，名为AbstractAnnotationConfigDispatcherServletInitializer，当我们的类扩展了AbstractAnnotationConfigDispatcherServletInitializer并将其部署到Servlet3.0容器的时候，容器会自动发现它，并用它来配置Servlet上下文。

```java
// 继承的类自动配置了DispatcherServelet
public class WebInit extends AbstractAnnotationConfigDispatcherServletInitializer {

    @Override
    // 设置一个配置类代替Spring的配置文件
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    @Override
    // 设置一个配置类代替SpringMVC的配置文件
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{WebConfig.class};
    }

    @Override
    // 设置DispatcherServlet的url-pattern
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    @Override
    // 设置当前的过滤器
    protected Filter[] getServletFilters() {
        // 创建编码过滤器
        CharacterEncodingFilter characterEncodingFilter = new CharacterEncodingFilter();
        // 设置请求和响应的编码
        characterEncodingFilter.setEncoding("UTF-8");
        characterEncodingFilter.setForceEncoding(true);
        // 创建请求方式的过滤器
        HiddenHttpMethodFilter hiddenHttpMethodFilter = new HiddenHttpMethodFilter();
        return new Filter[]{characterEncodingFilter,hiddenHttpMethodFilter};
    }
}
```

### 13.2 创建SpringConfig配置类，代替spring的配置文件

```java
/**
 * 代替Spring的配置文件
 */
@Configuration
public class SpringConfig {
    // ssm整合后,Spring的配置信息写在此类中
}
```

### 13.3 创建WebConfig配置类，代替SpringMVC的配置文件

> SpringMVC配置过的内容
>
> 1. 扫描控制层组件
> 2. 视图解析器
> 3. view-controller默认的servlet处理静态资源
> 4. 开启mvc注解驱动
> 5. 文件上传解析器
> 6. 拦截器
> 7. 异常解析器

```java
/**
 * 代替SpringMVC的配置文件
 */
// 将类表示为配置类
@Configuration
// 扫描组件
@ComponentScan("com.cjj.controller")
// 开启MVC注解驱动
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    // 默认的servlet处理静态资源
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
        WebMvcConfigurer.super.configureDefaultServletHandling(configurer);
    }

    @Override
    // 配置视图解析器
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/").setViewName("index");
        WebMvcConfigurer.super.addViewControllers(registry);
    }

    @Bean
    // 文件上传解析器：被Bean标识的方法的返回值将会作为bean被IOC容器所管理,bean的id为方法名
    public CommonsMultipartResolver multipartResolver() {
        return new CommonsMultipartResolver();
    }

    @Override
    // 配置拦截器
    public void addInterceptors(InterceptorRegistry registry) {
        FirstInterceptor firstInterceptor = new FirstInterceptor();
        registry.addInterceptor(firstInterceptor).addPathPatterns("/**");
        WebMvcConfigurer.super.addInterceptors(registry);
    }

    @Override
    // 异常处理器
    public void configureHandlerExceptionResolvers(List<HandlerExceptionResolver> resolvers) {
        SimpleMappingExceptionResolver exceptionResolver = new SimpleMappingExceptionResolver();
        Properties properties = new Properties();
        properties.setProperty("java.lang.ArithmeticException", "error");
        exceptionResolver.setExceptionMappings(properties);
        exceptionResolver.setExceptionAttribute("ex");
        resolvers.add(exceptionResolver);
        WebMvcConfigurer.super.configureHandlerExceptionResolvers(resolvers);
    }

    //配置生成模板解析器
    @Bean
    public ITemplateResolver templateResolver() {
        WebApplicationContext webApplicationContext =
                ContextLoader.getCurrentWebApplicationContext();
        // ServletContextTemplateResolver需要一个ServletContext作为构造参数，
        // 可通过WebApplicationContext的方法获得
        ServletContextTemplateResolver templateResolver =
                new ServletContextTemplateResolver(webApplicationContext.getServletContext());
        templateResolver.setPrefix("/WEB-INF/templates/");
        templateResolver.setSuffix(".html");
        templateResolver.setCharacterEncoding("UTF-8");
        templateResolver.setTemplateMode(TemplateMode.HTML);
        return templateResolver;
    }

    //生成模板引擎并为模板引擎注入模板解析器
    @Bean
    public SpringTemplateEngine templateEngine(ITemplateResolver templateResolver) {
        SpringTemplateEngine templateEngine = new SpringTemplateEngine();
        templateEngine.setTemplateResolver(templateResolver);
        return templateEngine;
    }

    //生成视图解析器并为解析器注入模板引擎
    @Bean
    public ViewResolver viewResolver(SpringTemplateEngine templateEngine) {
        ThymeleafViewResolver viewResolver = new ThymeleafViewResolver();
        viewResolver.setCharacterEncoding("UTF-8");
        viewResolver.setTemplateEngine(templateEngine);
        return viewResolver;
    }

}
```

## 14、SpringMVC执行流程

### 14.1 SpringMVC常用组件

> - DispatcherServlet：**前端控制器**，不需要工程师开发，由框架提供
>   - 作用：统一处理请求和响应，整个流程控制的中心，由它调用其它组件处理用户的请求
> - HandlerMapping：**处理器映射器**，不需要工程师开发，由框架提供
>   - 负责将浏览器请求匹配到控制器的方法上
>   - 作用：根据请求的url、method等信息查找Handler，即控制器方法
> - Handler：**处理器**，需要工程师开发
>   - 就是控制器方法
>   - 作用：在DispatcherServlet的控制下Handler对具体的用户请求进行处理
> - HandlerAdapter：**处理器适配器**，不需要工程师开发，由框架提供
>   - 由适配器调用控制器方法
>   - 作用：通过HandlerAdapter对处理器（控制器方法）进行执行
> - ViewResolver：**视图解析器**，不需要工程师开发，由框架提供
>   - 作用：进行视图解析，得到相应的视图，例如：ThymeleafView、InternalResourceView、RedirectView
>
> - View：**视图**
>   - 作用：将模型数据通过页面展示给用户

### 14.2 DispatcherServlet初始化过程

DispatcherServlet 本质上是一个 Servlet，所以天然的遵循 Servlet 的生命周期。所以宏观上是 Servlet生命周期来进行调度。

![image-20230628225647315](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230628225647315.png)

**①初始化WebApplicationContext**

所在类：org.springframework.web.servlet.FrameworkServlet

![image-20230628230354029](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702867.png)

**②创建WebApplicationContext**

所在类：org.springframework.web.servlet.FrameworkServlet

![image-20230628230433247](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702868.png)

**③DispatcherServlet初始化策略**

FrameworkServlet创建WebApplicationContext后，刷新容器，调用onRefresh(wac)，此方法在DispatcherServlet中进行了重写，调用了initStrategies(context)方法，初始化策略，即初始化DispatcherServlet的各个组件

所在类：org.springframework.web.servlet.DispatcherServlet

![image-20230628230514926](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702869.png)

### 14.3 DispatcherServlet调用组件处理请求

**①processRequest()**

FrameworkServlet重写HttpServlet中的service()和doXxx()，这些方法中调用了processRequest(request, response)

所在类：org.springframework.web.servlet.FrameworkServlet

![image-20230628230837189](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702870.png)

**②doService()**

所在类：org.springframework.web.servlet.DispatcherServlet

![image-20230628230919632](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702872.png)

**③doDispatch()**

所在类：org.springframework.web.servlet.DispatcherServlet

![image-20230628231003457](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702873.png)

**④processDispatchResult()**

![image-20230628231037356](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702874.png)

### 14.4 SpringMVC的执行流程

> - 1) 用户向服务器发送请求，请求被SpringMVC 前端控制器 DispatcherServlet捕获。
>
> - 2) DispatcherServlet对请求URL进行解析，得到请求资源标识符（URI），判断请求URI对应的映射：
>   - a) 不存在
>     - i. 再判断是否配置了mvc:default-servlet-handler
>     - ii. 如果没配置，则控制台报映射查找不到，客户端展示404错误
>
>     ![image-20230628231125922](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702875.png)
>
>     ![image-20230628231134818](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702876.png)
>
>     - iii. 如果有配置，则访问目标资源（一般为静态资源，如：JS,CSS,HTML），找不到客户端也会展示404错误
>
>     ![image-20230628231309907](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702877.png)
>
>     ![image-20230628231313245](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251702878.png)
>
>   - b) 存在则执行下面的流程
>
>     - 3) 根据该URI，调用HandlerMapping获得该Handler配置的所有相关的对象（包括Handler对象以及Handler对象对应的拦截器），最后以HandlerExecutionChain执行链对象的形式返回。
>     - 4) DispatcherServlet 根据获得的Handler，选择一个合适的HandlerAdapter。
>     - 5) 如果成功获得HandlerAdapter，此时将开始执行拦截器的preHandler(…)方法【正向】
>     - 6) 提取Request中的模型数据，填充Handler入参，开始执行Handler（Controller）方法，处理请求。在填充Handler的入参过程中，根据你的配置，Spring将帮你做一些额外的工作：
>       - a) HttpMessageConveter： 将请求消息（如Json、xml等数据）转换成一个对象，将对象转换为指定的响应信息
>       - b) 数据转换：对请求消息进行数据转换。如String转换成Integer、Double等
>       - c) 数据格式化：对请求消息进行数据格式化。 如将字符串转换成格式化数字或格式化日期等
>       - d) 数据验证： 验证数据的有效性（长度、格式等），验证结果存储到BindingResult或Error中
>     - 7) Handler执行完成后，向DispatcherServlet 返回一个ModelAndView对象。
>     - 8) 此时将开始执行拦截器的postHandle(...)方法【逆向】。
>     - 9) 根据返回的ModelAndView（此时会判断是否存在异常：如果存在异常，则执行HandlerExceptionResolver进行异常处理）选择一个适合的ViewResolver进行视图解析，根据Model和View，来渲染视图。
>     - 10) 渲染视图完毕执行拦截器的afterCompletion(…)方法【逆向】。
>     - 11) 将渲染结果返回给客户端。

# 四、SSM整合

> - SpringMVC
>   - 表述层框架，处理浏览器发送到服务器的请求并响应相应的数据
> - Mybatis
>   - 持久层框架，连接数据库，访问数据库，操作数据库中的数据
> - Spring
>   - 整合型框架，IOC和AOP。通过IOC容器帮我们管理对象，比如Mybatis里的操作数据库的SqlSession对象。而声明式事务是我们AOP的一个重要实现。
> - 整合：SpringMVC和Spring各自需要创建自己的ioc容器，管理各自的组件。
>   - SpringMVC的IOC容器是在DispatcherServlet进行初始化的过程(服务器启动时)中来创建的，主要管理控制层的组件。
>   - Spring的IOC容器是在DispatcherServlet进行初始化之前创建，因为controller控制层里的service对象依赖于我们的service层。所以需要提前创建Spring的IOC容器从而进行自动装配，所以可以在监听器初始化的过程中创建。
> - 服务器三大组件：监听器，过滤器，servlet

## 4.1 ContextLoaderListener

> 监听服务器的启动和关闭，里面的初始化方法只执行一次。在服务器启动时加载spring的配置文件，来获取我们的ioc容器。

Spring提供了监听器ContextLoaderListener，实现ServletContextListener接口，可监听ServletContext的状态，在web服务器的启动，读取Spring的配置文件，创建Spring的IOC容器。web应用中必须在web.xml中配置。

```xml
<listener>
    <!--
        配置Spring的监听器，在服务器启动时加载Spring的配置文件
        Spring配置文件默认位置和名称：/WEB-INF/applicationContext.xml
        可通过上下文参数自定义Spring配置文件的位置和名称
    -->
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
<!--自定义Spring配置文件的位置和名称-->
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:spring.xml</param-value>
</context-param>
```

## 4.2 准备工作

### ①创建Maven Module

### ②导入依赖

> - **`spring-context`**
>   - spring-context是Spring Framework的核心模块之一，它提供了IoC（Inversion of Control，控制反转）容器的实现和管理。IoC容器是Spring框架的关键部分，它负责将应用程序中的各种组件（例如Java类、对象、配置信息等）进行实例化、组装和管理。
>   - 使用spring-context，您可以通过将对象的创建和依赖关系交给IoC容器来实现解耦和灵活性。通过在配置文件中定义Bean（组件）的详细信息，包括其依赖、属性值和生命周期等，Spring容器可以根据这些信息自动创建和管理这些Bean实例。
>   - 此外，spring-context还提供了一些其他功能，例如：
>     - 事件处理：通过发布和监听事件，实现应用内部各个组件之间的解耦，使得应用更加灵活和可扩展。
>     - 国际化和本地化支持：提供了对应用程序本地化的支持，可以轻松地在应用中处理不同语言和区域的文本和消息。
>     - 资源管理：简化了对各种资源（如文件、数据库连接、RESTful服务等）的访问和管理。
>   - 总之，spring-context模块为Spring应用程序提供了一个强大而灵活的IoC容器，使得开发者可以更加专注于业务逻辑，而不必过多关心对象的创建和依赖关系的管理。
> - **`spring-beans`**
>   - spring-beans是Spring Framework的一个核心模块，提供了IoC（Inversion of Control）容器和BeanFactory的实现。它是spring-context模块的基础，用于定义和操作应用程序中的Bean。
>   - 主要功能包括：
>     - 定义和配置Bean：spring-beans模块支持通过XML、注解或Java代码来定义和配置Bean。您可以在配置文件中声明Bean的类型、依赖关系和属性值等信息。
>     - Bean的生命周期管理：spring-beans提供了一组回调接口和扩展点，允许您在Bean的创建、初始化和销毁等生命周期阶段执行自定义逻辑。
>     - 依赖注入（Dependency Injection）：使用spring-beans，您可以将Bean之间的依赖关系交给容器处理。容器会自动解析和注入Bean之间的依赖关系，减少了手动编写大量的代码来管理对象之间的依赖。
>     - 创建和管理Bean实例：通过IoC容器，spring-beans负责实例化、组装和管理Bean对象。您只需在配置文件中定义Bean的信息，容器会负责根据这些信息创建所需的Bean实例。
>   - spring-beans是Spring Framework中的一个基础模块，提供了Bean的定义和管理等核心功能。而spring-context模块在此基础上提供了更高级的功能和服务，使得应用程序的开发更加便捷和灵活。
> - **`spring-web`**
>   - spring-web是Spring Framework的一个核心模块，用于构建Web应用程序和提供与Web相关的功能和服务。它主要包含了以下几个方面的功能：
>     - Web开发支持：spring-web模块提供了对Web开发的支持，包括处理HTTP请求和响应、路由URL、处理表单数据、处理会话、处理文件上传等。它通过提供一组Web相关的类和注解，简化了开发Web应用程序的过程。
>     - MVC框架支持：spring-web模块还提供了Spring MVC（Model-View-Controller）框架的实现。Spring MVC是一种基于Java的Web开发框架，用于构建灵活、可扩展的Web应用程序。通过使用Spring MVC，您可以将Web应用程序组织成不同的模块，在不同的请求和响应之间进行分发和处理。
>     - RESTful Web服务：spring-web模块支持构建和发布RESTful Web服务。它提供了一组注解和类，用于定义和处理RESTful风格的URL、处理JSON或XML数据、进行内容协商等。这使得使用Spring Framework构建和管理RESTful Web服务变得更加简单和方便。
> - **`spring-webmvc`**
>   - spring-webmvc是Spring Framework的一个模块，用于构建基于Java的Web应用程序。它提供了一个强大而灵活的MVC（Model-View-Controller）框架，用于处理Web请求和生成响应
>   - spring-webmvc模块包含了以下几个方面的功能：
>     - MVC架构支持：spring-webmvc基于MVC设计模式，通过将应用程序划分为模型（Model）、视图（View）和控制器（Controller）三个部分，实现了灵活的应用程序组织和分离关注点。
>     - 控制器映射和请求处理：spring-webmvc通过控制器映射（HandlerMapping）和请求处理器（HandlerAdapter）来将HTTP请求映射到具体的Controller方法上，并进行相应的处理。
>     - 视图解析和渲染：spring-webmvc支持不同类型的视图解析器（ViewResolver），可以将逻辑视图名解析为具体的视图。它还提供了丰富的视图技术，如JSP、Thymeleaf、Freemarker等，用于将数据渲染到实际的响应内容中。
>     - 数据绑定和表单处理：spring-webmvc支持数据绑定和表单处理，可以将请求参数绑定到Controller方法的参数上，简化了处理表单数据的过程。
>     - 拦截器和过滤器：spring-webmvc提供了拦截器（Interceptor）和过滤器（Filter）的支持，可以对请求进行预处理和后处理。拦截器和过滤器可以实现日志记录、安全校验、权限控制等功能。
> - **`spring-jdbc`**
>   - spring-jdbc是Spring框架提供的一个模块，用于简化与关系型数据库交互的开发。它提供了一组类和方法，可以轻松地执行SQL查询、更新和存储过程调用等操作。spring-jdbc是Spring框架提供的一个模块，用于简化与关系型数据库交互的开发。它提供了一组类和方法，可以轻松地执行SQL查询、更新和存储过程调用等操作。

```xml
<packaging>war</packaging>
<!--自定义属性-->
<properties>
    <maven.compiler.source>8</maven.compiler.source>
    <maven.compiler.target>8</maven.compiler.target>
    <!--spring的版本号-->
    <spring.version>5.3.1</spring.version>
</properties>
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!--springmvc-->
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <!--
			有了mybatis为啥还要spring-jdbc呢？
			因为像我们使用的事务管理器DataSourceTransactionMannager就在该包下
		-->
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
    </dependency>
    <!-- Mybatis核心 -->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.7</version>
    </dependency>
    <!--mybatis和spring的整合包-->
    <dependency>
        <!-- 提供了相关工厂bean，可以直接获得工厂生成的对象 -->
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>2.0.6</version>
    </dependency>
    <!-- 连接池 -->
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.0.9</version>
    </dependency>
    <!-- junit测试 -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
    <!-- MySQL驱动 -->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.16</version>
    </dependency>
    <!-- log4j日志 -->
    <dependency>
        <groupId>log4j</groupId>
        <artifactId>log4j</artifactId>
        <version>1.2.17</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/com.github.pagehelper/pagehelper -->
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper</artifactId>
        <version>5.2.0</version>
    </dependency>
    <!-- 日志 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    <!-- ServletAPI -->
    <dependency>
        <groupId>javax.servlet</groupId>
        <artifactId>javax.servlet-api</artifactId>
        <version>3.1.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>2.12.1</version>
    </dependency>
    <!-- 文件上传 -->
    <dependency>
        <groupId>commons-fileupload</groupId>
        <artifactId>commons-fileupload</artifactId>
        <version>1.3.1</version>
    </dependency>
    <!-- Spring5和Thymeleaf整合包 -->
    <dependency>
        <groupId>org.thymeleaf</groupId>
        <artifactId>thymeleaf-spring5</artifactId>
        <version>3.0.12.RELEASE</version>
    </dependency>
</dependencies>
```

### ③创建表

```SQL
CREATE TABLE `t_emp` (
    `emp_id` int(11) NOT NULL AUTO_INCREMENT,
    `emp_name` varchar(20) DEFAULT NULL,
    `age` int(11) DEFAULT NULL,
    `sex` char(1) DEFAULT NULL,
    `email` varchar(50) DEFAULT NULL,
    PRIMARY KEY (`emp_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

## 4.3 配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    
    <!--配置spring的编码过滤器-->
    <filter>
        <filter-name>CharacterEncodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>CharacterEncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!--配置处理请求方式的过滤器-->
    <filter>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>HiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
    
    <!--配置SpringMVC的前端控制器-->
    <servlet>
        <servlet-name>SpringMVC</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--设置springmvc配置文件自定义的位置和名称-->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!--将DispatcherServlet的初始化时间提前到服务器启动时-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>SpringMVC</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
    
    <!--配置spring的监听器，在服务器启动时加载spring配置文件-->
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
    <!--设置spring配置文件的位置和名称-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:spring.xml</param-value>
    </context-param>
</web-app>
```

## 4.4 创建SpringMVC的配置文件并配置

spring.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--扫描组件（除控制层）-->
    <context:component-scan base-package="com.cjj.ssm">
        <!--通过注解的类路径排除指定注解下的组件-->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!--引入jdbc.properties文件-->
    <!--通过类路径也就是war包里的classes路径-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <!--配置数据源-->
    <bean id="DataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>
</beans>
```

## 4.5 搭建MyBatis环境

### ① 创建属性文件jdbc.properties

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:13306/ssm?serverTimezone=UTC
jdbc.username=root
jdbc.password=root
```

### ② 创建MyBatis的核心配置文件mybatis-config.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<!--
    MyBatis核心配置文件中，标签的顺序：
    properties?,settings?,typeAliases?,typeHandlers?,
    objectFactory?,objectWrapperFactory?,reflectorFactory?,
    plugins?,environments?,databaseIdProvider?,mappers?
-->
<configuration>

    <properties resource="jdbc.properties"></properties>

    <settings>
        <!-- 自动将下划线映射为驼峰 -->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <typeAliases>
        <package name=""/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>

    <!--引入映射文件-->
    <mappers>
        <package name="com.cjj.ssm.mapper"/>
    </mappers>
</configuration>
```

### ③ 创建Mapper接口和映射文件

```java
public interface EmployeeMapper {

}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.cjj.ssm.mapper.EmployeeMapper">

</mapper>
```

### ④ 创建日志文件log4j.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8"/>
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS}%m (%F:%L) \n"/>
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug"/>
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info"/>
    </logger>
    <root>
        <level value="debug"/>
        <appender-ref ref="STDOUT"/>
    </root>
</log4j:configuration>
```

## 4.6 创建Spring的配置文件并配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd">

    <!--扫描组件（除控制层）-->
    <context:component-scan base-package="com.cjj.ssm">
        <!--通过注解的类路径排除指定注解下的组件-->
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>

    <!--引入jdbc.properties文件-->
    <!--通过类路径也就是war包里的classes路径-->
    <context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <!--配置数据源-->
    <bean id="DataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${jdbc.driver}"></property>
        <property name="url" value="${jdbc.url}"></property>
        <property name="username" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!--配置SqlSessionFactoryBean-->
    <!--工厂bean：可以从IOC容器中直接获取当前工厂bean所提供的对象:SqlSessionFactory-->
    <bean class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--设置Mybatis核心配置文件的路径-->
        <property name="configLocation" value="classpath:mybatis-config.xml"></property>
        <!--如果设置通过spring配置数据源，那么mybatis-config文件里就不需要配置DataSource了-->
        <property name="dataSource" ref="DataSource"></property>
        <!--设置实体类所对应的包，那么mybatis-config文件里就不需要配置<typeAliases>了-->
        <property name="typeAliasesPackage" value="com.cjj.ssm.pojo"></property>
        <!--设置映射文件的路径:只有当映射文件的包和mapper接口的包不一致时需要设置-->
        <!--<property name="mapperLocations" value="classpath:mappers/*.xml"></property>-->
        <!--配置插件-->
        <!--<property name="plugins">-->
        <!--    <array>-->
        <!--        <bean class="com.github.pagehelper.PageInterceptor"></bean>-->
        <!--    </array>-->
        <!--</property>-->
    </bean>

    <!--mapper接口扫描配置-->
    <!--
        扫描我们指定包下所有的mapper接口通过我们当前SqlSessionFactory所提供的SqlSession对象
        来创建这些mapper接口的代理实现类对象并且交给IOC容器管理。把所有细节和操作隐藏了，直接自动装配
        mapper接口对象即可。
    -->
    <!--为我们提供的是每一个mapper接口代理实现类对象-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--设置mybatis映射文件所对应的包-->
        <property name="basePackage" value="com.cjj.ssm.mapper"></property>
    </bean>

</beans>
```

## 4.7 测试功能

### ① 创建组件

实体类Employee

 ```java
package com.cjj.ssm.pojo;

public class Employee {

    private Integer empId;

    private String empName;

    private Integer age;

    private String gender;

    private String email;

    public Employee(Integer empId, String empName, Integer age, String gender, String email) {
        this.empId = empId;
        this.empName = empName;
        this.age = age;
        this.gender = gender;
        this.email = email;
    }

    public Employee() {
    }

    public Integer getEmpId() {
        return empId;
    }

    public void setEmpId(Integer empId) {
        this.empId = empId;
    }

    public String getEmpName() {
        return empName;
    }

    public void setEmpName(String empName) {
        this.empName = empName;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "empId=" + empId +
                ", empName='" + empName + '\'' +
                ", age=" + age +
                ", gender='" + gender + '\'' +
                ", email='" + email + '\'' +
                '}';
    }
}
 ```

创建控制层组件EmployeeController

```java
@Controller
public class EmployeeController {

    @Autowired
    private EmployeeService employeeService;

    // 查询所有员工信息
    @RequestMapping(value = "/employee",method = RequestMethod.GET)
    public String getAllEmployee(Model model){
        // 查询所有员工信息
        List<Employee> list = employeeService.getAllEmployee();
        // 将员工信息在请求域中共享
        model.addAttribute("list",list);
        // 跳转到employee_list.html
        return "employee_list";
    }

    // 查询员工的分页信息
    @RequestMapping(value = "/employee/page/{pageNum}",method = RequestMethod.GET)
    public String getEmployeePage(@PathVariable("pageNum") Integer pageNum,Model model){
        // 获取员工的分页信息
        PageInfo<Employee> page = employeeService.getEmployeePage(pageNum);
        System.out.println(page);
        // 将分页数据共享到请求域中
        model.addAttribute("page",page);
        // 跳转到employee_list.html
        return "employee_list";
    }

    // 根据id查询员工信息

    // 跳转到添加页面

    // 添加员工信息

    // 修改员工信息

    // 删除员工信息

}
```

创建接口EmployeeService

```java
public interface EmployeeService {

    /**
     * 获取所有员工信息
     * @return
     */
    public List<Employee> getAllEmployee();

    /**
     * 获取员工的分页信息
     * @return
     */
    PageInfo<Employee> getEmployeePage(Integer pageNum);
}
```

创建实现类EmployeeServiceImpl

```java
@Service
@Transactional
public class EmployeeServiceImpl implements EmployeeService {

    @Autowired
    private EmployeeMapper employeeMapper;

    @Override
    public List<Employee> getAllEmployee() {
        return employeeMapper.getAllEmployee();
    }

    @Override
    public PageInfo<Employee> getEmployeePage(Integer pageNum) {
        // 分页之前，开启分页功能
        PageHelper.startPage(pageNum,4); // pageNum:当前页数 pageSize:设置每页显示条数
        // 分页本质上是一个拦截器，会对我们的查询功能进行拦截，在下述方法的SQL语句中直接加入limit实现分页功能
        List<Employee> list = employeeMapper.getAllEmployee();
        // 获取分页相关数据
        PageInfo<Employee> page = new PageInfo<>(list,5); // navigatePages:导航分页
        return page;
    }
}
```

### ② 创建页面

employee_list.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http//www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>employee_list</title>
</head>
<body>
<table>
  <tr>
    <th colspan="6">员工列表</th>
  </tr>
  <tr>
    <th>序号</th>
    <th>员工姓名</th>
    <th>年龄</th>
    <th>性别</th>
    <th>邮箱</th>
    <th>操作</th>
  </tr>
<!--  <br>-->
  <tr th:each="employee,status : ${page.list}">
    <td th:text="${status.count}"></td>
    <td th:text="${employee.empName}"></td>
    <td th:text="${employee.age}"></td>
    <td th:text="${employee.gender}"></td>
    <td th:text="${employee.email}"></td>
    <td>
      <a href="">删除</a>
      <a href="">修改</a>
    </td>
<!--    <br>-->
  </tr>
</table>
<div style="text-align: center;">
  <a th:if="${page.hasPreviousPage}" th:href="@{/employee/page/1}">首页</a>
  <a th:if="${page.hasPreviousPage}" th:href="@{'/employee/page/'+${page.prePage}}">上一页</a>
  <span th:each="num : ${page.navigatepageNums}">
    <a th:if="${page.pageNum == num}" style="color:pink;" th:href="@{'/employee/page/'+${num}}" th:text="${num}"></a>
    <a th:if="${page.pageNum != num}"  th:href="@{'/employee/page/'+${num}}" th:text="${num}"></a>
  </span>
  <a th:if="${page.hasNextPage}" th:href="@{'/employee/page/'+${page.nextPage}}">下一页</a>
  <a th:if="${page.hasNextPage}" th:href="@{'/employee/page/'+${page.pages}}">末页</a>
</div>
</body>
</html>
```

index.html

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http//www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>index</title>
</head>
<body>
  <h1>index.html</h1>
  <a th:href="@{/employee}">查询所有员工信息</a>
  <a th:href="@{/employee/page/1}">查询员工分页信息</a>
</body>
</html>
```

