---
title: Install PostgreSQL 12
date: 2020-11-20
tags:
  - postgresql
---


# PostgreSQL 12 Installation

1. add postgresql-12 repository

   ```bash
   $ wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
   $ echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee /etc/apt/sources.list.d/pgdg.list
   $ sudo apt update
   ```

   the repository contains packages including: 

   - postgresql-client
   - postgresql
   - libpq-dev
   - postgresql-server-dev
   - pgadmin packages

2. install postgresql server and client

   ```bash
   $ sudo apt -y install postgresql-12 postgresql-client-12
   ```

3. postgresql first-time login

   Switch to user postgres to login database postgres. Postgresql has by default 3 databases "postgres, template 0, template 1".
   
   ```bash
$ sudo -u postgres psql postgres
   ```

   create a test database and a test user
   
   (CREATE USER = CREATE ROLE + LOGIN permission)
   
   ```plsql
   postgres=# CREATE DATABASE mytestdb;
   CREATE DATABASE
   postgres=# CREATE USER mytestuser WITH ENCRYPTED PASSWORD 'MyStr0ngP@ss';
CREATE ROLE
   postgres=# GRANT ALL PRIVILEGES ON DATABASE mytestdb TO mytestuser;
GRANT
   ```

   now list all databases
   
   ```plsql
postgres=# \l
   ```

   connect to newly-created database
   
   ```plsql
postgres=# \c mytestdb
   ```
   

4. "Peer authentication failed for user XXX"

   edit in file `/etc/postgresql/12/main/pg_hba.conf`, replace `peer` with `md5`

   - `peer` means it will trust the authenticity of UNIX user hence does not prompt for the password.
   - `md5` means it will always ask for a password, and validate it after hashing with `MD5`

   from

   ```plsql
   # TYPE  DATABASE        USER            ADDRESS                 METHOD
   
   # "local" is for Unix domain socket connections only
   local   all             all                                     peer
   # IPv4 local connections:
   host    all             all             127.0.0.1/32            md5
   ```

   to

   ```plsql
   # TYPE  DATABASE        USER            ADDRESS                 METHOD
   
   # "local" is for Unix domain socket connections only
   local   all             all                                     md5
   # IPv4 local connections:
   host    all             all             127.0.0.1/32            md5
   ```

   then restart postgresql

   `sudo systemctl restart postgresql.service`

   

## Reference

[How To Install PostgreSQL 12 on Ubuntu 20.04/18.04/16.04](https://computingforgeeks.com/install-postgresql-12-on-ubuntu/)