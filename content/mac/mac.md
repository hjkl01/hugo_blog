---
title: "Mac Config && Softwares "
draft: true
---

### 刷新DNS
```sh
sudo dscacheutil -flushcache
```

### Mac终端录屏

```sh
# https://github.com/icholy/ttygif
brew install ttygif
ttyrec myrecording

# On OSX optionally you can set a -f flag which will bypass cropping which is needed for terminal apps which aren't full screen. Both standard Terminal and iTerm apps are supported.
ttygif myrecording -f
```

### 在touch bar 上显示歌词-LyricsX

https://github.com/ddddxxx/LyricsX


```json
# mtmr 配置
[{
        "type": "dnd",
        "align": "left",
        "width": 38
    },
    {
        "type": "brightnessDown",
        "width": 32,
        "bordered": false,
        "align": "left"
    },
    {
        "type": "brightnessUp",
        "width": 32,
        "bordered": false,
        "align": "left"
    },
    {
        "type": "dock",
        "align": "left",
        "width": 420
    },
    {
        "type": "group",
        "align": "right",
        "bordered": false,
        "title": "Media",
        "items": [{
                "type": "close",
                "bordered": false,
                "align": "left"
            },
            {
                "type": "brightnessDown",
                "bordered": false,
                "align": "left",
                "width": 36
            },
            {
                "type": "brightness",
                "width": 200,
                "align": "left",
                "image": {
                    "base64": "iVBORw0KGgoAAAANSUhEUgAAAIAAAACAAQMAAAD58POIAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAGUExURffLOPfLNyaSVzUAAAACdFJOU/kBxOqnWgAAAbJJREFUSMfVljtyhDAQBVulQCFH4CgcDR1NR9ERFBKoeA5GfGddtkNvwFINFKP5tED22+Zxwviv6QVKfIEc/iNoF5gkpLIeYI8SUp4PsAUJiekADQntF6isQjvxCTrhAJlFqMMBeIH9BMsD7DAb2BhvYbIyNAOCZIWqYKGTpDZJFQu9EKVd44RxQRq3IrULWD62C8wSssWUZEsR0k6wcDOrJZmoBpMKI+s5qkBQCQOUJADVOECdOsDS0gDbsgHMfT4rVwHSrZQFIN5ABka8BgDgAeZ+BztBgvUEnSgVlhNsTFJjvoF5HAZorBpdYKAiSRbqNyBIUr6AjZMdPwO72R40MElS+wZUWA+wQ6LAYkFvdIhkmA+wQSDDdIAGAZ6A34H0x0fca11gBZZsIHSIfnE/5+NjCn/OuiuUB+/aunZwDeNayjXdTpDN0wlY+r1PfWu75nfj8RogN2JuCN2Y5qgMwTI0wGPUnQw6Qarx0sVNKA5Mn6VUL22lIbZoYitDbPmlvocc9Umfl2D7adz1reC3pF8av4m+DCenp/ndZuG3E7fhuC3pH2+vnz8V3MfE+bnxBTXuuIMTrLWHAAAAAElFTkSuQmCC"
                }
            },
            {
                "type": "brightnessUp",
                "bordered": false,
                "align": "left",
                "width": 36
            },
            {
                "type": "volumeDown",
                "bordered": false,
                "align": "left",
                "width": 36
            },
            {
                "type": "volume",
                "width": 200,
                "align": "left"
            },
            {
                "type": "volumeUp",
                "bordered": false,
                "align": "left",
                "width": 36
            },
            {
                "type": "previous",
                "bordered": false,
                "align": "center"
            },
            {
                "type": "play",
                "bordered": false,
                "align": "center"
            },
            {
                "type": "next",
                "bordered": false,
                "align": "center"
            }
        ]
    },
    {
        "type": "displaySleep",
        "width": 20,
        "align": "right",
        "bordered": false
    },
    {
        "type": "weather",
        "align": "right",
        "icon_type": "images",
        "api_key": "ca93a0bb8cdb428552660d83249e4bc9",
        "bordered": false
    },
    {
        "type": "play",
        "align": "right",
        "width": 38
    },
    // {
    //     "type": "volumeDown",
    //     "bordered": false,
    //     "align": "right",
    //     "width": 28
    // },
    // {
    //     "type": "volumeUp",
    //     "bordered": false,
    //     "align": "right",
    //     "width": 28
    // },
    {
        "type": "mute",
        "bordered": false,
        "align": "right"
    },
    // {
    //     "type": "battery",
    //     "align": "right",
    //     "bordered": false
    // },
    // {
    //     "type": "timeButton",
    //     "formatTemplate": "HH:mm",
    //     "align": "right",
    //     "bordered": false,
    //     "longAction": "shellScript",
    //     "longExecutablePath": "/usr/bin/pmset",
    //     "longShellArguments": ["sleepnow"]
    // }
]
```
