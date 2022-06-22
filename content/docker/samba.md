---
title: "samba && nfs"
draft: true
---

### samba
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

### nfs
```sh 
version: "2.1"
services:
  # https://hub.docker.com/r/itsthenetwork/nfs-server-alpine
  nfs:
    image: itsthenetwork/nfs-server-alpine:12
    container_name: nfs
    restart: unless-stopped
    privileged: true
    environment:
      - SHARED_DIRECTORY=/data
    volumes:
      - ./data/jellyfin/movies:/data
    ports:
      - 2049:2049
```
