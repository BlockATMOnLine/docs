# Payout FAQ

Frequently asked questions about the payout (Batch Payout) function.

## What's the difference between balance payout and allowance payout?

| Comparison | Balance Mode | Allowance Mode |
|-------|---------|---------|
| Fund Location | Contract balance | User wallet balance |
| Pre-deposit | Required | Not required |
| Authorization | Not required | Requires user authorization |
| Fund Risk | Contract risk | User wallet risk |

**Selection Advice**:
- Funds already in contract: Use balance mode
- Need flexible dispatch: Use allowance mode

[Learn more →](../products/batch-payout/allowance-mode.md)

## How is allowance calculated?

Actual available allowance = min(authorized allowance, wallet balance)

**Example**:
```
User authorized allowance: 5000 USDT
User wallet balance: 3000 USDT
Actual available allowance: 3000 USDT
```

{% hint style="info" %}
**V5.8.0**: Frontend directly queries on-chain to get real-time authorized allowance.
{% endhint %}

## How long does payout take?

| Network | Arrival Time |
|------|---------|
| TRON | Instant (~3 sec) |
| Ethereum | 15-30 sec |
| Arbitrum | Instant (~1-3 min) |

{% hint style="info" %}
**Arrival Time** refers to blockchain confirmation time. Actual arrival also depends on network congestion.
{% endhint %}

## What to do if payout fails?

| Failure Reason | Solution |
|---------|---------|
| Insufficient balance | Deposit to contract |
| Insufficient allowance | User increases authorization amount |
| Whitelist restriction | Add address to whitelist (balance mode) |
| Signature verification failed | Check signature key and algorithm |

## Can I do batch payout?

Yes, batch payout is supported.

**Batch Endpoint**:
```
POST /order/api/v2/payout/batch
```

**Limitations**:
- Single order amount cannot be 0
- Total batch orders have limits (contact technical support)

## Does allowance payout require user operation?

Yes, allowance payout requires the user to pre-authorize the contract to spend their tokens.

**User Operation Steps**:
1. Execute authorization in wallet (Tronscan / Etherscan)
2. Or guide authorization through BlockATM admin dashboard

{% hint style="info" %}
**Authorization is one-time**: After user authorizes, multiple payouts can be made within the allowance without re-authorizing each time.
{% endhint %}

## How to revoke authorization?

Call in wallet:
```
approve(contract address, 0)
```

Or directly via [Tronscan](https://tronscan.org) / [Etherscan](https://etherscan.io).

## Is there a limit on contract balance?

No upper limit, but recommendations:

- Don't make single payout amount too large
- Execute large payments in batches

## What is the payout fee?

| Type | Fee |
|------|------|
| Contract Creation | 200 USD/contract |
| Payout | 1 USD/transaction |

[View detailed fees →](../products/batch-payout/fees.md)
