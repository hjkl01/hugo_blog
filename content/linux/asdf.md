---
title: "asdf"
draft: true
---

### install asdf
```sh
git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.10.0

# add following to ~/.zshrc 
. $HOME/.asdf/asdf.sh
```

### install plugin
```sh 
asdf plugin add nodejs
asdf list all nodejs
asdf install nodejs lts
# asdf install nodejs latest
asdf list nodejs
asdf global nodejs lts
```
