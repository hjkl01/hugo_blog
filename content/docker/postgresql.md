---
title: "postgresql"
draft: true
---

```sh
version: '3'
services:
  db:
    image: postgres:10-alpine
    restart: always
    ports:
      - 5432:5432
    environment:
      POSTGRES_PASSWORD: 'password'
      POSTGRES_USER: 'user'
      POSTGRES_DB: 'postgres'
      PGDATA: '/var/lib/postgresql/data'
    volumes:
      - ./postgres:/var/lib/postgresql/data

  admin:
    image: adminer
    restart: always
    depends_on: 
      - db
    ports:
      - 8080:8080

# 可视化工具推荐
docker run -d -e SESSIONS=true -p 8081:8081 sosedoff/pgweb

# mac
tableplus

# 在linux 中安装
sudo apt-get install postgresql-client
sudo apt-get install postgresql
# sudo apt-get install pgadmin3
# pgcli

sudo adduser dbuser
sudo su - postgres
# sudo -u postgres psql
psql
\password postgres
CREATE USER dbuser WITH PASSWORD 'password';
CREATE DATABASE exampledb OWNER dbuser;
GRANT ALL PRIVILEGES ON DATABASE exampledb to dbuser;

psql -U dbuser -d exampledb -h 127.0.0.1 -p 5432
psql exampledb
# psql exampledb < exampledb.sql  #恢复外部数据
pg_dump -U username -h localhost databasename >> sqlfile.sql

sudo vi /etc/postgresql/9.5/main/postgresql.conf
sudo gedit /etc/postgresql/9.5/main/pg_hba.conf		host all all 0.0.0.0/0 md5
sudo /etc/init.d/postgresql restart

# 查询有外键的数据
select count(*) from "case" where court_id in (select id from court where province ='');

# 导出数据结构
python -m pwiz -e postgresql -u user -P db > model.py
python -m pwiz -e mysql -H 192.168.1.x -u root -P dbname > model.py
```
