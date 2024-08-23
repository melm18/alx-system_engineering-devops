# 0x14. MySQL
System engineering & DevOps

## Resources
Read or watch:

* [What is a primary-replica cluster](https://www.digitalocean.com/community/tutorials/how-to-choose-a-redundancy-plan-to-ensure-high-availability#sql-replication)
* [MySQL primary replica setup](https://www.digitalocean.com/community/tutorials/how-to-set-up-replication-in-mysql)
* [Build a robust database backup strategy](https://www.databasejournal.com/features/mssql/developing-a-sql-server-backup-strategy.html)
* [Setup a Primary-Replica infrastructure using MySQL](https://medium.com/@estebandelahoz/setup-a-primary-replica-infrastructure-using-mysql-5bcab77c352)
* [How to Install MySQL 5.7 on Ubuntu 20.04 LTS](https://www.hackerxone.com/2021/06/02/how-install-mysql-57-ubuntu-2004-lts/)
* [How to configure a MySQL Slave Database](https://www.arubacloud.com/tutorial/how-to-configure-a-mysql-slave-database.aspx)



## Database administration

* [What is a database](http://searchsqlserver.techtarget.com/definition/database)
* [What is a database primary/replicate cluster](https://www.digitalocean.com/community/tutorials/how-to-choose-a-redundancy-plan-to-ensure-high-availability#sql-replication)
* [MySQL primary/replicate setup](https://www.digitalocean.com/community/tutorials/how-to-set-up-replication-in-mysql)
* [Build a robust database backup strategy](https://www.databasejournal.com/features/mssql/developing-a-sql-server-backup-strategy.html)
* [MySQL Master-Master Replication setup in 5 easy steps](https://www.ryadel.com/en/mysql-master-master-replication-setup-in-5-easy-steps)
* [How to Set Up Replication in MySQL](https://blogs.perficient.com/2021/07/28/how-to-set-up-replication-in-mysql/)
* [Setting Up Replication](https://mariadb.com/kb/en/setting-up-replication)
* [How to Configure Source-Replica Replication in MySQL](https://www.linode.com/docs/guides/configure-source-replica-replication-in-mysql/)

## Tasks

## [0. Install MySQL](./)
First things first, let’s get MySQL installed on both your web-01 and web-02 servers.

* MySQL distribution must be 5.7.x

## [1. Let us in!](./)
* Create a MySQL user named holberton_user on both web-01 and web-02 with the host name set to localhost and the password projectcorrection280hbtn. This will allow us to access the replication status on both servers.
* Make sure that holberton_user has permission to check the primary/replica status of your databases.
* In addition to that, make sure that task #3 of your SSH project is completed for web-01 and web-02. You will likely need to add the public key to web-02 as you only added it to web-01 for this project. The checker will connect to your servers to check MySQL status
```
ubuntu@229-web-01:~$ mysql -uholberton_user -p -e "SHOW GRANTS FOR 'holberton_user'@'localhost'"
Enter password:
+-----------------------------------------------------------------+
| Grants for holberton_user@localhost                             |
+-----------------------------------------------------------------+
| GRANT REPLICATION CLIENT ON *.* TO 'holberton_user'@'localhost' |
+-----------------------------------------------------------------+
ubuntu@229-web-01:~$
```

## [2. If only you could see what I've seen with your eyes](./)
In order for you to set up replication, you’ll need to have a database with at least one table and one row in your primary MySQL server (web-01) to replicate from.

* Create a database named tyrell_corp.
* Within the tyrell_corp database create a table named nexus6 and add at least one entry to it.
* Make sure that holberton_user has SELECT permissions on your table so that we can check that the table exists and is not empty.
```
ubuntu@229-web-01:~$ mysql -uholberton_user -p -e "use tyrell_corp; select * from nexus6"
Enter password:
+----+-------+
| id | name  |
+----+-------+
|  1 | Leon  |
+----+-------+
ubuntu@229-web-01:~$
```

## [3. Quite an experience to live in fear, isn't it?](./)
Before you get started with your primary-replica synchronization, you need one more thing in place. On your primary MySQL server (web-01), create a new user for the replica server.

* The name of the new user should be replica_user, with the host name set to %, and can have whatever password you’d like.
* replica_user must have the appropriate permissions to replicate your primary MySQL server.
* holberton_user will need SELECT privileges on the mysql.user table in order to check that replica_user was created with the correct permissions.

## [4. Setup a Primary-Replica infrastructure using MySQL](./4-mysql_configuration_primary)  [replica](./4-mysql_configuration_replica)
Having a replica member on for your MySQL database has 2 advantages:

* Redundancy: If you lose one of the database servers, you will still have another working one and a copy of your data
* Load distribution: You can split the read operations between the 2 servers, reducing the load on the primary member and improving query response speed

Requirements:
* MySQL primary must be hosted on web-01 - do not use the bind-address, just comment out this parameter
* MySQL replica must be hosted on web-02
* Setup replication for the MySQL database named tyrell_corp
* Provide your MySQL primary configuration as answer file(my.cnf or mysqld.cnf) with the name 4-mysql_configuration_primary
* Provide your MySQL replica configuration as an answer file with the name 4-mysql_configuration_replica

Tips:
* Once MySQL replication is setup, add a new record in your table via MySQL on web-01 and check if the record has been replicated in MySQL web-02. If you see it, it means your replication is working!
* Make sure that UFW is allowing connections on port 3306 (default MySQL port) otherwise replication will not work.

Web 01

```
ubuntu@web-01:~$ mysql -uholberton_user -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 1467
Server version: 5.5.49-0ubuntu0.14.04.1-log (Ubuntu)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show master status;
+------------------+----------+--------------------+------------------+
| File             | Position | Binlog_Do_DB       | Binlog_Ignore_DB |
+------------------+----------+--------------------+------------------+
| mysql-bin.000009 |      107 | tyrell_corp          |                  |
+------------------+----------+--------------------+------------------+
1 row in set (0.00 sec)

mysql> 
```

Web-02

```
root@web-02:/home/ubuntu# mysql -uholberton_user -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 53
Server version: 5.5.49-0ubuntu0.14.04.1-log (Ubuntu)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show slave status\G
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 158.69.68.78
                  Master_User: replica_user
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.000009
          Read_Master_Log_Pos: 107
               Relay_Log_File: mysql-relay-bin.000022
                Relay_Log_Pos: 253
        Relay_Master_Log_File: mysql-bin.000009
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
```

## [5. MySQL backup](./5-mysql_backup)
What if the data center where both your primary and replica database servers are hosted are down because of a power outage or even worse: flooding, fire? Then all your data would inaccessible or lost. That’s why you want to backup and store them in a different system in another physical location. This can be achieved by dumping your MySQL data, compressing them and storing them in a different data center.

Write a Bash script that generates a MySQL dump and creates a compressed archive out of it.

Requirements:

* The MySQL dump must contain all your MySQL databases
* The MySQL dump must be named backup.sql
* The MySQL dump file has to be compressed to a tar.gz archive
* This archive must have the following name format: day-month-year.tar.gz
* The user to connect to the MySQL database must be root
* The Bash script accepts one argument that is the password used to connect to the MySQL database
> ./5-mysql_backup mydummypassword
