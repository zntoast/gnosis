
##  1.准备工作

进行多设备之间的组网，需要一台中继服务器进行数据的转发。这里需要一台有公有ip地址的服务器，打通多个局域网之间的通信。
云服务最好选择linux , 内核版本是否为 5.6 以上 ， 可使用` uname -r`   查看内核版本

### 1.1 现在使用` modinfo wireguard `命令可以看到正常输出了

```
$ modinfo wireguard
filename:       /lib/modules/5.18.4-1.el8.elrepo.x86_64/kernel/drivers/net/wireguard/wireguard.ko.xz
alias:          net-pf-16-proto-16-family-wireguard
alias:          rtnl-link-wireguard
version:        1.0.0
author:         Jason A. Donenfeld <Jason@zx2c4.com>
description:    WireGuard secure network tunnel
license:        GPL v2
srcversion:     746FF227790A0865A542034
depends:        udp_tunnel,curve25519-x86_64,libchacha20poly1305,ip6_udp_tunnel,libcurve25519-generic
retpoline:      Y
intree:         Y
name:           wireguard
vermagic:       5.18.4-1.el8.elrepo.x86_64 SMP preempt mod_unload modversions
```

### 1.2 安装 wireguard-tools

```
sudo apt install elrepo-release epel-release wireguard-tools
```

## 2. 中继服务器配置

### 2.1 生成密钥对

在 /etc/wireguard 目录下 ，生成密钥对用于后续使用

```
 wg genkey | tee peer_A.key | wg pubkey > peer_A.pub && cat peer_A.key && cat peer_A.pub
```

### 2.2 配置文件 /etc/wireguard/wg0.conf 

vim /etc/wireguard/wg0.conf 在这个文件中配置隧道的相关信息 , 保存好配置并退出 :wq

在 WireGuard 中，你需要手动给各个设备分配 IP，并确保每个设备都有唯一的 IP

Interface 包含了当前设备的设置，对于“服务端”来说，ListenPort 是必须的。下面的每一个 Peer 段代表了能连接到本设备的一个其他设备

```
[Interface]
# 私钥
PrivateKey = 刚才创建的私钥
# 分配的ip (可根据需要自行更改)
Address = 10.20.0.1/32
# postUp和postDown参数用于指定在WireGuard接口启用和关闭时自动执行的脚本或命令
PostUp   = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
# MTU（Maximum Transmission Unit）指定了数据包在网络传输中的最大尺寸。
MTU = 1420
# 监听端口
ListenPort = 5182

# 终端节点1
[Peer]
PublicKey = 终端节点1公钥
# 分配的ip 10.20.0.2 (可根据需要自行更改)
AllowedIPs = 10.20.0.2/32

# 终端节点2
[Peer]
PublicKey = 终端节点2公钥
# 分配的ip 10.20.0.3 (可根据需要自行更改)
AllowedIPs = 10.20.0.3/32
```

### 2.3 如何启动，关闭 wg

- 	`wg-quick up wg0`  来启用配置文件。wg-quick 会自动配置路由表，无需我们手动设置
-   `wg-quick down wg0` 来关闭配置文件。
-    要判断` wg-quick up wg0` 命令是否成功启动了 WireGuard ，可以使用如下命令 ` wg show`  显示所有 WireGuard 接口的详细信息。你应该能看到名为 "wg0" 的接口及其状态。如果接口处于 "up" 状态，表示它已成功启动

### 2.4 放开端口，使互联网能进行服务访问

- 在云服务的路由组放开端口 udp  5182 

<br/>

## 3. 终端节点1配置 

 download link : 	[WireGuard ](https://www.wireguard.com/install/)是一个支持多平台的软件 

```
[Interface]
PrivateKey = 终端节点1私钥
Address = 10.20.0.2/32

[Peer]
PublicKey = 中继服务器公钥
AllowedIPs = 10.20.0.1/24
Endpoint = 云服务的公网ip:5182
# 定期发送一个空的数据包以保持连接活跃
PersistentKeepalive = 25
```


启动配置即可接入和中继服务器同一个网段当中，实现多设备的互联。 使用 `ping 10.20.0.1 `测试联通是否正常

## 4. 遇到的问题

- 单个终端尽量不要使用多个conf进入到该网络中，会导致上一个conf 失效
- 尽量一个终端 创建一个 conf ，不能进行混用，否则会导致终端网络断开。
- 尽量给每一个终端都分配一个ip 
- 启用的端口是udp 协议的 这点可以使用 netstat -unlp 进行查看
- 10.10.0.1/24 、10.10.0.1/32 后者的广播地址范围更大，推荐使用后者
- 无法访问到另一个局域网的ip,数据包被过滤，可以尝试关闭linux自身的防火墙

文章参考: [使用 WireGuard 搭建 VPN 访问家庭内网](https://dev.admirable.pro/using-wireguard/)
