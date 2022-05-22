---
title: "gitea"
draft: true
---

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
