Mysql8 配置允许远程连接

一、登陆MySql

```bash
mysql -uroot -p 
```

二、进入mysql库

```sql
use mysql
```

三、更新域属性，'%'表示允许外部访问

```sql
update user set host='%' where user ='root';
```

四、执行以上语句之后再执行

作用是：

将当前user和privilige表中的用户信息/权限设置从mysql库(MySQL数据库的内置库)中提取到内存里。

MySQL用户数据和权限有修改后，希望在"不重启MySQL服务"的情况下直接生效，那么就需要执行这个命令。

```sql
FLUSH PRIVILEGES;
```
[From MySQL documentation:](https://dev.mysql.com/doc/refman/5.7/en/privilege-changes.html)
> If you modify the grant tables directly using statements such as INSERT, UPDATE, or DELETE, your changes have no effect on privilege checking until you either restart the server or tell it to reload the tables. If you change the grant tables directly but forget to reload them, your changes have no effect until you restart the server. This may leave you wondering why your changes seem to make no difference!

> To tell the server to reload the grant tables, perform a flush-privileges operation. This can be done by issuing a FLUSH PRIVILEGES statement or by executing a mysqladmin flush-privileges or mysqladmin reload command.

> If you modify the grant tables indirectly using account-management statements such as GRANT, REVOKE, SET PASSWORD, or RENAME USER, the server notices these changes and loads the grant tables into memory again immediately.

五、执行授权语句

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'WITH GRANT OPTION;
```

六、注意问题：

1、用Navicat连接mysql，报错如下：
```
Client does not support authentication protocol requested by server；
```
报错原因：

mysql8.0 引入了新特性 caching_sha2_password；这种密码加密方式Navicat 12以下客户端不支持；

Navicat 12以下客户端支持的是mysql_native_password 这种加密方式；

解决方式：

用命令将他修改成mysql_native_password加密模式：

```sql
update user set plugin='mysql_native_password' where user='root';
```
2、还连接不上

请注意检查服务器是否开发3306端口

检查防火墙
