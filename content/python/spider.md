---
title: "spider-selenium"
draft: true
---

```python
import random
from selenium import webdriver
from time import sleep
from bs4 import BeautifulSoup as BS


options = webdriver.ChromeOptions()
UA = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/89.0.4389.128 Safari/537.36'
options.add_argument(f'user-agent={UA}')

options.add_experimental_option("excludeSwitches", ["enable-automation"])
options.add_experimental_option('useAutomationExtension', False)

# 没有配置环境变量的话需要填写Chromedriver的路径：webdriver.Chrome(executable_path="***")
driver = webdriver.Chrome(options=options)
driver.maximize_window()

# 去掉window.navigator.webdriver字段，防止被检测出是使用selenium
driver.execute_cdp_cmd("Page.addScriptToEvaluateOnNewDocument", {
  "source": """
    Object.defineProperty(navigator, 'webdriver', {
      get: () => undefined
    })
  """
})
```
