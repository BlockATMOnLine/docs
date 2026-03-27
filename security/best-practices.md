# 安全最佳实践

本指南介绍使用 BlockATM 时的安全最佳实践，帮助您保护资金和 API 安全。

## 目录

1. [API Key 管理](#api-key-管理)
2. [签名地址管理](#签名地址管理)
3. [提币地址安全](#提币地址安全)
4. [监控告警配置](#监控告警配置)
5. [应急响应](#应急响应)

---

## API Key 管理

### 风险等级

| 操作 | 风险等级 | 影响 |
|------|---------|------|
| API Key 泄露 | 🔴 高 | 可创建订单、查询数据 |
| Secret Key 泄露 | 🔴 高 | 可伪造签名、恶意调用 |
| Webhook Key 泄露 | 🟡 中 | 可伪造 Webhook 通知 |

### 最佳实践

#### 1. 安全存储

✅ **推荐做法**：

```bash
# 使用环境变量
export BLOCKATM_API_KEY="pck_xxxxxx"
export BLOCKATM_SECRET_KEY="sck_xxxxxx"

# 使用密钥管理服务
AWS Secrets Manager
Azure Key Vault
HashiCorp Vault
```

```javascript
// Node.js 示例
const apiKey = process.env.BLOCKATM_API_KEY;
const secretKey = process.env.BLOCKATM_SECRET_KEY;

// 不要在代码中硬编码
// ❌ 错误示例
const apiKey = "pck_xxxxxx"; // 不要这样做！
```

❌ **危险做法**：

- 将 Key 提交到 Git 仓库
- 在前端代码中暴露 Secret Key
- 通过邮件/聊天工具发送 Key
- 将 Key 写在文档中

#### 2. 权限分离

为不同环境创建不同的 API Key：

| 环境 | 用途 | 权限 |
|------|------|------|
| 开发环境 | 本地开发测试 | 仅测试网络 |
| 生产环境 | 线上业务 | 完整权限 |
| 只读 Key | 数据查询 | 仅查询权限 |

#### 3. 定期轮换

建议每 90 天轮换一次 API Key：

```bash
# 轮换流程
1. 在管理后台生成新 Key
2. 更新应用配置
3. 测试新 Key
4. 禁用旧 Key
5. 记录轮换日志
```

#### 4. 监控使用

设置使用告警：

```javascript
// 监控 API 调用频率
const callCount = await getApiCallCount(last24Hours);
if (callCount > threshold) {
  sendAlert('API 调用异常', { callCount });
}

// 监控失败率
const failRate = await getFailRate(last1Hour);
if (failRate > 0.1) { // 超过 10%
  sendAlert('API 失败率过高', { failRate });
}
```

---

## 签名地址管理

### 什么是签名地址？

签名地址（Signer）是有权从智能合约提取资金的地址。这是最重要的安全控制点。

### 风险等级

| 配置 | 风险等级 | 后果 |
|------|---------|------|
| 使用交易所地址 | 🔴 极高 | 可能无法提现 |
| 使用在线钱包 | 🔴 高 | 私钥可能泄露 |
| 使用热钱包 | 🟡 中 | 联网风险 |
| 使用硬件钱包 | 🟢 低 | 最佳选择 |

### 最佳实践

#### 1. 使用硬件钱包

**推荐设备**：

| 设备 | 价格 | 安全性 | 购买链接 |
|------|------|--------|---------|
| Ledger Nano X | $149 | ⭐⭐⭐⭐⭐ | [ledger.com](https://www.ledger.com) |
| Ledger Nano S Plus | $79 | ⭐⭐⭐⭐⭐ | [ledger.com](https://www.ledger.com) |
| Trezor Model T | $219 | ⭐⭐⭐⭐⭐ | [trezor.io](https://trezor.io) |

**设置步骤**：

```
1. 从官方渠道购买硬件钱包
2. 初始化设备并备份助记词
3. 在 MetaMask 中连接硬件钱包
4. 复制硬件钱包地址作为签名地址
5. 将硬件钱包存放在安全地方
```

#### 2. 多签配置（高级）

对于大额资金，建议使用多签钱包：

| 配置 | 适用场景 | 安全性 |
|------|---------|--------|
| 2/3 多签 | 中小企业 | ⭐⭐⭐⭐ |
| 3/5 多签 | 大型企业 | ⭐⭐⭐⭐⭐ |
| 4/7 多签 | 机构级 | ⭐⭐⭐⭐⭐ |

**多签服务**：
- [Gnosis Safe](https://safe.global) - EVM 网络
- [BitGo](https://www.bitgo.com) - 多链支持

#### 3. 地址验证

在设置签名地址前：

```bash
# 1. 小额测试（推荐）
1. 设置签名地址
2. 创建小额收币订单（如 1 USDT）
3. 完成支付
4. 尝试提现到签名地址
5. 确认到账后，再正式使用

# 2. 地址格式检查
TRON 地址：以 T 开头，34 个字符
Ethereum 地址：以 0x 开头，42 个字符
```

#### 4. 助记词保管

✅ **正确做法**：
- 用笔抄写在纸上
- 存放在保险箱
- 可以存放在银行保险柜
- 考虑防火防水存储

❌ **错误做法**：
- 不要截图保存
- 不要存储在电脑/手机
- 不要通过微信/邮件发送
- 不要告诉任何人

---

## 提币地址安全

### 白名单机制

建议启用提币地址白名单：

```
1. 在管理后台添加常用提币地址
2. 启用白名单验证
3. 只有白名单地址可以提现
4. 新增地址需要审核期（如 24 小时）
```

### 地址验证

每次提币前验证地址：

```javascript
function validateWithdrawAddress(address, network) {
  // TRON 地址
  if (network === 'TRON') {
    if (!address.startsWith('T')) return false;
    if (address.length !== 34) return false;
  }
  
  // Ethereum 地址
  if (network === 'ETHEREUM' || network === 'ARBITRUM') {
    if (!address.startsWith('0x')) return false;
    if (address.length !== 42) return false;
  }
  
  return true;
}

// 首次提现时人工验证
if (isNewAddress(address)) {
  requireManualApproval();
}
```

### 提现限额

设置合理的提现限额：

| 场景 | 单笔限额 | 每日限额 | 审核要求 |
|------|---------|---------|---------|
| 小额提现 | < 1000 USD | < 5000 USD | 自动审批 |
| 中额提现 | 1000-10000 USD | < 50000 USD | 单人审核 |
| 大额提现 | > 10000 USD | > 50000 USD | 双人审核 |

---

## 监控告警配置

### 关键指标

监控以下指标并及时告警：

#### 1. 订单监控

```javascript
// 异常订单告警
const abnormalOrders = await getAbnormalOrders();
if (abnormalOrders.length > 0) {
  sendAlert('发现异常订单', {
    count: abnormalOrders.length,
    orders: abnormalOrders
  });
}

// 大额订单告警
const largeOrders = await getOrders({ amount: { $gt: 10000 } });
if (largeOrders.length > 0) {
  sendAlert('大额订单通知', { orders: largeOrders });
}
```

#### 2. 余额监控

```javascript
// 合约余额不足告警
const contractBalance = await getContractBalance();
const threshold = 1000; // USD

if (contractBalance < threshold) {
  sendAlert('合约余额不足', {
    balance: contractBalance,
    threshold: threshold
  });
}
```

#### 3. Webhook 监控

```javascript
// Webhook 失败告警
const webhookFailures = await getWebhookFailures(last1Hour);
if (webhookFailures > 5) {
  sendAlert('Webhook 连续失败', {
    count: webhookFailures
  });
}
```

### 告警渠道

配置多种告警渠道：

| 渠道 | 优先级 | 响应时间 |
|------|-------|---------|
| 电话 | 🔴 P0 | 立即 |
| 短信 | 🔴 P0 | 1 分钟内 |
| 邮件 | 🟡 P1 | 15 分钟内 |
| Slack/钉钉 | 🟡 P1 | 30 分钟内 |

### 告警分级

```javascript
// P0 - 严重告警（电话 + 短信）
- 资金异常
- 大额盗刷风险
- 系统不可用

// P1 - 重要告警（邮件 + 即时通讯）
- API 失败率过高
- Webhook 连续失败
- 合约余额不足

// P2 - 一般告警（邮件）
- 单笔订单异常
- 配置变更
```

---

## 应急响应

### 发现可疑活动时

#### 1. 立即止损

```bash
# 步骤 1：暂停收银台
进入管理后台 → 收银台 → 暂停

# 步骤 2：禁用 API Key
进入管理后台 → API 设置 → 禁用当前 Key

# 步骤 3：转移资金
如有风险，立即将资金转移到安全地址
```

#### 2. 调查原因

收集以下信息：

- [ ] 异常订单列表
- [ ] API 调用日志
- [ ] Webhook 日志
- [ ] 区块链交易记录
- [ ] 受影响的时间范围

#### 3. 联系 BlockATM

```
邮件：support@blockatm.net
主题：【紧急】安全事件报告 - {商户 ID}

内容：
1. 事件描述
2. 发现时间
3. 影响范围
4. 已采取措施
5. 需要协助事项
```

### 密钥泄露应急

#### Secret Key 泄露

```
1. 立即禁用泄露的 API Key
2. 生成新的 API Key
3. 更新应用配置
4. 检查是否有异常调用
5. 通知用户（如影响用户）
```

#### 助记词泄露

```
1. 立即转移所有资金到新地址
2. 创建新的智能合约
3. 更新所有配置
4. 通知相关方
```

---

## 安全清单

### 上线前检查

- [ ] API Key 使用环境变量存储
- [ ] Secret Key 未提交到代码库
- [ ] 签名地址使用硬件钱包
- [ ] 助记词已安全备份
- [ ] 启用提币地址白名单
- [ ] 配置监控告警
- [ ] 完成小额测试
- [ ] 编写应急响应流程

### 定期审查

- [ ] 每月审查 API 调用日志
- [ ] 每季度轮换 API Key
- [ ] 每半年审查安全配置
- [ ] 每年进行安全审计

---

## 安全资源

### 学习材料

- [以太坊安全最佳实践](https://ethereum.org/zh/developers/docs/security/)
- [智能合约安全](https://github.com/slowmist/Knowledge-Base)
- [OWASP 加密存储](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)

### 安全工具

| 工具 | 用途 | 链接 |
|------|------|------|
| Ledger | 硬件钱包 | [ledger.com](https://www.ledger.com) |
| Gnosis Safe | 多签钱包 | [safe.global](https://safe.global) |
| Revoke.cash | 取消授权 | [revoke.cash](https://revoke.cash) |

---

## 下一步

- [合约审计报告 →](audit-report.md)
- [自托管说明 →](self-custody.md)
- [异常处理指南 →](../integration/guides/exception-handling.md)
