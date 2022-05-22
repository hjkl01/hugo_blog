---
title: "npm"
draft: true
---

## yarn/npm 设置国内源


```sh
# 临时
npm --registry https://registry.npm.taobao.org install express
```

```sh
# 写入文件
yarn config set registry 'https://registry.npm.taobao.org'
yarn config get registry

npm config set registry https://registry.npm.taobao.org
npm config get registry
```

#### use yrm
```sh
npm install -g yrm
# yarn global add yrm
yrm ls
yrm use taobao
yrm test
```
