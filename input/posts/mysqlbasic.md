Title: My SQL Basic
Published: 10/10/2019
Tags:
    - MySql
    - Connection
    - Introduction
    - Sql
---

Mysql command line connection 
=================================

connect db
********************

```shell

    mysql -h {hostip} -P {port} -u {username} -p {database}
```
Example

```shell

    mysql -h 127.0.0.1 -P 3306 -u root -p
```
database backup

```shell

    mysqldump -h 127.0.0.1 -P 3306 -u root -p billpayment> sqlback.sql

```
If mysqldump throws: Unknown table 'COLUMN_STATISTICS' in information_schema (1109)
Example:

```shell 

    mysqldump --column-statistics=0 -h 127.0.0.1 -P 3306 -u root -p billpayment> sqlback.sql
```

**Create User and Grant Permission for remote connection**

```shell
    select host, user from mysql.user;
```

```shell
    mysql> CREATE USER 'dev'@'%' IDENTIFIED BY 'secret';
    Query OK, 0 rows affected (0.00 sec)
    mysql> GRANT ALL PRIVILEGES ON * . * TO 'dev'@'%';
    Query OK, 0 rows affected (0.00 sec)
    mysql> quit
```

Restore Database
==================

```
    mysql -u [user] -p [database_name] < [filename].sql
```


PHP My Admin Connecting to MySQL Server Config 
===================================================

Change the following paramert as appropriate  ```C:\wamp64\apps\phpmyadmin4.8.4\config.inc.php``` and 

```php
    $cfg['Servers'][$i]['verbose'] = 'MySQL';
	$cfg['Servers'][$i]['host'] = '127.0.0.1';
	$cfg['Servers'][$i]['port'] =3306;//$wampConf['mysqlPortUsed'];// 3307;//$wampConf['mysqlPortUsed'];
	$cfg['Servers'][$i]['extension'] = 'mysqli';
	$cfg['Servers'][$i]['auth_type'] = 'cookie';
	$cfg['Servers'][$i]['user'] = '';
	$cfg['Servers'][$i]['password'] = '';
```

WampServer MYSQL port changes
===============================

Finding the configuration file

Example:

```xml
C:\wamp64\bin\mysql\mysql5.7.24\my.ini
```

change 

```
# The MySQL server
[wampmysqld]
port = 3306
#
#
[mysqld]
port=3306
```

Change both instances to another port (i.e. 3307) and restart Wamp MYSQL