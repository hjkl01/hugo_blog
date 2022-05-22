---
title: "arch pppoe"
draft: true
---

### pppoe 拨号

```sh
yay -S --noconfirm rp-pppoe

ifconfig # 查看对应的网卡
sudo ifconfig 网卡 down # 停止网卡服务
sudo pppoe-setup # 输入对应的用户名密码等信息
sudo pppoe-start

sudo route delete default
sudo route add default ppp0 # ppp0可能是其他名字 ifconfig查看

sudo nvim /etc/resolvconf.conf # 更新name_servers
sudo resolvconf -u # 更新DNS
```
