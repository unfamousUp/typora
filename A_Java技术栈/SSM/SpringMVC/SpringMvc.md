# SpringMVC

## 一、SpringMVC简介

### 1. SpringMVC概述

> SpringMVC是一种基于JAVA实现MVC模型的轻量级Web框架（表现层框架）
>
> 1. 页面发送的请求由`表现层`接收
> 2. 获取用户的请求参数后，传递到`业务层`
> 3. 再有业务层访问`数据层`
> 4. 得到用户需要访问的数据后，将数据返回给表现层
> 5. 表现层拿到数据后，将数据转换成`JSON`格式发送给前端页面
> 6. 前端页面接收到数据后，解析数据，并组织成用户浏览的信息交给浏览器

![image-20221201192222810](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744319.png)

### 2. 入门案例

#### 2.1 导入坐标

![image-20221201204201620](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744320.png)

#### 2.2 创建SpringMVC控制器类

![image-20221201204156067](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744321.png)

#### 2.3 初始化SpringMVC环境

![image-20221201204150700](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744322.png)

#### 2.4 初始化Servlet容器，加载SpringMVC环境

![image-20221201212030402](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744323.png)

#### 2.5 相关注解

![image-20221201212053997](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744324.png)

![image-20221201212100922](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744325.png)

![image-20221201212111787](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744326.png)

#### 2.6 总结

![image-20221201212343717](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744327.png)

![image-20221201212351821](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744328.png)

![image-20221201212401577](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744329.png)

![image-20221201212407906](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744330.png)

### 3. 入门案例工作流程分析

#### 3.1 启动服务器初始化过程

![image-20221201212822009](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744331.png)

#### 3.2 单次请求工作流程

![image-20221201212825950](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744332.png)

### 4. Controller加载控制

![image-20221201221332052](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744333.png)

![image-20221201221345406](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744334.png)

![image-20221201221339377](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744335.png)

### 5. PostMan(邮差)

#### 5.1  简介

![image-20221201221932849](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744336.png)

#### 5.2  基本使用

![image-20221201221937307](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744337.png)

## 二、请求与响应

### 1. 请求映射路径

![image-20221201231207615](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744338.png)

### 2. 请求参数

#### 2.1 GET

![image-20221201235125977](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744339.png)

#### 2.2 POST

![image-20221201235130685](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744340.png)

#### 2.3 POST请求乱码问题

![image-20221201235148083](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744341.png)

#### 2.4 五种类型参数传递

##### 2.4.1 普通参数

![image-20221202141904431](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744342.png)

![image-20221202141914137](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744343.png)

##### 2.4.2 POJO参数

![image-20221202141929266](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744344.png)

##### 2.4.3 嵌套POJO类型参数

![image-20221202141937037](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744345.png)

##### 2.4.4 数组类型参数

![image-20221202141943282](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744346.png)

##### 2.4.5 集合类型参数

![image-20221202141954143](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744347.png)

#### 2.5 传递Json数据

##### 2.5.1 步骤

![image-20221202150642118](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744348.png)

![image-20221202150646143](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744349.png)

![image-20221202150650267](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744350.png)

![image-20221202150654652](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744351.png)

##### 2.5.2 相关注解

![image-20221202150809428](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744352.png)

![image-20221202150813278](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744353.png)

##### 2.5.3 相关参数

![image-20221202151007197](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744354.png)

![image-20221202150949854](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744356.png)

##### 2.5.4 @RequestBody和@RequestParam区别

![image-20221202151614700](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744357.png)

### 3. 日期类型参数传递

#### 3.1 参数传递

![image-20221202152914335](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744358.png)

#### 3.2 相关注解

![image-20221202152925613](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744359.png)

#### 3.3 内部如何工作

![image-20221202153141270](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744360.png)

### 4. 响应json数据

#### 4.1 响应页面

![image-20221202154059864](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744361.png)

#### 4.2 响应文本信息

![image-20221202154105843](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744362.png)

#### 4.3 响应json数据

![image-20221202154112368](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744363.png)

![image-20221202154135031](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744364.png)

#### 4.4 @ResponseBody

> 激活该注解会帮我们自动转换数据的类型，把POJO转成JSON对象

<img src="https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744365.png" alt="image-20221202154457939" style="zoom:80%;" />

![image-20221202154504364](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744366.png)

## 三、REST风格

### 1. Rest简介

![image-20221202155120265](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744367.png)

![image-20221202155127020](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744368.png)

### 2. RESTful入门案例

#### 2.1 步骤

![image-20221202170249342](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744369.png)

![image-20221202170254340](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744370.png)

#### 2.2 @RequsetMapping

![image-20221202170503323](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744371.png)

#### 2.3 @PathVariable

![image-20221202170508587](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744372.png)

#### 2.4 总结

![image-20221202170514165](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744373.png)

### 3. REST快速开发

#### 3.1 @RestController

![image-20221202182743722](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744374.png)

#### 3.2 RESTful相关方法注解

![image-20221202182758685](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744375.png)

### 4. 案例：基于RESTful页面数据交互

![image-20221202190215358](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744376.png)

![image-20221202190218922](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744377.png)

![image-20221202190223199](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251744378.png)

## 四、SSM整合

## 五、拦截器