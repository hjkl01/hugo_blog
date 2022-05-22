---
title: "proxy"
draft: true
---

## trojan-go
```sh
brew install trojan-go

修改 /usr/local/etc/trojan-go/config.json

brew service start trojan-go
```

## [glider](https://github.com/nadoo/glider/blob/master/config/glider.conf.example)
```sh
yay -S glider

glider -listen :1080 -forward trojan://password@ip:443

# with auth
glider -listen http://user:user_passwd@:61000 -forward trojan://password@ip:443
```

## trojan
```sh
https://github.com/trojan-gfw/trojan
### 机场推荐: https://portal.shadowsocks.nz/aff.php?aff=24252

### 部署 https://github.com/Jrohy/trojan

```

# 旧
## server:
```sh
install libsodium
pip install shadowsocks
pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip -U

# path : /etc/shadowsocks.json
{
    "server":"0.0.0.0",
    "port_password": {
        "8000": "password"
    },
    "timeout":300,
    "method":"chacha20-ietf-poly1305",
    "fast_open":true,
    "pid-file": "/path/ss.pid",
    "log-file": "/path/ss.log"
}

(sudo) ssserver -c /etc/shadowsocks.json -d start
sudo ssserver -d stop

https://github.com/shadowsocks/shadowsocks/wiki/Shadowsocks-%E4%BD%BF%E7%94%A8%E8%AF%B4%E6%98%8E
```


#### 开启bbr
```sh
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh

chmod +x bbr.sh

./bbr.sh

sysctl net.ipv4.tcp_available_congestion_control
#返回值一般为：
#net.ipv4.tcp_available_congestion_control = bbr cubic reno

sysctl net.ipv4.tcp_congestion_control
#返回值一般为：
#net.ipv4.tcp_congestion_control = bbr

sysctl net.core.default_qdisc
#返回值一般为：
#net.core.default_qdisc = fq

lsmod | grep bbr
#返回值有 tcp_bbr 模块即说明bbr已启动。
```

## client
#### ubuntu下使用， Mac下载 https://github.com/shadowsocks/ShadowsocksX-NG/releases/
```sh
pip install shadowsocks

path : ~/.shadowsocks/shadowsocks.json

{
  "server":"my_server_ip",
  "server_port":my_server_port,
  "password":"my_password",
  "local_address": "127.0.0.1",
  "local_port":1080,
  "timeout":300,
  "method":"chacha20-ietf-poly1305",
  "fast_open":true,
  "pid-file": "/path",
  "log-file": "/path"
}

sslocal -c ~/.shadowsocks/shadowsocks.json -d start
可先在系统设置里设置全局代理，在浏览器里安装 https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif
```
