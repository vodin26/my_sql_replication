# Master-slave replication

docker compose rm -vf && docker compose up

## Master-master

mysql -uroot -psecret

### Master-1

create user 'repl_1'@'%' identified by 'pass';
grant replication slave on *.* to 'repl_2'@'%';
show grants for repl@'%';

show master status;

CHANGE MASTER TO 
MASTER_HOST = 'master-2', 
MASTER_USER ='repl_2', 
MASTER_PASSWORD = 'pass', 
MASTER_LOG_FILE = 'mysql-bin.000003', 
MASTER_LOG_POS = 660;

start slave;

create database test1;

show databases;


### Master-2

create user 'repl_2'@'%' identified by 'pass';
grant replication slave on *.* to 'repl_2'@'%';
show grants for repl@'%';

show master status;

CHANGE MASTER TO 
MASTER_HOST = 'master-1', 
MASTER_USER ='repl_1', 
MASTER_PASSWORD = 'pass', 
MASTER_LOG_FILE = 'mysql-bin.000003', 
MASTER_LOG_POS = 660;

start slave;

DBeaver connect:
jdbc:mysql://localhost:3306/db?allowPublicKeyRetrieval=true&useSSL=false
