---
title: "nginx"
draft: true
---

### 推荐在线配置
> https://digitalocean.github.io/nginxconfig.io/?global.app.lang=zhCN

### https证书

```sh
# 安装certbot
yay -S --noconfirm certbot
sudo certbot certonly --standalone -d domain
sudo certbot certonly -d domain --webroot -w /html/filepath/

sudo crontab -e 
15 2 * */2 * systemctl stop nginx.service && certbot renew && systemctl restart nginx.service
```

### 基本配置
```sh
server {
    listen 80;
    listen [::]:80;
    server_name blog.hjkl01.cn;

    # 静态文件
    root /html/github;
    location / {
       index index.html index.htm;
    }

    # 转发端口
    location / {
        proxy_pass http://127.0.0.1:8000/;
    }

    # 重定向
    return 301 https://$host$request_uri;
    # rewrite ^(.*)$ https://blog.hjkl01.cn; #将所有HTTP请求通过rewrite指令重定向到HTTPS。
}
```

### ssl
```sh
server {
    listen 443 ssl;
    server_name blog.hjkl01.cn;

    ssl_certificate /etc/letsencrypt/live/blog.hjkl01.cn/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/blog.hjkl01.cn/privkey.pem;
    ssl_trusted_certificate /etc/letsencrypt/live/blog.hjkl01.cn/chain.pem;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2; 
    ssl_prefer_server_ciphers on;

    # 静态文件
    location / {
        root /html/github;  #站点目录。
        index index.html index.htm;
    }

    # 转发端口
    location / {
        proxy_pass http://127.0.0.1:8080/;
    }

}
```

### 转发mongo端口(TCP)
```sh
stream {
    server {
        listen  <your incoming Mongo TCP port>;
        proxy_connect_timeout 1s;
        proxy_timeout 3s;
        proxy_pass    stream_mongo_backend;
    }

    upstream stream_mongo_backend {
      server <localhost:your local Mongo TCP port>;
  }
}
```
