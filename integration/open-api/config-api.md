# 配置查询 API

配置查询 API 用于获取收银台信息、网络列表、代币列表等公共配置数据。

## 接口列表

| 接口 | 方法 | 说明 | 认证 |
|------|------|------|------|
| `/admin/api/v2/pub/cashier/info` | GET | 查询收银台配置信息 | 需要 API Key |
| `/admin/api/v2/pub/coin/list` | GET | 查询支持的代币列表 | 公开接口 |
| `/admin/api/v2/pub/network/list` | GET | 查询支持的网络列表 | 公开接口 |

## 环境

| 环境 | Base URL |
|------|----------|
| 生产环境 | `https://open.blockatm.net` |
| 测试环境 | `https://test-open.blockatm.net` |

---

## 查询收银台配置信息

获取指定收银台的详细配置信息，包括支持的代币、网络、支付方式等。

### 请求

```http
GET /admin/api/v2/pub/cashier/info?cashierId={cashierId}
Content-Type: application/json
BlockATM-API-Key: your_api_key
```

### 请求参数

| 参数 | 类型 | 位置 | 必填 | 说明 |
|------|------|------|------|------|
| cashierId | String | Query | 是 | 收银台 ID |

### 响应示例

```json
{
  "code": "0",
  "message": "success",
  "data": {
    "cashierId": "cs_1234567890",
    "merchantId": "mch_9876543210",
    "merchantName": "示例商户",
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
        "name": "连接钱包支付",
        "enabled": true
      },
      {
        "type": "scan",
        "name": "扫码支付",
        "enabled": true
      }
    ],
    "webhookUrl": "https://your-domain.com/webhook/blockatm",
    "status": "active",
    "createdAt": 1711900800000
  }
}
```

### 响应字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| cashierId | String | 收银台 ID |
| merchantId | String | 商户 ID |
| merchantName | String | 商户名称 |
| supportedNetworks | Array | 支持的网络列表 |
| supportedNetworks[].chainId | String | 网络 Chain ID |
| supportedNetworks[].networkName | String | 网络名称 |
| supportedNetworks[].symbol | String | 主网代币符号 |
| supportedCoins | Array | 支持的代币列表 |
| supportedCoins[].symbol | String | 代币符号 |
| supportedCoins[].contractAddress | String | 代币合约地址 |
| supportedCoins[].minAmount | String | 最小支付金额 |
| supportedCoins[].maxAmount | String | 最大支付金额 |
| paymentMethods | Array | 支付方式列表 |
| webhookUrl | String | Webhook 通知地址 |
| status | String | 收银台状态：active/inactive |

---

## 查询代币列表

获取 BlockATM 支持的所有代币信息。

### 请求

```http
GET /admin/api/v2/pub/coin/list
Content-Type: application/json
```

{% hint style="info" %}
**说明**：此接口为公开接口，无需 API Key 认证。
{% endhint %}

### 响应示例

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

### 响应字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| symbol | String | 代币符号 |
| name | String | 代币全称 |
| networks | Array | 支持的网络列表 |
| networks[].chainId | String | 网络 Chain ID |
| networks[].contractAddress | String | 代币合约地址 |
| networks[].decimals | Number | 代币精度 |
| networks[].minAmount | String | 最小交易金额 |
| networks[].maxAmount | String | 最大交易金额 |
| networks[].withdrawFee | String | 提现手续费 |

---

## 查询网络列表

获取 BlockATM 支持的所有区块链网络信息。

### 请求

```http
GET /admin/api/v2/pub/network/list
Content-Type: application/json
```

{% hint style="info" %}
**说明**：此接口为公开接口，无需 API Key 认证。
{% endhint %}

### 响应示例

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

### 响应字段说明

| 字段 | 类型 | 说明 |
|------|------|------|
| chainId | String | 网络 Chain ID |
| networkName | String | 网络全称 |
| shortName | String | 网络简称 |
| symbol | String | 主网代币符号 |
| decimals | Number | 代币精度 |
| explorerUrl | String | 区块浏览器 URL |
| rpcUrl | String | RPC 节点 URL |
| status | String | 网络状态：active/maintenance |
| avgBlockTime | Number | 平均出块时间（秒） |
| avgGasFee | String | 平均 Gas 费用 |

---

## 错误码

| 错误码 | 说明 | 解决方案 |
|--------|------|---------|
| 0 | 成功 | - |
| ERROR_000101 | 收银台不存在 | 检查 cashierId 是否正确 |
| ERROR_000102 | 收银台已关闭 | 联系管理员启用收银台 |
| ERROR_000401 | 未授权访问 | 检查 API Key 是否正确 |
| ERROR_000500 | 服务器内部错误 | 联系技术支持 |

## 使用示例

### cURL 示例

```bash
# 查询收银台配置
curl -X GET "https://open.blockatm.net/admin/api/v2/pub/cashier/info?cashierId=cs_123456" \
  -H "BlockATM-API-Key: your_api_key"

# 查询代币列表
curl -X GET "https://open.blockatm.net/admin/api/v2/pub/coin/list"

# 查询网络列表
curl -X GET "https://open.blockatm.net/admin/api/v2/pub/network/list"
```

### JavaScript 示例

```javascript
// 查询收银台配置
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

// 查询代币列表
async function getCoinList() {
  const response = await fetch(
    'https://open.blockatm.net/admin/api/v2/pub/coin/list'
  );
  const result = await response.json();
  return result.data;
}
```

## 最佳实践

### 1. 缓存配置数据

配置数据变化不频繁，建议在应用启动时获取并缓存：

```javascript
// 应用启动时获取配置
const coinList = await getCoinList();
localStorage.setItem('coinList', JSON.stringify(coinList));

// 24 小时后刷新缓存
const cacheTime = localStorage.getItem('coinListTime');
if (Date.now() - cacheTime > 24 * 60 * 60 * 1000) {
  // 刷新缓存
}
```

### 2. 错误处理

```javascript
try {
  const cashierInfo = await getCashierInfo(cashierId, apiKey);
  if (!cashierInfo) {
    throw new Error('收银台不存在');
  }
  // 处理收银台信息
} catch (error) {
  console.error('获取收银台信息失败:', error);
  // 显示友好错误提示
}
```

### 3. 网络切换

根据用户选择自动切换网络：

```javascript
function switchNetwork(chainId) {
  const network = networkList.find(n => n.chainId === chainId);
  if (network) {
    // 更新 UI 显示
    // 更新可用代币列表
  }
}
```

## 下一步

- [收币 API →](payment-api.md)
- [付币 API →](payout-api.md)
- [Widget SDK →](../widget-sdk/README.md)
