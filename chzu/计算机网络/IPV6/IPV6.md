# IPV6

## 一、IPV6基础

### 1. IPV6地址

![image-20231102103626079](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311021036402.png)

> - 每段4个16进制数字，每个16进制数字可以用4个二进制位来表示。
>   - ![](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311021041598.png)

#### 1.1 地址的缩写

![image-20231102104258102](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311021042317.png) 

#### 1.2 IPV6地址的划分

![image-20231102143409631](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311021434917.png)

#### 1.3 同一网络（同一链路）

> - 网络地址：网络前缀
>   - 2001:0000:0000:0000
> - 主机地址
>   - WEB：2001:0000:0000:0000:`0000:0000:0000:0001`
>     - 2001网络的第一台主机...
>   - PC0：2001:0000:0000:0000:`0000:0000:0000:0002`
>   - PC1：2001:0000:0000:0000:`0000:0000:0000:0003`
> - 同一局域网，主机通信
>   - PC0可以ping通WEB服务器：`ping 2001::1`
>   - PC0可以访问WEB服务器：`http://2001::1`

![](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311021440369.png)

#### 1.4 IPV6地址的种类

> - 特殊地址
>   - `::/128`：所有位均为网络前缀，多用来表示未指定IPV6地址的设备
>   - `::1/128`

![image-20231102145722989](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311021457183.png)