# 第03章_基本的SELECT语句

## 1. SQL概述

### 1.1 SQL分类

- **DDL（Data Definition Languages）数据定义语言**。这些语句定义了不同的数据库、表、视图、索引等数据库对象，还可以用来创建、删除、修改数据库和数据表的结构。
  - 关键词包括 `CREATE、DROP、ALTER`等。
- **DML（Data Manipulation Language）数据操作语言**，用于增加、删除、更新和查询数据库记录，并检查数据完整性。
  - 关键字包括`INSERT、DELETE、UPDATE、SELECT`等。
  - **SELECT是SQL语言的基础，最为重要**
- **DCL（Data Control Language）数据控制语言**，用于定义数据库、表、字段、用户的访问权限和安全级别。
  - 关键字包括`GRANT、REVOKE、COMMIT、ROLLBACK、SAVEPOINT`等。
- **DQL（Data Query Language) 数据库查询语言**，用来查询数据库中的表的记录

### 1.2 SQL导入数据库脚本

- 方式一：source 路径
- 方式二：图形化界面直接导入

## 2. 基本的SELECT语句

### 2.1 SELECT...FROM...

- 语法

```MYSQL
SELECT 属性1，属性2...(列)
FROM 表名
```

- 选择employees表中的全部列

```mysql
SELECT *
FROM employees
```

### 2.2 AS（alias别名） 给列起别名

![image-20220519124546603](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251659288.png)

- 语法

```mysql
SELECT account AS "账户",PASSWORD AS "密码"
FROM USER;
```

![image-20220519124621327](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251659289.png)

### 2.3 去除重复行

![image-20220519220253468](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251659290.png)

- 语法

```mysql
SELECT DISTINCT 属性
FROM 表;
```

- 去除updata表中wid属性的重复行

```mysql
SELECT DISTINCT wid
FROM updata;
```

![image-20220519220304784](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251659291.png)

### 2.4 空值参与运算

![image-20220519222926429](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251659292.png)

- 空值：null
- null不等同于0，' '，'null'
- 空值参与运算，结果为null

```mysql
SELECT id "学号", name "姓名", salary + 100 "增加后的奖学金", salary "之前的奖学金"
FROM employees;
```

![image-20220519223129492](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251659293.png)

- **解决方案**
- 使用IFNULL判断该字段是否为NULL，如果为NULL则替换为0

```mysql
SELECT id "学号", name "姓名", IFNULL(salary,0) + 100 "增加后的奖学金", salary "之前的奖学金"
FROM employees;
```

![image-20220519224144089](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251659294.png)

### 2.5 where过滤数据

- WHERE声明在FROM表后面

> **查询指定学号的学生信息**

```mysql
SELECT stuNum
FROM student
WHERE stuNum=
```

