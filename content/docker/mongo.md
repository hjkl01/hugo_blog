---
title: "mongoDB"
draft: true
---

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
