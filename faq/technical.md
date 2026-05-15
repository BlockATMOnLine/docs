# Technical

Frequently asked technical questions about API and integration.

## What is the API rate limit?

| Rate Limit Dimension | Limit                |
| -------------------- | -------------------- |
| Per API Key          | 1000 requests/minute |
| Per IP               | 2000 requests/minute |
| Per Endpoint         | 100 requests/minute  |

[View rate limit details →](../integration/open-api/rate-limit.md)

## What to do if Webhook is not received?

{% stepper %}
{% step %}
#### Check Configuration

Confirm Webhook URL is correctly configured and publicly accessible.
{% endstep %}

{% step %}
#### Check Response

Ensure your server returns HTTP 200.
{% endstep %}

{% step %}
#### Check Logs

Check server logs to confirm if requests arrived.
{% endstep %}

{% step %}
#### Verify Signature

Confirm Webhook signature verification passes.
{% endstep %}

{% step %}
#### Contact Support

If problem persists, contact technical support.
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**Retry Mechanism**: BlockATM will retry within 24 hours. Please ensure your processing logic is idempotent.
{% endhint %}

## How to solve signature verification failure?

Common causes:

| Cause           | Solution                                 |
| --------------- | ---------------------------------------- |
| Sorting error   | Sort parameters in ASCII ascending order |
| Timestamp error | Use millisecond timestamp                |
| Key error       | Check Secret Key                         |
| Encoding error  | Use UTF-8 encoding                       |

[View signature generation examples →](../integration/open-api/authentication.md)

## How to handle concurrent requests?

**Idempotency**: The same order number cannot be repeatedly created.

**Recommendations**:

1. Use unique order numbers (like UUID)
2. Prevent duplicate submissions on frontend
3. Implement idempotent checks on backend

## How to handle HTTP 429 response?

Indicates request exceeds rate limit.

**Solutions**:

1. Reduce request frequency
2. Implement request queue
3. Use cache to reduce duplicate requests

## How to debug API requests?

{% tabs %}
{% tab title="Using cURL" %}
```bash
curl -X POST https://test-open.blockatm.net/order/api/v2/payout/order \
  -H "Content-Type: application/json" \
  -H "BlockATM-api-Key: YOUR_API_KEY" \
  -H "BlockATM-Request-Time: 1742725435000" \
  -H "BlockATM-Signature-V2: YOUR_SIGNATURE" \
  -d '{"contractId":"xxx","orderNo":"xxx",...}'
```
{% endtab %}

{% tab title="Using Postman" %}
1. Create new request
2. Set Headers (apiKey, timestamp, signature)
3. Set Body (JSON)
4. Send request
{% endtab %}
{% endtabs %}

## What to do if blockchain network is congested?

| Solution       | Description                             |
| -------------- | --------------------------------------- |
| Wait           | Most congestion recovers within minutes |
| Increase Gas   | (Ethereum) Increase Gas Price           |
| Switch Network | Switch to TRON or Arbitrum              |

{% hint style="info" %}
**Recommendation**: For time-sensitive businesses, recommend using TRON network with fast confirmation.
{% endhint %}
