# Payout Fee Description

Batch Payout service detailed fee description.

## Fee Overview

| Type | Fee | Charge Timing |
|------|------|---------|
| Create contract | 200 USD/contract | When creating contract |
| Payout | 1 USD/order | When payout executed |

{% hint style="info" %}
**Fee Currency**: All fees are charged in stablecoins (such as USDT, USDC), exchange rate fixed at 1:1.
{% endhint %}

## Detailed Description

### Contract Creation Fee

200 USD service fee is charged for each payout contract created.

| Contract Type | Fee |
|---------|------|
| Balance mode contract | 200 USDT |
| Allowance mode contract | 200 USDT |

### Payout Handling Fee

| Fee Item | Amount |
|-------|------|
| Fixed fee | 1 USD/order |

**Example**: You pay user 1000 USDT, handling fee is 1 USDT, actual deduction is 1001 USDT (for balance mode deducted from contract, for allowance mode deducted from user authorization amount).

## Fee Calculation Examples

### Scenario 1: Single Payout

```javascript
// Merchant initiates payout
Payout amount: 1000 USDT
Fee: 1 USDT
Total deduction: 1001 USDT
```

### Scenario 2: Batch Payout

```javascript
// Merchant initiates batch payout of 3 orders
Order 1: 500 USDT
Order 2: 300 USDT
Order 3: 200 USDT

Total payout amount: 1000 USDT
Fee: 3 USDT (1 USDT × 3 orders)
Total deduction: 1003 USDT
```

{% hint style="info" %}
**Batch Payout**: Each order is charged handling fee separately, no discount for batching.
{% endhint %}

### Scenario 3: Authorization Payout

```javascript
// User authorizes 5000 USDT to contract
// Merchant initiates payout of 1000 USDT

Payout amount: 1000 USDT
Fee: 1 USDT
Total deduction: 1001 USDT (deducted from user authorization amount)

User remaining authorization: 5000 - 1001 = 3999 USDT
```

## Comparison with Collection Fees

| Comparison | Collection | Payout |
|-------|------|------|
| Create contract | 200 USD | 200 USD |
| Transaction fee | 2 USD or 0.4% | 1 USD |
| Fee bearer | Merchant | Merchant |

## FAQ

**Q: Are handling fees the same for authorization payout and balance payout?**

A: Yes, both are 1 USD/order.

**Q: Is handling fee charged if payout fails?**

A: No, handling fee is only charged for successfully executed payouts.

**Q: Can I customize the fee rate?**

A: Currently customization not supported, fee standard is fixed.

**Q: Do I need to pay fees in test environment?**

A: Test environment uses test tokens, no actual fees.
