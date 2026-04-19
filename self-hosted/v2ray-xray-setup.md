# V2Ray / XRay 搭建教程

> V2Ray 和 XRay 都是常见的代理软件，XRay 是 V2Ray 的分支，性能更好，推荐使用 XRay。

## 什么是 XRay？

XRay 是 V2Ray 的高性能分支，兼容 V2Ray 所有协议，支持 VLESS、Trojan、VMess 等，配合 Reality 伪装是目前最优防封方案。

---

## 第一步：购买 VPS

**推荐服务商**：

| 服务商 | 特点 | 推荐配置 |
|--------|------|---------|
| [RackNerd](https://www.racknerd.com) | 便宜稳定 | $10-20/年 |
| [DMIT](https://www.dmit.io) | CN2 GIA线路 | $20-30/年 |
| [Friendhosting](https://friendhosting.net) | 欧洲机房多 | €3-5/月 |
| [Vultr](https://www.vultr.com) | 按小时计费 | $5/月 |

**推荐配置**：
- 1核 CPU
- 1GB 内存
- 20GB SSD
- 每月 500GB-1TB 流量

**推荐系统**：Debian 11/12 或 Ubuntu 20.04/22.04

---

## 第二步：连接 VPS

```bash
# Mac/Linux 打开终端，Windows 用 PowerShell 或 Termius
ssh root@你的VPS_IP
```

如果是密码登录，系统会发邮件给你临时密码，第一次登录需要改密码。

---

## 第三步：一键安装脚本

推荐使用 [mack-a 脚本](https://github.com/mack-a/v2ray-agent)，支持 VLESS、Vmess、Trojan、Reality 等多种协议：

```bash
curl -Ls https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh -o va.sh
chmod +x va.sh
./va.sh
```

按提示选择：
1. 安装
2. 安装 VLESS + XTLS + Reality
3. 按默认选项一路回车

---

## 第四步：获取节点信息

安装完成后会显示：
- 节点协议类型
- 节点地址（你的VPS IP）
- 端口
- UUID
- 额外ID（AlterId）

**把这些信息复制到客户端使用。**

---

## 第五步：开放防火墙

```bash
# 查看防火墙状态
ufw status

# 如果是 active，放行端口
ufw allow 443/tcp
ufw allow 22/tcp
```

---

## 常见协议对比

| 协议 | 安全性 | 防封 | 性能 | 推荐度 |
|------|--------|------|------|--------|
| VMess | 中 | 较差 | 中 | ⭐⭐ |
| VLESS | 高 | 中 | 高 | ⭐⭐⭐⭐ |
| Trojan | 高 | 中 | 高 | ⭐⭐⭐⭐ |
| VLESS+Reality | 最高 | 极强 | 极高 | ⭐⭐⭐⭐⭐ |

**强烈推荐直接配置 VLESS+Reality**，参考：[VLESS+Reality 教程](reality.md)

---

## 客户端推荐

- Windows: [v2rayN](https://github.com/2dust/v2rayN) / [Clash Verge](https://github.com/clash-verge-rev/clash-verge-rev)
- Android: [v2rayNG](https://github.com/2dust/v2rayNG) / [Clash for Android](https://github.com/GUI-for-Cores/ClashForAndroid)
- iOS: [Shadowrocket](https://apps.apple.com/) / [Stash](https://apps.apple.com/)

---

## 故障排查

**连不上？按顺序检查：**
1. 防火墙是否放行端口
2. 账号信息是否输入错误
3. VPS 是否欠费/被暂停
4. 节点协议是否支持 Reality

---

## 相关阅读

- [VLESS+Reality 终极防封教程](reality.md)
- [Clash 配置教程](clash-meta.md)
- [客户端教程](../client-guides/clash-verge.md)
