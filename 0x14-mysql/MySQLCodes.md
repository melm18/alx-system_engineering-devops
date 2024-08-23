## MySQL Codes

```
source:~$ sudo ufw allow from replica_server_ip to any port 3306
   sudo ufw allow from 34.74.230.21 to any port 3306

source:~$ sudo vi /etc/mysql/mysql.conf.d/mysqld.cnf

. . .
bind-address            = 127.0.0.1
. . .

on the replica ==>
. . .
bind-address            = source_server_ip
. . .


. . .
# server-id             = 1
. . .

. . .
log_bin                       = /var/log/mysql/mysql-bin.log
. . .

. . .
# binlog_do_db          = include_database_name


. . .
binlog_do_db          = db


source:~$ sudo systemctl restart mysql

source:~$ sudo mysql
source:~$ mysql -u sammy -p
mysql> GRANT REPLICATION SLAVE ON *.* TO 'replica_user'@'replica_server_ip';
mysql> FLUSH PRIVILEGES;
mysql> FLUSH TABLES WITH READ LOCK;
mysql> SHOW MASTER STATUS;

mysql> UNLOCK TABLES;
mysql> CREATE DATABASE db;

mysql -u root -p
CREATE USER 'holberton_user'@'34.74.230.21' IDENTIFIED BY 'projectcorrection280hbtn';
GRANT REPLICATION SLAVE ON *.* TO 'holberton_user'@'34.74.230.21'
FLUSH PRIVILEGES;
FLUSH TABLES WITH READ LOCK;
SHOW MASTER STATUS;
UNLOCK TABLES;
CREATE DATABASE tyrell_corp;
USE tyrell_corp;
CREATE TABLE nexus6(
    name varchar(30)
    );
INSERT INTO example_table VALUES
CREATE TABLE nexus6( id int ,name varchar(30) );
INSERT INTO nexus6 VALUES (1, 'Leon');
SELECT * FROM nexus6;
exit
sudo mysqldump -u root -p tyrell_corp > tyrell_corp.sql
mysql> CHANGE REPLICATION SOURCE TO
    -> SOURCE_HOST='34.139.184.21',
    -> SOURCE_USER='holberton_user',
    -> SOURCE_PASSWORD='projectcorrection280hbtn',
    -> SOURCE_LOG_FILE='mysql-bin.000238',
    -> SOURCE_LOG_POS=154;
    
    mysql> CHANGE MASTER TO
    -> MASTER_HOST='34.139.184.21',
    -> MASTER_USER='holberton_user',
    -> MASTER_PASSWORD='projectcorrection280hbtn',
    -> MASTER_LOG_FILE='mysql-bin.000238',
    -> MASTER_LOG_POS=154;
REVOKE REPLICATION SLAVE ON *.* FROM 'holberton_user'@'localhost';

GRANT SELECT ON *.* TO 'holberton_user'@'localhost';

mysql> START SLAVE
    -> ;
Query OK, 0 rows affected (0.00 sec)

mysql> SHOW SLAVE STATUS\G
*************************** 1. row ***************************
               Slave_IO_State: Connecting to master
                  Master_Host: 34.139.184.21
                  Master_User: holberton_user
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000238
          Read_Master_Log_Pos: 154
               Relay_Log_File: mysql-relay-bin.000001
                Relay_Log_Pos: 4
        Relay_Master_Log_File: mysql-bin.000238
             Slave_IO_Running: Connecting
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
           Replicate_Do_Table: 
       Replicate_Ignore_Table: 
      Replicate_Wild_Do_Table: 
  Replicate_Wild_Ignore_Table: 
                   Last_Errno: 0
                   Last_Error: 
                 Skip_Counter: 0
          Exec_Master_Log_Pos: 154
              Relay_Log_Space: 154
              Until_Condition: None
               Until_Log_File: 
                Until_Log_Pos: 0
           Master_SSL_Allowed: No
           Master_SSL_CA_File: 
           Master_SSL_CA_Path: 
              Master_SSL_Cert: 
            Master_SSL_Cipher: 
               Master_SSL_Key: 
        Seconds_Behind_Master: NULL
Master_SSL_Verify_Server_Cert: No
                Last_IO_Errno: 2003
                Last_IO_Error: error connecting to master 'holberton_user@34.139.184.21:3306' - retry-time: 60  retries: 1
               Last_SQL_Errno: 0
               Last_SQL_Error: 
  Replicate_Ignore_Server_Ids: 
             Master_Server_Id: 0
                  Master_UUID: 
             Master_Info_File: /var/lib/mysql/master.info
                    SQL_Delay: 0
          SQL_Remaining_Delay: NULL
      Slave_SQL_Running_State: Slave has read all relay log; waiting for more updates
           Master_Retry_Count: 86400
                  Master_Bind: 
      Last_IO_Error_Timestamp: 210907 18:45:48
     Last_SQL_Error_Timestamp: 
               Master_SSL_Crl: 
           Master_SSL_Crlpath: 
           Retrieved_Gtid_Set: 
            Executed_Gtid_Set: 
                Auto_Position: 0
         Replicate_Rewrite_DB: 
                 Channel_Name: 
           Master_TLS_Version: 
1 row in set (0.00 sec)

mysql> select user,host from mysql.user;
+----------------+--------------+
| user           | host         |
+----------------+--------------+
| holberton_user | 34.74.230.21 |
| holberton_user | localhost    |
| mysql.session  | localhost    |
| mysql.sys      | localhost    |
| root           | localhost    |
+----------------+--------------+
5 rows in set (0.09 sec)

mysql> GRANT REPLICATION SLAVE ON *.* TO 'holberton_user'@'localhost';
Query OK, 0 rows affected (0.10 sec)

mysql>

GRANT REPLICATION SLAVE ON *.* TO 'tyrell_corp'@'%' IDENTIFIED BY 'projectcorrection280hbtn'

ysql> SHOW MASTER STATUS;
+------------------+----------+--------------+------------------+-------------------+
| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
+------------------+----------+--------------+------------------+-------------------+
| mysql-bin.000271 |      596 | tyrell_corp  |                  |                   |
+------------------+----------+--------------+------------------+-------------------+
1 row in set (0.04 sec)

mysql> 


CREATE USER 'replica_user'@'%' IDENTIFIED BY 'projectcorrection280hbtn';
GRANT REPLICATION SLAVE ON *.* TO 'replica_user'@'%';
```

