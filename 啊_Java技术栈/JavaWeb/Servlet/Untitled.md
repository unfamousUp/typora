# Servlet

## 一、快速入门

![image-20220621231738533](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719268.png)

## 二、Servlet执行流程

![image-20220621234937489](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719269.png)

## 三、Servlet生命周期

![image-20220622000339816](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719270.png)

## 四、Servlet体系结构

![image-20220622002157646](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719271.png)

![image-20220622002238325](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719272.png)

![image-20220622002315236](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719273.png)

## 五、Servlet urlPattern配置

![image-20220622010857546](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719274.png)

![image-20220622010906950](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719275.png)

## 六、XML配置方式编写Servlet

![image-20220622011117960](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719276.png)

# Request（请求）

## 一、Requset继承体系

![image-20220622012727064](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719277.png)

## 二、Requset获取请求数据

![image-20220622015241557](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719278.png)

![image-20220622023107408](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719279.png)

![image-20220622134648511](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719280.png)

## 三、Request请求转发

![image-20220622140148220](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719281.png)

# Response（响应）

## 一、Response设置响应数据功能介绍

![image-20220622143058647](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719282.png)

## 二、Response完成重定向

·![image-20220622145435917](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719283.png)

![image-20220622145450903](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719284.png)

## 三、Response响应字符数据

![image-20220622152736917](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719285.png)

## 四、Response响应字节数据

![image-20220622153135431](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719286.png)

# 案例

> request-demo

## 1.SqlSessionFactory 代码优化

![image-20220622172130038](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251719287.png)

```java
package com.cjj.utils;

import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;

import java.io.IOException;
import java.io.InputStream;

public class SqlSessionFactoryUtils {
    private static SqlSessionFactory sqlSessionFactory;
    static {
        // 静态代码块会随着类的加载而自动执行，且执行一次
        try {
            String resource = "mybatis-config.xml";
            InputStream inputStream = Resources.getResourceAsStream(resource);
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    public static SqlSessionFactory getSqlSessionFactory(){
        return sqlSessionFactory;
    }
}
```

