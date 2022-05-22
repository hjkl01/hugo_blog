---
title: "vnc"
draft: true
---

### 服务端安装

```sh
# ubuntu 
  sudo apt-get install x11vnc

  x11vnc -storepasswd

  x11vnc -auth guess -once -loop -noxdamage -repeat -rfbauth ~/.vnc/passwd -rfbport 5900 -shared

  x11vnc -forever

  https://www.realvnc.com/en/connect/download/viewer/


# arch 
    yay -S x11vnc net-tools
    update -> /etc/gdm/custom.conf:
        WaylandEnable=false

    x11vnc -wait 50 -noxdamage -passwd PASSWORD -display :0 -forever -o /var/log/x11vnc.log -bg 
```

### 客户端
> https://www.realvnc.com/

```sh
# 错误
# display_server_not_supported

# /etc/gdm3/custom.conf
[daemon]
    # Enabling automatic login
    AutomaticLoginEnable=true
    AutomaticLogin=$USERNAME
    
sudo systemctl restart gdm.service
```
