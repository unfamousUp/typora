# Spring JDBC

> - Spring框架JDBC的简单封装，提供了一个JDBCTemplate对象简化JDBC的开发
>
> - 步骤
>
>   1. 导入jar包
>
>   2. 创建JdbcTemplate对象，数据源依赖于DataSource
>
>      `JdbcTemplate template = new DataSource(ds)`
>
>   3. 调用JdbcTemplate方法来完成CRUD的操作
>
>      - `update()`执行DML语句，增删改
>      - `queryForMap`查询结果集封装为map集合
>        - 只能查询一条记录
>      - `queryForList()`查询结果将结果集封装为list集合
>        - 将每一条记录封装为一条map集合，再讲map集合装载到list集合中
>      - `query()`查询结果，将结果封装为JavaBean对象
>        - 参数：`RowMapper` 接口
>          - 一般我们使用`BeanPropertyRowMapper`实现类，可以完成数据到JavaBean的自动封装
>   
>      List<Emp> list = template.query(sql, new BeanPropertyRowMapper<Emp> (Emp.class));
>   
>      - `queryForObject()`查询结果，将结果集封装为对象
>        - 聚合函数
>   
>   4. 练习
>   
>      - 需求
>        1. 修改emp表 1 号数据的 money 为 1234
>        2. 添加一条记录
>        3. 删除刚才添加的记录
>        4. 查询id为1的记录，并封装为map集合
>        5. 查询所有的记录，并封装为List
>        6. 查询所有的记录，封装为Emp对象的List集合
>        7. 查询总记录数