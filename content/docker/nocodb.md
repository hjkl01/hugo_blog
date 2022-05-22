---
title: "nocodb"
draft: true
---

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
