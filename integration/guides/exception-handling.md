# Exception Order Handling

Under normal circumstances, collection orders are processed smoothly. However, sometimes exception situations may occur, such as:

- **Order Lost**: User selected scan payment and completed payment, but order status didn't update to "Success"
- **Notification Exception**: Order's Webhook notification was sent, but merchant didn't receive it

For the above two exception situations, handling is required - but only operator address of cashier has permission to handle such exceptions. Operator address must be added to cashier by Owner.

## Add Cashier Operator

### Operation Steps

1. **Enter Operator Management**: On cashier page, click "Operation" button to open operator popup

2. **Add Operator**: In operator popup, click "Add Operator"

3. **Fill Information**: In add operator popup, enter operator's wallet address and nickname

4. **Confirm Add**: After filling, click "Add" to successfully add operator

{% hint style="info" %}
**Permission Description**: Operators can only handle exception orders for their assigned cashiers, cannot access other cashier's data.
{% endhint %}

## Operator Login

After operator wallet connects to BlockATM DApp, can only see cashiers they have permission for.

**Login Steps**:

1. Connect operator wallet to BlockATM DApp
2. Find corresponding cashier in cashier list
3. Click "Collection Orders" in cashier to go to order page

## Order Lost — Manually Complete Order

Scan payment orders may encounter order lost (transaction status is processing, cancelled, expired or failed).

### Identify Order Lost

If user reports payment completed but order status not updated, can check in order list:

| Order Status | Description | Handling Method |
|---------|------|---------|
| Processing | Waiting for blockchain confirmation | Wait or manually complete |
| Cancelled | User actively cancelled | No handling needed |
| Expired | Payment not completed due to timeout | No handling needed |
| Failed | Payment failed | Troubleshoot cause and retry |

### Manually Complete Order

If order indeed paid but not received, operator can manually complete order within **72 hours**.

**Manual Completion Conditions**:
- Order status is processing, cancelled, expired or failed
- User indeed completed on-chain transfer
- Operator has operation permission for that cashier

**Operation Steps**:

1. Find exception order to handle in order list
2. Click "Complete Transaction" button in operation column
3. Select order completion reason in popup
4. Enter TXID (blockchain transaction hash) for user's completed transfer
5. Click "Confirm" to complete order

{% hint style="warning" %}
**TXID Verification**: Before manually completing order, must verify authenticity of TXID on blockchain explorer to confirm transaction indeed exists and transfer amount is correct.
{% endhint %}

### Completion Reason Options

| Option | Description |
|------|------|
| User paid but not received | User paid via scan method, but order status not updated |
| On-chain confirmed but status not synced | Blockchain confirmed, but BlockATM system didn't update timely |

## Notification Exception — Resend Notification

If business system did not receive Webhook notification for collection order with "Completed" or "Failed" status, operator can resend notification within **72 hours**.

**Resend Notification Conditions**:
- Order status is completed or failed
- Business system indeed didn't receive Webhook notification
- Operator has operation permission for that cashier

**Operation Steps**:

1. Find order to resend notification in order list
2. Click "Resend Notification" button in operation column
3. System will resend Webhook notification to your configured Webhook address

{% hint style="info" %}
**Notification Resend Limit**: Each order can be resent up to 3 times. After exceeding limit, cannot resend.
{% endhint %}

## FAQ

**Q: Why only 72-hour window?**

A: To ensure transaction security, BlockATM limits the time window for exception order handling. After 72 hours, on-chain data correlation with order data may weaken, continuing to handle may cause data inconsistency.

**Q: What will order status become after manually completing?**

A: Manually completed order status will become "Completed".

**Q: Can I batch handle multiple exception orders?**

A: Currently batch processing not supported, need to handle order by order.

**Q: After resending notification, business system still didn't receive?**

A: Please check:
1. Is Webhook address publicly accessible
2. Is server responding normally with HTTP 200
3. Is firewall blocking requests
4. Is there receiving record in logs

## Best Practices

{% hint style="info" %}
**Recommendations**:

1. **Handle Timely**: Should handle promptly after receiving exception order notification, avoid exceeding 72-hour window

2. **Verify First**: Must verify TXID on blockchain explorer before manually completing orders

3. **Log Monitoring**: Recommend configuring log monitoring to timely discover Webhook notification failures

4. **Data Reconciliation**: Regularly reconcile with order data in BlockATM admin dashboard to ensure data consistency
{% endhint %}
