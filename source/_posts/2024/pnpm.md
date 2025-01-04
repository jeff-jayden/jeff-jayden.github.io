---
title: pnpm
comments: true
date: 2025-01-02 12:06:23
tags:
---


软硬链接区别

在 node_modules 下出现的包是软链接到.pnpm下的，.pnpm 下的是硬连接，也可能包含软链接，具体看列出的文件信息

ls -liah 可以看到当前文件是软链接还是硬连接

https://juejin.cn/post/6844903438560526343?searchId=2025010211401736AE6E98B68076288E45