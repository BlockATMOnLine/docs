# Collection FAQ

Frequently asked questions about the collection (Safepay) function.

## Is there a minimum amount limit for collection?

**No hard limit**.

However, it is recommended to set a reasonable minimum amount because blockchain transfers have Gas fees, and small payments may not be cost-effective.

| Network | Recommended Minimum |
| -------- | ------ |
| TRON | 1 USD |
| Ethereum | 10 USD |
| Arbitrum | 1 USD |

## What to do if order fails?

{% stepper %}
{% step %}
### Check Failure Reason

Check order details in the admin dashboard to understand the failure reason.
{% endstep %}

{% step %}
### Handle Exception Order

**Payment Timeout**: User can initiate payment again
**Amount Mismatch**: User needs to pay again with the correct order amount
**On-chain Failure**: Check wallet authorization or network status
{% endstep %}

{% step %}
### Re-initiate

After handling the exception order, the original order number cannot be reused. A new order needs to be created.
{% endstep %}
{% endstepper %}

## Which is better, TRC20 or ERC20?

| Comparison | TRC20 | ERC20 |
| --- | ---------- | ----------- |
| Fees | Low (~1 TRX) | High ($0.1-$5) |
| Speed | Fast (3 sec) | Slow (15-30 sec) |
| Ecosystem | TRON ecosystem | Ethereum ecosystem |

**Recommendations**:

* Large payments: Recommend TRC20 (low fees)
* Need Ethereum ecosystem: Use ERC20

## How long does it take for funds to arrive after user pays?

| Network | Confirmation Time |
| -------- | --------------- |
| TRON | ~3 sec (1 confirmation) |
| Ethereum | ~1 min (12 confirmations) |
| Arbitrum | ~1-3 min (1 confirmation) |

{% hint style="info" %}
**Arrival Standard**: Funds are considered arrived after blockchain confirmation. BlockATM will immediately update order status and send Webhook notification.
{% endhint %}

## Can I issue refunds?

BlockATM, as a payment channel, does not provide automatic refund functionality.

**Solutions**:

* Manual refund through admin dashboard (withdraw from contract and return to user)
* Or use payout function to refund to user

## What to do if Scan2Pay amount is entered incorrectly?

Scan2Pay requires the user to pay exactly the same amount as the order amount.

**Consequences of Amount Mismatch**:

* Order cannot be automatically matched
* Funds may be lost

**Recommendations**:

1. Prompt user to confirm amount before payment
2. If user enters wrong amount, contact technical support

## How to view collection records?

1. Login to BlockATM admin dashboard
2. Go to "Collection" → "Order List"
3. Filter by time, status, order number, etc.

Or query via API:

```
GET /order/api/v2/payorder/list
```
