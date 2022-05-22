---
title: "settings.py"
draft: true
---

```sh
# pip install python-dotenv pyyaml loguru
import os
import sys

import toml
import yaml
from loguru import logger
from dotenv import load_dotenv

BASE_DIR = os.path.abspath(os.path.dirname(__file__)).rstrip("/common")

log_file_path = os.path.join(BASE_DIR, "logs/stdout.log")
err_log_file_path = os.path.join(BASE_DIR, "logs/error.log")

logger.add(
    log_file_path,
    format="{process} {thread} {time:YYYY.MM.DD HH.mm.ss} {level}.{file}.{name}.{module}.{line} {message}",
    rotation="100 MB",
    colorize=True,
    enqueue=True,
    backtrace=True,
    diagnose=True,
    level="INFO",
)
logger.add(
    err_log_file_path,
    format="{time:YYYY.MM.DD HH.mm.ss} {level}.{file}.{name}.{module}.{line} {message}",
    rotation="100 MB",
    level="ERROR",
    colorize=True,
    enqueue=True,
    backtrace=True,
    diagnose=True,
)


class SettingsMeta:
    def __init__(self, _file=None):
        self.file = _file

    # read config.yaml
    def read_yaml(self, key, file="settings.yaml"):
        if os.path.exists(file):
            with open(file, "r") as f:
                con = yaml.safe_load(f)
            if con:
                #  logger.debug(con)
                return con.get(key)

    # read .secrets.toml
    def read_toml(self, key, file=".secrets.toml"):
        if os.path.exists(file):
            con = toml.load(file)
            if con:
                #  logger.debug(con)
                return con.get(key)

    # read .env
    def read_env(self, key):
        load_dotenv()
        return os.getenv(key)

    def __getattr__(self, key):
        result = None

        file_function = {
            "yaml": self.read_yaml,
            "toml": self.read_toml,
            "env": self.read_env,
        }

        if self.file:
            file_type = self.file.split(".")
            func = file_function.get(file_type[-1])
            if func:
                return func(key)
            else:
                return

        functions = [self.read_yaml, self.read_toml, self.read_env]
        for ft in functions:
            result = ft(key)
            if result:
                return result
        return result


settings = SettingsMeta()
```

```python
# settings.py
# pip install dynaconf loguru
import os

from loguru import logger
from dynaconf import Dynaconf

BASE_DIR = os.path.abspath(os.path.dirname(__file__)).rstrip("/common")

log_file_path = os.path.join(BASE_DIR, "logs/stdout.log")
err_log_file_path = os.path.join(BASE_DIR, "logs/error.log")

logger.add(
    log_file_path,
    format="{process} {thread} {time:YYYY.MM.DD HH.mm.ss} {level}.{file}.{name}.{module}.{line} {message}",
    rotation="100 MB",
    colorize=True,
    enqueue=True,
    backtrace=True,
    diagnose=True,
    level="INFO",
)
logger.add(
    err_log_file_path,
    format="{time:YYYY.MM.DD HH.mm.ss} {level}.{file}.{name}.{module}.{line} {message}",
    rotation="100 MB",
    level="ERROR",
    colorize=True,
    enqueue=True,
    backtrace=True,
    diagnose=True,
)


Config = Dynaconf(settings_files=[".secrets.toml"])

print(Config.__dict__)
print(Config.redis_ip)
```
