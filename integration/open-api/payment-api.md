# 收款 API

收币相关接口说明。

## 查询收币订单

查询收币订单详情。

### 请求

```
GET /order/api/v2/payorder/detail
```

### 请求参数

| 参数      | 类型     | 必填 | 说明    |
| ------- | ------ | -- | ----- |
| orderNo | String | 是  | 商户订单号 |

### 响应参数

| 参数         | 类型     | 说明     |
| ---------- | ------ | ------ |
| orderNo    | String | 订单号    |
| cashierId  | String | 收银台 ID |
| amount     | String | 订单金额   |
| symbol     | String | 代币符号   |
| chainId    | String | 网络     |
| status     | String | 订单状态   |
| txId       | String | 链上交易哈希 |
| createTime | Long   | 创建时间   |
| updateTime | Long   | 更新时间   |

### 订单状态

| 状态值       | 说明  |
| --------- | --- |
| PENDING   | 待支付 |
| SUCCESS   | 已支付 |
| EXPIRED   | 已过期 |
| CANCELLED | 已取消 |
