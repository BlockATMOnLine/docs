# 收币 API

收币相关接口说明。

## 创建收币订单

创建一个新的收币订单。

### 请求

```
POST /order/api/v2/pay/order
```

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| cashierId | String | 是 | 收银台 ID |
| orderNo | String | 是 | 商户订单号（建议唯一） |
| amount | String | 是 | 收币金额 |
| symbol | String | 是 | 代币符号（USDT/USDC） |
| chainId | String | 否 | 网络（TRON/ETH/ARB），默认 TRON |
| redirectUrl | String | 否 | 支付完成后跳转 URL |
| sign | String | 是 | 签名 |

### 请求示例

```json
{
  "cashierId": "CASHIER_001",
  "orderNo": "ORDER_20250401_001",
  "amount": "100.00",
  "symbol": "USDT",
  "chainId": "TRON",
  "redirectUrl": "https://your-app.com/payment/callback",
  "sign": "abc123..."
}
```

### 响应参数

| 参数 | 类型 | 说明 |
|------|------|------|
| orderNo | String | 订单号 |
| payUrl | String | 支付链接 |
| qrCode | String | 收款二维码（Base64） |
| expireTime | Long | 订单过期时间（毫秒） |

### 响应示例

```json
{
  "code": "200",
  "success": true,
  "data": {
    "orderNo": "ORDER_20250401_001",
    "payUrl": "https://pay.blockatm.net/cashier/xxx",
    "qrCode": "data:image/png;base64,...",
    "expireTime": 1742729435000
  }
}
```

## 查询收币订单

查询收币订单详情。

### 请求

```
GET /order/api/v2/payorder/detail
```

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| orderNo | String | 是 | 商户订单号 |

### 响应参数

| 参数 | 类型 | 说明 |
|------|------|------|
| orderNo | String | 订单号 |
| cashierId | String | 收银台 ID |
| amount | String | 订单金额 |
| symbol | String | 代币符号 |
| chainId | String | 网络 |
| status | Integer | 订单状态 |
| txId | String | 链上交易哈希 |
| createTime | Long | 创建时间 |
| updateTime | Long | 更新时间 |

### 订单状态

| 状态值 | 说明 |
|--------|------|
| 0 | 待支付 |
| 1 | 支付中 |
| 2 | 已支付 |
| 3 | 已取消 |
| 4 | 已过期 |

## 查询收币订单列表

查询商户的收币订单列表。

### 请求

```
GET /order/api/v2/payorder/list
```

### 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| startTime | Long | 否 | 开始时间（毫秒） |
| endTime | Long | 否 | 结束时间（毫秒） |
| status | Integer | 否 | 订单状态 |
| page | Integer | 否 | 页码，默认 1 |
| pageSize | Integer | 否 | 每页数量，默认 20 |

### 响应参数

| 参数 | 类型 | 说明 |
|------|------|------|
| list | Array | 订单列表 |
| total | Integer | 总数 |
| page | Integer | 当前页 |
| pageSize | Integer | 每页数量 |
