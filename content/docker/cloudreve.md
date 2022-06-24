---
title: "cloudreve 网盘"
draft: true
---

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

```sh 
# nginx config 
server {
    listen 15212 ssl http2;
    listen [::]:15212 ssl http2;
    server_name hjkl01.cn;

    ssl_certificate  /etc/nginx/cert/hjkl01.cn_nginx/hjkl01.cn_bundle.crt;
    ssl_certificate_key /etc/nginx/cert/hjkl01.cn_nginx/hjkl01.cn.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;

    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header Host $http_host;
    proxy_redirect off;
    client_max_body_size 90000m;

    location / {
        proxy_pass http://192.168.50.4:5212;
    }
}
```
