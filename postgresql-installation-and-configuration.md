postgresql-installation-and-configuration.md
---

|server|service|port|
|---|---|---|
|ubuntu| postgresql|5432|

* The default configuration file is located here

`/etc/postgresql/14/main/postgresql.conf`


_installation_

```bash
sudo apt-get update 
sudo apt-get install postgresql postgresql-contrib -y

```

_uninstall_

```bash

sudo apt-get --purge remove postgresql
sudo apt autoremove

```
* using this command  `dpkg -l |grep postgres`  remove another postgres files

---

_To check running status_

```bash

sudo systemctl status postgresql.service

```
To check Port Listen

```bash

sudo ss -tulpn | grep 5432

```
---

_login commands_

login Default user 

```bash

^
$ sudo -u postgres psql

```

* Note - `postgres` is default sudo user

_Set Password_

```bash
#inside the psql

alter user postgres with password 'your_password';

```

_To remote login_

```bash

psql -h <host> -p <port> -U <username> <database>

```
---

_database creation_


```bash

$ sudo -u postgres createdb dodo

or
#inside psql

createdb dodo;

```
* To delete db --> `drop db_name`
* To list the users --> `\l`
* To list connect the DB --> `\c`
* To switch the DB --> `\c db_name`
* To list the tables--> `\dt`
* To describe the table --> `\d tablename`

---

_user creation and role assignment_

TO create the new user 

```bash

$ sudo -u postgres createuser --interactive
#Enter name of role to add: dodo
#Shall the new role be a superuser? (y/n) n
#Shall the new role be allowed to create databases? (y/n) n
#Shall the new role be allowed to create more new roles? (y/n) n

or 
#insede psql

CREATE ROLE username WITH PASSWORD 'very-strong-password' LOGIN;

```

change superuser role

```bash

ALTER USER dodo WITH SUPERUSER;

```
* To delete user --> `drop user_name.`
* To delete db --> `drop db_name`
* To list the users --> `\du`

---

_backup_

```bash

 pg_dump db1 > db1.sql

```
_RemoteBackup_

```bash

pg_dump -U tecmint -h <host> -p 5432 db1 > db1.sql

```
---

_restore_


```bash

 pg tecmintdb < tecmintdb.sql

```


replication


```bash


```
