MySql Master Slave Replication Docker Compose
=============================================

1. Start containers:

```
$ docker-compose up -d
```

2. Get master status:

```
$ docker exec -ti mysql-master bash
# mysql -pmst
mysql> show master status;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000003 |      154 |              |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.00 sec)
```

get `File` and `Position` value: `mysql-bin.000003`, `154`

3. Set slave

```
$ docker exec -ti mysql-slave bash
# mysql -pslv
mysql> change master to
    -> master_host='mysql-master',
    -> master_port=3306,
    -> master_user='mst',
    -> master_password='mst',
    -> master_log_file='mysql-bin.000003',
    -> master_log_pos=154;
Query OK, 0 rows affected, 2 warnings (0.01 sec)

mysql> start slave;
Query OK, 0 rows affected (0.01 sec)

mysql> show slave status \G

```