# VLESS+Reality 终极防封教程

> 目前最强防封方案，适合对稳定性有高要求的用户。

## 什么是 Reality？

Reality 是 V2Ray/XRay 的新协议，是 XTLS 的简化版，特点：
- **强抗封锁**：能有效对抗 GFILTER 检测
- **零证书依赖**：不需要伪造证书
- **性能优秀**：比传统 XTLS 更快

## 架构说明

```
用户 → XRay(VLESS+Reality) → 直连目标网站（无中转）
```

**核心原理**：伪装成访问正常 HTTPS 网站流量，GFW 无法区分。

---

## 第一步：购买 VPS

推荐服务商：
- [RackNerd](https://www.racknerd.com) — 性价比高
- [DMIT](https://www.dmit.io) — 高性能
- [Friendhosting](https://friendhosting.net) — 欧洲机房便宜

**推荐配置**：1核/1G内存/500G流量/月，约 $5-10/年

**推荐地区**：香港、日本、美国西海岸（CN2 GIA线路最优）

---

## 第二步：安装 XRay

一键脚本（推荐）：
```bash
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install
```

或使用 [mack-a脚本](https://github.com/mack-a/v2ray-agent)：
```bash
curl -Ls https://raw.githubusercontent.com/mack-a/v2ray-agent/master/install.sh -o va.sh && chmod +x va.sh && ./va.sh
```

---

## 第三步：配置 Reality

编辑 `/usr/local/etc/xray/config.json`：

```json
{
  "log": {"loglevel": "warning"},
  "inbounds": [{
    "port": 443,
    "protocol": "vless",
    "settings": {
      "clients": [{
        "id": "你的UUID",
        "flow": "xtls-rprx-vision"
      }],
      "decryption": "none"
    },
    "streamSettings": {
      "network": "tcp",
      "security": "reality",
      "realitySettings": {
        "show": false,
        "dest": "www.microsoft.com:443",
        "xver": 0,
        "serverNames": [
          "www.microsoft.com",
          "www.apple.com",
          "www.amazon.com",
          "www.cloudflare.com"
        ],
        "privateKey": "服务器私钥",
        "shortIds": [""]
      }
    }
  }],
  "outbounds": [{"protocol": "freedom"}]
}
```

---

## 第四步：生成密钥

```bash
# 生成私钥
xray x25519

# 输出示例：
# Private key: xxxx
# Public key: xxxx
```

---

## 第五步：获取客户端配置

服务端配置好后，生成客户端链接：

```
vless://[UUID]@[服务器IP]:443?encryption=none&flow=xtls-rprx-vision&security=reality&sni=www.microsoft.com&fp=chrome&pbk=[公钥]&sid=[shortId]&type=tcp#节点名称
```

**参数说明**：
- `UUID`：生成私钥时得到的
- `服务器IP`：你的VPS IP
- `sni`：填一个常用域名（如www.microsoft.com）
- `fp`：指纹，填chrome
- `pbk`：公钥

---

## 第六步：客户端配置

### 使用 v2rayN（Windows）
1. 下载 v2rayN：https://github.com/2dust/v2rayN/releases
2. 服务器 → 添加 VLESS 服务器
3. 填入节点信息
4. 勾选「启用 Reality」
5. 保存并连接

### 使用 Clash.Meta
```yaml
proxies:
  - name: "my-reality"
    type: vless
    server: 你的IP
    port: 443
    uuid: 你的UUID
    network: tcp
    tls: true
    flow: xtls-rprx-vision
    client-fingerprint: chrome
    reality-opts:
      public-key: 公钥
      short-id: ""
```

---

## 常见问题

**Q: 连上了但无法上网？**
A: 检查防火墙是否放行443端口。

**Q: 速度很慢？**
A: Reality 直连不推荐用亚洲线路以外，延迟太高。优先选香港/日本节点。

**Q: 如何判断 Reality 生效了？**
A: 访问 https://www.youtube.com 看是否正常，用在线工具检测节点是否泄露 DNS。

---

## 相关阅读

- [V2Ray/XRay 基础搭建](v2ray-xray-setup.md)
- [Clash 配置教程](clash-meta.md)
- [客户端教程](../client-guides/clash-verge.md)
