---
title: "supervisor "
draft: true
---

#### 后缀为conf或ini
```sh
[program:frp_ssh]

command     = /home/user/frp/frpc -c /home/user/frp/config.ini 
directory = /home/user/somedir
user = user
autostart = true
autorestart = true
startsecs   = 3

redirect_stderr         = true
stdout_logfile_maxbytes = 50MB
stdout_logfile_backups  = 10
stdout_logfile          = /home/user/frp/log
```
