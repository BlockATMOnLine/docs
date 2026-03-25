# Operation Process

Users invoke the cashier of BlockATM on your business platform, confirm the order information, and choose a payment method (wallet payment, scan code transfer payment) to make a payment. After the user pays, they wait for blockchain confirmation. During this process, BlockATM will continue to monitor blockchain transaction information. Once the smart contract for receiving coins successfully receives funds, it will send a notification to your business system via Webhook.

<figure><img src="../../.gitbook/assets/English (2).jpg" alt=""><figcaption></figcaption></figure>

{% hint style="info" %}
The cashier payment method can be configured as needed. If you need to connect wallet payments, the cashier needs to be associated with the Web3 receiving contract. If you need to scan and transfer payments, the cashier needs to be associated with the scanning receiving contract. Both contracts are for receiving coins. For more details, please see: [Collection contract](shou-bi-zhi-neng-he-yue.md)
{% endhint %}

