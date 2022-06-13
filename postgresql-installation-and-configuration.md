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

_service management_

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
sudo systemctl status postgresql
```

_How do I login and authenticate to Postgresql after a fresh install?_

```bash
sudo -u postgres psql postgres
```
