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

**创建索引注意事项**

1）对于那些在查询中很少使用或者参考的列不应该创建索引。这是因为，既然这些列很少使用到，因此有索引或者无索引，并不能提高查询速度。相反，由于增加了索引，反而降低了系统的维护速度和增大了空间需求。

2）对于那些只有很少数据值的列也不应该增加索引。这是因为，由于这些列的取值很少，例如人事表的性别列，在查询的结果中，结果集的数据行占了表中数据行的很大比例，即需要在表中搜索的数据行的比例很大。增加索引，并不能明显加快检索速度。

3）对于那些定义为text, image和bit数据类型的列不应该增加索引。这是因为，这些列的数据量要么相当大，要么取值很少。

4）当修改性能远远大于检索性能时，不应该创建索引。这是因为，修改性能和检索性能是互相矛盾的。当增加索引时，会提高检索性能，但是会降低修改性能。当减少索引时，会提高修改性能，降低检索性能。因此，当修改性能远远大于检索性能时，不应该创建索引。

**查询注意事项**

1）应尽量避免在 where 子句中使用!=或<>操作符，否则将引擎放弃使用索引而进行全表扫描。
2）应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如：
select id from t where num is null
可以在num上设置默认值0，确保表中num列没有null值，然后这样查询：
select id from t where num=0
3）很多时候用 exists 代替 in 是一个好的选择（根据主表和子表的大写决定 in先查子表，返回笛卡尔积再进行查询，exists先查主表，再依次执行子查询）
4）用Where子句替换HAVING 子句 因为HAVING 只会在检索出所有记录之后才对结果集进行过滤

**优化**

1）范式优化： 比如消除冗余（节省空间。。）
2）反范式优化：比如适当加冗余等（减少join） 
3）拆分表： 分区将数据在物理上分隔开，不同分区的数据可以制定保存在处于不同磁盘上的数据文件里。这样，当对这个表进行查询时，只需要在表分区中进行扫描，而不必进行全表扫描，明显缩短了查询时间，另外处于不同磁盘的分区也将对这个表的数据传输分散在不同的磁盘I/O，一个精心设置的分区可以将数据传输对磁盘I/O竞争均匀地分散开。对数据量大的时时表可采取此方法。可按月自动建表分区。
4）拆分其实又分垂直拆分和水平拆分： 案例： 简单购物系统暂设涉及如下表： 1.产品表（数据量10w，稳定） 2.订单表（数据量200w，且有增长趋势） 3.用户表 （数据量100w，且有增长趋势） 以mysql为例讲述下水平拆分和垂直拆分，mysql能容忍的数量级在百万，静态数据可以到千万 垂直拆分：解决问题：表与表之间的io竞争 不解决问题：单表中数据量增长出现的压力 方案： 把产品表和用户表放到一个server上 订单表单独放到一个server上 水平拆分： 解决问题：单表中数据量增长出现的压力 不解决问题：表与表之间的io争夺
方案： 用户表通过性别拆分为男用户表和女用户表 订单表通过已完成和完成中拆分为已完成订单和未完成订单 产品表 未完成订单放一个server上 已完成订单表盒男用户表放一个server上 女用户表放一个server上(女的爱购物 哈哈)
