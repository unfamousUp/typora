![img](file:///G:\文件\QQ\消息记录\510618293\Image\C2C\UZKRU0DO29}$0{CGHGW1840.png)

## 一、

![image-20230325100103889](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230325100103889.png)

> - 应用层
>   - 位于不同主机中的**多个应用进程**之间进行通信和协同工作
> - 传输层
>   - 两个主机中**进程之间**的通信提供通用的数据传输服务
> - 网络层
>   - **分组**怎样从一个网络通过路由器转发到另一个网络
> - 数据链路层
>   - 在同一个局域网中，分组怎样从一个主机传送到另一个主机
>   - 数据帧（封装成帧，透明传输，差错检测）
> - 物理层
>   - 考虑怎样才能在连接各种计算的传输媒体（同轴电缆，双绞线）上传输数据比特流

## 二、保证信息机密性、完整性、不可否认性使用的技术

> 密码学技术：使用加密技术可以将明文信息转化为密文，从而保证信息的机密性。
>
> 报文摘要：使用哈希函数可以生成一个固定长度的“摘要”，这样就可以确定原始数据是否被篡改，从而保证数据的完整性。
>
> 数字签名：使用公钥加密和私钥解密的方式来验证信息来自何处、是否已被经过了人为干扰；这样就能够保证不可否认性。

## 三、

### 1.交换机分组转发![image-20230325100551651](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230325100551651.png)

### 2.4 广播带来的危害、Vlan技术的应用

> 广播是一种在局域网中广泛使用的通信方式，它能够让一台主机向所有其他主机发送消息。然而，广播也会带来一些危害，包括：
>
> 1. 网络拥塞：当网络中的广播流量过大时，会导致网络拥塞，影响网络性能。
> 2. 安全威胁：攻击者可以利用广播功能，向整个局域网中发送恶意广播消息，从而发起拒绝服务攻击（DoS）或窃取信息。

> 为了缓解广播带来的危害，网络管理员可以使用VLAN技术，将局域网分割成多个虚拟网段，从而限制广播的范围，提高网络的安全性和性能。VLAN技术可以将多个物理交换机划分成多个虚拟网段，不同的虚拟网段之间相互隔离，各自形成独立的广播域。这样，一个广播消息只会在同一个虚拟网段内传播，不会影响到其他虚拟网段。

## 四、ARP协议

