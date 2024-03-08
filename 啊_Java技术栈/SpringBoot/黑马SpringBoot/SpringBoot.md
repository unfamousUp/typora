> - @Value注解：标识在属性上，读取配置文件中的属性值注入到标识的属性上
>
>   ```java
>   @Value("name") // 注入字面量属性
>   private String name;
>   @Value("user.name") // 注入对象里属性
>   private String name;
>   ```
>
>   - 

# 整合第三方技术

## 1、整合JUnit

![image-20230705214431691](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251737411.png)

![image-20230705214447106](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251737412.png)

## 2、整合MyBatis

- 核心配置：数据库连接相关信息
  - 连哪个数据库
  - 连数据库中哪一个具体的库、地址、库名、端口号
  - 用什么权限连
- 映射配置：SQL映射
  - XML
  - 注解

- 导入mybatis-spring-boot-starter
- 导入mysql-connector-java
- 配置数据源

```yaml
spring:
  datasource: # 配置数据库连接
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:13306/chzu?serverTimezone=UTC
    username: root
    password: root
```

- 注解配置Mapper接口

```java
// 注解配置mapper接口后，数据库SQL映射才能被容器识别到，使用代理类帮我们封装好SQL语句并实现接口
@Mapper
public interface BookDao {

    @Select("select * from book where number = #{id}")
    public Book getById(Integer id);

}
```

## 3、整合MyBatis-Plus

- MyBatis-Plus与MyBatis区别
  - 导入坐标不同
  - 数据层实现简化
- 添加坐标场景

```xml
<!--MyBatis-Plus场景-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
</dependency>
```

- 定义Dao数据层接口与映射配置，继承BaseMapper

```java
// 注解配置mapper接口
@Mapper
public interface BookDao extends BaseMapper<Book> {

    // 使用MP就不需要手动写接口了
    // @Select("select * from book where number = #{id}")
    // public Book getById(Integer id);

}
```

- 测试

```java
@SpringBootTest
class ChzuSystemApplicationTests {

    @Autowired
    private BookDao bookDao;

    @Test
    void contextLoads() {
        /*
        	根据id查询，首先javabean得有id这个属性
        **/
        Book book = bookDao.selectById(101);
        System.out.println(book);
    }

}
```

## 4、整合Druid

```xml
<!--Druid数据源-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.6</version>
</dependency>
```

![image-20230705225307857](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251737413.png)

![image-20230705225339564](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251737414.png)

- 整合第三方技术总结
  - 导入对应的starter
  - 根据提供的配置格式，配置非默认值对应的配置项

# 基于SpringBoot的SSMP整合案例

