# Clash Verge 使用教程

> Clash Verge 是目前 Windows/Mac 上最强大的代理客户端，支持 VLESS、Trojan、VMess、Reality 等所有主流协议。

## 下载安装

### Windows
1. 打开：https://github.com/clash-verge-rev/clash-verge-rev/releases
2. 下载最新版的 `.exe` 安装包
3. 安装后打开

### Mac
1. 打开：https://github.com/clash-verge-rev/clash-verge-rev/releases
2. 下载 `.dmg` 安装包
3. 安装后打开（首次需要授权网络权限）

---

## 基本使用

### 方式一：订阅链接导入（推荐）

1. 复制机场订阅链接
2. 点击左侧「配置」
3. 点击「新建配置」或左下角「+」
4. 选择「URL 导入」
5. 粘贴订阅链接，点击确定
6. 点击「更新订阅」
7. 订阅更新成功后，点击左侧「代理」
8. 选择你要使用的节点（点击即可切换）
9. 点击「启用系统代理」

### 方式二：手动节点导入

1. 点击左侧「代理」
2. 点击右上角「+」→「手动导入」
3. 粘贴节点信息（格式：vmess://... 或 vless://... 等）
4. 保存并选择该节点

---

## 节点分组管理

Clash Verge 支持分组功能：

1. 点击左侧「代理」
2. 可以看到「全球」和各个分组
3. 点击节点右侧的 ⭐ 可收藏常用节点
4. 右键节点可移动到不同分组

---

## 规则设置

### 内置规则模式
点击左侧「规则」，可选择：
- **规则（Rule）**：按规则分流（推荐）
- **全局（Global）**：所有流量都走代理
- **直连（Direct）**：所有流量直连

### 自定义规则
可以编辑配置文件添加自定义规则：

```yaml
rules:
  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-SUFFIX,youtube.com,Proxy
  - DOMAIN-KEYWORD,netflix,Netflix
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

---

## 设置

### 开机启动
设置 → 系统设置 → 开机启动

### 代理端口
默认代理端口是 `7890`，可在设置中修改。

### 启用 TUN 模式（推荐）
TUN 模式可以接管所有系统流量，包括不支持代理的 APP：
1. 设置 → 高级设置 → 开启 TUN 模式
2. 推荐同时开启「自动混站」

---

## 常见问题

**Q: 显示已连接但打不开网站？**
A: 尝试切换节点，可能是当前节点被阻。

**Q: 订阅更新失败？**
A: 可能是订阅地址被阻，试试在机场后台获取新订阅链接。

**Q: TUN 模式无法开启？**
A: Windows 需要以管理员权限运行 Clash Verge。

**Q: Clash Verge 和 Clash for Windows 有什么区别？**
A: Clash Verge 是 Clash for Windows 的重构版，性能更好，支持更多协议，功能更丰富。

---

## 相关阅读

- [Clash for Android](clash-for-android.md)
- [Sing-Box 使用教程](sing-box.md)
- [机场推荐](../airport-reviews/README.md)
