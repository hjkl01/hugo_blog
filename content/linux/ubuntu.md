---
title: "ubuntu"
draft: true
---

#### 中文乱码
```sh
sudo apt-get -y install language-pack-zh-hans

#写入 ~/.zshrc:
LC_ALL=zh_CN.utf8
export LC_ALL

source ~/.zshrc
```


#### 配置静态IP
```sh
path: `/etc/network/interface`

auto eth1
iface eth1 inet static
address 192.168.56.xx
netmask 255.255.255.0
network 192.168.56.0
broadcast 192.168.56.255

if possible, 配置路由器中的dhcp.
```

#### ip route
```sh
sudo route add -net 66.1.254.0/24 gw 66.16.62.254 enp4s0
sudo ip addr flush dev enp4s0
```

#### 安装Ubuntu 分区
```sh
efi
swap	交换空间
/ ext4
/boot	200M左右	ext4	
/tmp	5G左右	ext4	
```

#### 手动配置swap内存
```sh

sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

sudo vim /etc/fstab
/swapfile none swap sw 0 0
```
