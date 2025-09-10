---
title: "Docker 使用手册"
date: 2024-01-02
draft: false
tags: ["Docker", "容器化"]
---

## Docker 简介
Docker 是一个开源的容器化平台，它可以让开发者将应用及其依赖打包成一个可移植的容器，然后发布到任何支持 Docker 的机器上运行。

## 基本操作
### 安装 Docker
在不同的操作系统上安装 Docker 的方法不同，以 Ubuntu 为例，可以使用以下命令进行安装：
```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io