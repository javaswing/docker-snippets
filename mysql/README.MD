## 说明
- mysql 8 镜像安全性更高，docker启动之后需要设置外网访问才能正常链接。操作如下：

    1. 登录mysql
    ```shell
        mysql -u root -p
    ```
    2. 选择 mysql 数据库
    ```sql
        use mysql;
    ```
    3. 在 mysql 数据库的 user 表中查看当前 root 用户的相关信息
    ```sql
        select host, user, authentication_string, plugin from user; 
    ```
    4. 授权 root 用户的所有权限并设置远程访问
    ```sql
       GRANT ALL ON *.* TO 'root'@'%';
    ```
    5. 刷新权限
    ```sql
       flush privileges;
    ```
    6. 查看 root 用户的 host, 你会发现 root 用户的 host 已经变成 %，说明我们的修改已经成功，可以远程访问了。
    ```sql
       select host, user, authentication_string, plugin from user; 
    ```
    7. 访问数据库，远程访问数据库的可视化工具比较多如：Navicat、SQLyog、MySQL workbench 等，我这里使用 Navicat输入访问的 host 和密码，报 2059 错误，这是因为 MySql8.0 版本 和 5.0 的加密规则不一样，而现在的可视化工具只支持旧的加密方式。此问题有两种方法，一种是更新 Navicat 驱动来解决此问题，另一种是将 MySQL 用户登录的加密规则修改为 mysql_native_password，第一种方法我试过了没有起作用，我这里采用第二种方法。
    8. 修改加密规则
    ```sql
    ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; 
    ```
    9. 更新 root 用户密码
    ```sql
    ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; 
    ```
    10. 刷新权限
    ```sql
    FLUSH PRIVILEGES; 
    ```
- 如果换用mysql 5.7则不会有这个问题
    

