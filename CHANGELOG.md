# 修改日志（Changelog）

> 本文件记录 ClashForSelf.yaml 配置的历次修改。每次修改前请先阅读本文件了解当前状态，修改后必须在此追加记录。

## 2026-09-06（五）· Telegram 专属分流（消息延迟根治）

### 排查结论
与 QX 配置同步。TG 此前无规则无策略组：**App 收发消息直连 DC IP 不查 DNS**，域名规则全部失配，流量掉兜底 → 香港自动 →（空住宅组）→ 香港节点 url-test，每次新连接落在测速组当时挑中的节点，晚高峰拥塞时消息延迟、点开转圈。Cornix 同理（WebSocket 落在 url-test 选中的节点）。

### 修改
- 新增 Telegram select 组：`[香港节点, 日本节点, 新加坡节点, 台湾节点, 韩国节点, 美国节点]`，默认香港节点（直挂测速组绕开空住宅链路，可手动钉节点）
- 新增 rule-provider Telegram（规则仓库 `clash/Telegram.yaml`：22 域名 + 官方 DC IP 段，IP-CIDR 带 no-resolve）+ `RULE-SET,Telegram,Telegram`（置于 X 之前）
- Cornix 组扩充备选：增加 台湾/韩国/美国节点（默认香港不变）

## 2026-09-06（四）· Binance 默认出口改香港自动

### 排查结论（大陆网络实测）
与 QX 配置同步。用户怀疑「Binance 有时走代理导致不顺畅」，实测结论相反：**direct 默认已是死路**——api/accounts/stream/cdn-apps.binance.com、public.bnbstatic.com 直连全部被墙，大陆镜像域（binancecnt/binancezh/bnappzh/binance.me/binance.cloud）基本失效，仅 bnbzh.ac 慢通（2.9s）。不顺畅的真实原因 = 主域请求 5 秒级超时重试 + 出口 IP 在大陆/香港间跳变引发风控摩擦。规则文件（clash/Binance.yaml）覆盖度完备，无需改动。

### 修改
- Binance 组默认 `DIRECT` → `香港自动`：`[香港自动, 香港住宅IP, 香港节点, DIRECT, 日本节点, 新加坡节点, 台湾节点, 韩国节点]`，DIRECT 保留为手动选项

## 2026-09-06（三）· 兜底分流重排

### 修改
- **兜底分流仅保留四个地区并重排**：`香港自动 → 台湾节点 → 韩国节点 → 日本节点`（与 QX 配置同步；默认香港自动=住宅优先/无住宅回退香港）
- **移出未提到的选项**：自动选择 / 美国节点 / 新加坡节点 / 英国节点 / DIRECT / 手动切换 全部从兜底分流移除

## 2026-09-06（二）· 连接稳定性修复

### 修复
- **所有 url-test 组 `tolerance: 0` → `tolerance: 100`**：与 QX 配置同步修复。零容差导致任何微小波动即切换节点，IP 频繁跳变：长连接（Cornix WebSocket `wss://dashboard.cornix.io/ws/ws/` 等）频繁断开重连，消息类 App 断连、界面反复转圈

### 规则集
- **Cornix 规则大幅扩充**（[Proxy-Rules-Collection](https://github.com/Jason3u/Proxy-Rules-Collection) `clash/Cornix.yaml`，与 `qx/Cornix.list` 同步）：原规则仅 `cornix.io` 一条。实测 dashboard.cornix.io 的 JS 包，补全其运行时第三方依赖：intercom.io（客服组件）、country.is（国家检测）、mixpanel.com / mxpnl.com（埋点）、avo.app、hotjar.com、sentry.io（错误上报）、whop.com（支付）、calendly.com、googletagmanager.com。这些域名此前漏到兜底分流且每个都要本地 DNS 解析后判断 geoip，是 Cornix 界面「每次点击都转圈」的主因之一；极验 geetest.com 有国内节点，维持 geoip cn 直连，不收入规则

## 2026-09-06

### 订阅
- 机场订阅从「良心云」更换为「吹雪云」（与 QX 配置同步更换）

### 策略组
- **新增「香港自动」fallback 组**：住宅优先 → 无住宅时自动回退香港节点（对应 QX `available`；与 QX 不同，Clash 的 fallback 支持嵌套策略组）
- **X**：单一「香港节点」→ 香港自动 + 香港住宅IP/多地区备选
- **OKX / Bybit / Bitget / Gate**：默认从「香港住宅IP」改为「香港自动」（可手动强制住宅）
- **Binance**：默认 DIRECT，备选加入香港自动
- **Cornix**：固定新加坡 → 默认香港节点，可手动切新加坡/日本
- **新增 Fomo 策略组**：默认 DIRECT，可手动切换地区节点
- **新增 InternationalStreaming 国际流媒体策略组**：默认香港节点，备选按风控严重性排序（台湾→新加坡→韩国→日本→英国→美国）
- **AppleTV**：兜底分流 → InternationalStreaming
- **新增 TradingView 策略组**（与 QX 配置同步）：默认「香港自动」（住宅优先，无住宅回退香港节点），备选香港住宅IP / 香港 / 日本 / 新加坡 / 台湾 / 韩国，可手动切换

### 规则集
- 规则仓库 [Proxy-Rules-Collection](https://github.com/Jason3u/Proxy-Rules-Collection) 的 `clash/` 目录补齐：
  - `Fomo.yaml`（与 `qx/Fomo.list` 同步）
  - `InternationalStreaming.yaml`（与 `qx/InternationalStreaming.list` 同步，来源 ddgksf2013/Filter）
  - `TradingView.yaml`（与 `qx/TradingView.list` 同步，覆盖 `tradingview.com` 主域：官网 / 中文站 / 图表数据 / 静态资源 / API）

### 文档
- 配置顶部新增「AI 助手必读」提示：修改前必读 CHANGELOG.md，改后必须追加记录

### 当前策略组默认值
| 策略组 | 默认 | 备注 |
| --- | --- | --- |
| OKX / Bybit / Bitget / Gate | 香港自动 | 可手动强制香港住宅IP |
| TradingView | 香港自动 | 可手动切地区 |
| Binance | DIRECT | 可手动切友好地区 |
| Cornix | 香港节点 | 可手动切新加坡/日本 |
| Fomo | DIRECT | 可手动切地区节点 |
| X | 香港自动 | 可手动切地区 |
| InternationalStreaming | 香港节点 | 备选按风控严重性排序，最严重排最后 |
