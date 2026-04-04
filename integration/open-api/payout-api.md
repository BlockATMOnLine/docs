# Payout API

Payout API documentation.

## Create Payout Order

Create a new payout order.

### Request

```
POST /order/api/v2/payout/order
```

{% hint style="warning" %}
**Signature Required**: This API requires signature authentication.
{% endhint %}

### Request Parameters

| Parameter | Type | Required | Description |
|------|------|------|------|
| contractId | String | Yes | Payout contract ID |
| orderNo | String | Yes | Merchant order number (recommended unique) |
| recipient | String | Yes | Recipient address |
| amount | String | Yes | Payout amount |
| symbol | String | Yes | Token symbol (USDT/USDC) |
| payoutType | Integer | No | Payment type: 1=balance, 2=allowance, default 1 |
| fromAddress | String | Required for allowance mode | Authorization address |
| remark | String | No | Remark |

### Request Example

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

### Response Parameters

| Parameter | Type | Description |
|------|------|------|
| orderNo | String | Order number |
| orderId | String | Order ID |
| status | String | Order status |
| createTime | Long | Create time |

### Order Status

| Status | Description |
|--------|------|
| SUCCESS | Payout successful |
| REFUSE | Payout refused |

## Query Payout Order

Query payout order details.

### Request

```
GET /order/api/v2/payout/detail
```

### Request Parameters

| Parameter | Type | Required | Description |
|------|------|------|------|
| orderNo | String | Yes | Merchant order number |

### Response Parameters

| Parameter | Type | Description |
|------|------|------|
| orderNo | String | Order number |
| contractId | String | Contract ID |
| recipient | String | Recipient address |
| amount | String | Payout amount |
| symbol | String | Token symbol |
| payoutType | Integer | Payment type |
| txId | String | Blockchain transaction hash |
| status | String | Order status |
| fee | String | Fee |
| gasAmount | String | Gas consumption |
| createTime | Long | Create time |
| updateTime | Long | Update time |

## Batch Create Payout Orders

Batch create payout orders.

### Request

```
POST /order/api/v2/payout/batch
```

### Request Parameters

| Parameter | Type | Required | Description |
|------|------|------|------|
| contractId | String | Yes | Payout contract ID |
| orders | Array | Yes | Order list |
| orders[].orderNo | String | Yes | Merchant order number |
| orders[].recipient | String | Yes | Recipient address |
| orders[].amount | String | Yes | Payout amount |
| orders[].symbol | String | Yes | Token symbol |
| payoutType | Integer | No | Payment type |

### Request Example

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

### Response Parameters

| Parameter | Type | Description |
|------|------|------|
| successCount | Integer | Success count |
| failCount | Integer | Fail count |
| orders | Array | Order result list |
