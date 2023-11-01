## SQL语法

### 1. 条件表达式

#### 1.1 CASE

**示例 1：基本的 `CASE` 表达式**

假设有一个 `students` 表，其中有一个 `score` 列，我们想根据不同的分数范围返回不同的等级：

```sql
SELECT
    student_name,
    score,
    CASE
        WHEN score >= 90 THEN 'A'
        WHEN score >= 80 THEN 'B'
        WHEN score >= 70 THEN 'C'
        WHEN score >= 60 THEN 'D'
        ELSE 'F'
    END AS grade
FROM students;
```

执行完这个查询后，数据库表中不会多出 "grade" 列。相反，查询的结果集中将包含一个名为 "grade" 的列，该列包含了根据分数条件计算得出的学生等级。这个计算的结果仅出现在查询结果中，不会影响数据库表本身的结构。

**示例 2：搜索 `CASE` 表达式**

假设有一个 `employees` 表，其中有一个 `job_title` 列，我们想为不同的职位设置不同的薪水系数：

```sql
SELECT
    employee_name,
    job_title,
    CASE job_title
        WHEN 'Manager' THEN salary * 1.2
        WHEN 'Developer' THEN salary * 1.1
        ELSE salary
    END AS adjusted_salary
FROM employees;
```

**示例 3：计数**

```sql
select area_resources_id machineRoomId,area_name machineRoomName,
count(*) alarmCount,
count(CASE WHEN event_level = 1 THEN 1 ELSE NULL END) urgentCount,
count(CASE WHEN event_level = 2 THEN 1 ELSE NULL END) importantCount,
count(CASE WHEN event_level = 3 THEN 1 ELSE NULL END) secondaryCount,
count(CASE WHEN event_level = 4 THEN 1 ELSE NULL END) warnCount
from omc_event_detail
group by area_resources_id, area_name
```

![image-20230919172223500](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251701182.png)

