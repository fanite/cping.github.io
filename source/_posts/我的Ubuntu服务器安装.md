---
title: 我的Ubuntu服务器安装
date: 2019-09-19 00:01:24
categories: "Linux"
tags: ["Linux","VPS", "Ubuntu18"]
---

&ensp;&ensp;&ensp;&ensp;因为想用Huginn但是小鸡装Dockers有点吃不消，所以想把服务器从CentOS7换成Ubuntu18。
更悲催的是硬盘小，内存小，装lnmp和Oneinstack都不理想，所以只有安装二进制lnmp环境了。

> 安装Mysql5.7

```sh
# 安装MySQL
apt install mysql-server
# 启动MySQL服务
service mysql start
# 对数据进行初始化设置
mysql_secure_installation
```

```sh
root@localhost:~# mysql_secure_installation

Securing the MySQL server deployment.

Connecting to MySQL using a blank password.

VALIDATE PASSWORD PLUGIN can be used to test passwords
and improve security. It checks the strength of password
and allows the users to set only those passwords which are
secure enough. Would you like to setup VALIDATE PASSWORD plugin?

Press y|Y for Yes, any other key for No: y

There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary                  file

Please enter 0 = LOW, 1 = MEDIUM and 2 = STRONG: 1
Please set the password for root here.

New password: 

Re-enter new password: 

Estimated strength of the password: 50 
Do you wish to continue with the password provided?(Press y|Y for Yes, any other key for No) : y
By default, a MySQL installation has an anonymous user,
allowing anyone to log into MySQL without having to have
a user account created for them. This is intended only for
testing, and to make the installation go a bit smoother.
You should remove them before moving into a production
environment.

Remove anonymous users? (Press y|Y for Yes, any other key for No) : y
Success.


Normally, root should only be allowed to connect from
'localhost'. This ensures that someone cannot guess at
the root password from the network.

Disallow root login remotely? (Press y|Y for Yes, any other key for No) : y
Success.

By default, MySQL comes with a database named 'test' that
anyone can access. This is also intended only for testing,
and should be removed before moving into a production
environment.


Remove test database and access to it? (Press y|Y for Yes, any other key for No) : y
 - Dropping test database...
Success.

 - Removing privileges on test database...
Success.

Reloading the privilege tables will ensure that all changes
made so far will take effect immediately.

Reload privilege tables now? (Press y|Y for Yes, any other key for No) : y
Success.

All done! 
```

> 安装PHP7.2

```sh
# 安装PHP
apt install php7.2 php7.2-common php7.2-fpm php7.2-cli php7.2-bz2 php7.2-bcmath php7.2-gmp php7.2-json php7.2-mbstring php7.2-mysql php7.2-opcache
```

> 安装Nginx

```sh
# 安装Nginx
apt install nginx
# 开放防火墙(ufw app list)
ufw allow "Nginx Full"
# 开启服务
service nginx start
```