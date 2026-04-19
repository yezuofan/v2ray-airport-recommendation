# Clash for Android 使用教程

## 下载安装

1. 打开 GitHub：https://github.com/GUI-for-Cores/ClashForAndroid/releases
2. 下载最新版的 `.apk` 文件
3. 在手机上安装（需要允许「安装未知来源应用」）

**注意**：国内应用市场版本可能不是最新，建议从 GitHub 下载。

---

## 订阅导入

### 方式一：URL 导入（推荐）

1. 打开 Clash for Android
2. 点击右上角「配置」
3. 点击右上角「+」→「URL」
4. 粘贴订阅链接
5. 填入配置名称（随便填）
6. 点击「保存」
7. 返回首页，点击「代理」卡片
8. 选择节点（长按可测试延迟）
9. 点击右上角开关启动代理

### 方式二：手动导入

1. 点击右上角「配置」→「新建配置」
2. 选择「本地导入」
3. 选择 `.yaml` 文件
4. 保存并启动

---

## 分流规则

Clash for Android 支持规则分流：

1. 点击「配置」→ 「规则」
2. 选择规则模式：
   - **规则（Rule）**：按自定义规则分流
   - **全局（Global）**：全部走代理
   - **直连（Direct）**：全部直连

### 推荐规则

```yaml
rules:
  - DOMAIN-SUFFIX,google.com,Proxy
  - DOMAIN-SUFFIX,youtube.com,Proxy
  - DOMAIN-SUFFIX,netflix.com,Netflix
  - DOMAIN-SUFFIX,openai.com,Proxy
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

---

## 设置

### 开启 TUN 模式（推荐）

TUN 模式可以接管所有应用流量，包括不原生支持代理的APP：

1. 设置 → 高级设置 → 开启 TUN 模式
2. 推荐同时开启「增强模式」

### 开机启动

设置 → 开机启动 → 开启

### 通知栏快捷

App 内开启后，通知栏会显示快捷开关。

---

## 常见问题

**Q: 为什么有的APP走代理了还是打不开？**
A: 部分APP有严格的检测，需要开启 TUN 模式或增强模式。

**Q: 如何测试节点延迟？**
A: 长按节点名称，会自动测试并显示延迟。

**Q: 订阅更新失败？**
A: 部分订阅地址被运营商拦截，试试在机场后台获取新链接。

---

## 相关阅读

- [Clash Verge（Windows/Mac）](../client-guides/clash-verge.md)
- [Sing-Box 使用教程](sing-box.md)
- [机场推荐](../airport-reviews/README.md)
