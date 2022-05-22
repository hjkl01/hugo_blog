---
title: "pgadmin4"
draft: true
---

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
