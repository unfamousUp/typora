# JDBC

## 一、JDBC简介

### 1. JDBC概念

- JDBC就是使用Java语言操作关系型数据库的一套API接口
- 全称：（**J**ava **D**ata**B**ase **C**onnectivity）Java 数据库连接

### 2. JDBC本质

- sun公司定义的一套操作所有关系型数据库的规则，即接口
- 各个数据库厂商去实现这套接口，提供数据库驱动jar包
- 我们可以使用这套接口（JDBC）编程，真正执行的代码是驱动jar包中的实现类

> 例如
>
> Person接口 worker实现类
>
> Person p = new Worker();
>
> p.eat(); 调用的是 worker这个实现类里的方法

## 二、快速入门

1. 导入驱动jar包
2. 注册驱动
3. 获取数据库连接对象（Connection）
4. 定义sql
5. 获取执行sql语句的对象 Statement
6. 执行sql，接受返回的结果
7. 处理结果
8. 释放资源

```java
package cn.cjj.jdbc;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;
import java.sql.Statement;

public class JdbcDemo1 {
    public static void main(String[] args) throws Exception {
        // 1.导入驱动jar包
        // 2.注册驱动
        Class.forName("com.mysql.jdbc.Driver"); // 将该字节码文件加载进内存，返回class对象
        // 3.获取数据库的连接对象
        Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:13306/db1", 			"root", "root");
        // 4.定义sql语句
        String sql = "update account set money = 500 where id = 1";
        // 5.获取执行sql的对象 Statement
        Statement stmt = conn.createStatement();
        // 6.执行sql
        int count = stmt.executeUpdate(sql);
        // 7.处理结果
        System.out.println(count);
        // 8.释放资源
        stmt.close();
        conn.close();
    }
}
```

## 三、详解JDBC各个对象

> 1. #### **DriverManager：驱动管理对象（是个类）**
>
>    - 功能
>
>      1. 注册驱动：告诉程序该使用哪一个数据库驱动jar包
>
>         ```java
>         // DriverManager类的一个方法
>         static void registerDriver(Driver driver) // 注册与给定的驱动程序
>
>         // 实际使用以下代码注册驱动
>         Class.forName("com.mysql.jdbc.Driver"); 
>
>         // 查看Driver类原码发现：在com.mysql.jdbc.Driver类中存在静态代码块（在内存中自动执行）
>         static {
>             try {
>                 DriverManager.registerDriver(new Driver()); // 注册驱动
>             } catch (SQLException var1) {
>                 throw new RuntimeException("Can't register driver!");
>             }
>         }
>         ```
>
>         > - 综上所述，Class.forName("com.mysql.jdbc.Driver") 获取了Driver类的字节码文件，加载到了内存中，并执行了static静态代码块里的方法注册驱动。
>         > - 注意：mysql5之后的驱动jar包可以省略注册驱动步骤
>
>      2. 获取数据库连接
>
>         ```java
>         // 方法 （静态方法可以通过类名（ManagerDriver.getConnection）直接调用）
>         static Connection getConnection(String url, String user, String password)
>         // 参数
>         url:指定连接的路径
>             - jdbc:mysql://ip地址(域名):端口号/数据库名称
>         	- 例子：jdbc:mysql://localhost:13306/db1
>         	- 细节：如果连接的是本机的mysql服务器，并且mysql服务默认端口是3306则，url可以简写为
>                    jdbc:mysql:///数据库名称
>         user:用户名
>         password:密码
>         ```
>
> 
>
> 2. #### **Connection：数据库连接对象（接口）**
>
>    - 功能（提供的方法）
>
>      ```java
>      1. 获取执行sql的对象
>      // 方法1 获取执行sql的对象
>      Statement createStatement() // 返回一个Statement对象
>      // 方法2 用于将参数化的SQL语句发送到数据库。
>      PreparedStatement prepareStatement(String sql) // 创建一个 PreparedStatement对象
>                
>      2. 管理事务
>      // 开启事务
>      void setAutoCommit(boolean autoCommit) // 调用该方法设置参数为false，即开启事务
>      // 提交事务
>      void commit()
>      // 回滚事务
>      void rollback()
>      ```
>
> 
>
> 3. #### **Statement：执行sql的对象**
>
>    - 执行sql的方法
>
>      ```java
>      // 方法1 执行给定的sql语句,返回值是执行之后受影响的表记录的行数,判断增删改是否执行成功
>      int executeUpdate(String sql) // 一般执行DML(insert、update、delete)语句
>      // 方法2 返回一个结果集
>      ResultSet executeQuery(String sql) // 执行DQL(select)语句
>      ```
>
> 
>
> 4. #### **ResultSet：结果集对象 ，封装查询结果**
>
>    - 获取表中数据的方法
>
>      ```java
>      // 方法1：游标向下移动一行,判断当前行是否是最后一行末尾,是返回false,不是范围ture
>      boolean next()
>      // 方法2：获取游标所在行的某一列的数值
>      Xxx getXxx(参数)
>          -- Xxx代表数据类型，如 int getInt(), String getString()
>      	-- 参数:
>          	-- int:代表列的编号,从1开始,如 getString(1) 获取某行第一列的 员工名字
>              -- String:代表列名称,如 getDouble("money") 参数为列名
>      ```
>
>      > 方法一
>
>      ```java
>      package cn.cjj.jdbc;
>                
>      import java.sql.*;
>                
>      public class JdbcDemo5 {
>          public static void main(String[] args) {
>              // 提升作用域
>              ResultSet rs = null;
>              Connection conn = null;
>              Statement stmt = null;
>              try {
>                  // 1.注册数据库驱动
>                  Class.forName("com.mysql.jdbc.Driver");
>                  // 2.获取Connection数据库连接对象
>                  conn = DriverManager.getConnection("jdbc:mysql://localhost:13306/db", 			  "root", "root");
>                  // 3.定义sql语句:查询account表
>                  String sql = "select * from account";
>                  // 4.获取执行sql对象
>                  stmt = conn.createStatement();
>                  // 5.执行sql: ResultSet executeQuery(String sql)方法
>                  rs = stmt.executeQuery(sql);
>                  // 6.查询结果
>                  rs.next(); // 游标下移
>                  int id = rs.getInt(1); // 获取第一行第一列的数据
>                  String name = rs.getString("name"); // 获取第一行第二列的数据
>                  double money = rs.getDouble(3);
>                  // 7.输出结果
>                  System.out.println("id:" + id + "---" + "name:" + name + "---" + 				"money:" + money);
>              } catch (ClassNotFoundException | SQLException e) {
>                  e.printStackTrace();
>              }finally {
>                  if(stmt != null) {
>                      try {
>                          stmt.close();
>                      } catch (SQLException e) {
>                          e.printStackTrace();
>                      }
>                  }
>                  if(conn != null) {
>                      try {
>                          conn.close();
>                      } catch (SQLException e) {
>                          e.printStackTrace();
>                      }
>                  }
>                  if(rs != null) {
>                      try {
>                          rs.close();
>                      } catch (SQLException e) {
>                          e.printStackTrace();
>                      }
>                  }
>              }
>          }
>      }
>      ```
>
>      > 该方法实际写法
>
>      ```java
>      // 使用while循环判断
>      while(rs.next()){ // 游标下移
>          int id = rs.getInt(1); // 获取第一行第一列的数据
>          String name = rs.getString("name"); // 获取第一行第二列的数据
>          double money = rs.getDouble(3);
>          // 输出结果
>          System.out.println("id:" + id + "---" + "name:" + name + "---" + "money:" + 	money);
>      }
>      ```
>
>    #### 5.PreparedStatement：执行sql的对象
>    
>    - SQL注入问题：在拼接sql语句时，有一些sql的特殊关键字也参与字符串的拼接，会造成安全性问题
>    
>      1. 假如用户输入任意用户名，而密码是 `a' or 'a' = 'a`
>    
>      2. 对应的sql语句：`select * from user where username = 'abc' and password = 'a' or 'a' = 'a';` where后面'a' = 'a'值为ture始终成立
>    
>         ![image-20220606183123213](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251715069.png)
>    
>    - 解决sql注入问题：使用`PreparedStatement`对象来解决
>    
>    - 此时SQL处于预编译状态：参数使用？作为占位符
>    
>    - 步骤
>    
>      - 导入驱动jar包
>      - 注册驱动
>      - 获取数据库连接对象（Connection）
>      - 定义sql
>        - 注意：sql语句的参数使用?作为占位符
>        - 例如：`select * from user where username = ? and password = ?;`
>      - 获取执行sql语句的对象 
>        - `PreparedStatement pstmt = conn.prepareStatement(String sql)`
>      - 给sql赋值
>        - 方法：setXxx(参数1，参数2)
>          - 参数1：？的位置编号，从1开始
>          - 参数二：？的值
>      - 执行sql，接受返回的结果
>      - 处理结果
>      - 释放资源
>    
>    - 注意：后期都会使用`PreparedStatement`来完成增删改查的所有操作
>    
>      - 可以防止SQL注入
>      - 效率高

## 四、练习

> 1. ### account表 添加一条记录
>
>    ```java
>    package cn.cjj.jdbc;
>
>    import java.sql.Connection;
>    import java.sql.DriverManager;
>    import java.sql.SQLException;
>    import java.sql.Statement;
>
>    public class JdbcDemo2 {
>        public static void main(String[] args){
>            // 提升作用域
>            Connection conn = null;
>            Statement stmt = null;
>            try {
>                // 1.注册驱动
>                Class.forName("com.mysql.jdbc.Driver");
>                // 2.定义sql语句 添加记录
>                String sql = "insert into account values(null, '徐天天', 2000)";
>                // 3.获取Connection连接对象
>                conn = DriverManager.getConnection("jdbc:mysql://localhost:13306/db", 				"root", "root");
>                // 4.获取执行sql的Statement对象
>                stmt = conn.createStatement();
>                // 5.执行sql语句
>                int count = stmt.executeUpdate(sql); // 返回值：影响的行数
>                // 6.处理结果
>                System.out.println(count);
>                if(count > 0) {
>                    System.out.println("添加成功");
>                } else {
>                    System.out.println("添加失败");
>                }
>            } catch (ClassNotFoundException | SQLException e) {
>                e.printStackTrace();
>            } finally {
>                // 避免空指针异常(stmt = null)
>                if(stmt != null) {
>                    try {
>                        stmt.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>                if(conn != null) {
>                    try {
>                        conn.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>            }
>        }
>    }
>    ```
>
> 2. ### account表 修改记录
>
>    ```java
>    package cn.cjj.jdbc;
>
>    import java.sql.Connection;
>    import java.sql.DriverManager;
>    import java.sql.SQLException;
>    import java.sql.Statement;
>
>    public class JdbcDemo3 {
>        public static void main(String[] args) {
>            Connection conn = null;
>            Statement stmt = null;
>            try {
>                // 注册数据库驱动
>                Class.forName("com.mysql.jdbc.Driver");
>                // 定义sql语句 修改记录
>                String sql = "update account set money = 4000 where id in (3,4)";
>                // 获取数据库Connection连接对象
>                conn = DriverManager.getConnection("jdbc:mysql://localhost:13306/db", 				"root", "root");
>                // 获取Statement执行sql语句对象
>                stmt = conn.createStatement();
>                // 执行sql语句
>                int count = stmt.executeUpdate(sql);
>                System.out.println(count);
>            } catch (ClassNotFoundException | SQLException e) {
>                e.printStackTrace();
>            }finally {
>                if(stmt != null) {
>                    try {
>                        stmt.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>                if(conn != null) {
>                    try {
>                        conn.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>            }
>        }
>    }
>    ```
>
> 3. ### account表 删除一条记录
>
>    ```java
>    package cn.cjj.jdbc;
>          
>    import java.sql.Connection;
>    import java.sql.DriverManager;
>    import java.sql.SQLException;
>    import java.sql.Statement;
>          
>    public class JdbcDemo4 {
>        public static void main(String[] args) {
>            Connection conn = null;
>            Statement stmt = null;
>            try {
>                Class.forName("com.mysql.jdbc.Driver");
>                // 删除
>                String sql = "delete from account where id = 4";
>                conn = DriverManager.getConnection("jdbc:mysql://localhost:13306/db", 				"root", "root");
>                stmt = conn.createStatement();
>                int count = stmt.executeUpdate(sql);
>                System.out.println(count);
>            } catch (ClassNotFoundException | SQLException e) {
>                e.printStackTrace();
>            } finally {
>                if(stmt != null) {
>                    try {
>                        stmt.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>                if(conn != null) {
>                    try {
>                        conn.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>            }
>        }
>    }
>    ```
>
>    ### 4. JDBC查询emp表中数据并输出
>
>    > 创建Emp员工表类
>
>    ```java
>    package cn.cjj.domain;
>          
>    import java.util.Date;
>          
>    // Emp实体 javabean(标准的java类)
>    public class Emp {
>        private int id;
>        private String ename;
>        private int job_id;
>        private Date joindate;
>        private double salary;
>        private double bonus;
>        private int dept_id;
>          
>        public int getId() {
>            return id;
>        }
>          
>        public void setId(int id) {
>            this.id = id;
>        }
>          
>        public String getEname() {
>            return ename;
>        }
>          
>        public void setEname(String ename) {
>            this.ename = ename;
>        }
>          
>        public int getJob_id() {
>            return job_id;
>        }
>          
>        public void setJob_id(int job_id) {
>            this.job_id = job_id;
>        }
>          
>        public Date getJoindate() {
>            return joindate;
>        }
>          
>        public void setJoindate(Date joindate) {
>            this.joindate = joindate;
>        }
>          
>        public double getSalary() {
>            return salary;
>        }
>          
>        public void setSalary(double salary) {
>            this.salary = salary;
>        }
>          
>        public double getBonus() {
>            return bonus;
>        }
>          
>        public void setBonus(double bonus) {
>            this.bonus = bonus;
>        }
>          
>        public int getDept_id() {
>            return dept_id;
>        }
>          
>        public void setDept_id(int dept_id) {
>            this.dept_id = dept_id;
>        }
>          
>        @Override
>        public String toString() {
>            return "emp{" +
>                    "id=" + id +
>                    ", ename='" + ename + '\'' +
>                    ", job_id=" + job_id +
>                    ", joindate=" + joindate +
>                    ", salary=" + salary +
>                    ", bonus=" + bonus +
>                    ", dept_id=" + dept_id +
>                    '}';
>        }
>    }
>    ```
>
>    > 创建JdbcTest1测试类
>
>    ```java
>    package cn.cjj.jdbc;
>    import cn.cjj.domain.Emp;
>    import java.sql.*;
>    import java.util.ArrayList;
>    import java.util.List;
>    public class JdbcTest1 {
>        // main方法测试非静态方法,需要new对象
>        public static void main(String[] args) {
>            List<Emp> list = new JdbcTest1().findAll();
>            System.out.println(list);
>        }
>        // 方法：查询emp表的数据并封装为对象,然后装载集合,返回集合
>        public List<Emp> findAll() {
>            // 创建list集合
>            List<Emp> list = new ArrayList<Emp>();
>            Connection conn = null;
>            Statement stmt = null;
>            ResultSet rs = null;
>            try {
>                // 1.注册驱动
>                Class.forName("com.mysql.jdbc.Driver");
>                // 2.获取连接
>                conn = DriverManager.getConnection("jdbc:mysql://localhost:13306/db", 				"root", 
>                // 3.定义sql语句
>                String sql = "select * from emp";
>                // 4.获取执行sql对象
>                stmt = conn.createStatement();
>                // 5.执行sql 返回查询结果集
>                rs = stmt.executeQuery(sql);
>                // 6.遍历结果集,封装对象,装载集合
>                Emp emp = null; // 创建emp引用,方便后面复用
>                while(rs.next()) {
>                    // 获取数据
>                    int id = rs.getInt("id");
>                    String ename = rs.getString("ename");
>                    int job_id = rs.getInt("job_id");
>                    Date joindate = rs.getDate("joindate");
>                    double salary = rs.getDouble("salary");
>                    double bonus = rs.getDouble("bonus");
>                    // 创建emp对象,并赋值
>                    emp = new Emp();
>                    emp.setId(id);
>                    emp.setEname(ename);
>                    emp.setJob_id(job_id);
>                    emp.setJoindate(joindate);
>                    emp.setSalary(salary);
>                    emp.setBonus(bonus);
>                    // 装载集合
>                    list.add(emp);
>                }
>            } catch (ClassNotFoundException | SQLException e) {
>                e.printStackTrace();
>            }finally {
>                if(stmt != null) {
>                    try {
>                        stmt.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>                if(conn != null) {
>                    try {
>                        conn.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>                if(rs != null) {
>                    try {
>                        rs.close();
>                    } catch (SQLException e) {
>                        e.printStackTrace();
>                    }
>                }
>            }
>            return list;
>        }
>    }
>    ```
>    
>    ### 5.JDBC_登录案例
>    
>    ```java
>    package cn.cjj.jdbc;
>          
>    import cn.cjj.util.JDBCUtils;
>          
>    import java.sql.*;
>    import java.util.Scanner;
>          
>    public class JDBCLoginTest {
>        public static void main(String[] args) {
>            // 接受用户名和密码
>            Scanner sc = new Scanner(System.in);
>            System.out.println("请输入用户名：");
>            String username = sc.nextLine();
>            System.out.println("请输入密码：");
>            String password = sc.nextLine();
>            // 调用方法
>            boolean flag = new JDBCLoginTest().login(username, password);
>            // 判断是否登录成功
>            if(flag) {
>                System.out.println("登录成功");
>            }else{
>                System.out.println("登录失败");
>            }
>        }
>        // 登录方法
>        public boolean login(String username, String password) {
>            // 判断账号密码是否为空
>            if(username == null || password == null) {
>                return false;
>            }
>            // 提升作用域
>            Connection conn = null;
>            Statement stmt = null;
>            ResultSet rs = null;
>            try {
>                // 1.获取数据库连接对象
>                conn = JDBCUtils.getConnection();
>                // 2.定义sql
>                String sql = "select * from user where username = '"+username+"' and 				password = '"+password+"'";
>                // 3.获取执行sql对象
>                stmt = conn.createStatement();
>                // 4.执行查询
>                rs = stmt.executeQuery(sql);
>                // 5.判断结果
>                return rs.next();
>            } catch (SQLException e) {
>                e.printStackTrace();
>            }finally {
>                JDBCUtils.close(rs, stmt, conn);
>            }
>                  
>            return false;
>        }
>    }
>          
>    ```
>

## 五、抽取JDBC工具类：JDBCUtils

> ```java
> package cn.cjj.util;
> 
> import java.io.FileReader;
> import java.io.IOException;
> import java.net.URL;
> import java.sql.*;
> import java.util.Properties;
> 
> // JDBC工具类
> public class JDBCUtils {
>     // 只有静态变量才能被静态代码块和静态方法所使用
>     private static String url;
>     private static String user;
>     private static String password;
>     private static String driver;
>     /**
>      * 文件的读取，只需读取一次则使用静态代码块，在类加载到内存里即可自动执行。
>      * 注册驱动方法
>      */
>     static{
>         try {
>             // 1.创建Properties集合类
>             Properties pro = new Properties();
>             // 获取src路径下文件的方法：使用ClassLoader类加载器(可以加载字节码文件进内存和获取src路径下			文件资源的路径)
>             ClassLoader classLoader = JDBCUtils.class.getClassLoader();
>             // 以src为根目录获取资源文件,返回一个URL对象,可以定位一个资源的绝对路径
>             URL res = classLoader.getResource("jdbc.properties");
>             // 获取字符串文件路径
>             String path = res.getPath();
> 
>             // 2.加载文件
>             pro.load(new FileReader(path));
> 
>             // 3.获取数据
>             url = pro.getProperty("url");
>             user = pro.getProperty("user");
>             password = pro.getProperty("password");
>             driver = pro.getProperty("driver");
> 
>             // 4.注册驱动
>             Class.forName(driver);
>         } catch (IOException | ClassNotFoundException e) {
>             e.printStackTrace();
>         }
>     }
>     /**
>      * 获取连接
>      * @return 连接对象
>      */
>     public static Connection getConnection() throws SQLException {
>         return DriverManager.getConnection(url, user, password);
>     }
> 
>     /**
>      * 释放资源
>      * @param stmt
>      * @param conn
>      */
>     public static void close(Statement stmt, Connection conn) {
>         if (stmt != null) {
>             try {
>                 stmt.close();
>             } catch (SQLException e) {
>                 e.printStackTrace();
>             }
>         }
>         if (conn != null) {
>             try {
>                 conn.close();
>             } catch (SQLException e) {
>                 e.printStackTrace();
>             }
>         }
>     }
> 
>     /**
>      * 释放资源
>      * 方法的重载
>      * @param rs
>      * @param stmt
>      * @param conn
>      */
>     public static void close(ResultSet rs, Statement stmt, Connection conn) {
>         if (stmt != null) {
>             try {
>                 stmt.close();
>             } catch (SQLException e) {
>                 e.printStackTrace();
>             }
>         }
>         if (conn != null) {
>             try {
>                 conn.close();
>             } catch (SQLException e) {
>                 e.printStackTrace();
>             }
>         }
>         if (rs != null) {
>             try {
>                 rs.close();
>             } catch (SQLException e) {
>                 e.printStackTrace();
>             }
>         }
>     }
> }
> ```

## 六、JDBC管理事务

> - 使用Connection对象来管理事务
>
>   ```java
>   conn.setAutoCommit(flase) //开启事务
>     
>   conn.commit() //提交事务，当所有事务都执行完
>         
>   conn.rollback() //回滚事务，在Catch中
>   ```
>
>   
