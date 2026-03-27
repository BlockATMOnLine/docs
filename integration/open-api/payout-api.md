# 付币 API

付币相关接口说明。

## 创建付币订单

创建一个新的付币订单。

### 请求

```
POST /order/api/v2/payout/order
```

{% hint style="warning" %}
**签名要求**：此接口需要签名认证。
{% endhint %}

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| contractId | String | 是 | 付币合约 ID |
| orderNo | String | 是 | 商户订单号（建议唯一） |
| recipient | String | 是 | 收款地址 |
| amount | String | 是 | 付币金额 |
| symbol | String | 是 | 代币符号（USDT/USDC） |
| payoutType | Integer | 否 | 支付类型：1=余额，2=授权，默认 1 |
| fromAddress | String | 授权模式必填 | 授权地址 |
| remark | String | 否 | 备注 |

### 请求示例

```json
{
  "contractId": "CONTRACT_001",
  "orderNo": "PAYOUT_20250401_001",
  "recipient": "TR7NHqjeK692FJNHQpJd7cY8x",
  "amount": "100.00",
  "symbol": "USDT",
  "payoutType": 1,
  "remark": "User withdrawal"
}
```

### 响应参数

| 参数 | 类型 | 说明 |
|------|------|------|
| orderNo | String | 订单号 |
| orderId | String | 订单 ID |
| status | Integer | 订单状态 |
| createTime | Long | 创建时间 |

### 订单状态

| 状态值 | 说明 |
|--------|------|
| 0 | 待审核 |
| 1 | 审核通过 |
| 2 | 审核拒绝 |
| 3 | 执行中 |
| 4 | 执行成功 |
| 5 | 执行失败 |

## 查询付币订单

查询付币订单详情。

### 请求

```
GET /order/api/v2/payout/detail
```

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| orderNo | String | 是 | 商户订单号 |

### 响应参数

| 参数 | 类型 | 说明 |
|------|------|------|
| orderNo | String | 订单号 |
| contractId | String | 合约 ID |
| recipient | String | 收款地址 |
| amount | String | 付币金额 |
| symbol | String | 代币符号 |
| payoutType | Integer | 支付类型 |
| txId | String | 链上交易哈希 |
| status | Integer | 订单状态 |
| fee | String | 手续费 |
| createTime | Long | 创建时间 |
| updateTime | Long | 更新时间 |
| failReason | String | 失败原因（失败时返回） |

## 批量创建付币订单

批量创建付币订单。

### 请求

```
POST /order/api/v2/payout/batch
```

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| contractId | String | 是 | 付币合约 ID |
| orders | Array | 是 | 订单列表 |
| orders[].orderNo | String | 是 | 商户订单号 |
| orders[].recipient | String | 是 | 收款地址 |
| orders[].amount | String | 是 | 付币金额 |
| orders[].symbol | String | 是 | 代币符号 |
| payoutType | Integer | 否 | 支付类型 |

### 请求示例

```json
{
  "contractId": "CONTRACT_001",
  "payoutType": 1,
  "orders": [
    {
      "orderNo": "PAYOUT_001",
      "recipient": "TR7NHqjeK692FJNHQpJd7cY8x",
      "amount": "50.00",
      "symbol": "USDT"
    },
    {
      "orderNo": "PAYOUT_002",
      "recipient": "TR7NHqjeK692FJNHQpJd7cY8x",
      "amount": "30.00",
      "symbol": "USDT"
    }
  ]
}
```

### 响应参数

| 参数 | 类型 | 说明 |
|------|------|------|
| successCount | Integer | 成功数量 |
| failCount | Integer | 失败数量 |
| orders | Array | 订单结果列表 |
