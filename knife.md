## Knife

1. 导入依赖

```pom
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-spring-boot-starter</artifactId>
    <version>2.0.9</version>
</dependency>
```

2. SwaggerConfig配置类

```java
@Configuration
@EnableSwagger2WebMvc
public class SwaggerConfig {
    // 创建Docket存入容器，Docket代表一个接口文档
    @Bean
    public Docket webApiConfig(){
        return new Docket(DocumentationType.SWAGGER_2)
                // 创建接口文档的具体信息
                .apiInfo(webApiInfo())
                // 创建选择器，控制哪些接口被加入文档
                .select()
                // 指定@ApiOperation标注的接口被加入文档
                .apis(RequestHandlerSelectors.withMethodAnnotation(ApiOperation.class))
                .build();
    }

    // 创建接口文档的具体信息，会显示在接口文档页面中
    private ApiInfo webApiInfo(){
        return new ApiInfoBuilder()
                // 文档标题
                .title("标题：用户管理系统接口文档")
                // 文档描述
                .description("描述：本文档描述了用户管理系统的接口定义")
                // 版本
                .version("1.0")
                // 联系人信息
                .contact(new Contact("baobao", "http://baobao.com", "baobao@qq.com"))
                // 版权
                .license("baobao")
                // 版权地址
                .licenseUrl("http://www.baobao.com")
                .build();
    }
}
```

## bug

![image-20231114121959391](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311141219500.png)