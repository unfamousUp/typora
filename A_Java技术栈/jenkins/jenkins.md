# Jenkins

## 一、Jenkins安装maven

- Jenkins面板安装Maven插件

- Linux下载maven包，解压，修改settings文件配置本地仓库和镜像源，配置环境变量

```bash
# 解压
tar -zxvf apache-maven-3.6.3-bin.tar.gz
# 进入/etc/profile 配置环境变量
vim /etc/profile 

export MAVEN_HOME=/maven/apache-maven-3.6.3
export JAVA_HOME=$JAVA8_HOME
export JAVA8_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.392.b08-2.el7_9.x86_64
export JAVA11_HOME=/usr/lib/jvm/java-11-openjdk-11.0.21.0.9-1.el7_9.x86_64
export CLASSPATH=.:$JAVA_HOME/jre/lib/rt.jar:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
export PATH=$PATH:$JAVA_HOME/bin:$MAVEN_HOME/bin
# 务必要保存profile文件修改
source /etc/profile
```

- 配置jdk相关信息

```bash
# 查看安装的jdk信息
yum list installed | grep java

# 安装java运行环境
sudo yum install java-11-openjdk-devel
sudo yum install java-1.8.0-openjdk-devel
```

