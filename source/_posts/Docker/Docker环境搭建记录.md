---
title: Docker环境搭建记录
date: 2019-02-28 22:17:38
categories: Docker
feature: /images/Docker/docker_logo.png
tags: docker
toc: true
---

Docker 介绍

<!-- More -->

## Ubuntu Docker 安装过程
参考网址：
1. [Docker官方教程](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
1. [菜鸟教程](http://www.runoob.com/docker/ubuntu-docker-install.html)

1. AliYun 镜像站安装 docker
   ``` c
   // 添加 GPG 秘钥和软件源
   curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
   sudo add-apt-repository "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
   sudo apt update
   sudo apt-get install docker-ce docker-ce-cli containerd.io
   sudo systemctl enable docker
   sudo systemctl start docker
   sudo docker run hello-world

   sudo groupadd docker
   sudo usermod -aG docker $USER
   docker run hello-world
   ```

## 管理 Docker 容器
1. 新装 Docker 后普通用户无权限访问 Docker 服务
   官方解释：Docker 的守护进程绑定了 Unix socket 而不是 TCP 端口。默认情况下，Unix socket 属于 root 用户，Docker 守护进程总是以 root 用户运行。
   处理方案：
   （1） 使用 sudo 获取管理员权限；
   （2） 创建 docker 用户并加入 docker 用户组。
   ``` c
   sudo groupadd docker # 提示 docker 组已存在
   sudo usermod -aG docker $USER # 把当前用户添加到 docker 组
#  sudo gpasswd -a $USER docker # 效果同上
   sudo systemctl restart docker # 重启 docker 服务
   docker iamges # 测试权限，如果不成功，尝试登出后重新登录该用户
#  newgrp docker # 更细 docker 用户组，同上解决加入 docker 组后不生效
   ```


