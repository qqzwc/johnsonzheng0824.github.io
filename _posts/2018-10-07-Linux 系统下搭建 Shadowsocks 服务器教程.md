---
layout: post
title: "Linux 系统下搭建 Shadowsocks 服务器"
date: 2018-10-07
tag: 梯子
--- 

Google 云新用户注册可以免费领取 $300 一年期使用权，如果你用的是其他云服务商，教程通用，这里只是以 Google 云为例，操作系统推荐大家用 CentOS6（就没蓝底那个窗口了），也可以使用 *debian9(推荐）* 使用 debian9 可以只用从第 5 步开始（当然 sudo -i 这一步还是要的）只需 2 步就可以搭建好。不用装 BBR 加速器 速度也非常的快 直接破百兆！

Google 云地址：[http://cloud.google.com](http://cloud.google.com)

### Google 云创建虚拟机

首先创建好一台 VM 实例 (位置在 Google 云 Compute Engin-VM 实例)

![](https://ww3.sinaimg.cn/large/006LWy2zgy1fvyg45ym5tj30aq0fv750.jpg)

创建一个实例，地区一定要选择台湾，为了更快：

![](https://ww1.sinaimg.cn/large/006LWy2zgy1fvyg71x3y2j30l10mn76a.jpg)

创建好之后 ping 测试一下，如果延迟在 100ms 以内就可以，否则就销毁重建：

![](https://ww1.sinaimg.cn/large/006LWy2zly1fvyg8l8nhqj30q307taae.jpg)

![](https://ww2.sinaimg.cn/large/006LWy2zly1fvygaw7nx5j30b205a417.jpg)

### 开始搭建 Shadowsocks

1-4 是安装 BBR 加速器部分 5-6 是 ssr 部分

1：sudo -i(最前面显示 root@xxxx)

2：wget -N --no-check-certificate https://raw.githubusercontent.com/FunctionClub/YankeeBBR/master/bbr.sh && bash bbr.sh install

蓝底窗口按 TAB 键选 NO

选择重启 Y

这里会断开连接，大家可以关掉窗口再重新打开或几秒钟后在界面随便按几个字母 便会提示重新连接。

3：sudo -i (最前面显示 root@xxxx)

4：bash bbr.sh start

5：wget --no-check-certificate https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocksR.sh && chmod +x shadowsocksR.sh

6：./shadowsocksR.sh

输入 shadowsocks 密码

输入端口号

其他一路回车（也可自行选择混淆 协议），大约需要等个十来分钟...

在最后出现红底数据以后，就是 Shadowsocks 服务端配置信息，在你的 Shadowsocks 客户端配置上即可。

### 谷歌云防火墙规则添加 （位置在谷歌云 VPC 网络 - 防火墙）

点击添加新规则，然后按照一下这个设置好。这样 SSR 设置任何端口都可以使用。并且后续不需要再来防火墙规则做设置了。

<img width="360px" src="https://ww3.sinaimg.cn/large/006LWy2zgy1fvyg3z1s1mj30ra10iq5o.jpg"/>


其他云服务商的你也得检查一下你的网络防火墙设置。
