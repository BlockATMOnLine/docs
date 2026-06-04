---
hidden: true
---

# README

BlockATM 是面向企业的自托管 Web3 支付协议。它通过区块链智能合约帮助企业完成加密货币收款与批量付币，让企业在获得链上透明度和资金控制权的同时，享受清晰固定的费用规则，并减少对原生代币 GAS 的操作依赖。

## 为什么选择 BlockATM

传统加密支付服务通常要求企业把资金交给平台托管，企业需要相信平台不会挪用、冻结或限制资金。BlockATM 的设计理念不同：平台负责协调支付流程，资金控制权仍由企业自己的智能合约和指定签名地址掌握。

这让 BlockATM 更适合需要安全收款、批量付币、费用可预估、资金可追踪、权限可拆分和财务可审计的企业团队。

{% hint style="info" %}
**安全理念**：BlockATM 不以“替企业保管资金”为核心，而是帮助企业建立一套费用清晰、无需额外准备原生代币付 GAS、可自托管、可验证、可追踪的 Web3 支付流程。资金在链上智能合约中流转，关键操作由企业指定的钱包和权限执行。
{% endhint %}

## 核心价值

| 价值              | 对企业意味着什么                                           |
| --------------- | -------------------------------------------------- |
| **费用清晰**        | 收币、付币、合约创建等费用规则明确，企业可以在上线前评估长期支付成本。                |
| **无需原生代币付 GAS** | 企业和用户无需额外准备 TRX、ETH 等原生代币支付链上 GAS，降低 Web3 支付的使用门槛。 |
| **自托管资产**       | 智能合约和关键钱包由企业控制，BlockATM 无法直接访问或提取企业资金。             |
| **链上透明**        | 收款、付币、合约余额和交易哈希可追踪，便于财务对账和问题追溯。                    |
| **权限分离**        | 可按业务角色拆分收款、付币、签名、配置、赎回等操作权限，降低单点操作风险。              |
| **多链支持**        | 支持 TRON、Ethereum、Arbitrum 等主流网络，覆盖常见稳定币支付场景。       |



## 产品服务

### 收币 Safepay

企业可以通过连接钱包或二维码收款，让用户将加密货币支付到企业的智能合约或收款地址中。相比中心化托管收款，Safepay 更强调资金归属清晰、链上记录透明和后续对账可追溯。

适合场景：

* 商户接收 USDT、USDC 等稳定币付款
* 平台需要为用户生成可追踪的支付订单
* 企业希望收款资金不经过平台托管账户

[了解收币产品 →](https://app.gitbook.com/s/XEfzS05BPO0tTODCeSnr/shou-bi)

### 付币 Batch Payout

企业可以批量向用户、合作方或结算对象支付加密货币。BlockATM 支持余额支付和授权支付两种模式，帮助财务团队在效率和资金控制之间选择合适的付币方式。

适合场景：

* 商户结算
* 用户提现
* 合作方或供应商付款
* 需要保留交易记录和付币审批路径的财务流程

[了解付币产品 →](products/batch-payout/)

## 安全与控制模型

BlockATM 的安全设计围绕一个原则：平台可以协调支付流程、统一费用体验并降低 GAS 操作门槛，但不应成为企业资金的托管方。

<table><thead><tr><th width="160.0625">安全能力</th><th>说明</th><th>相关页面</th></tr></thead><tbody><tr><td>自托管</td><td>资金由企业控制的钱包和智能合约管理，关键操作依赖指定签名地址。</td><td><a href="security/self-custody.md">自托管说明</a></td></tr><tr><td>合约可审计</td><td>合约逻辑、资金流转和链上交易可被验证，降低黑盒风险。</td><td><a href="security/audit-report.md">合约审计</a></td></tr><tr><td>操作最佳实践</td><td>企业可通过钱包管理、权限拆分、网络确认和操作复核降低人为风险。</td><td><a href="security/best-practices.md">安全最佳实践</a></td></tr><tr><td>漏洞响应</td><td>安全问题可通过披露流程进入处理和跟踪。</td><td><a href="security/vulnerability-report.md">漏洞披露政策</a></td></tr></tbody></table>

## 如何开始

如果你是第一次了解 BlockATM，可以按下面路径阅读：

1. 先了解 BlockATM 的定位和核心概念。
2. 根据业务需要选择收币或付币产品。
3. 查看支持的网络、钱包和费用。
4. 选择 Widget SDK、Open API 或 Webhook 进行接入。
5. 按安全最佳实践配置企业钱包和操作权限。

| 阅读目标            | 推荐页面                                                                        |
| --------------- | --------------------------------------------------------------------------- |
| 了解 BlockATM 是什么 | [概述](blockatm/what-is-blockatm.md)                                          |
| 理解基础概念          | [核心概念](blockatm/core-concepts.md)                                           |
| 快速体验流程          | [快速开始](blockatm/quickstart.md)                                              |
| 查看支持网络          | [支持的网络](https://app.gitbook.com/s/XEfzS05BPO0tTODCeSnr/zhi-chi-de-wang-luo) |
| 配置钱包            | [钱包教程](zhi-chi-de-wang-luo/qian-bao-jiao-cheng/)                            |

## 快速接入

| 接入方式           | 适合场景                           | 接入时间     |
| -------------- | ------------------------------ | -------- |
| **Widget SDK** | 快速集成收银台，适合希望尽快上线收款能力的团队。       | 约 30 分钟  |
| **Open API**   | 深度定制业务流程，适合已有支付、订单或财务系统的团队。    | 约 2-4 小时 |
| **Webhook**    | 接收支付、付币和订单事件，适合需要自动对账和状态同步的系统。 | 约 1 小时   |

[查看 Widget SDK →](https://app.gitbook.com/s/XEfzS05BPO0tTODCeSnr/widget-sdk)\
[查看 Open API →](https://app.gitbook.com/s/XEfzS05BPO0tTODCeSnr/open-api)\
[查看 Webhook →](https://app.gitbook.com/s/XEfzS05BPO0tTODCeSnr/webhook)

## 费用说明

| 类型       | 费用        |
| -------- | --------- |
| 创建智能合约   | 200 USD/个 |
| 收币（连接钱包） | 2 USD/笔   |
| 收币（扫描支付） | 0.4%/笔    |
| 付币       | 1 USD/笔   |



## 角色分工

| 角色  | 可以重点关注                           |
| --- | -------------------------------- |
| 运营  | 关注异常订单、用户支付状态和日常处理流程。            |
| 财务  | 关注收款、付币费用规则、余额变化、付币记录、链上交易和对账审计。 |
| 管理员 | 关注合约创建、钱包配置、权限分配、安全策略和集成配置。      |

## 更新日志

| 版本     | 日期         | 说明                  |
| ------ | ---------- | ------------------- |
| V2.3.0 | 2026-01-13 | 上线授权管理模式            |
| V2.2.0 | 2025-10-15 | 支持 Ethereum/Tron 提现 |
| V2.1.0 | 2025-06-10 | 初始版本                |

[查看完整更新日志 →](https://app.gitbook.com/s/XEfzS05BPO0tTODCeSnr/geng-xin-ri-zhi)

***

## 需要帮助？

* Telegram：@Passto\_john
* 邮箱：[john.feng@chixi88.com](mailto:john.feng@chixi88.com)
* [常见问题](https://app.gitbook.com/s/XEfzS05BPO0tTODCeSnr/chang-jian-wen-ti)
* [收款集成指南](faq/collect.md)
* [付币集成指南](faq/payout.md)

