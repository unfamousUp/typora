# HTTP

## 一、概念

- HyperText Transfer Protocol 超文本传输协议，规定了客户端浏览器和服务器之间数据传输的规则（格式）

## 二、HTTP协议特点

1. 基于TCP协：面向连接、安全
2. 基于请求-响应模型的：一次请求对应一次响应
3. HTTP协议是无状态的协议：对于事务处理没有记忆能力，每次请求-响应都是独立的
   - 缺点：多次请求间不能共享数据，java中使用会话技术（Cookie、Session)来解决这个问题
   - 优点：速度快

## 三、HTTP-请求数据格式

![image-20220608173403622](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251715054.png)

## 四、HTTP-响应数据格式

