# Collection  Params

When changes occur to a payment order, BlockATM will send notifications via the Webhook Payment event. The included statuses are:

* PENDING: When the payment order has reached the required number of on-chain confirmations.
* SUCCESS: When the payment order has been confirmed as successfully paid.
* EXPIRED: When the customer's order has timed out due to incomplete payment.
* CANCELLED: When the customer cancels the payment order.

BlockATM will send the following fields to your server via the configured Webhook URL:

| name        | comment                                                                                                                                                                                               | example                                    |
| ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| id          | The order number corresponding to the blockATM platform                                                                                                                                               | 8210000374                                 |
| orderNo     | Optional. The order number corresponding to your platform.                                                                                                                                            | O20231022                                  |
| chainId     | ChainId is a unique identifier used in blockchain networks to distinguish a particular network。.you can find the chainId in [https://chainlist.org/](https://chainlist.org/)                          | 1                                          |
| cashierId   | <p>The payment cashier ID for receiving funds.</p><p><br></p>                                                                                                                                         | 199                                        |
| amount      | Actual payment amount by the customer                                                                                                                                                                 | 13.410037                                  |
| custNo      | Optional. Your customerNo who pay the order.                                                                                                                                                          | 860021                                     |
| network     | Network name used to describe the occurrence of this transaction order. If you want to uniquely identify the corresponding network chain, it is recommended to use the chainId. (e.g: Ethereum, TRON) | Ethereum                                   |
| symbol      | The token identifier that the customer pays with.                                                                                                                                                     | USDT                                       |
| txId        | The transaction hash corresponding to the order on the blockchain. You can view the transaction details on the corresponding network's block explorer.                                                | 0x....                                     |
| orderType   | 1：Smart Contract Payment 2.QR Code Payment 3:Smart Contract Address Direct                                                                                                                            | 1                                          |
| status      | <p><strong>CANCELLED</strong><br>EXPIRED</p><p><strong>PENDING</strong></p><p><strong>SUCCESS</strong></p>                                                                                            | **SUCCESS**                                |
| fromAddress | <p>The address used to pay for this order (empty if unpaid).</p><p><br></p>                                                                                                                           | 0xa9e358E33a57E67c9B84618a52f0194C345C8e35 |
| blockTime   | The time the order was on the blockchain                                                                                                                                                              | 1693212861016                              |

Example:

```json
{

  ​      "custNo": "CustNo_00240101",

  ​      "orderNo": "OrderNo_202504010023",

  ​      "id": 8210003616,

  ​      "symbol": "USDT",

  ​      "amount": "2000.00",

  ​      "status": "SUCCESS",

  ​      "txId": "0x7614a9840d9422feaef4671e0ee98dd7092ebcba6e41076285f99d0b2b0de5fe",

  ​      "network": "Ethereum",

  ​      "chainId": "1",

  ​      "fromAddress": "0xa9e358e33a57e67c9b84618a52f0194c345c8e35",

  ​      "cashierId": 86100021

  ​}
```
