# Config Query API

Config Query API is used to retrieve public configuration data such as cashier info, network list, and token list.

## API List

| API | Method | Description | Auth |
|------|------|------|------|
| `/admin/api/v2/pub/cashier/info` | GET | Query cashier configuration info | API Key required |
| `/admin/api/v2/pub/coin/list` | GET | Query supported token list | Public |
| `/admin/api/v2/pub/network/list` | GET | Query supported network list | Public |

## Environment

| Environment | Base URL |
|------|----------|
| Production | `https://open.blockatm.net` |
| Test | `https://test-open.blockatm.net` |

---

## Query Cashier Configuration Info

Get detailed configuration info for a specified cashier, including supported tokens, networks, payment methods, etc.

### Request

```http
GET /admin/api/v2/pub/cashier/info?cashierId={cashierId}
Content-Type: application/json
BlockATM-API-Key: your_api_key
```

### Request Parameters

| Parameter | Type | Location | Required | Description |
|------|------|------|------|------|
| cashierId | String | Query | Yes | Cashier ID |

### Response Example

```json
{
  "code": "0",
  "message": "success",
  "data": {
    "cashierId": "cs_1234567890",
    "merchantId": "mch_9876543210",
    "merchantName": "Sample Merchant",
    "supportedNetworks": [
      {
        "chainId": "1",
        "networkName": "Ethereum",
        "symbol": "ETH",
        "decimals": 18
      },
      {
        "chainId": "42161",
        "networkName": "Arbitrum",
        "symbol": "ETH",
        "decimals": 18
      },
      {
        "chainId": "TRON",
        "networkName": "TRON",
        "symbol": "TRX",
        "decimals": 6
      }
    ],
    "supportedCoins": [
      {
        "symbol": "USDT",
        "name": "Tether USD",
        "chainId": "1",
        "contractAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
        "decimals": 6,
        "minAmount": "10",
        "maxAmount": "1000000"
      },
      {
        "symbol": "USDC",
        "name": "USD Coin",
        "chainId": "1",
        "contractAddress": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
        "decimals": 6,
        "minAmount": "10",
        "maxAmount": "1000000"
      }
    ],
    "paymentMethods": [
      {
        "type": "wallet",
        "name": "Wallet Connect Payment",
        "enabled": true
      },
      {
        "type": "scan",
        "name": "Scan Payment",
        "enabled": true
      }
    ],
    "webhookUrl": "https://your-domain.com/webhook/blockatm",
    "status": "active",
    "createdAt": 1711900800000
  }
}
```

### Response Field Description

| Field | Type | Description |
|------|------|------|
| cashierId | String | Cashier ID |
| merchantId | String | Merchant ID |
| merchantName | String | Merchant name |
| supportedNetworks | Array | Supported network list |
| supportedNetworks[].chainId | String | Network Chain ID |
| supportedNetworks[].networkName | String | Network name |
| supportedNetworks[].symbol | String | Mainnet token symbol |
| supportedCoins | Array | Supported token list |
| supportedCoins[].symbol | String | Token symbol |
| supportedCoins[].contractAddress | String | Token contract address |
| supportedCoins[].minAmount | String | Minimum payment amount |
| supportedCoins[].maxAmount | String | Maximum payment amount |
| paymentMethods | Array | Payment method list |
| webhookUrl | String | Webhook notification URL |
| status | String | Cashier status: active/inactive |

---

## Query Token List

Get all tokens supported by BlockATM.

### Request

```http
GET /admin/api/v2/pub/coin/list
Content-Type: application/json
```

{% hint style="info" %}
**Note**: This is a public API, no API Key authentication required.
{% endhint %}

### Response Example

```json
{
  "code": "0",
  "message": "success",
  "data": [
    {
      "symbol": "USDT",
      "name": "Tether USD",
      "networks": [
        {
          "chainId": "1",
          "networkName": "Ethereum",
          "contractAddress": "0xdac17f958d2ee523a2206206994597c13d831ec7",
          "decimals": 6,
          "minAmount": "10",
          "maxAmount": "1000000",
          "withdrawFee": "1"
        },
        {
          "chainId": "42161",
          "networkName": "Arbitrum",
          "contractAddress": "0xfd086bc7cd5c481dcc9c85ebe478a1c0b69fcbb9",
          "decimals": 6,
          "minAmount": "10",
          "maxAmount": "1000000",
          "withdrawFee": "0.5"
        },
        {
          "chainId": "TRON",
          "networkName": "TRON",
          "contractAddress": "TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t",
          "decimals": 6,
          "minAmount": "10",
          "maxAmount": "1000000",
          "withdrawFee": "1"
        }
      ]
    },
    {
      "symbol": "USDC",
      "name": "USD Coin",
      "networks": [
        {
          "chainId": "1",
          "networkName": "Ethereum",
          "contractAddress": "0xa0b86991c6218b36c1d19d4a2e9eb0ce3606eb48",
          "decimals": 6,
          "minAmount": "10",
          "maxAmount": "1000000",
          "withdrawFee": "1"
        }
      ]
    }
  ]
}
```

### Response Field Description

| Field | Type | Description |
|------|------|------|
| symbol | String | Token symbol |
| name | String | Token full name |
| networks | Array | Supported network list |
| networks[].chainId | String | Network Chain ID |
| networks[].contractAddress | String | Token contract address |
| networks[].decimals | Number | Token decimals |
| networks[].minAmount | String | Minimum transaction amount |
| networks[].maxAmount | String | Maximum transaction amount |
| networks[].withdrawFee | String | Withdrawal fee |

---

## Query Network List

Get all blockchain networks supported by BlockATM.

### Request

```http
GET /admin/api/v2/pub/network/list
Content-Type: application/json
```

{% hint style="info" %}
**Note**: This is a public API, no API Key authentication required.
{% endhint %}

### Response Example

```json
{
  "code": "0",
  "message": "success",
  "data": [
    {
      "chainId": "1",
      "networkName": "Ethereum",
      "shortName": "ETH",
      "symbol": "ETH",
      "decimals": 18,
      "explorerUrl": "https://etherscan.io",
      "rpcUrl": "https://mainnet.infura.io/v3/",
      "status": "active",
      "avgBlockTime": 12,
      "avgGasFee": "10-50 Gwei"
    },
    {
      "chainId": "42161",
      "networkName": "Arbitrum One",
      "shortName": "Arbitrum",
      "symbol": "ETH",
      "decimals": 18,
      "explorerUrl": "https://arbiscan.io",
      "rpcUrl": "https://arb1.arbitrum.io/rpc",
      "status": "active",
      "avgBlockTime": 1,
      "avgGasFee": "0.1-1 Gwei"
    },
    {
      "chainId": "TRON",
      "networkName": "TRON",
      "shortName": "TRX",
      "symbol": "TRX",
      "decimals": 6,
      "explorerUrl": "https://tronscan.org",
      "rpcUrl": "https://api.trongrid.io",
      "status": "active",
      "avgBlockTime": 3,
      "avgGasFee": "~1 USDT"
    }
  ]
}
```

### Response Field Description

| Field | Type | Description |
|------|------|------|
| chainId | String | Network Chain ID |
| networkName | String | Network full name |
| shortName | String | Network short name |
| symbol | String | Mainnet token symbol |
| decimals | Number | Token decimals |
| explorerUrl | String | Block explorer URL |
| rpcUrl | String | RPC node URL |
| status | String | Network status: active/maintenance |
| avgBlockTime | Number | Average block time (seconds) |
| avgGasFee | String | Average Gas fee |

---

## Error Codes

| Error Code | Description | Solution |
|--------|------|---------|
| 0 | Success | - |
| ERROR_000101 | Cashier does not exist | Check if cashierId is correct |
| ERROR_000102 | Cashier is closed | Contact admin to enable cashier |
| ERROR_000401 | Unauthorized access | Check if API Key is correct |
| ERROR_000500 | Internal server error | Contact technical support |

## Usage Examples

### cURL Examples

```bash
# Query cashier configuration
curl -X GET "https://open.blockatm.net/admin/api/v2/pub/cashier/info?cashierId=cs_123456" \
  -H "BlockATM-API-Key: your_api_key"

# Query token list
curl -X GET "https://open.blockatm.net/admin/api/v2/pub/coin/list"

# Query network list
curl -X GET "https://open.blockatm.net/admin/api/v2/pub/network/list"
```

### JavaScript Examples

```javascript
// Query cashier configuration
async function getCashierInfo(cashierId, apiKey) {
  const response = await fetch(
    `https://open.blockatm.net/admin/api/v2/pub/cashier/info?cashierId=${cashierId}`,
    {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json',
        'BlockATM-API-Key': apiKey
      }
    }
  );
  const result = await response.json();
  return result.data;
}

// Query token list
async function getCoinList() {
  const response = await fetch(
    'https://open.blockatm.net/admin/api/v2/pub/coin/list'
  );
  const result = await response.json();
  return result.data;
}
```

## Best Practices

### 1. Cache Config Data

Config data changes infrequently, recommended to fetch and cache on application start:

```javascript
// Fetch config on application start
const coinList = await getCoinList();
localStorage.setItem('coinList', JSON.stringify(coinList));

// Refresh cache after 24 hours
const cacheTime = localStorage.getItem('coinListTime');
if (Date.now() - cacheTime > 24 * 60 * 60 * 1000) {
  // Refresh cache
}
```

### 2. Error Handling

```javascript
try {
  const cashierInfo = await getCashierInfo(cashierId, apiKey);
  if (!cashierInfo) {
    throw new Error('Cashier does not exist');
  }
  // Handle cashier info
} catch (error) {
  console.error('Failed to get cashier info:', error);
  // Show user-friendly error message
}
```

### 3. Network Switching

Automatically switch networks based on user selection:

```javascript
function switchNetwork(chainId) {
  const network = networkList.find(n => n.chainId === chainId);
  if (network) {
    // Update UI display
    // Update available token list
  }
}
```

## Next Steps

- [Collection API →](payment-api.md)
- [Payout API →](payout-api.md)
- [Widget SDK →](../widget-sdk/README.md)
