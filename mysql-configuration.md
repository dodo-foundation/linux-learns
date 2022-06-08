## MySQL Install And create user Process (Ubuntu OS)
---

_What is Mysql_

MySQL creates a database for storing and manipulating data, defining the relationship of each table. Clients can make requests by typing specific SQL statements on MySQL. The server application will respond with the requested information and it will appear on the clients' side.


_Install Mysql Service_

```bash

sudo apt update
sudo apt-get install mysql-server -y

```

check running status on the server using 

```bash

sudo systemctl status mysql.service

```
---

**MYSQL Config process**

First, use this command to run security processes such as passwords, privileges, and so on.
 
 ```bash
 
 sudo mysql_secure_installation
 
```
* Change the password for root ? ((Press y|Y for Yes, any other key for No) :
* Remove anonymous users? (Press y|Y for Yes, any other key for No) : 
* Disallow root login remotely? (Press y|Y for Yes, any other key for No) : 
* Remove test database and access to it? (Press y|Y for Yes, any other key for No) : 
* Reload privilege tables now? (Press y|Y for Yes, any other key for No) : 
 

**_Mysql user,datebase,table creation & set privileges_**

We'll create a user, a database, a table, and permissions in this section. first of all enter with ur root user and password using this comment

```bash

mysql -u root -p

```

_create the user_

|username|password|server address| database |
|---|---|---|---|
|developer|password|localhost| apps |

```bash

CREATE USER 'developer'@'localhost' IDENTIFIED BY 'password';

```

_Grant ALL privileges to user_

```bash

GRANT ALL PRIVILEGES ON *.* TO 'developer'@'localhost' WITH GRANT OPTION;
FLUSH PRIVILEGES;

```

Restting the root password

```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
```

_Revoke Permmission_

```bash

REVOKE ALL PRIVILEGES ON *.* FROM 'developer'@'localhost';
or
REVOKE permission ON *.* FROM 'developer'@'localhost';
FLUSH PRIVILEGES;

```
_view the user permissions_

```bash

SHOW GRANTS FOR developer@localhost;

```

_list out the all mysql users_

```sql

SELECT user FROM mysql.user;

```

_Query statements_

* Select
* Insert
* Update
* delete

---

**Create database on Root user Set privilege to specify user**

Create a database on the Root User account and grant specific permissions to this account's users.

```bash

create database apps;
show databases;

```
_In the database, set user permissions_

```bash

GRANT all ON apps.* TO 'developer'@'localhost';
flush privileges;

```

_connected to particular database_

```bash

use apps;
#  To view the current database name
status;
# exit the database
exit;

```

**Create table in the databases**

|table name|
|---|
|users|

_create table_ 

```sql

use apps;
CREATE TABLE users ( 
 name varchar(50), 
 age int,
 address varchar(50)
);

```
 _view the table structure_
 
```sql

describe (table-name):`

```
_Insert values to the tables_

```sql

INSERT INTO users (name,MemberName,MemeberAge) VALUES (3, 'deva1', 2);

```
_Copy the row from One table to Another table_

first create the same table with column of Source table

```sql

INSERT INTO destination_table select * from source_table;

```

To copy the specific row to use this command

```bash

insert into destination_table select * from source_table where city='New York';

```

* To view the value you insert the table ---> `select * from (table-name);`
*To delete the table row ---> 'DELETE FROM Table_Name WHERE Address='pmk';'


_Add Column to the tables_

```bash

ALTER TABLE vendors
ADD COLUMN phone VARCHAR(15) AFTER name;
```

```bash

ALTER TABLE vendors
ADD COLUMN phone VARCHAR(15) FIRST;

```

to refer for more ---> https://dev.mysql.com/doc/refman/8.0/en/alter-table.html

* To view the value you insert the table ---> `select * from table name;`
*To delete the table column ---> 'ALTER TABLE table_name DROP COLUMN exisiting_column_name;'

 _To update the Table_
 
```bash

UPDATE employees SET email = 'mary.patterson@classicmodelcars.com' WHERE employeeNumber = 1056;
 
```
---

_Backup single database to Mysql_
 
 ```bash
 mysqldump -u root -p user1data > /home/deva/Mysql_bak/user1data_bak.sql
 ```

then create multiple databases backup in single comment

```bash
 mysqldump -u root -p --databases user1data user2data user3data > /home/deva/Mysql_bak/alldatabase_bak.sql
 ```

and Next create back in single Table

```bash
mysqldump -u root -p user1data A >/home/deva/Table_bak/single_bak.sql
```

and next Multiple table in single  command

```bash
mysqldump -u root -p user1data A B C >/home/deva/Table_bak/all_bak.sql
```

##Restore

in this section how to restore the bacup tables in single file

```bash
mysql -u root -p user3data < /home/deva/Table_bak/a3_bak.sql
```

Multiple table restore

```bash
mysql -u root -p user3data < /home/deva/Table_bak/all3_bak.sql
```

in this section restore single data base in mysql

in restore process first you remove privious databases using drop commant

create new empty database 

```bash
mysql -u root -p user1data < /home/deva/Mysql_bak/user1data_bak.sql
```

in this section restore single data base in mysql
**in this commant use is base terminal not mysql**

```bash
mysql -u root -p uset1 < /home/deva/Mysql_bak/alldatabase_bak.sql
```

restore single and mutiple restore process cmd is same.

to check the restore database table record
```bash
select count(*) from data_2;
```
---

## MySql backup database file transfer one server to another server

first enter the mysql console
backup ur need database using this command

```bash
 mysqldump -u root -p database name > file path/user1data_bak.sql
 ```
and tranfer ur file to another server using this command

```bash
scp source_file_name username@destination_host:destination_folder
```
once file transfered then next restore using this command **using command in base terminal not mysql console**

```bash
mysql -u root -p databasename < filepath/backup.sql;
```

to check the restore database table record
```bash
select count(*) from data_2;
```
