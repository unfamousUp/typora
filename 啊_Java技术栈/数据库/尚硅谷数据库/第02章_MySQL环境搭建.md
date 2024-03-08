# 第02章_MySQL环境搭建

___

## 1. MySQL的卸载

## 2. MySQL的下载、安装、配置

___

## 3. MySQL的登陆

### 3.1 服务的启动与停止

MySQL安装完毕之后，需要启动服务器进程，不然客户端无法连接数据库

**方式一：使用图形界面工具**

- 步骤1：打开windows服务
  - 方式1：右键计算机 --> 管理 --> 服务
  - 方式2：控制面板 --> 系统和安全 --> 管理 --> 服务
  - 方式3：任务管理器 --> 服务
  - 方式4： 开始 --> 搜索 "services.msc"

- 步骤2：右键MySQL服务停止/启动

**方式二：使用命令行工具**

```mysql
# 启动 MySQL 服务命令
net start MySQL服务名

# 停止 MySQL 服务命令
net stop MySQL服务名
```

### 3.2 自带客户端的登录与退出

当MySQL服务启动完成后，便可通过客户端来登录MySQL数据库

#### **登录方式1：MySQL自带客户端**

开始菜单 --> ==MySQL Command Line Client==

```mysql
mysql -uroot -p
enter password: root
```

#### 登录方式2：windows命令行

```mysql
# mysql -uroot -P端口号 -h地址 -p
mysql -uroot -P 3306 -h localhost -p # 本机地址 端口号为3306的数据库服务器
enter password: root
```

___

## 4. MySQL演示使用

### 4.1 MySQL的使用演示

1、查看所有的数据库

```mysql
show databases;
# 查询某个数据库   
```

2、创建自己的数据库

```mysql
create database 数据库名;
# 创建数据库，判断不存在，再创建
create database if not exists 数据库名称;
# 创建数据库，并指定字符集
create database 数据库名称 character set 字符集名称;
```

3、使用自己的数据库

```mysql
use 数据库名;
```

4、查看某个库中的所有表格

```mysql
show tables; #前面要有use语句

show tables from 数据库名;
```

5、创建新的表格

```mysql
create table 表名(
	列名1 数据类型1,
    列名2 数据类型
);
```

```mysql
# 创建student学生表
create table Student {
	id int,
	name varchar(32),
	age int,
	score double(4,1)
	birthday date,
	insert_time timestamp
}
```

6、查看表中数据

```mysql
select * from 数据库表名;
```

7、增加一条记录

```mysql
insert into 表名 values(值列表)
```

8、查看表的创建信息

```mysql
show create table 表名;
```

9、查看数据库的创建信息

```mysql
show create database 数据库名;
```

10、删除表格

```mysql
drop table 表名;
```

11、删除数据库

```mysql
drop database 数据库名
```

12、显示表的结构

```mysql
DESCRIBE 表名;
DESC 表名
```



### 4.2 MySQL的编码设置

#### MySQL5.7中

**问题再现**：命令行操作sql乱码问题

```mysql
# 在employees表中插入一条记录
mysql> insert into employees values(2020211466, '贾成程');
ERROR 1366 (HY000): Incorrect string value: '\xBC\xD6\xB3\xC9\xB3\xCC' for column 'name' at row 1
```

**问题解决**

步骤1：查看编码命令

```mysql
show variables like 'character_%';
show variables like 'collation_%';
```

步骤2：修改mysql的数据目录下的my.ini配置文件

```ini
[mysql]  #大概在63行下添加
...
default-character-set=utf8 #默认字符集

[mysqld] # 大概在76行下添加
...
character-set-server=utf8
collation-server=utf8_general_ci
```

>建议使用notepad++等高级文本编辑器修改

#### 