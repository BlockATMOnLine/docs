# Error Codes

BlockATM API complete error code reference.

## System Errors (000xxx)

| code | Description |
|------|------|
| 000101 | System error |
| 000102 | Parameter type error |
| 000103 | Missing required parameter |
| 000104 | Object cannot be empty |
| 000105 | Invalid request |
| 000106 | Duplicate request |
| 000107 | System busy |
| 000108 | Token required |
| 000109 | Session expired |
| 000110 | No access permission |
| 000112 | API Key required |
| 000113 | Service unavailable, please retry later |
| 000114 | Service under maintenance |

## Business Errors (0002xx)

| code | Description |
|------|------|
| 000201 | Data already exists |
| 000202 | Data does not exist |
| 000203 | Data unchanged |
| 000204 | Invalid status |
| 000205 | Service request failed |
| 000206 | Field exceeds maximum length |
| 000207 | Field below minimum length |
| 000208 | Unsupported file format |

## Payment Errors (0003xx)

| code | Description |
|------|------|
| 000301 | Order already paid |
| 000302 | Order cancelled |
| 000303 | Order expired |
| 000304 | Order amount mismatch |
| 000305 | Payment method unavailable |
| 000306 | Signature verification failed |
| 000307 | Insufficient balance |
| 000308 | Insufficient authorization |

## Contract Errors (0004xx)

| code | Description |
|------|------|
| 000401 | Contract does not exist |
| 000402 | Invalid contract address |
| 000403 | Insufficient contract balance |
| 000404 | Unsupported token |
| 000405 | Recipient address not in whitelist |

## Handling Suggestions

| Error Type | Suggested Handling |
|---------|---------|
| 000101/000107 | System error, wait and retry |
| 000103/000105 | Check request parameters |
| 000202 | Check if order/contract exists |
| 000303 | Order expired, recreate |
| 000306 | Check if signature is correct |
| 000307/000308 | Recharge or increase authorization |
