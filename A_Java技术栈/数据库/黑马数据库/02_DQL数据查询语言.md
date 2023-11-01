# 02_DQL数据查询语言

## 1. 语法

``` mysql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组列表
having
	分组之后的条件
order by
	排序
limit
	分页限定
```

## 2. 基础查询

> 1. 多个字段的查询
> 2. 去除重复
> 3. 计算列
> 4. 条件查询

## 3. 条件查询

> 1. where子句后跟条件
>
> 2. 运算符
>
>    - <、>、<=、>=、=、<>
>
>    - BETWEEN...AND
>    - IN(集合)
>    - LIKE 模糊查询
>      - 占位符
>        - _：单个任意字符
>        - %：任意多个字符
>    - IS NULL
>    - add 或 &&
>    - or 或 ||
>    - not 或 !
>
> ```mysql
> # BETWEEN AND
> SELECT * FROM student WHERE age BETWEEN 20 AND 30;
> # AND
> SELECT * FROM student WHERE age >= 20 AND age <= 30;
> # OR
> SELECT * FROM student WHERE age = 20 OR age = 30;
> # IN
> SELECT * FROM student WHERE age IN (20,30);
> # IS NULL
> SELECT * FROM student WHERE age IS NULL
> # 查询姓陈的同学
> SELECT * FROM student WHERE name LIKE "陈%"
> ```

## 4. DQL查询语句

### 1. 排序查询

- 语法

  - ``` mysql
    order by 子句;
    order by 排序字段1 排序方式1, 排序字段2 排序方式2...
    ```

- 排序方式

  - ASC：升序，默认
  - DESC：降序

- 注意：如果有多个排序条件，则当前面条件值一样时，才会判断第二条件

![image-20220603165430747](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656704.png)

```mysql
# 按数学成绩降序排序
SELECT * FROM student ORDER BY Math DESC;
```

![image-20220603165627584](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656705.png)

### 2. 聚合函数

> 主要做列的纵向计算，如计算student表中所有学生Math字段的平均分
>
> - 注意：聚合函数的计算排除了NULL值
>   - 一般选择非空的列
>   - count(*)

1. count：计算个数

   ```mysql
   # 查询student表中的学生人数
   SELECT COUNT(id) FROM student;
   # 如果id记录为NULL值解决方案
   SELECT COUNT(IFNULL(id, 0)) FROM student;
   ```

   ![image-20220603170754113](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656706.png)

2. max：计算最大值

   ```mysql
   # 查询数学最高分
   SELECT MAX(Math) FROM student;
   ```

3. min：计算最小值

   ```mysql
   # 查询数学最低分
   SELECT MIN(Math) FROM student;
   ```

4. sum：计算和

   ```mysql
   # 查询数学成绩总分
   SELECT SUM(Math) FROM student;
   ```

5. avg：计算平均值

   ```mysql
   # 查询数学成绩平均分
   SELECT AVG(Math) FROM student;
   ```

### 3. 分组查询

> 比如以sex字段分成男女两组，再分别对这两组整体进行聚合函数运算
>
> 1. 语法：group by 分组字段;
> 2. 注意：
>    1. 分组之后SELECT查询的字段：分组字段、聚合函数
>    2. where 和 having的区别
>       - where在分组之前进行限定，如果不满足条件，则不参与分组
>       - having在分组之后进行限定，如果不满足条件，则不会被查询出来
>    3. where后不可跟聚合函数，having可以进行聚合函数的判断

![image-20220603172241457](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656707.png)

```mysql
# 按sex性别分组求各组平均数学成绩
SELECT sex, AVG(Math) FROM student GROUP BY sex;
```

![image-20220603172427554](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656708.png)

```mysql
# 按sex性别分组，分别查询男、女同学的Math平均分和人数，并且要求分数低于70的人不参与分组，分组之后，人数要大于2人
SELECT sex, AVG(Math), COUNT(id) AS 人数 # 起别名
FROM student
WHERE Math >= 70 
GROUP BY sex HAVING 人数 > 2;
```

![image-20220603175850981](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656709.png)

### 4. 分页查询

> - 语法：`limit 开始的索引号,每页查询记录的条数;`
> - 公式：`开始的索引 = (当前的页码 - 1) * 每页显示记录的条数`
> - 分页操作是一个"方言"，只有Mysql特有的分页方式

```mysql
# 每页显示三条记录,共两页
SELECT * FROM student LIMIT 0,3; # 从索引号为0,即第1条记录开始查询
SELECT * FROM student LIMIT 3,3; # 从索引号为3,即第4条记录开始查询
```

![image-20220603180829041](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656710.png)

![image-20220603180839835](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251656711.png)