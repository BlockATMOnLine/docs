# Collection API

Collection API documentation.

## Create Collection Order

Create a new collection order.

### Request

```
POST /order/api/v2/pay/order
```

### Request Parameters

| Parameter | Type | Required | Description |
|------|------|------|------|
| cashierId | String | Yes | Cashier ID |
| orderNo | String | Yes | Merchant order number (recommended unique) |
| amount | String | Yes | Collection amount |
| symbol | String | Yes | Token symbol (USDT/USDC) |
| chainId | String | No | Network (TRON/ETH/ARB), default TRON |
| redirectUrl | String | No | Redirect URL after payment |
| sign | String | Yes | Signature |

### Request Example

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

### Response Parameters

| Parameter | Type | Description |
|------|------|------|
| orderNo | String | Order number |
| payUrl | String | Payment link |
| qrCode | String | Collection QR code (Base64) |
| expireTime | Long | Order expiration time (milliseconds) |

### Response Example

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

## Query Collection Order

Query collection order details.

### Request

```
GET /order/api/v2/payorder/detail
```

### Request Parameters

| Parameter | Type | Required | Description |
|------|------|------|------|
| orderNo | String | Yes | Merchant order number |

### Response Parameters

| Parameter | Type | Description |
|------|------|------|
| orderNo | String | Order number |
| cashierId | String | Cashier ID |
| amount | String | Order amount |
| symbol | String | Token symbol |
| chainId | String | Network |
| status | String | Order status |
| txId | String | Blockchain transaction hash |
| createTime | Long | Create time |
| updateTime | Long | Update time |

### Order Status

| Status | Description |
|--------|------|
| PENDING | Pending payment |
| SUCCESS | Paid |
| EXPIRED | Expired |
| CANCELLED | Cancelled |

## Query Collection Order List

Query merchant's collection order list.

### Request

```
GET /order/api/v2/payorder/list
```

### Request Parameters

| Parameter | Type | Required | Description |
|------|------|------|------|
| startTime | Long | No | Start time (milliseconds) |
| endTime | Long | No | End time (milliseconds) |
| status | String | No | Order status |
| page | Integer | No | Page number, default 1 |
| pageSize | Integer | No | Page size, default 20 |

### Response Parameters

| Parameter | Type | Description |
|------|------|------|
| list | Array | Order list |
| total | Integer | Total count |
| page | Integer | Current page |
| pageSize | Integer | Page size |
