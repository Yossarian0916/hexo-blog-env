---
title: Install PostgreSQL 12
tags:
  - postgresql
date: 2020-12-13 10:07:13
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

   

## Reference

[How To Install PostgreSQL 12 on Ubuntu 20.04/18.04/16.04](https://computingforgeeks.com/install-postgresql-12-on-ubuntu/)