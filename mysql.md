## Centos7 安装mysql

**Centos7 已结将mysql-server从默认软件库中移除，使用MairaDB代替，原因是因为Oracle收购mysql之后会有闭源风险，MariaDB可以完全兼容Mysql的操作，包括以下内容**

1、数据和表定义文件（.frm）是二进制兼容的
2、所有客户端API、协议和结构都是完全一致的
3、所有文件名、二进制、路径、端口等都是一致的
4、所有的MySQL连接器，比如PHP、Perl、Python、Java、.NET、MyODBC、Ruby以及MySQL C connector等在MariaDB中都保持不变
5、mysql-client包在MariaDB服务器中也能够正常运行
6、共享的客户端库与MySQL也是二进制兼容的

**安装MariaDB**
```
yum install mysql
yum install mariadb-server
yun install mysql-devel
```
 
**MariaDB指令**

```
systemctl start mariadb  #启动MariaDB

systemctl stop mariadb  #停止MariaDB

systemctl restart mariadb  #重启MariaDB

systemctl enable mariadb  #设置开机启动
```

**设置远程访问**

设置了root可以远程访问

```
MariaDB [mysql]> grant all privileges on *.* to root@'%'identified by 'password';
Query OK, 0 rows affected (0.00 sec)

MariaDB [mysql]> flush privileges;
Query OK, 0 rows affected (0.00 sec)
```

