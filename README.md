# 前言
利用AI写的一个iSCSI服务，基于tgt docker实现，让绿联、极空间、飞牛等无iSCSI服务的NAS用户也可以提供块存储。

# 项目介绍

# iSCSI 容器服务

这个容器提供了一个完整的iSCSI Target服务，并包含Web管理界面，端口占用3260（iscsi），13260（web）。

## 持久化配置

容器内的iSCSI配置可以持久化到宿主机，即使容器重启或重建，配置也不会丢失。

### 持久化目录

容器内的以下目录需要持久化：

- `/app/config`: 主要的持久化目录
- `/app/iscsi`: 包含虚拟镜像文件

### 运行容器并持久化配置

使用以下命令运行容器并持久化配置：

#### x86_64 使用
```bash
docker run -itd \
  --name d-tgtadm \
  --network=host \
  --restart unless-stopped \
  -v ./d-tgtadm/iscsi:/app/iscsi \
  -v ./d-tgtadm/config:/app/config \
  registry.cn-hangzhou.aliyuncs.com/rd-public/d-tgtadm:amd64-0.0.1
# 原镜像 ghcr.io/coracoo/d-tgtadm:latest
```

#### arm64 使用
```bash
docker run -itd \
  --name d-tgtadm \
  --network=host \
  --restart unless-stopped \
  -v ./d-tgtadm/iscsi:/app/iscsi \
  -v ./d-tgtadm/config:/app/config \
  registry.cn-hangzhou.aliyuncs.com/rd-public/d-tgtadm:aarch64-0.0.1
```
---

# Web管理界面

容器启动后，可以通过 `http://<宿主机IP>:13260` 访问Web管理界面，管理iSCSI Target和LUN。

# docker build
```
如果是arm64的服务器，则需要git clone后，进入源码包中 使用 docker build 构建arm镜像（也能用上面build好的）
docker build -t d-tgtadm:0.0.1 .
```

