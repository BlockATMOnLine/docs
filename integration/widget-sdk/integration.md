# Three-Step Integration

This guide helps you complete Widget SDK integration in 30 minutes.

## Overview

Widget SDK is the fastest way to integrate BlockATM,只需三步即可在您的网站或应用中嵌入加密货币收银台：

1. Import SDK script
2. Add cashier container
3. Initialize SDK

## Integration Flowchart

The following diagram shows the complete integration process:

## Step 1: Import SDK

Import BlockATM SDK in your HTML page:

{% tabs %}
{% tab title="Production" %}
```html
<script src="https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=YOUR_API_KEY"></script>
```
{% endtab %}

{% tab title="Test" %}
```html
<script src="https://test-pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=YOUR_API_KEY"></script>
```
{% endtab %}
{% endtabs %}

## Step 2: Add Container

Add a container element on the page where the cashier will be displayed:

```html
<div id="blockatm-container"></div>
```

{% hint style="info" %}
**Container Style**: It is recommended to set appropriate height and width for the container to accommodate the cashier interface.
{% endhint %}

## Step 3: Initialize

Initialize SDK in your application code:

```javascript
window.BlockATM.init(
  document.getElementById('blockatm-container'),
  {
    cashierId: 'YOUR_CASHIER_ID',
    orderNo: 'ORDER_' + Date.now(),
    amount: '100',
    symbol: 'USDT',
    chainId: 'TRON',
    callback: function(result) {
      if (result.type === 'finish') {
        console.log('Payment successful!', result.data);
      }
    }
  }
);
```

## Get API Key

You can get API Key in BlockATM admin dashboard:

1. Login to BlockATM admin dashboard
2. Go to "Cashier" → "Integration"

{% hint style="info" %}
**Integration Info**: On the integration page, you can get cashier ID and API key.
{% endhint %}

## Complete Example

```html
<!DOCTYPE html>
<html>
<head>
  <title>BlockATM Integration Example</title>
  <style>
    #blockatm-container {
      min-height: 400px;
      width: 100%;
      max-width: 500px;
    }
  </style>
</head>
<body>
  <h1>Cryptocurrency Payment</h1>

  <div id="blockatm-container"></div>

  <script src="https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=YOUR_API_KEY"></script>

  <script>
    window.BlockATM.init(
      document.getElementById('blockatm-container'),
      {
        cashierId: 'YOUR_CASHIER_ID',
        orderNo: 'ORDER_' + Date.now(),
        amount: '100',
        symbol: 'USDT',
        chainId: 'TRON',
        lang: 'en-US',
        callback: function(result) {
          if (result.type === 'finish') {
            console.log('Payment successful!', result.data);
          } else if (result.type === 'cancel') {
            console.log('User cancelled');
          }
        }
      }
    );
  </script>
</body>
</html>
```

## Next Steps

* [View complete parameter description →](parameters.md)
* [View API documentation →](../open-api/)
* [View Webhook configuration →](../webhooks/)
