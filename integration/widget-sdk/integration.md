# 三步集成

本指南将帮助您在 30 分钟内完成 Widget SDK 的集成。

## 概述

Widget SDK 是接入 BlockATM 最快速的方式，只需三步即可在您的网站或应用中嵌入加密货币收银台：

1. 引入 SDK 脚本
2. 添加收银台容器
3. 初始化 SDK

## 集成流程图

下图展示了集成的完整流程：

## 步骤 1：引入 SDK

在您的 HTML 页面中引入 BlockATM SDK：

{% tabs %}
{% tab title="生产环境" %}
```html
<script src="https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=YOUR_API_KEY"></script>
```
{% endtab %}

{% tab title="测试环境" %}
```html
<script src="https://test-pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=YOUR_API_KEY"></script>
```
{% endtab %}
{% endtabs %}

## 步骤 2：添加容器

在页面中添加一个容器元素，收银台将显示在这里：

```html
<div id="blockatm-container"></div>
```

{% hint style="info" %}
**容器样式**：建议为容器设置合适的高度和宽度，以容纳收银台界面。
{% endhint %}

## 步骤 3：初始化

在您的应用代码中初始化 SDK：

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

## 获取 API Key

您可以在 BlockATM 管理后台获取 API Key：

1. 登录 BlockATM 管理后台
2. 进入「收银台」→「集成」

{% hint style="info" %}
**集成信息**：在集成页面，您可以获取收银台 ID 和 API 密钥。
{% endhint %}

## 完整示例

```html
<!DOCTYPE html>
<html>
<head>
  <title>BlockATM 集成示例</title>
  <style>
    #blockatm-container {
      min-height: 400px;
      width: 100%;
      max-width: 500px;
    }
  </style>
</head>
<body>
  <h1>加密货币支付</h1>
  
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
        lang: 'zh-CN',
        callback: function(result) {
          if (result.type === 'finish') {
            console.log('支付成功!', result.data);
          } else if (result.type === 'cancel') {
            console.log('用户取消');
          }
        }
      }
    );
  </script>
</body>
</html>
```

## 下一步

* [查看完整参数说明 →](parameters.md)
* [查看 API 文档 →](../open-api/)
* [查看 Webhook 配置 →](../webhooks/)
