# Widget SDK

Widget SDK 是接入 BlockATM 最快速的方式，只需三步即可在您的应用中嵌入加密货币收银台。

## 特点

- ✅ **快速集成**：30 分钟内完成接入
- ✅ **无需后端**：签名可在前端完成
- ✅ **多支付方式**：支持钱包连接和扫码支付
- ✅ **多链支持**：TRON、Ethereum、Arbitrum

## 支持环境

| 环境 | URL |
|------|-----|
| 生产环境 | `https://pay.blockatm.net/libs/v2/BlockATM.umd.js` |
| 测试环境 | `https://test-pay.blockatm.net/libs/v2/BlockATM.umd.js` |

## 集成流程

1. 在 HTML 中引入 SDK
2. 初始化 SDK
3. 调起收银台

[查看详细集成步骤 →](integration.md)

## 完整参数列表

[查看所有参数 →](parameters.md)

## 适用场景

| 场景 | 推荐度 |
|------|--------|
| 电商网站收款 | ⭐⭐⭐⭐⭐ |
| 移动端 H5 应用 | ⭐⭐⭐⭐⭐ |
| DApp 内嵌支付 | ⭐⭐⭐⭐ |
| 已有后端系统 | ⭐⭐⭐ |

{% hint style="info" %}
**安全性**：敏感操作（如签名）的逻辑应放在后端实现，Widget SDK 负责展示和交互。
{% endhint %}
