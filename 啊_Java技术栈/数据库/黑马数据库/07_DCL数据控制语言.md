# 07_DCL数据控制语言

## 一、管理用户

1. 添加用户

2. 删除用户

3. 修改用户密码

4. 查询用户

   ```mysql
   USE mysql;
   # 查询用户
   SELECT * FROM USER;
   
   # 添加用户
   CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
   
   CREATE USER 'cjj'@'localhost' IDENTIFIED BY '123
   
   # 删除用户
   DROP USER '用户名' @ '主机名';
   
   # 修改密码
   UPDATE USER SET PASSWORD = PASSWORD('新密码') where user = '用户名';
   
   SET PASSWORD FOR '用户名' @ '主机名' = PASSWORD('新密码');
   ```

   

   ![image-20220604200618845](https://gitee.com/chen-jiujia/typora-picgo/raw/master/img/202309251658707.png)

5. 忘记root密码解决方案

   ```mysql
   1. cmd --> net stop mysql # 停止mysql服务
   2. 打开新的cmd窗口，直接输入mysql命令回车，登录成功
   3. use mysq; 
   4. UPDATE USER SET PASSWORD = PASSWORD('新密码') where user = 'root';
   5. 关闭两个窗口
   6. 打开任务管理器，手动结束mysqld.exe进程
   7. 启动mysql服务
   8. 登录
   ```

## 二、权限管理

1. 查询权限

   ```mysql
   # 查询权限
   SHOW GRANTS FOR '用户名' @ '主机名';
   SHOW GRANTS FOR 'cjj'@'localhost';
   SHOW GRANTS FOR 'root'@'localhost'; -- 拥有全部权限
   ```

2. 授予权限

   ```mysql
   # 授予权限
   GRANT 权限列表 ON 数据库名.表名 TO '用户名' @ '主机名';
   
   # 给cjj赋予在db1数据库下account表的SELECT权限
   GRANT SELECT ON db1.`account` TO 'cjj'@'localhost';
   
   # 给cjj用户授予所有权限，在任意数据库任意表上
   GRANT ALL ON *.* TO 'cjj'@'localhost';
   ```

3. 撤销权限

   ```mysql
   # 撤销权限
   REVOKE 权限列表 ON 数据库名.表名 FROM '用户名' @ '主机名';
   
   # 撤销cjj在db1数据库下account表的UPDATE权限
   REVOKE UPDATE ON db1.account FROM 'cjj'@'localhost';
   ```

   