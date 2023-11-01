# swagger2

## 导入依赖

```xml
<!--swagger-->
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

## 创建SwaggerConfig配置类

```java
package com.chzu.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import springfox.documentation.builders.ApiInfoBuilder;
import springfox.documentation.builders.PathSelectors;
import springfox.documentation.builders.RequestHandlerSelectors;
import springfox.documentation.service.ApiInfo;
import springfox.documentation.service.Contact;
import springfox.documentation.spi.DocumentationType;
import springfox.documentation.spring.web.plugins.Docket;
import springfox.documentation.swagger2.annotations.EnableSwagger2;

@Configuration // 配置类
@EnableSwagger2 // 开启Swagger2
public class SwaggerConfig {

    /**
     * 创建API应用
     *
     * @return
     */
    @Bean
    public Docket restApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                // 定义一些我们项目的描述信息
                .apiInfo(apiInfo("SwaggerAPI文档", "1.0"))
                // 组名
                .groupName("标准接口")
                .useDefaultResponseMessages(true)
                .forCodeGeneration(false)
                // 返回一个ApiSelectorBuilder实例对象,用来控制哪些接口暴露给Swagger来展现
                .select()
                // 扫描接口层路径,指定扫描的包路径来定义指定要建立API的目录
                .apis(RequestHandlerSelectors.basePackage("com.cjj.boot.controller"))
                .paths(PathSelectors.any())
                .build();
    }

    /**
     * 创建该API的默认文档配置信息（这些基本信息会展现在文档页面中）
     * 访问地址：http://ip:port/swagger-ui.html
     *
     * @param title
     * @param version
     * @return
     */
    private ApiInfo apiInfo(String title, String version) {
        return new ApiInfoBuilder()
                // 标题
                .title(title)
                // 版本号
                .version(version)
                // 描述
                .description("NO GAME NO LIFE!")
                // 所属组织
                .termsOfServiceUrl("https://blog.csdn.net/xqnode")
                // 作者信息
                .contact(new Contact("chenjiujia", "", ""))
                // 开源版本号
                .license("Apache 2.0")
                //  官网
                .licenseUrl("http://www.apache.org/licemses/LICENSE-2.0")
                // 返回ApiInfo对象
                .build();
    }

}
```

## 编写控制层接口

```java
package com.chzu.controller;

import com.chzu.pojo.Book;
import io.swagger.annotations.Api;
import io.swagger.annotations.ApiOperation;
import io.swagger.annotations.ApiParam;
import org.springframework.web.bind.annotation.*;

@RestController
public class SwaggerController {

    @GetMapping("/hello")
    @ApiOperation("hello控制器方法") // 配置接口显示信息
    public String hello(){
        return "hello";
    }

    @PostMapping("/testPost")
    @ApiOperation("testPost控制器方法") // 配置接口显示信息
    // 如要使用form表单提交测试Post请求，则不需使用@RequestBody注解
    public Book testPost(@ApiParam("书籍对象")Book book){
        return book;
    }

    /**
     * 只要我们接口的返回值中存在实体类对象
     * 该实体类就会被扫描到Swagger中到页面显示
     * @return
     */
    @PostMapping("/book")
    @ApiOperation("Book控制器方法") // 配置接口显示信息
    public Book book(@RequestBody Book book){
        return book;
    }

}
```



