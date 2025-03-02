---
title: docker_nginx
comments: true
date: 2025-03-02 20:03:54
tags:
---

# nginx
总结： 前端项目打包后直接将静态资源放到nginx服务器上，最重要的还是对配置文件熟练

1. /usr/share/nginx/html 存放静态资源目录
2. /etc/nginx/conf.d/default.conf 配置文件路径




# docker
总结：使用 dockerfile 构建镜像，然后启动容器

1. 制作镜像（需要对常用的几个 dockerfile 命令熟悉，比如 from,run,args,copy,add,expose,cmd），就是基于一个基础镜像，然后把打包好的产物放到指定目录下
   1. 中间还可以挂载 volumes
   2. 设置参数
2. 运行命令

```shell
docker build -t 自定义镜像名 dockerfile路径
```

4. 使用镜像运行一个容器
