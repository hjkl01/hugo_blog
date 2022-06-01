---
title: "API tool: httpbin && hoppscotch"
draft: true
---

```sh
docker run -p 80:80 kennethreitz/httpbin
http://127.0.0.1/get?show_env=1
```

```sh
docker run --rm --name hoppscotch -p 3000:3000 hoppscotch/hoppscotch:latest
```
