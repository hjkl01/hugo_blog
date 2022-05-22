---
title: "电影播放 embyserver 和 下载管理 aria2"
draft: true
---

```sh
version: "3.8"

services:

  Aria2-Pro:
    container_name: aria2-pro
    image: p3terx/aria2-pro
    environment:
      - PUID=65534
      - PGID=65534
      - UMASK_SET=022
      - RPC_SECRET=update_me
      - RPC_PORT=6800
      - LISTEN_PORT=6888
      - DISK_CACHE=64M
      - IPV6_MODE=true
      - UPDATE_TRACKERS=true
      - CUSTOM_TRACKER_URL=
      - TZ=Asia/Shanghai
    volumes:
      - ${PWD}/data/aria2-config:/config
      - ${PWD}/data/embyserver/movies:/downloads
# If you use host network mode, then no port mapping is required.
# This is the easiest way to use IPv6 networks.
    # network_mode: host
#    network_mode: bridge
    ports:
     - 6800:6800
     - 6888:6888
     - 6888:6888/udp
    restart: unless-stopped
# Since Aria2 will continue to generate logs, limit the log size to 1M to prevent your hard disk from running out of space.
    logging:
      driver: json-file
      options:
        max-size: 1m

# AriaNg is just a static web page, usually you only need to deploy on a single host.
  AriaNg:
    container_name: ariang
    image: p3terx/ariang
    command: --port 6880 --ipv6
    # network_mode: host
#    network_mode: bridge
    ports:
      - 192.168.50.4:6880:6880
    restart: unless-stopped
    logging:
      driver: json-file
      options:
        max-size: 1m
        
        
        
version: "2.3"
services:
  emby:
    image: emby/embyserver
    container_name: embyserver
    runtime: nvidia # Expose NVIDIA GPUs
    # network_mode: host # Enable DLNA and Wake-on-Lan
    environment:
      - UID=1000 # The UID to run emby as (default: 2)
      - GID=100 # The GID to run emby as (default 2)
      - GIDLIST=100 # A comma-separated list of additional GIDs to run emby as (default: 2)
    volumes:
      - ./data/embyserver/programdata:/config # Configuration directory
      - ./data/embyserver/tvshows:/mnt/share1 # Media directory
      - ./data/embyserver/movies:/media
    ports:
      - 8096:8096 # HTTP port
      # - 8920:8920 # HTTPS port
    devices:
      - /dev/dri:/dev/dri # VAAPI/NVDEC/NVENC render nodes
    #   - /dev/vchiq:/dev/vchiq # MMAL/OMX on Raspberry Pi
    restart: unless-stopped      
```


