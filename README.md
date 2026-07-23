# 前言

个人利用AI写的一个iSCSI服务，基于tgt docker实现，让绿联、极空间、飞牛等无iSCSI服务的NAS用户也可以提供块存储。


# 项目介绍

# iSCSI 容器服务

这个容器提供了一个完整的iSCSI Target服务，并包含Web管理界面。

## 持久化配置

容器内的iSCSI配置可以持久化到宿主机，即使容器重启或重建，配置也不会丢失。

### 持久化目录

容器内的以下目录需要持久化：

- `/app/config`: 主要的持久化目录
- `/app/iscsi`: 包含虚拟镜像文件

### 运行容器并持久化配置

使用以下命令运行容器并持久化配置：

```bash
docker run -itd \
  --name d-tgtadm \
  # 占用3260（iscsi），13260（web）
  --network=host \
  # 虚拟磁盘存放路径
  -v /tgt/iscsi:/app/iscsi \
  # 现有虚拟磁盘，可以单独导入
  -v /现有虚拟磁盘路径/1.img:/app/iscsi/1.img \
  # 日志、配置文件永久存储文件夹
  -v /tgt/config:/app/config \
  ghcr.io/coracoo/d-tgtadm:latest
```

---

# Web管理界面

容器启动后，可以通过 `http://<宿主机IP>:13260` 访问Web管理界面，管理iSCSI Target和LUN。

# docker build
如果是arm64的服务器，则需要git clone后，进入源码包中 使用 docker build 构建arm镜像

