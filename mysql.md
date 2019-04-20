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

## MYSQL

** 创建索引注意事项 **
第一，对于那些在查询中很少使用或者参考的列不应该创建索引。这是因为，既然这些列很少使用到，因此有索引或者无索引，并不能提高查询速度。相反，由于增加了索引，反而降低了系统的维护速度和增大了空间需求。

第二，对于那些只有很少数据值的列也不应该增加索引。这是因为，由于这些列的取值很少，例如人事表的性别列，在查询的结果中，结果集的数据行占了表中数据行的很大比例，即需要在表中搜索的数据行的比例很大。增加索引，并不能明显加快检索速度。

第三，对于那些定义为text, image和bit数据类型的列不应该增加索引。这是因为，这些列的数据量要么相当大，要么取值很少。

第四，当修改性能远远大于检索性能时，不应该创建索引。这是因为，修改性能和检索性能是互相矛盾的。当增加索引时，会提高检索性能，但是会降低修改性能。当减少索引时，会提高修改性能，降低检索性能。因此，当修改性能远远大于检索性能时，不应该创建索引。

根据数据库的功能，可以在数据库设计器中创建三种索引：唯一索引、主键索引和聚集索引。
