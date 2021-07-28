---
id: install-Redis6.0
sidebar_position: 1
---

# Redis 6.0 安装

> 基于 Centos 7.8

## 第一步：下载安装包

访问 [Redis 官网](https://redis.io/)，获取安装包，这里以安装 6.0.9 版本为例，其他版本类似。

```shell
wget https://download.redis.io/releases/redis-6.0.9.tar.gz
```

## 第二步：解压

```shell
tar -zvxf redis-6.0.9.tar.gz
```

## 第三步：安装依赖

```shell
yum install -y gcc-c++ autoconf automake
```

由于在 Centos 7 下安装的 gcc-c++ 默认为 4 版本，但是要编译 Redis 6 需要新版本的 gcc-c++ ，所以需要将 gcc-c++ 版本升级为 9 版本。
如果已经是 9 版本，可以直接跳到第四步。

```shell
# 安装 scl 源

yum install -y centos-release-scl scl-utils-build

# 安装 9 版本的 GCC 及工具链

yum install -y devtoolset-9-toolchain

# 临时覆盖老版本 GCC

scl enable devtoolset-9 bash

```

## 第四步：预编译

先切换到解压目录，然后执行 make

```shell
cd redis-6.0.9 && make
```

## 第五步：安装

为了统一管理，先创建安装文件夹，然后指定安装目录进行安装。此处有个坑，**PREFIX 大小写敏感**，一定要是大写否则还是会安装到默认路径而非指定路径。

```shell
mkdir -p /usr/local/redis && make PREFIX=/usr/local/redis/ install
```

## 第六步：启动

安装完成后会在安装目录 `/usr/local/redis/` 生成一个 bin 文件夹，里面是 Redis 相关的可执行文件，调用 redis-server 即可按默认配置启动 redis

```shell
cd /usr/local/redis/bin && ./redis-server
```

如果想要指定配置文件启动，可以先将 redis 包下的配置文件拷贝至安装目录，按需求修改配置，启动的时候加上配置文件路径即可

```shell
# 复制配置文件
cp /usr/local/src/redis-6.0.9/redis.conf /usr/local/redis/bin

# 省略配置文件修改部分，可以根据需要自行修改 /usr/local/redis/bin/redis.conf 内容

# 指定配置文件启动
./redis-server ./redis.conf
```

## 第七步：配置开机启动

在系统服务目录创建 redis.service 文件

```shell
vim /etc/systemd/system/redis.service
```

写入以下内容

```shell
[Unit]
Description=redis-server
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/redis/bin/redis-server /usr/local/redis/bin/redis.conf
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

重载系统服务

```shell
systemctl daemon-reload
```

将服务加入开机自启

```shell
systemctl enable redis.service
```
