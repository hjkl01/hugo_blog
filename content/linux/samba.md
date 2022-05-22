---
title: "samba"
draft: true
---

### 在arch 中安装参考 
```sh
https://wiki.archlinux.org/title/samba
```

### 在ubuntu中安装
```sh 
sudo apt-get install samba
sudo useradd xxx
sudo smbpasswd -a xxx
sudo vi /etc/samba/smb.conf

#### 配置内加入以下内容
[dev]
comment = dev
path = /var/dev
valid user = xxx
write list = xxx
create mask = 0664
directory mask = 0775
force user = xxx
force group = xxx
public = no
available = yes
browseable = yes
security = user
### 重启samba【sudo service smbd restart】
```
