# Clash-config · Jason3u 自用分流配置

由 Quantumult X 配置（[Quantumult-X-Config](https://github.com/Jason3u/Quantumult-X-Config)）逐条转换而来，分流行为与 QX 完全一致，用于 PC / PE 与 Android 多端同步。

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
| Apple 外区（AppleTV/AppleNews） | 兜底分流 | 兜底分流组 |
| AI（OpenAI/Gemini/Claude/Grok） | 各自独立策略组 | 美国节点（可手动切地区） |
| X（Twitter） | X | 香港节点 |
| Binance 币安 | Binance | DIRECT 直连（可切港住宅/港/日/新/台/韩） |
| OKX / Bybit / Bitget / Gate | 各自独立策略组 | 香港住宅IP |
| Cornix 自动交易 | Cornix | 新加坡节点（区内自动最低延迟） |
| 节点池 | 自动选择 + 各地区 url-test | 区内自动测速最低延迟 |
| 国内网站 | 直连 | DIRECT（GEOIP,CN） |
| 其余外网（含流媒体） | 兜底分流 | 兜底分流组（默认自动选择） |

## 说明

- 规则集来源：blackmatrix7 [ios_rule_script](https://github.com/blackmatrix7/ios_rule_script)（Clash 版）
- X / 交易所（Binance/OKX/Bybit/Bitget/Gate）/ Cornix 为自建规则，来自 [Quantumult-X-Rules](https://github.com/Jason3u/Quantumult-X-Rules)，已逐条内联进配置
- 节点筛选在 QX 中文名基础上补充了常见英文缩写（HK/JP/SG 等），防止机场节点名为英文时筛不出节点
- QX 的去广告/MitM 重写脚本 Clash 无法实现，未迁移
