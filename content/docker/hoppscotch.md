---
title: "API tool: httpbin && hoppscotch"
draft: true
---

```sh
version: '3.1'

services:

  httpbin:
    image: kennethreitz/httpbin
    container_name: httpbin
    restart: always
    ports:
      - "127.0.0.1:7999:80"

  hoppscotch:
    image: hoppscotch/hoppscotch:latest
    container_name: hoppscotch
    restart: always
    ports:
      - "127.0.0.1:3000:3000"
```
