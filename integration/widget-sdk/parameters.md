# Widget SDK 参数说明

本文档介绍 Widget SDK 的完整参数，帮助您正确配置收银台。

## 快速参考

### 必填参数

| 参数 | 说明 | 示例 |
|------|------|------|
| apiKey | API 密钥，用于识别您的账户 | pk_tXIWLvPP7Z0ckeyTrWvn2en0... |
| cashierId | 收银台 ID，从管理后台获取 | 86100021 |

### 常用参数

| 参数 | 说明 | 默认值 | 示例 |
|------|------|--------|------|
| orderNo | 商户订单号（建议唯一） | 自动生成 | Order20250101 |
| amount | 支付金额 | - | 100 |
| symbol | 代币符号 | USDT | USDT |
| chainId | 网络 | TRON | TRON / ETH / ARB |
| lang | 语言 | en-US | zh-CN / en-US |
| theme | 主题 | light | light / dark |

## 完整参数列表

```javascript
window.BlockATM.init(document.getElementById('container'), {
  // ========== 必填参数 ==========
  cashierId: '86100021',       // 收银台 ID（必填）
  
  // ========== 订单参数 ==========
  orderNo: 'ORDER_001',       // 商户订单号（可选，建议唯一）
  amount: '100',              // 支付金额（可选）
  symbol: 'USDT',             // 代币符号：USDT、USDC 等
  chainId: 'TRON',            // 网络：TRON / ETH / ARB
  
  // ========== 界面参数 ==========
  lang: 'zh-CN',              // 语言：zh-CN / en-US
  theme: 'light',             // 主题：light / dark
  
  // ========== 高级参数 ==========
  defaultChainId: 'TRON',     // 默认网络（用户可切换）
  currency: 'USDT',          // 固定货币（用户不可选）
  defaultCurrency: 'USDT',    // 默认货币（用户可选）
  redirectUrl: 'https://...', // 支付完成后跳转 URL
  hidePaymentMethods: [],      // 隐藏支付方式
  defaultPaymentMethod: 'connect_wallet',  // 默认支付方式
  
  // ========== 回调 ==========
  callback: function(result) {
    if (result.type === 'finish') {
      // 支付完成
      console.log('支付成功', result.data);
    } else if (result.type === 'cancel') {
      // 用户取消
      console.log('用户取消');
    }
  }
});
```

## 参数说明

### 必填参数

| 参数 | 类型 | 说明 |
|------|------|------|
| cashierId | string | 收银台 ID，在管理后台「收银台」→「集成」中获取 |

### 订单参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| orderNo | string | 自动生成 | 商户订单号，建议唯一，便于对账 |
| amount | string | - | 支付金额 |
| symbol | string | USDT | 代币符号，支持 USDT、USDC 等 |
| chainId | string | TRON | 网络标识：TRON / ETH / ARB |

{% hint style="info" %}
**chainId 说明**：指定后用户不能切换到其他网络。如需让用户选择网络，请使用 `defaultChainId`。
{% endhint %}

### 界面参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| lang | string | en-US | 语言：zh-CN（简体中文）、en-US（英文） |
| theme | string | light | 主题：light（浅色）、dark（深色） |
| redirectUrl | string | - | 支付完成后跳转的 URL |

### 货币与网络

| 参数 | 类型 | 说明 |
|------|------|------|
| currency | string | 固定货币，指定后用户不能选择其他货币 |
| defaultCurrency | string | 默认货币，用户仍可选择其他货币 |
| defaultChainId | string | 默认网络，用户仍可切换到其他网络 |

{% hint style="info" %}
**优先级**：如果同时指定 `currency` 和 `defaultCurrency`，则 `currency` 优先，用户不能选择其他货币。
{% endhint %}

### 支付方式

| 方式 | 说明 |
|------|------|
| connect_wallet | 钱包连接支付 |
| scan | 扫码支付 |

| 参数 | 类型 | 说明 |
|------|------|------|
| defaultPaymentMethod | string | 默认支付方式 |
| hidePaymentMethods | array | 要隐藏的支付方式列表 |

### 回调参数

回调函数 `callback` 接收结果对象：

```javascript
callback: function(result) {
  // result.type: 'cancel' 或 'finish'
  // result.data: 支付结果数据
}
```

**回调数据示例**：

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

## 网络与代币

| chainId | 网络 | 支持代币 |
|---------|------|---------|
| TRON | TRON (TRC20) | USDT, USDC, TRX |
| ETH | Ethereum (ERC20) | USDT, USDC, ETH |
| ARB | Arbitrum (ARB20) | USDT, USDC, ETH |

## 错误码

| code | 说明 |
|------|------|
| 1001 | 参数错误 |
| 1002 | 收银台不存在 |
| 1003 | 签名验证失败 |
| 1004 | 订单已过期 |
| 1005 | 网络错误 |

## 下一步

- [查看集成示例 →](integration.md)
- [查看签名认证 →](../open-api/authentication.md)
