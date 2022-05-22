---
title: "gops"
draft: true
---

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
