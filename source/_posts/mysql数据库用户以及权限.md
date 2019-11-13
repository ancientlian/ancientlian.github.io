---
title: mysql数据库用户以及权限
date: 2018-12-19 22:36:53
tags: 
    - mysql
categories: mysql
---
进入mysql界面，进行用户的基本相关操作

## 查看当前有的账户名称

`mysql> SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;` 

或者直接：`select user,host from mysql.user;`

## 创建用户
<!--more-->
通用命令为：
`CREATE USER 'username'@'host' IDENTIFIED BY 'password';`

Or命令2：
当数据库存在用户的时候GRANT会对用户进行授权，但当数据库不存在该用户的时候，就会创建相应的用户并进行授权。（说明上面那步是多余的，并且他创建之后还要赋权限才能使用=.=!）
`GRANT <privileges> ON username IDENTIFIED BY 'password'<WITH GRANT OPTION>;`

说明：

+ username：创建的用户名
+ host：指定用户可以在哪个主机登录。**本地用户用'localhost'，从任意远程主机登录，用通配符‘%’**
+ password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

## 授权

命令1：
`GRANT privileges ON databasename.tablename TO 'username'@'host'`

说明：

+ privileges：用户的操作权限，如`SELECT`，`INSERT`，`UPDATE`等，如果要授予所的权限则使用`ALL`

+ 用这种命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:

  ```
  GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
  ```

## 撤销用户权限

命令：
`REVOKE privilege ON databasename.tablename FROM 'username'@'host';`

**注意：**假如你在给用户`'name'@'%'`授权的时候是这样的（或类似的）：`GRANT SELECT ON test.user TO 'name'@'%'`，则在使用`REVOKE SELECT ON *.* FROM 'name'@'%';`命令并不能撤销该用户对test数据库中user表的`SELECT` 操作。**相反**，如果授权使用的是`GRANT SELECT ON *.* TO 'name'@'%';`则`REVOKE SELECT ON test.user FROM 'name'@'%';`命令也**不能**撤销该用户对test数据库中user表的`Select`权限。

## 删除用户

命令：`DROP USER 'username'@'host';`

也可以是：`Delete FROM user Where User='username' and Host='host';`

如果就修改当前用户的密码，可以直接：`SET PASSWORD = PASSWORD("newpassword");`

## 修改用户密码

命令：`update mysql.user set password=password('new_password') where User="username" and Host="host";`

也可以是：`SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');`

## 备注

如果授权和删除之后没有显示，可以用`flush privileges;`刷新一下系统权限表

## 附：数据库/数据表/数据列权限：

Alter: 修改已存在的数据表(例如增加/删除列)和索引。
Create: 建立新的数据库或数据表。
Delete: 删除表的记录。
Drop: 删除数据表或数据库。
INDEX: 建立或删除索引。
Insert: 增加表的记录。
Select: 显示/搜索表的记录。
Update: 修改表中已存在的记录。

全局管理MySQL用户权限：

file: 在MySQL服务器上读写文件。
PROCESS: 显示或杀死属于其它用户的服务线程。
RELOAD: 重载访问控制表，刷新日志等。
SHUTDOWN: 关闭MySQL服务。

特别的权限：

ALL: 允许做任何事(和root一样)。
USAGE: 只允许登录--其它什么也不允许做。