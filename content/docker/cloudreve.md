---
title: "网盘 cloudreve"
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
