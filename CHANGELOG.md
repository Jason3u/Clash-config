# 修改日志（Changelog）

> 本文件记录 ClashForSelf.yaml 配置的历次修改。每次修改前请先阅读本文件了解当前状态，修改后必须在此追加记录。

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

### 规则集
- 规则仓库 [Proxy-Rules-Collection](https://github.com/Jason3u/Proxy-Rules-Collection) 的 `clash/` 目录补齐：
  - `Fomo.yaml`（与 `qx/Fomo.list` 同步）
  - `InternationalStreaming.yaml`（与 `qx/InternationalStreaming.list` 同步，来源 ddgksf2013/Filter）

### 文档
- 配置顶部新增「AI 助手必读」提示：修改前必读 CHANGELOG.md，改后必须追加记录

### 当前策略组默认值
| 策略组 | 默认 | 备注 |
| --- | --- | --- |
| OKX / Bybit / Bitget / Gate | 香港自动 | 可手动强制香港住宅IP |
| Binance | DIRECT | 可手动切友好地区 |
| Cornix | 香港节点 | 可手动切新加坡/日本 |
| Fomo | DIRECT | 可手动切地区节点 |
| X | 香港自动 | 可手动切地区 |
| InternationalStreaming | 香港节点 | 备选按风控严重性排序，最严重排最后 |
