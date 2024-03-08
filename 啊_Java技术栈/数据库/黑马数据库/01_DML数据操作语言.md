# 01_DML数据操作语言

## 1. 添加数据

> - 语法
>   - `insert into 表名(列名1,列名2,...,列名n) values(值1,值2,...,值n)`
>
>   - `insert into 表名 values(值1,值2,...,值n)`
>
>     ```mysql
>     INSERT INTO student(id, NAME, age, score) VALUES(1, '陈久佳', 18, 91)
>         
>     INSERT INTO student VALUES(2, '徐天天', 18, 91, NULL);
>     ```
>
>     ![image-20220603154836800](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251655101.png)

## 2. 删除数据

> - 语法
>
>   - `delete from 表名 [where 条件]` 
>   - `delete from 表名` 删除全部记录，不推荐使用
>   - `TRUNCATE TABLE 表名` 删除全部数据，先删除表，在创建一张一样的空表
>
>   ```mysql
>   # 删除id为1的记录
>   DELETE FROM student WHERE id=1;
>   ```
>
>   ![image-20220603155721931](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251655102.png)
>
>   ```mysql
>   # 删除表中所有记录
>   TRUNCATE TABLE student;
>   ```
>
>   ![image-20220603155837908](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251655103.png)

## 3. 修改数据

> - 语法
>
>   - `update 表名 set 列名1 = 值1, 列名2 = 值2,... [where 条件]`
>
>   ```mysql
>   # 修改id=1记录的age=2
>   UPDATE student SET age = 20 WHERE (id = 1);
>   ```
>
>   ![image-20220603160331494](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251655104.png)
>
> - 注意：如果不加where则会将表中所有记录修改