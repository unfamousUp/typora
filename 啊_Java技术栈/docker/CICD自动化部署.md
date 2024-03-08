# CI/CD

## Jenkins自动化部署

### 1. 安装java环境

```bash
yum install java-1.8.0-openjdk.x86_64
```

### 2. 安装Jenkins

```bash
curl -o /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

```bash
# 导入GPG秘钥保证软件合法
rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
```

```bash
[jenkins]
name=Jenkins-stable
baseurl=http://pkg.jenkins.io/redhat
gpgcheck=1
```

```bash
sudo sed -i 's|https://pkg.jenkins.io|https://pkg.jenkins.io/redhat-stable|' /etc/yum.repos.d/jenkins.repo
```

![image-20231129163605519](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311291636736.png)

```bash
rpm -ql java-1.8.0-openjdk

# 
find / -iname jenkins
```

![image-20231201110842898](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202312011108983.png)

### 3. nginx

![image-20231130112841457](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202311301128532.png)

# Bug报错

1. yum install jenkins 无法安装

> jenkins-2.426.1-1.1.noarch.rpm 的[公钥](https://so.csdn.net/so/search?q=公钥&spm=1001.2101.3001.7020)没有安装
> 下载的[软件包](https://so.csdn.net/so/search?q=软件包&spm=1001.2101.3001.7020)保存在缓存中，直到下次成功执行事务。
> 您可以通过执行 'yum clean packages' 删除软件包缓存。

```
export JAVA_HOME=$JAVA8_HOME
export JAVA8_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.392.b08-2.el7_9.x86_64
export JAVA11_HOME=/usr/lib/jvm/java-11-openjdk-11.0.21.0.9-1.el7_9.x86_64
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin
```

