---
title: "arch"
draft: true
---

### wiki
> link: https://gitlab.com/drizzt/vps2arch/-/wikis/Tested-VPS-Providers

> #### ⚠️ 设置root的密码

> #### wget https://tinyurl.com/vps2arch 也会被重定向到以下 url
```sh
wget https://gitlab.com/drizzt/vps2arch/-/raw/master/vps2arch
```

> #### 启动脚本
```sh
sh ./vps2arch

# 当你从脚本默认的源下载速度较慢的时候，可以使用 -m 参数指定源，例如
sudo sh ./vps2arch -m https://mirrors.neusoft.edu.cn/archlinux/
sync ; reboot -f
```

### 源设置 /etc/pacman.d/mirrorlist
```sh 
# 清华大学
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
# 南京大学
Server = https://mirror.nju.edu.cn/archlinux/$repo/os/$arch
# or
sudo pacman-mirrors -i -c China

sudo pacman -Syy
```

### yay
```sh
pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay-bin.git
cd yay-bin
makepkg -si
```

### 使用 ntp
```sh
pacman -S ntp
timedatectl set-ntp true
# 设置时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

### utf8

> 编辑 /etc/locale.gen 取消一下行的注释（你可能需要一个编辑器，如 vim，请自行安装）
```sh
en_GB.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```
> 执行 locale-gen

> 创建 /etc/locale.conf 并编辑 LANG 这一 变量，比如：

> LANG=zh_CN.UTF-8

### 我的配置
```
https://github.com/hjkl01/dotfiles
```

### 输入法
``` sh
sudo pacman -Rs $(pacman -Qsq fcitx)

sudo pacman -S fcitx5 fcitx5-configtool fcitx5-qt fcitx5-gtk fcitx5-chinese-addons

vi ~/.xprofile

export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
fcitx5 &

```
