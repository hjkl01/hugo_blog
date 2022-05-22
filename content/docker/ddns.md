---
title: "ddns"
draft: true
---

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
