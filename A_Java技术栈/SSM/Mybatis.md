# Mybatis

![image-20221128130259762](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748204.png)

![image-20221128130304990](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748205.png)

## 一、快速入门

![image-20220620213139143](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748206.png)

### 1.  创建account表

![image-20220620213216468](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748207.png)

### 2. 创建模块，导入坐标

![image-20220620213352842](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748208.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>Mybatis-demo</artifactId>
    <version>1.0-SNAPSHOT</version>



    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- mybatis 依赖 -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>
        <!-- mysql 驱动 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.6</version>
        </dependency>
		<!-- junit 单元测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

</project>
```

### 3. 编写Mybatis核心配置文件

![image-20220620213457056](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748209.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:13306/db?useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 加载sql映射文件 -->
    <mappers>
        <mapper resource="AccountMapper.xml"/>
    </mappers>
</configuration>
```

### 4. 编写sql映射文件

![image-20220620213641907](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748210.png)

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
    namespace:名称空间
	id: id选择器
 -->
<mapper namespace="test">
    <select id="selectAll" resultType="com.cjj.pojo.Account">
        select * from account;
    </select>
</mapper>
```

### 5. 编码

![image-20220620213824520](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748211.png)

```java
package com.cjj;
import com.cjj.pojo.Account;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MybatisDemo {
    public static void main(String[] args) throws IOException {
        // 1.加载Mybatis核心配置文件，获取SqlSessionFactory对象
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new 			SqlSessionFactoryBuilder().build(inputStream);

        // 2.获取SqlSession对象，用它执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // 3.执行sql
        List<Account> accounts = sqlSession.selectList("test.selectAll");

        System.out.println(accounts);

        // 4.释放资源
        sqlSession.close();
    }
}
```

## 二、Mapper代理开发

![image-20220620230629019](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748212.png)

![image-20220620230718050](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748213.png)

![image-20220620230808002](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748214.png)

![image-20220620230831162](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251748215.png)

```java
package com.cjj;

import com.cjj.mapper.AccountMapper;
import com.cjj.pojo.Account;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class MybatisDemo2 {
    public static void main(String[] args) throws IOException {
        // 1.加载Mybatis核心配置文件，获取SqlSessionFactory对象
        String resource = "mybatis-config.xml";
        InputStream inputStream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

        // 2.获取SqlSession对象，用它执行sql
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // 3.执行sql
//        List<Account> accounts = sqlSession.selectList("test.selectAll");
        // 3.1 获取AccountMapper接口的代理对象
        AccountMapper AccountMapper = sqlSession.getMapper(AccountMapper.class);
        List<Account> accounts = AccountMapper.selectAll();

        System.out.println(accounts);

        // 4.释放资源
        sqlSession.close();
    }
}
```

## 三、案例

#### 1. 
