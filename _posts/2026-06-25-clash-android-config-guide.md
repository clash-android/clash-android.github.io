---
layout: post
date: 2026-06-25 00:00:00 +0800
title: "Clash for Android 配置文件详解"
description: "深入了解 Clash for Android 的 YAML 配置文件格式、规则设置和高级配置选项。"
keywords: "Clash for Android,配置文件,YAML,规则配置,高级设置"
tags: [配置指南, Clash for Android, 高级用法]
categories: [Clash for Android]
image: "/assets/images/clash-android-config.png"
---

## Clash for Android 配置文件详解

Clash for Android 使用 YAML 格式的配置文件来定义代理规则和行为。本文将详细介绍配置文件的结构和常见选项。

### 配置文件的基本结构

一个典型的 Clash 配置文件包含以下主要部分：

```yaml
# 端口配置
port: 7890
socks-port: 7891
redir-port: 7892

# 代理模式
mode: rule

# 日志级别
log-level: info

# 允许局域网连接
allow-lan: true

# 外部控制
external-controller: 127.0.0.1:9090

# 代理组和规则
proxies:
  - name: "Proxy1"
    type: ss
    server: example.com
    port: 8388
    cipher: aes-256-gcm
    password: "password"

proxy-groups:
  - name: "Proxy"
    type: select
    proxies:
      - "Proxy1"

rules:
  - DOMAIN-SUFFIX,google.com,Proxy
  - GEOIP,CN,DIRECT
  - MATCH,Proxy
```

### 主要配置选项

| 选项 | 说明 | 示例 |
|------|------|------|
| port | HTTP 代理端口 | 7890 |
| socks-port | SOCKS5 代理端口 | 7891 |
| redir-port | 重定向端口 | 7892 |
| mode | 代理模式 | rule/global/direct |
| log-level | 日志级别 | info/warning/error/debug |
| allow-lan | 允许局域网访问 | true/false |
| external-controller | 外部控制地址 | 127.0.0.1:9090 |

### 代理类型支持

Clash for Android 支持多种代理协议：

- **Shadowsocks (ss)**：轻量级代理协议
- **ShadowsocksR (ssr)**：Shadowsocks 的增强版本
- **Trojan**：基于 TLS 的代理协议
- **Vmess**：V2Ray 的协议
- **VLESS**：V2Ray 的轻量级协议
- **Hysteria**：基于 QUIC 的协议
- **TUIC**：新型代理协议

### 规则配置详解

Clash 支持多种规则类型：

**1. 域名规则**

```yaml
# 精确匹配
DOMAIN,google.com,Proxy

# 后缀匹配
DOMAIN-SUFFIX,google.com,Proxy

# 前缀匹配
DOMAIN-PREFIX,www,Proxy

# 正则表达式
DOMAIN-REGEX,^.*\.google\.com$,Proxy
```

**2. IP 规则**

```yaml
# IP CIDR 匹配
IP-CIDR,192.168.0.0/16,DIRECT

# IP CIDR6 匹配（IPv6）
IP-CIDR6,2001:db8::/32,DIRECT
```

**3. 地理位置规则**

```yaml
# 中国 IP
GEOIP,CN,DIRECT

# 非中国 IP
GEOIP,!CN,Proxy
```

**4. 其他规则**

```yaml
# 端口匹配
DST-PORT,443,Proxy

# 协议匹配
PROCESS-NAME,com.example.app,Proxy

# 匹配所有流量
MATCH,Proxy
```

### 代理组配置

**Select 模式（手动选择）**

```yaml
proxy-groups:
  - name: "Proxy"
    type: select
    proxies:
      - "Proxy1"
      - "Proxy2"
      - "Proxy3"
```

**Auto 模式（自动选择）**

```yaml
proxy-groups:
  - name: "Auto"
    type: auto
    proxies:
      - "Proxy1"
      - "Proxy2"
    interval: 300
    tolerance: 50
```

**Fallback 模式（故障转移）**

```yaml
proxy-groups:
  - name: "Fallback"
    type: fallback
    proxies:
      - "Proxy1"
      - "Proxy2"
    interval: 300
```

### 性能优化建议

1. **合理设置规则顺序**：将常用规则放在前面
2. **使用规则集**：减少配置文件大小
3. **启用 DNS 缓存**：提高解析速度
4. **调整日志级别**：生产环境使用 info 级别

### 常见问题

**Q: 如何导入远程配置文件？**
A: 在应用中选择"导入"，输入配置文件的 URL，应用会自动下载并使用。

**Q: 规则优先级如何确定？**
A: 规则按顺序匹配，第一个匹配的规则生效。

**Q: 如何调试配置文件？**
A: 启用 debug 日志级别，查看应用日志了解规则匹配情况。

## 总结

通过合理配置 Clash for Android，可以实现灵活的网络流量控制和代理分流。建议根据实际需求逐步调整配置。
