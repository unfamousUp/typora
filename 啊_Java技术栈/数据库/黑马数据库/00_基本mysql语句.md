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

## 1. 表的修改

```mysql
# 修改表名
alter table 表名 rename to 新表名;
# 增加一列
alter table 表名 add 列名 数据类型;
# 删除列
alter table 表名 drop 列名
# 修改列名称
alter table 表名 change 列名 新列名 新数据类型;
# 修改列数据类型
alter table 表名 modify 列名 新数据类型
```

