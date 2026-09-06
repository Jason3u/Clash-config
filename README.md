# Clash-config · Jason3u 自用分流配置

由 Quantumult X 配置（[Quantumult-X-Config](https://github.com/Jason3u/Quantumult-X-Config)）逐条转换而来，分流行为与 QX 完全一致，用于 PC / PE 与 Android 多端同步。

> 配置修改记录见 [CHANGELOG.md](CHANGELOG.md)，每次修改前请先阅读。

## 使用

- 订阅 URL（客户端「配置 URL」填这个，注意不要用 GitHub 网页链接 /blob/ 链接，否则会报 yaml 解析错误）：
  - https://raw.githubusercontent.com/Jason3u/Clash-config/main/ClashForSelf.yaml
  - GitHub 访问不畅时用：https://cdn.jsdelivr.net/gh/Jason3u/Clash-config@main/ClashForSelf.yaml
- 需要 **Meta 内核（mihomo）**：订阅为 VLESS 节点，原版 Clash 内核不支持
  - PC：Clash Verge Rev / Mihomo Party
  - Android：ClashMetaForAndroid

## 分流总览（与 QX 完全一致）

| 分类 | 策略组 | 默认出口 |
|---|---|---|
| Apple 核心（AppStore/iCloud/AppleMusic） | 直连 | DIRECT |
| Apple 外区（AppleNews） | 兜底分流 | 兜底分流组 |
| AppleTV | InternationalStreaming | 国际流媒体策略组（默认香港节点） |
| AI（OpenAI/Gemini/Claude/Grok） | 各自独立策略组 | 美国节点（可手动切地区） |
| X（Twitter） | X | 香港自动（住宅优先/无住宅回退香港节点） |
| Binance 币安 | Binance | DIRECT 直连（可切香港自动/住宅/港/日/新/台/韩） |
| OKX / Bybit / Bitget / Gate | 各自独立策略组 | 香港自动（住宅优先，可手动强制住宅） |
| TradingView 行情图表 | TradingView | 香港自动（可手动切地区） |
| Cornix 自动交易 | Cornix | 香港节点（可手动切新加坡/日本） |
| Fomo | Fomo | DIRECT 直连（可手动切地区节点） |
| 国际流媒体（Netflix/Disney+/HBO/YouTube/Spotify/TikTok 等） | InternationalStreaming | 香港节点（备选按风控严重性排序） |
| 节点池 | 自动选择 + 各地区 url-test | 区内自动测速最低延迟 |
| 国内网站 | 直连 | DIRECT（GEOIP,CN） |
| 其余外网 | 兜底分流 | 兜底分流组（默认自动选择） |

## 说明

- 规则集来源：blackmatrix7 [ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)（Clash 版）
- X / 交易所（Binance/OKX/Bybit/Bitget/Gate）/ TradingView / Cornix / Fomo / 国际流媒体为自建规则，来自 [Proxy-Rules-Collection](https://github.com/Jason3u/Proxy-Rules-Collection)，通过 Clash YAML `rule-providers` 远程引用并自动更新
- 节点筛选在 QX 中文名基础上补充了常见英文缩写（HK/JP/SG 等），防止机场节点名为英文时筛不出节点
- QX 的去广告/MitM 重写脚本 Clash 无法实现，未迁移
