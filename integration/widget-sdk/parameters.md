# Widget SDK Parameters

This document introduces complete Widget SDK parameters to help you configure the cashier correctly.

## Quick Reference

### Required Parameters

| Parameter | Description | Example |
|------|------|------|
| apiKey | API key, used to identify your account | pk_tXIWLvPP7Z0ckeyTrWvn2en0... |
| cashierId | Cashier ID, obtained from admin dashboard | 86100021 |

### Common Parameters

| Parameter | Description | Default | Example |
|------|------|--------|------|
| orderNo | Merchant order number (recommended unique) | Auto-generated | Order20250101 |
| amount | Payment amount | - | 100 |
| symbol | Token symbol | USDT | USDT |
| chainId | Network | TRON | TRON / ETH / ARB |
| lang | Language | en-US | zh-CN / en-US |
| theme | Theme | light | light / dark |

## Complete Parameter List

```javascript
window.BlockATM.init(document.getElementById('container'), {
  // ========== Required Parameters ==========
  cashierId: '86100021',       // Cashier ID (required)

  // ========== Order Parameters ==========
  orderNo: 'ORDER_001',       // Merchant order number (optional, recommended unique)
  amount: '100',              // Payment amount (optional)
  symbol: 'USDT',             // Token symbol: USDT, USDC, etc.
  chainId: 'TRON',            // Network: TRON / ETH / ARB

  // ========== UI Parameters ==========
  lang: 'en-US',              // Language: zh-CN / en-US
  theme: 'light',             // Theme: light / dark

  // ========== Advanced Parameters ==========
  defaultChainId: 'TRON',     // Default network (user can switch)
  currency: 'USDT',           // Fixed currency (user cannot select)
  defaultCurrency: 'USDT',    // Default currency (user can select)
  redirectUrl: 'https://...', // Redirect URL after payment
  hidePaymentMethods: [],      // Payment methods to hide
  defaultPaymentMethod: 'connect_wallet',  // Default payment method

  // ========== Callback ==========
  callback: function(result) {
    if (result.type === 'finish') {
      // Payment completed
      console.log('Payment success', result.data);
    } else if (result.type === 'cancel') {
      // User cancelled
      console.log('User cancelled');
    }
  }
});
```

## Parameter Description

### Required Parameters

| Parameter | Type | Description |
|------|------|------|
| cashierId | string | Cashier ID, obtained from admin dashboard "Cashier" → "Integration" |

### Order Parameters

| Parameter | Type | Default | Description |
|------|------|--------|------|
| orderNo | string | Auto-generated | Merchant order number, recommended unique for reconciliation |
| amount | string | - | Payment amount |
| symbol | string | USDT | Token symbol, supports USDT, USDC, etc. |
| chainId | string | TRON | Network identifier: TRON / ETH / ARB |

{% hint style="info" %}
**chainId Description**: Once specified, user cannot switch to other networks. If you want users to select networks, use `defaultChainId`.
{% endhint %}

### UI Parameters

| Parameter | Type | Default | Description |
|------|------|--------|------|
| lang | string | en-US | Language: zh-CN (Simplified Chinese), en-US (English) |
| theme | string | light | Theme: light, dark |
| redirectUrl | string | - | Redirect URL after payment |

### Currency and Network

| Parameter | Type | Description |
|------|------|------|
| currency | string | Fixed currency, user cannot select other currencies after specifying |
| defaultCurrency | string | Default currency, user can still select other currencies |
| defaultChainId | string | Default network, user can still switch to other networks |

{% hint style="info" %}
**Priority**: If both `currency` and `defaultCurrency` are specified, `currency` takes priority, user cannot select other currencies.
{% endhint %}

### Payment Methods

| Method | Description |
|------|------|
| connect_wallet | Wallet connect payment |
| scan | Scan payment |

| Parameter | Type | Description |
|------|------|------|
| defaultPaymentMethod | string | Default payment method |
| hidePaymentMethods | array | List of payment methods to hide |

### Callback Parameters

Callback function `callback` receives result object:

```javascript
callback: function(result) {
  // result.type: 'cancel' or 'finish'
  // result.data: Payment result data
}
```

**Callback Data Example**:

```json
{
  "type": "finish",
  "data": {
    "orderNo": "Order_001",
    "txId": "0x1234...",
    "status": "SUCCESS"
  }
}
```

## Network and Token

| chainId | Network | Supported Tokens |
|---------|------|---------|
| TRON | TRON (TRC20) | USDT, USDC, TRX |
| ETH | Ethereum (ERC20) | USDT, USDC, ETH |
| ARB | Arbitrum (ARB20) | USDT, USDC, ETH |

## Error Codes

| code | Description |
|------|------|
| 1001 | Parameter error |
| 1002 | Cashier does not exist |
| 1003 | Signature verification failed |
| 1004 | Order expired |
| 1005 | Network error |

## Next Steps

- [View integration examples →](integration.md)
- [View signature authentication →](../open-api/authentication.md)
