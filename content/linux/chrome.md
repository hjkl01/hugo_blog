---
title: "chrome"
draft: true
---

### web scraper
```sh
https://chrome.google.com/webstore/detail/web-scraper-free-web-scra/jnhgnonknehpejjnehehllkliplmbmhn
```

```json
{"_id":"test","startUrl":["https://list.jd.com/list.html?cat=9987,653,655","https://list.jd.com/list.html?cat=9987,653,655&page=2&s=58&click=0"],"selectors":[{"delay":0,"id":"name","multiple":true,"parentSelectors":["_root"],"selector":"#J_goodsList > ul > li","type":"SelectorElement"},{"delay":0,"id":"sku_name","multiple":false,"parentSelectors":["name"],"regex":"","selector":"a em","type":"SelectorText"},{"delay":0,"id":"price","multiple":false,"parentSelectors":["name"],"regex":"","selector":" div > div.p-price > strong > i","type":"SelectorText"},{"delay":0,"id":"comments","multiple":false,"parentSelectors":["name"],"regex":"","selector":".p-commit strong a","type":"SelectorText"},{"delay":0,"id":"shop","multiple":false,"parentSelectors":["name"],"regex":"","selector":".p-shop span a","type":"SelectorText"}]}
```

```json
{"_id":"tianmao","startUrl":["https://list.tmall.com/search_product.htm?spm=a220m.1000858.0.0.219b2a680geXJ0&s=120&q=%CA%D6%BB%FA&sort=s&style=g&from=mallfp..pc_1_searchbutton&active=1&type=pc#J_Filter"],"selectors":[{"delay":0,"id":"sku","multiple":true,"parentSelectors":["_root"],"selector":"#J_ItemList .product","type":"SelectorElement"},{"delay":0,"id":"name","multiple":false,"parentSelectors":["sku"],"regex":"","selector":".productTitle a","type":"SelectorText"},{"delay":0,"id":"price","multiple":false,"parentSelectors":["sku"],"regex":"","selector":".productPrice em","type":"SelectorText"},{"delay":0,"id":"shop","multiple":false,"parentSelectors":["sku"],"regex":"","selector":"a.productShop-name","type":"SelectorText"},{"delay":0,"id":"sales","multiple":false,"parentSelectors":["sku"],"regex":"","selector":"span em","type":"SelectorText"},{"delay":0,"id":"comments","multiple":false,"parentSelectors":["sku"],"regex":"","selector":"span a[data-p]","type":"SelectorText"}]}
```

### chrome devtools

```sh
var result = $x('xpath').map(function(i){return i.textContent} ); copy(result);
```
