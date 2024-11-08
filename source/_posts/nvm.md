---
title: nvm快速安装
tags:
  - 实用软件操作
---

# Install

```shell
# install file
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash

# This loads nvm
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" 
```

# Reference
```
https://github.com/nvm-sh/nvm?tab=readme-ov-file#install--update-script
```