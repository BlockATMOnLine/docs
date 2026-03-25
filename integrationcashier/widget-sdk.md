---
description: Three steps to complete the integration
---

# Widget SDK

With BlockATM's SDK, you can easily integrate BlockATM's checkout system into your website. Use the interactive diagram below to quickly understand the integration process：



<figure><img src="../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

### 1. import

Add the SDK as a script to your HTML file.

{% tabs %}
{% tab title="Prod " %}
```
<script src="https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=[API_KEY]"></script>
```
{% endtab %}

{% tab title="Sandbox " %}
```
<script src="https://test-pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=[API_KEY]"></script>
```
{% endtab %}
{% endtabs %}

Once you've included this script, you're ready to initialize the Web SDK and start integrating with our suite of cryptocurrency payment solutions.



You can obtain the Api Key in the merchant APP backend, **【Cashier】** ->[**【Integrate】**](https://app.blockatm.net/)

<figure><img src="../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

### 2. Initialize

Initialize the SDK in your application with the flow, variant,lang and any parameters related to deposit cryptocurrency.

[See full parameters](https://github.com/BlockATMOnLine/BlockATM_V2/blob/main_en/integrationCashier/widget-param.md)

```javascript
// Initialize and show cashier.
window.BlockATM.init(
  document.getElementById('blockatm-container'), // Container element
  {
    ...options,
    signature,                                  // Signature from backend
    callback: ({ type }) => {                   // Payment result callback
      switch(type) {
        case 'cancel': 
          // Handle payment cancellation
          break;
        case 'finish': 
          // Handle successful payment
          break;
      }
    }
  }
);
```



