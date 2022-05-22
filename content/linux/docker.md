---
title: "docker"
draft: true
---

## install
```sh
wget -qO- https://get.docker.com/ | sh
sudo usermod -aG docker $USER

# 修改源 " lang:sh %}
# path: /etc/docker/daemon.json
{
  "registry-mirrors": ["http://hub-mirror.c.163.com"]
}
```

## tools
> ctop
>
> lazydocker
```sh
docker run -it -v /var/run/docker.sock:/var/run/docker.sock -v /tmp:/.config/jesseduffield/lazydocker lazyteam/lazydocker
```


## docker-compose.yml 

#### ddns
```sh
version: '3.1'
services:
  ddns_go:
    image: jeessy/ddns-go
    restart: unless-stopped
    network_mode: "host"
    volumes:
      - ./data/ddns:/root
  # port: 9876
  
  ddns:
    image: sanjusss/aliyun-ddns
    restart: always
    network_mode: "host"
    environment:
      #  https://usercenter.console.aliyun.com/
      AKID: 
      AKSCT: 
      DOMAIN: 
      REDO: 30
      TTL: 600
      TIMEZONE: 8.0
      TYPE: A,AAAA
```

#### gops
```sh
version: '3'
services:
  db:
    image: postgres:11-alpine
    restart: unless-stopped
    environment:
      POSTGRES_USER: 'gogs'
      POSTGRES_PASSWORD: 'gogs'
      POSTGRES_DB: 'postgres'
    ports:
      - "5432:5432"
    networks:
      - gogs_net
    volumes:
      - ./data/postgres_data:/var/lib/postgresql/data

  gogs:
    image: gogs/gogs:latest
    networks:
      - gogs_net
    depends_on:
      - db
    links:
      - db
    ports:
      - "10022:22"
      - "10080:3000"
    restart: unless-stopped
    volumes:
      - ./data/gogs_data:/data:rw

networks:
  gogs_net:
    driver: bridge
```


#### httpbin
```sh
docker run -p 80:80 kennethreitz/httpbin
http://127.0.0.1/get?show_env=1
```


#### hoppscotch
```sh
docker run --rm --name hoppscotch -p 3000:3000 hoppscotch/hoppscotch:latest
```


#### mongoDB
```sh
# .env
MONGO_ROOT_USER=username
MONGO_ROOT_PASSWORD=password
MONGODB_URL=mongodb://username:password@mongo:27017
  
version: '3.1'

services:

  mongo:
    image: mongo
    restart: always
    ports:
      - 27017:27017
    environment:
      - MONGO_INITDB_ROOT_USERNAME=${MONGO_ROOT_USER}
      - MONGO_INITDB_ROOT_PASSWORD=${MONGO_ROOT_PASSWORD}
    volumes:
      - ./data/mongo:/data/db

  mongo-express:
    image: mongo-express
    restart: always
    ports:
      - 8081:8081
    links:
      - mongo
    environment:
      - ME_CONFIG_MONGODB_URL=${MONGODB_URL}
      - ME_CONFIG_BASICAUTH_USERNAME=${MONGO_ROOT_USER}
      - ME_CONFIG_BASICAUTH_PASSWORD=${MONGO_ROOT_PASSWORD}
```

#### MySQL
```sh
version: '3.1'

services:
  db:
    # We use a mariadb image which supports both amd64 & arm64 architecture
    image: mariadb:10.6.4-focal
    # If you really want to use MySQL, uncomment the following line
    #image: mysql:8.0.27
    command: '--default-authentication-plugin=mysql_native_password'
    volumes:
      - ./data/mysql:/var/lib/mysql
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=password
      - MYSQL_DATABASE=data
      - MYSQL_USER=user
      - MYSQL_PASSWORD=password
      - MYSQL_ROOT_HOST=%
    ports:
      - 3306:3306

# linux配置
/etc/mysql/my.cnf:

[client]
default-character-set = utf8

[mysqld]
default-storage-engine = INNODB
character-set-server = utf8
collation-server = utf8_general_ci


# others
protected-mode yes

mysqldump -u root -p --all-databases > data.txt
source data.txt

create database testdb default charset utf8 COLLATE utf8_general_ci;

http://docs.peewee-orm.com/en/latest/peewee/playhouse.html#pwiz-a-model-generator
```


# pgadmin4
```sh
version: '3.5'
services:
  pgadmin:
    container_name: pgadmin4_container
    image: dpage/pgadmin4
    restart: always
    environment:
      PGADMIN_DEFAULT_EMAIL: xx@xx.com
      PGADMIN_DEFAULT_PASSWORD: password
    ports:
      - "80:80"
```

# PostgreSQL
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

#### Postwoman
```sh
docker run -p 3000:3000 liyasthomas/postwoman:latest
```

#### Redis
```sh
# redis 及其持久化
# redis.conf
requirepass 123456
appendonly yes
daemonize no

version: '3'
services:
  redis:
      image: redis
      restart: unless-stopped
      # command: redis-server --requirepass 123456
      command: redis-server /usr/local/etc/redis/redis.conf
      ports:
        - 6379:6379
      volumes:
        - ./redis.conf:/usr/local/etc/redis/redis.conf
        - ./data/redis:/data/
```


#### 书签管理器 shiori
```sh
# 原链接 https://github.com/go-shiori/shiori/
# loginuser: shiori 	
# passwd: gopher
version: "2.1"
services:
  shiori:
    image: nicholaswilde/shiori:latest
    container_name: shiori-default
    environment:
      TZ: Asia/Shanghai
      PUID: 1000
      PGID: 1000
      SHIORI_PG_HOST: db
      SHIORI_PG_PORT: 5432
      SHIORI_PG_USER: user
      SHIORI_PG_PASS: password
      SHIORI_PG_NAME: ""
    ports:
      - 8080:8080
    restart: unless-stopped
    volumes:
      - ./data/shiori:/data
    depends_on:
      - db
  db:
    image: postgres
    restart: always
    environment:
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - ./data/shiori_postgres:/var/lib/postgresql/data
```

#### Gitea
```sh
# web管理界面里 默认端口3000和22不要改
# example: ssh://git@git.hjkl01.cn:58001/user/project.git

version: "3"

networks:
  gitea:
    external: false

services:
  server:
    image: gitea/gitea:1.15.4
    container_name: gitea
    environment:
      - USER_UID=1000
      - USER_GID=1000
      - DB_TYPE=postgres
      - DB_HOST=db:5432
      - DB_NAME=gitea
      - DB_USER=username
      - DB_PASSWD=password
    restart: always
    networks:
      - gitea
    volumes:
      - ./data/gitea/data:/data
    ports:
      - "58000:3000"
      - "58001:22"
    depends_on:
      - db

  db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=username
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=gitea
    networks:
      - gitea
    volumes:
      - ./data/gitea/postgres:/var/lib/postgresql/data
```

#### samba
```sh
version: '3.4'

services:
  samba:
    image: dperson/samba
    environment:
      TZ: 'EST5EDT'
    networks:
      - default
    ports:
      - "137:137/udp"
      - "138:138/udp"
      - "139:139/tcp"
      - "445:445/tcp"
    read_only: true
    tmpfs:
      - /tmp
    restart: unless-stopped
    stdin_open: true
    tty: true
    volumes:
      - /data:/mnt:z
    command: '-s "Volume;/mnt;yes;no;no;USER" -u "USER;PASSWORD" -p'
   ```

#### automatic-api: nocodb
```sh
version: '3.3'

services:
  root_db:
    image: postgres:13-alpine
    restart: unless-stopped
    ports:
      - 5432:5432
    command: postgres -c 'max_connections=500'
    environment:
      POSTGRES_PASSWORD: 'passwd'
      POSTGRES_USER: 'username'
      POSTGRES_DB: 'postgres'
      PGDATA: '/var/lib/postgresql/data'
    healthcheck:
      test: pg_isready -U "$$POSTGRES_USER" -d "$$POSTGRES_DB"
      interval: 10s
      timeout: 2s
      retries: 10
    volumes:
      - ./data/nocodb_pg:/var/lib/postgresql/data

  nocodb:
    depends_on:
      root_db:
        condition: service_healthy
    image: nocodb/nocodb:latest
    ports:
      - "8080:8080"
      - "8081:8081"
      - "8082:8082"
      - "8083:8083"
    restart: always
    environment:
      NC_DB: "pg://root_db:5432?u=username&p=passwd&d=postgres"
```

#### automatic-api: prest
```sh
# https://github.com/prest/prest#test-using-docker
version: "3"
services:
  postgres:
    image: postgres
    volumes:
      - "./data/postgres:/var/lib/postgresql/data"
    environment:
      - POSTGRES_USER=prest
      - POSTGRES_DB=prest
      - POSTGRES_PASSWORD=prest
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready", "-U", "prest"]
      interval: 30s
      retries: 3
  prest:
    # use latest build - analyze the risk of using this version in production
    image: prest/prest
    links:
      - "postgres:postgres"
    environment:
      - PREST_DEBUG=false
      - PREST_AUTH_ENABLED=true
      - PREST_PG_HOST=postgres
      - PREST_PG_USER=prest
      - PREST_PG_PASS=prest
      - PREST_PG_DATABASE=prest
      - PREST_PG_PORT=5432
      - PREST_SSL_MODE=disable
    depends_on:
      postgres:
        condition: service_healthy
    ports:
      - "3000:3000"
```


#### 电影播放 embyserver 和 下载管理 aria2
```sh
version: "3.8"

services:

  Aria2-Pro:
    container_name: aria2-pro
    image: p3terx/aria2-pro
    environment:
      - PUID=65534
      - PGID=65534
      - UMASK_SET=022
      - RPC_SECRET=update_me
      - RPC_PORT=6800
      - LISTEN_PORT=6888
      - DISK_CACHE=64M
      - IPV6_MODE=true
      - UPDATE_TRACKERS=true
      - CUSTOM_TRACKER_URL=
      - TZ=Asia/Shanghai
    volumes:
      - ${PWD}/data/aria2-config:/config
      - ${PWD}/data/embyserver/movies:/downloads
# If you use host network mode, then no port mapping is required.
# This is the easiest way to use IPv6 networks.
    # network_mode: host
#    network_mode: bridge
    ports:
     - 6800:6800
     - 6888:6888
     - 6888:6888/udp
    restart: unless-stopped
# Since Aria2 will continue to generate logs, limit the log size to 1M to prevent your hard disk from running out of space.
    logging:
      driver: json-file
      options:
        max-size: 1m

# AriaNg is just a static web page, usually you only need to deploy on a single host.
  AriaNg:
    container_name: ariang
    image: p3terx/ariang
    command: --port 6880 --ipv6
    # network_mode: host
#    network_mode: bridge
    ports:
      - 192.168.50.4:6880:6880
    restart: unless-stopped
    logging:
      driver: json-file
      options:
        max-size: 1m
        
        
        
version: "2.3"
services:
  emby:
    image: emby/embyserver
    container_name: embyserver
    runtime: nvidia # Expose NVIDIA GPUs
    # network_mode: host # Enable DLNA and Wake-on-Lan
    environment:
      - UID=1000 # The UID to run emby as (default: 2)
      - GID=100 # The GID to run emby as (default 2)
      - GIDLIST=100 # A comma-separated list of additional GIDs to run emby as (default: 2)
    volumes:
      - ./data/embyserver/programdata:/config # Configuration directory
      - ./data/embyserver/tvshows:/mnt/share1 # Media directory
      - ./data/embyserver/movies:/media
    ports:
      - 8096:8096 # HTTP port
      # - 8920:8920 # HTTPS port
    devices:
      - /dev/dri:/dev/dri # VAAPI/NVDEC/NVENC render nodes
    #   - /dev/vchiq:/dev/vchiq # MMAL/OMX on Raspberry Pi
    restart: unless-stopped      
```

#### 网盘 cloudreve
```sh
# mkdir -vp cloudreve/{uploads,avatar} \
&& touch cloudreve/conf.ini \
&& touch cloudreve/cloudreve.db \
&& mkdir -p aria2/config \
&& mkdir -p data/aria2 \
&& chmod -R 777 data/aria2

version: "3.8"
services:
  cloudreve:
    container_name: cloudreve
    image: cloudreve/cloudreve:latest
    restart: unless-stopped
    ports:
      - "5212:5212"
    volumes:
      - ./data/aria2/downloads:/downloads
      - ./data/cloudreve/uploads:/cloudreve/uploads
      - ./data/cloudreve/conf.ini:/cloudreve/conf.ini
      - ./data/cloudreve/cloudreve.db:/cloudreve/cloudreve.db
      - ./data/cloudreve/avatar:/cloudreve/avatar
    depends_on:
      - Aria2-Pro

  Aria2-Pro:
    container_name: aria2-pro
    image: p3terx/aria2-pro
    environment:
      - PUID=65534
      - PGID=65534
      - UMASK_SET=022
      - RPC_SECRET=0b5c74bcc83fc89f29b6f9f4e8a812ef87f69258
      - RPC_PORT=6800
      - LISTEN_PORT=6888
      - DISK_CACHE=64M
      - IPV6_MODE=true
      - UPDATE_TRACKERS=true
      # - CUSTOM_TRACKER_URL=
      - TZ=Asia/Shanghai
    volumes:
      - ./data/aria2/config:/config
      - ./data/aria2/downloads:/downloads
    restart: unless-stopped
``` 
