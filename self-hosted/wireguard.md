# WireGuard 搭建教程

> WireGuard 是新一代轻量级 VPN 协议，速度快、安全性高、配置简单。

## 为什么用 WireGuard

| 特点 | WireGuard | OpenVPN | IPSec |
|------|-----------|---------|-------|
| 速度 | 极快 | 慢 | 中等 |
| 代码量 | ~4000行 | ~100万行 | ~50万行 |
| 配置 | 极简 | 复杂 | 复杂 |
| 安全性 | 高 | 高 | 高 |
| 抗封锁 | 中等 | 较高 | 较高 |

---

## 第一步：购买 VPS

推荐：
- [Vultr](https://www.vultr.com) — 按小时计费，灵活
- [DigitalOcean](https://www.digitalocean.com) — 稳定
- [搬瓦工](https://bandwagonhost.com) — 性价比高（CN2线路）

系统推荐：**Ubuntu 22.04**

---

## 第二步：安装 WireGuard

### 服务器端

```bash
# Ubuntu/Debian
apt update && apt install -y wireguard

# 开启 IP 转发
echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
sysctl -p
```

### 生成密钥

```bash
# 生成服务器密钥对
wg genkey > server_private.key
wg pubkey < server_private.key > server_public.key

# 生成客户端密钥对
wg genkey > client_private.key
wg pubkey < client_private.key > client_public.key
```

---

## 第三步：配置服务器

```bash
# 创建配置文件
cat > /etc/wireguard/wg0.conf << EOF
[Interface]
# 服务器私钥
PrivateKey = $(cat server_private.key)
# 服务器监听端口
ListenPort = 51820
# 服务器 IP（内网）
Address = 10.0.0.1/24

# NAT 转发（重要！）
PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -A FORWARD -o wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -D FORWARD -o wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

# 如果你的网卡不是 eth0，用 ip addr 确认
EOF
```

**注意**：把 `$(cat server_private.key)` 替换为实际密钥内容

---

## 第四步：添加客户端配置

在 `/etc/wireguard/wg0.conf` 末尾添加：

```ini
[Peer]
# 客户端公钥
PublicKey = $(cat client_public.key)
# 客户端 IP（内网）
AllowedIPs = 10.0.0.2/32
```

---

## 第五步：配置客户端

在**客户端设备**上创建配置文件 `wg0.conf`：

```ini
[Interface]
# 客户端私钥
PrivateKey = $(cat client_private.key)
# 客户端 IP（内网）
Address = 10.0.0.2/24
# DNS
DNS = 8.8.8.8,1.1.1.1

[Peer]
# 服务器公钥
PublicKey = $(cat server_public.key)
# 服务器地址和端口
Endpoint = 你的VPS_IP:51820
# 持久化 Keep-Alive（推荐加，防止断流）
PersistentKeepalive = 25
# 允许的流量（0.0.0.0/0 表示全部流量）
AllowedIPs = 0.0.0.0/0
```

---

## 第六步：启动服务

```bash
# 启动 WireGuard
systemctl start wg-quick@wg0

# 开机自启
systemctl enable wg-quick@wg0

# 查看状态
wg show
```

---

## 第七步：放行防火墙

```bash
# 放行 UDP 51820 端口
ufw allow 51820/udp

# 或者直接关闭防火墙测试
ufw disable
```

---

## 客户端下载

| 平台 | 客户端 |
|------|--------|
| Windows | [WireGuard Windows](https://www.wireguard.com/install/) |
| macOS | [WireGuard macOS](https://www.wireguard.com/install/) |
| iOS | App Store 搜索 WireGuard |
| Android | F-Droid 或 Google Play 搜索 WireGuard |
| Linux | `apt install wireguard` |

---

## 常见问题

**Q: 连接成功但无法上网？**
A: 检查服务器是否开启了 IP 转发，PostUp 命令是否正确执行。

**Q: 如何让多台设备同时连接？**
A: 每台设备生成独立密钥对，在服务器配置中添加多个 [Peer] 即可。

**Q: WireGuard 和 VLESS+Reality 哪个好？**
A: 如果对抗封锁，Reality 更强；如果只是正常上网且需要高速，WireGuard 体验更好。

---

## 相关阅读

- [VLESS+Reality 防封教程](reality.md)
- [V2Ray/XRay 搭建](v2ray-xray-setup.md)
