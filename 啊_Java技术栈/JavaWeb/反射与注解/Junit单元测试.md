# Junit单元测试

## 一、Junit使用

### 1. 定义一个测试类

- 测试类名：被测试的类名Test		`CalculatorTest`
- 包名：xxx.xxx.xx.test     `cn.itcast.test`

### 2. 定义测试方法：可以独立运行

- 方法名：test测试的方法名	`testAdd()`
- 返回值：void
- 参数列表：空参

### 3. 给方法加@Test注解

### 4. 导入junit相关包

- 判断结果
  - 红色：失败
  - 绿色：成功

```java
package cn.itcast.junit.Test;

import cn.itcast.junit.Calculator;
import org.junit.Assert;
import org.junit.Test;

// 测试add方法
public class CalculatorTest {
    @Test
    public void testAdd() {
        // 1. 创建计算器对象
        Calculator c = new Calculator();
        // 2. 调用方法
        int result = c.add(1, 2);
        System.out.println(result);
        // 3. 断言测试结果与预期是否相等
        Assert.assertEquals(2,result);
    }
}

```

![image-20220528154213730](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251705915.png)

### 5. 补充

> @Before 修饰的方法会在测试方法之前被执行
>
> @After 修饰的方法会在测试方法执行之后被执行

```java
package cn.itcast.junit.Test;

import cn.itcast.junit.Calculator;
import org.junit.After;
import org.junit.Assert;
import org.junit.Before;
import org.junit.Test;

// 测试add方法
public class CalculatorTest {
    @Before
    public void init(){
        System.out.println("init..");
    }
    
    @Test
    public void testAdd() {
        // 1. 创建计算器对象
        Calculator c = new Calculator();
        // 2. 调用方法
        int result = c.add(1, 2);
        System.out.println(result);
        // 3. 断言测试结果与预期是否相等
        Assert.assertEquals(2,result);
    }
    
    @After
    public void close(){
        System.out.println("close...");
    }
}

```

