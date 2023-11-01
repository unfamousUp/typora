# SSM整合

## 一、SSM整合

### 1. 整合流程

![image-20221205222715338](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746232.png)

## 二、表现层数据封装

![image-20221205223312084](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746233.png)

### 1. 设置统一数据返回结果类

![image-20221205223334486](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746234.png)

### 2. 设置统一数据返回结果编码

![image-20221205230901010](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746236.png)

### 3. 根据情况设定合理的Result

![image-20221205230904220](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746237.png)

## 三、异常处理器

![image-20221205231958139](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746238.png)

![image-20221205232208125](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746239.png)

### 1. 常见的异常

![image-20221205232001651](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746240.png)

### 2. 异常处理器

![image-20221205232130458](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746241.png)

### 3. @RestControllerAdvice

![image-20221205232142162](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746242.png)

### 4. @ExceptionHandler

![image-20221205232147601](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746243.png)

## 四、项目异常处理方案

### 1. 项目异常分类

![image-20221205233332174](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746244.png)

### 2. 项目异常处理方案

![image-20221205233339656](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746245.png)

![image-20221205233420678](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746246.png)

![image-20221205233426801](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746247.png)

![image-20221205233434907](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746248.png)

![image-20221205233448378](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746249.png)

![image-20221205233451265](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746250.png)

![image-20221205233459831](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746251.png)

## 案例：SSM整合标准开发

## 五、拦截器

### 1. 拦截器概念

![image-20221213204719073](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746252.png)

![image-20221213204723039](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746253.png)

![image-20221213204726682](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746254.png)

### 2. 入门案例

#### 2.1 制作拦截器功能类

![image-20221213215208047](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746255.png)

![image-20221213215224579](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746256.png)

#### 2.2 配置拦截器的执行位置

![image-20221213215246420](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746257.png)

![image-20221213215252276](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746258.png)

![image-20221213215256551](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746259.png)

### 3. 拦截器参数

![image-20221213215615766](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746260.png)

![image-20221213215618810](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746261.png)

![image-20221213215622064](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251746262.png)

### 4. 拦截器工作流程分析