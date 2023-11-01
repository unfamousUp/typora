# 学生学籍管理系统

# 一、要求

设计题目五：学生学籍管理系统
设计内容：
调查所在学校学生处、教务处，设计本校学籍管理系统。要求：
① 建立学生档案，设计学生入学、管理及查询界面。
② 设计学生各学期、学年成绩输入及查询界面，并打印各项报表。
③ 根据各年度总成绩，查询、输出学生学籍管理方案（优秀、合格、试读、退学）。
④ 毕业管理。
⑤ 系统维护。
学生学习预期成果：
软件（系统）、设计报告（或说明书）、设计总结。

# 二、需求

### 1. 数据库表

| 数据库表名   | 关系模式名称 | 备注           |
| ------------ | ------------ | -------------- |
| Student      | 学生         | 学生学籍信息表 |
| Course       | 课程         | 课程基本信息表 |
| selectKe     | 选修课程成绩 | 选课成绩信息表 |
| teacher      | 教师         | 教师信息表     |
| Class        | 班级表       | 教师开课信息表 |
| studentTable | 学生账号     | 学生账号表     |



### 2. Student表

| 字段名     | 字段类型    | Not Null             | 说明        |
| ---------- | ----------- | -------------------- | ----------- |
| sNum       | varchar(20) | PRIMARY KEY NOT NULL | 学号 主键   |
| sName      | varchar(20) | NOT NULL             | 姓名        |
| sAge       | varchar(20) | NOT NULL             | 年龄        |
| //classNum | varchar(20) | NOT NULL             | 班级号      |
| CNum       | varchar(20) | NOT NULL             | 课程号 外键 |

### 3. Class班级表

| 字段名  | 字段类型    | Not Null | 说明        |
| ------- | ----------- | -------- | ----------- |
| CNum    | varchar(20) | NOT NULL | 班级号 主键 |
| CName   | varchar(20) | NOT NULL | 班级名称    |
| // DNum | varchar(20) | NOT NULL | 院系号      |

### //4.Department院系表

| 字段名 | 字段类型    | Not Null | 说明     |
| ------ | ----------- | -------- | -------- |
| DNum   | varchar(20) | NOT NULL | 院系号   |
| DName  | varchar(20) | NOT NULL | 院系名称 |

### 5.Teacher教师表

| 字段名 | 字段类型    | Not Null             | 说明     |
| ------ | ----------- | -------------------- | -------- |
| TNum   | varchar(20) | PRIMARY KEY NOT NULL | 教师号   |
| TName  | varchar(20) | NOT NULL             | 教师姓名 |
| //DNum | varchar(20) | NOT NULL             | 所属院系 |

### 6.User表

| 字段名    | 字段类型    | Not Null | 说明     |
| --------- | ----------- | -------- | -------- |
| UId       | varchar(20) | NOT NULL | 用户id   |
| UName     | varchar(20) | NOT NULL | 用户名   |
| UPassword | varchar(20) | NOT NULL | 用户密码 |

### 7.Course表

| 字段名 | 字段类型    | Not Null             | 说明      |
| ------ | ----------- | -------------------- | --------- |
| CoNum  | varchar(20) | PRIMARY KEY NOT NULL | 课程号    |
| CoName | varchar(20) | NOT NULL             | 课程名    |
| SNo    | varchar(20) |                      | 学号 外键 |

### 8.Score表

| 字段名 | 字段类型    | Not Null             | 说明   |
| ------ | ----------- | -------------------- | ------ |
| Id     | varchar(20) | PRIMARY KEY NOT NULL |        |
| SNum   |             |                      | 学号   |
| CoNum  |             |                      | 课程号 |
| Grade  |             |                      | 成绩   |

