# Payment API

Collection API documentation.

## Query Collection Order

Query collection order details.

### Request

```
GET /order/api/v2/payorder/detail
```

### Request Parameters

| Parameter | Type   | Required | Description           |
| --------- | ------ | -------- | --------------------- |
| orderNo   | String | Yes      | Merchant order number |

### Response Parameters

| Parameter  | Type   | Description                 |
| ---------- | ------ | --------------------------- |
| orderNo    | String | Order number                |
| cashierId  | String | Cashier ID                  |
| amount     | String | Order amount                |
| symbol     | String | Token symbol                |
| chainId    | String | Network                     |
| status     | String | Order status                |
| txId       | String | Blockchain transaction hash |
| createTime | Long   | Create time                 |
| updateTime | Long   | Update time                 |

### Order Status

| Status    | Description     |
| --------- | --------------- |
| PENDING   | Pending payment |
| SUCCESS   | Paid            |
| EXPIRED   | Expired         |
| CANCELLED | Cancelled       |
