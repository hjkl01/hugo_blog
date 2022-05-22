---
title: "jupyter"
draft: true
---

```sh
# jupyter config
pip install jupyter
jupyter notebook --generate-config
```

```sh
# ipython
from notebook.auth import passwd
passwd()
# or
jupyter notebook password

vim ~/.jupyter/jupyter_notebook_config.py 

c.NotebookApp.ip='*'
c.NotebookApp.password = u''
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8000

jupyter notebook
```

```python
import os
import re
import secrets

import yaml
from rich import print


class ConfigMeta:
    def __init__(self, _file="config.yaml"):
        self.file = _file

    def __getattr__(self, key):
        with open(self.file, "r") as file:
            self.con = yaml.safe_load(file)
        result = self.con.get(key)
        return result


Config = ConfigMeta()


def generate_password():
    result = []
    for i in range(1, 9):
        temp = {}
        temp["port"] = 9120 + i
        temp["dirname"] = f"njrd_venv_{9120+i}"
        temp["password"] = secrets.token_urlsafe(32)
        result.append(temp)

    with open("config.yaml", "w") as file:
        yaml.dump({"config": result}, file, allow_unicode=True)


def stop_old_jupyter():
    cmd = "ps aux | grep jupyter"
    temp = os.popen(cmd)
    for t in temp:
        print(t)
        ppid = re.findall("\d+", t)[0]
        cmd = f"kill -9 {ppid}"
        print(cmd)
        os.system(cmd)


def main():

    print(Config.config)
    for con in Config.config:
        dirname = con["dirname"]
        port = con["port"]
        password = con["password"]

        if os.path.exists(f"./{dirname}"):
            print(f"exists {dirname}")
        else:
            cmds = [
                f"/usr/bin/python3.6 -m venv {dirname}/.venv",
                f"{dirname}/.venv/bin/pip install --upgrade pip",
                f"{dirname}/.venv/bin/pip install jupyter ",
            ]
            for cmd in cmds:
                print(cmd)
                os.system(cmd)

        cmd = f"source {dirname}/.venv/bin/activate && nohup {dirname}/.venv/bin/jupyter notebook --ip='*' --port='{port}' --notebook-dir='{dirname}' --NotebookApp.token='{password}' --NotebookApp.password='{password}' >> /dev/null 2>&1 & "
        print(cmd)
        os.system(cmd)
    return


if __name__ == "__main__":
    # generate_password()
    stop_old_jupyter()
    main()
```
