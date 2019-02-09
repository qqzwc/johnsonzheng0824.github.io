---
layout: post
title: "搭建 Telegram 代理：MTProxy"
date: 2019-2-9
tag: Telegram
--- 


## 什么是 MTProxy

Telegram MTProxy 是 Telegram 官方出的一个轻量级的代理工具 (也叫 MTProto），可以直接配置在 Telegram 客户端中，不需要开启其他代理就可以直连 Telegram。官方地址：[Telegram MTProxy](https://github.com/TelegramMessenger/MTProxy)。

# MTProxy 搭建教程

必须要一台**国外的服务器**（可以直连 Telegram 的）。


## 编译 MTProxy

首先安装依赖：

在 Debian/Ubuntu 系统上:
```bash
apt install git curl build-essential libssl-dev zlib1g-dev
```
在 CentOS/RHEL 系统上:
```bash
yum install openssl-devel zlib-devel
yum groupinstall "Development Tools"
```

之后下载源码库:
```bash
git clone https://github.com/TelegramMessenger/MTProxy
cd MTProxy
```

最后直接 make 安装，并进入到 bin 文件夹：

```bash
make && cd objs/bin
```

如果安装失败，需要重新 make，则先执行 `make clean` 。

## 运行 MTProxy
1. 下载位于 Telegram 服务器的 secret（就是下载文件）
```bash
curl -s https://core.telegram.org/getProxySecret -o proxy-secret
```
2. 下载 telegram 配置文件（还是下载文件）
```bash
curl -s https://core.telegram.org/getProxyConfig -o proxy-multi.conf
```
3. 生成一个 32 位 16 进制 secret 用于客服端连接服务器（运行完这个会在界面输出一个密码）
```bash
head -c 16 /dev/urandom | xxd -ps
```
4. 运行 mtproto-proxy
```bash
./mtproto-proxy -u nobody -p 8888 -H 443 -S <secret> --aes-pwd proxy-secret proxy-multi.conf -M 0
```
要改的参数:
- `443` 为代理服务器端口，客户端使用此端口与代理服务器连接。
- `8888` 为本地端口，用于获取统计数据。
- `<secret>` 为此前生成的密钥，同样用于客户端。也可同时指定多个密钥：`-S <secret1> -S <secret2>`。
- `-M` 参数指定除主线程之外的工作线程数目，此处指定为 `0`，仅用主线程。

5. Enjoy.

## FAQ

使用阿里云服务器或者 AWS 服务器搭建 Telegram MTProxy 时，发现这个 MTProxy 绑定的是内网 IP，解决方案也很简单，使用 NAT 模式就行，指定内网 IP 和外网 IP：
```bash
./mtproto-proxy -u nobody -p 8888 -H 443 -S <secret> --nat-info <intranet ip>:<public ip> --aes-pwd proxy-secret proxy-multi.conf -M 0
```