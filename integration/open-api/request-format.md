# Request Format

BlockATM Open API request and response format description.

## Request Format

### Base URL

| Environment | URL |
|------|-----|
| Production | `https://open.blockatm.net` |
| Test | `https://test-open.blockatm.net` |

### Headers

| Header | Description | Example |
|--------|------|------|
| Content-Type | Request content type | application/json |
| BlockATM-API-Key | API Key | pk_payment_xxx |
| BlockATM-Request-Time | Request timestamp | 1742725435000 |
| BlockATM-Signature-V2 | Signature | abc123... |
| BlockATM-Rec_Window | Validity window (milliseconds) | 30000 |

### Request Body

Use JSON format:

```json
{
  "custNo": "860001",
  "orderNo": "ORDER_20250401_001",
  "amount": "100.00",
  "symbol": "USDT"
}
```

## Response Format

### Success Response

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

### Failure Response

```json
{
  "code": "000101",
  "success": false,
  "data": null,
  "trace": "uuid-xxx",
  "msg": "System error"
}
```

## Response Parameters

| Parameter | Type | Description |
|------|------|------|
| code | String | Status code, 200 means success |
| success | Boolean | Whether request succeeded |
| data | Object | Response data body |
| trace | String | Trace ID |
| msg | String | Message |

## Status Codes

| code | Description |
|------|------|
| 200 | Success |
| 000101 | System error |
| 000102 | Parameter type error |
| 000103 | Missing required parameter |
| 000105 | Invalid request |
| 000106 | Duplicate request |
| 000112 | API Key required |
| 000201 | Data already exists |
| 000202 | Data does not exist |
| 000204 | Invalid status |

[View complete error codes →](error-codes.md)
