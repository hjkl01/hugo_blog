---
title: "nginx"
draft: true
---

### 推荐在线配置
    https://digitalocean.github.io/nginxconfig.io/?global.app.lang=zhCN
    {{% button href="https://digitalocean.github.io/nginxconfig.io/" icon="fas fa-download" %}} click it {{% /button %}}

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
    
    # django
    location /static/ {
        alias /home/ubuntu/djangoapp/static/; 
    }

    location /media/ {
        alias /home/ubuntu/djangoapp/media/; 
    }

    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header X-Forwarded-Host $server_name;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_redirect off;
        add_header P3P 'CP="ALL DSP COR PSAa OUR NOR ONL UNI COM NAV"';
        add_header Access-Control-Allow-Origin *;
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
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
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

### 限流
#### 正常限流
```shell
# nginx.conf
http {
    limit_req_zone $binary_remote_addr zone=myRateLimit:10m rate=10r/s;
}
# server
server {
        location / {
            limit_req zone=myRateLimit;
            proxy_pass http://my_upstream;
        }
}
```
- key ：定义限流对象，binary_remote_addr 是一种key，表示基于 remote_addr(客户端IP) 来做限流，binary_ 的目的是压缩内存占用量。
- zone：定义共享内存区来存储访问信息， myRateLimit:10m 表示一个大小为10M，名字为myRateLimit的内存区域。1M能存储16000 IP地址的访问信息，10M可以存储16W IP地址访问信息。
- rate 用于设置最大访问速率，rate=10r/s 表示每秒最多处理10个请求。Nginx 实际上以毫秒为粒度来跟踪请求信息，因此 10r/s 实际上是限制：每100毫秒处理一个请求。这意味着，自上一个请求处理完后，若后续100毫秒内又有请求到达，将拒绝处理该请求。

#### 处理突发流量
```shell
server {
        location / {
            limit_req zone=myRateLimit burst=20 nodelay;
            proxy_pass http://my_upstream;
        }
    }
```

#### 限制连接数
```shell
limit_conn_zone $binary_remote_addr zone=perip:10m;
limit_conn_zone $server_name zone=perserver:10m;
server {
    ...
    limit_conn perip 10;
    limit_conn perserver 100;
}
```
- limit_conn perip 10 作用的key 是 $binary_remote_addr，表示限制单个IP同时最多能持有10个连接。

- limit_conn perserver 100 作用的key是 $server_name，表示虚拟主机(server) 同时能处理并发连接的总数。

- 需要注意的是：只有当 request header 被后端server处理后，这个连接才进行计数。

#### 设置白名单
```shell
# nginx.conf
geo $limit {
    default 1;
    10.0.0.0/8 0;
    192.168.0.0/24 0;
    172.20.0.35 0;
}
map $limit $limit_key {
    0 "";
    1 $binary_remote_addr;
}
limit_req_zone $limit_key zone=myRateLimit:10m rate=10r/s;
```
- geo 对于白名单(子网或IP都可以) 将返回0，其他IP将返回1。

- map 将 $limit 转换为 $limit_key，如果是 $limit 是0(白名单)，则返回空字符串；如果是1，则返回客户端实际IP。

- limit_req_zone 限流的key不再使用 $binary_remote_addr，而是 $limit_key 来动态获取值。如果是白名单，limit_req_zone 的限流key则为空字符串，将不会限流；若不是白名单，将会对客户端真实IP进行限流。

#### 限制数据传输速度
```shell
location /flv/ {
    flv;
    limit_rate_after 20m;
    limit_rate       100k;
}
```
- 这个限制是针对每个请求的，表示客户端下载前20M时不限速，后续限制100kb/s。
