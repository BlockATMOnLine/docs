# 请求格式

BlockATM Open API 的请求和响应格式说明。

## 请求格式

### Base URL

| 环境 | URL |
|------|-----|
| 生产环境 | `https://open.blockatm.net` |
| 测试环境 | `https://test-open.blockatm.net` |

### Headers

| Header | 说明 | 示例 |
|--------|------|------|
| Content-Type | 请求内容类型 | application/json |
| BlockATM-api-Key | API 密钥 | pk_payment_xxx |
| BlockATM-Request-Time | 请求时间戳 | 1742725435000 |
| BlockATM-Signature-V2 | 签名 | abc123... |
| BlockATM-Rec_Window | 有效时间窗口（毫秒） | 30000 |

### 请求体

使用 JSON 格式：

```json
{
  "custNo": "860001",
  "orderNo": "ORDER_20250401_001",
  "amount": "100.00",
  "symbol": "USDT"
}
```

## 响应格式

### 成功响应

```json
{
  "code": "200",
  "success": true,
  "data": {
    "orderNo": "ORDER_20250401_001",
    "txId": "abc123...",
    "status": "SUCCESS"
  },
  "trace": "uuid-xxx",
  "msg": ""
}
```

### 失败响应

```json
{
  "code": "000101",
  "success": false,
  "data": null,
  "trace": "uuid-xxx",
  "msg": "System error"
}
```

## 响应参数

| 参数 | 类型 | 说明 |
|------|------|------|
| code | String | 状态码，200 表示成功 |
| success | Boolean | 请求是否成功 |
| data | Object | 响应数据体 |
| trace | String | 链路追踪 ID |
| msg | String | 提示信息 |

## 状态码

| code | 说明 |
|------|------|
| 200 | 成功 |
| 000101 | 系统错误 |
| 000102 | 参数类型错误 |
| 000103 | 缺少必填参数 |
| 000105 | 请求无效 |
| 000106 | 重复请求 |
| 000112 | API Key 必填 |
| 000201 | 数据已存在 |
| 000202 | 数据不存在 |
| 000204 | 状态无效 |

[查看完整错误码 →](error-codes.md)
