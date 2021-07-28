# mysql 手册
***默认环境：Ubuntu(64 - bit)***

*指令中带*‘_’*前缀的词素表示参数*
## 安装 mysql
终端中输入：
```
#: sudo apt-get install mysql-server
```

## 登入mysql
### 根用户登入
```
#: sudo mysql -uroot -p
```
### 本地本机登入
指令：`mysql -u _username -p _database_name`
-u: == user
-p: == password(带密码登入)
示例：

```
#: mysql -u admin -p mysql
Enter password: 123
```
### 线上登入

## 用户管理
***记得使用***`FLUSH PRIVILEGES;`***指令来保存用户信息的修改，以免在mysql服务被关闭后导致用户管理信息丢失***
### 新建用户
在mysql内切换至`mysql`库：
```
mysql> use mysql;
```
指令：`CREATE USER '_username'@'_host_ip_address' IDENTIFIED BY '_password';`
示例：

```
mysql> CREATE USER 'dog'@'localhost' IDENTIFIED BY '123456';
mysql> CREATE USER 'pig'@'192.168.1.101_' IDENDIFIED BY '123456';
mysql> CREATE USER 'pig'@'%' IDENTIFIED BY '123456';
mysql> CREATE USER 'pig'@'%' IDENTIFIED BY '';
mysql> CREATE USER 'pig'@'%';
```
注：通配符`%`表示任意地址；密码可以为空。

### 权限管理
#### 授权
指令：`GRANT _privileges ON _databasename._tablename TO '_username'@'_host_ip_address';`
示例：
```
mysql> GRANT ALL ON *.* TO 'dog' @ '%';
Query OK, 0 rows affected (0.00sec)

mysql> GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
mysql> GRANT ALL ON *.* TO 'pig'@'%';
mysql> GRANT ALL ON maindataplus.* TO 'pig'@'%';
```
注：用以上命令授权的用户不能给其它用户授权，如果想让该用户可以给其他用户授权，用以下命令:`GRANT _privileges ON _databasename._tablename TO '_username'@'_host_ip_address' WITH GRANT OPTION;`
#### 撤销权限
指令：`REVOKE _privilege ON _databasename._tablename FROM '_username'@'_host_ip_address';`
示例：
```
REVOKE SELECT ON *.* FROM 'pig'@'%';
```
注：假如你在给用户'pig'@'%'授权的时候是这样的（或类似的）：`GRANT SELECT ON test.user TO 'pig'@'%'`，则在使用`REVOKE SELECT ON *.* FROM 'pig'@'%';`命令并不能撤销该用户对test数据库中user表的SELECT 操作。相反，如果授权使用的是`GRANT SELECT ON *.* TO 'pig'@'%';`则`REVOKE SELECT ON test.user FROM 'pig'@'%';`命令也不能撤销该用户对test数据库中user表的Select权限。
## 配置 mysql
### 初始化配置
终端中输入：
```
#: sudo mysql_secure_installation
```
将对以下内容进行配置：
```
SSWORD PLUGIN can be used to test passwords...
Press y|Y for Yes, any other key for No: N 

#2
Please set the password for root here...
New password: (输入密码)
Re-enter new password: (重复输入)

#3
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them...
Remove anonymous users? (Press y|Y for Yes, any other key for No) : N 

#4
Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network...
Disallow root login remotely? (Press y|Y for Yes, any other key for No) : Y 

#5
By default, MySQL comes with a database named 'test' that
anyone can access...
Remove test database and access to it? (Press y|Y for Yes, any other key for No) : N 

#6
Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.
Reload privilege tables now? (Press y|Y for Yes, any other key for No) : Y 
```
### mysql 服务状态
检查 mysql 服务状态：
```
#: systemctl status mysql.service
```
启动 mysql 服务：
```
#: service mysql start
```
关闭 mysql 服务：
```
#: service mysql stop
```
重启 mysql 服务：
```
#: service mysql restart
```

