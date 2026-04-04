# Signature Verification

BlockATM Webhook uses HMAC-SHA256 signature to verify authenticity of requests.

## Verification Principle

```
HMAC-SHA256(secretKey, payload)
```

## Request Headers

| Header | Description |
| --------------------- | -------------- |
| BlockATM-Signature-V2 | HMAC-SHA256 signature |
| BlockATM-Request-Time | Request timestamp (milliseconds) |
| BlockATM-Event | Event type |

## Verification Steps

### Step 1: Extract Parameters

Extract from request headers:

* `BlockATM-Signature-V2`: Signature
* `BlockATM-Request-Time`: Timestamp
* Request body: payload

### Step 2: Calculate Signature

Calculate signature using the same algorithm:

```python
# Python example
import hmac
import hashlib

def verify_signature(payload, timestamp, signature, secret_key):
    # Concatenate string to sign
    message = payload + "&time=" + timestamp

    # Calculate signature
    expected_signature = hmac.new(
        secret_key.encode('utf-8'),
        message.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    # Compare signatures
    return hmac.compare_digest(expected_signature, signature)
```

### Step 3: Timestamp Verification

Verify if request is within valid time window:

```python
def verify_timestamp(timestamp, max_window=300000):  # 5 minutes
    current_time = int(time.time() * 1000)
    return abs(current_time - int(timestamp)) < max_window
```

## Complete Verification Example

{% tabs %}
{% tab title="Python" %}
```python
import hmac
import hashlib
import time

WEBHOOK_SECRET = "your_webhook_secret"

def verify_webhook(request):
    # Get request headers
    signature = request.headers.get('BlockATM-Signature-V2')
    timestamp = request.headers.get('BlockATM-Request-Time')
    event = request.headers.get('BlockATM-Event')

    # Get request body
    payload = request.get_data(as_text=True)

    # Verify timestamp
    current_time = int(time.time() * 1000)
    if abs(current_time - int(timestamp)) > 300000:
        return False, "Timestamp expired"

    # Calculate signature
    message = payload + "&time=" + timestamp
    expected_sig = hmac.new(
        WEBHOOK_SECRET.encode('utf-8'),
        message.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    # Compare signatures
    if not hmac.compare_digest(expected_sig, signature):
        return False, "Invalid signature"

    return True, "OK"

# Handle Webhook
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    valid, msg = verify_webhook(request)
    if not valid:
        return msg, 401

    data = request.json
    event = data.get('event')

    if event == 'payment':
        # Handle collection event
        pass
    elif event == 'payout':
        # Handle payout event
        pass

    return "OK", 200
```
{% endtab %}

{% tab title="Java" %}
```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.security.MessageDigest;

public class WebhookVerifier {

    private static final String SECRET = "your_webhook_secret";

    public static boolean verify(String payload, String timestamp, String signature) {
        try {
            // Verify timestamp
            long currentTime = System.currentTimeMillis();
            if (Math.abs(currentTime - Long.parseLong(timestamp)) > 300000) {
                return false;
            }

            // Calculate signature
            String message = payload + "&time=" + timestamp;
            Mac sha256Hmac = Mac.getInstance("HmacSHA256");
            SecretKeySpec secretKeySpec = new SecretKeySpec(
                SECRET.getBytes(StandardCharsets.UTF_8), "HmacSHA256"
            );
            sha256Hmac.init(secretKeySpec);
            byte[] hash = sha256Hmac.doFinal(message.getBytes(StandardCharsets.UTF_8));

            // Convert to hex
            StringBuilder expectedSig = new StringBuilder();
            for (byte b : hash) {
                expectedSig.append(String.format("%02x", b));
            }

            return expectedSig.toString().equals(signature);
        } catch (Exception e) {
            return false;
        }
    }
}
```
{% endtab %}

{% tab title="Go" %}
```go
package main

import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/hex"
    "fmt"
    "time"
)

const SECRET = "your_webhook_secret"

func verify(payload, timestamp, signature string) (bool, string) {
    // Verify timestamp
    currentTime := time.Now().UnixMilli()
    ts, _ := time.ParseDuration("300000ms")
    if currentTime-int64(timestamp) > int64(ts.Milliseconds()) {
        return false, "Timestamp expired"
    }

    // Calculate signature
    message := payload + "&time=" + timestamp
    h := hmac.New(sha256.New, []byte(SECRET))
    h.Write([]byte(message))
    expectedSig := hex.EncodeToString(h.Sum(nil))

    // Compare signatures
    if !hmac.Equal([]byte(expectedSig), []byte(signature)) {
        return false, "Invalid signature"
    }

    return true, "OK"
}

func main() {
    valid, msg := verify("{\"amount\":\"13.41\"}", "1693212861000", "abc123...")
    fmt.Printf("Valid: %v, Message: %s\n", valid, msg)
}
```
{% endtab %}

{% tab title="C++" %}
```cpp
#include <iostream>
#include <string>
#include <sstream>
#include <iomanip>
#include <openssl/hmac.h>
#include <openssl/sha.h>
#include <chrono>
#include <cmath>

const std::string SECRET = "your_webhook_secret";

std::string hmacSha256(const std::string& secret, const std::string& data) {
    unsigned char* hash = HMAC(EVP_sha256(),
                               secret.c_str(), secret.length(),
                               (unsigned char*)data.c_str(), data.length(),
                               NULL, NULL);

    std::stringstream ss;
    for(int i = 0; i < SHA256_DIGEST_LENGTH; i++) {
        ss << std::hex << std::setw(2) << std::setfill('0') << (int)hash[i];
    }
    return ss.str();
}

bool verifyTimestamp(const std::string& timestamp, int maxWindowMs = 300000) {
    auto now = std::chrono::system_clock::now();
    auto nowMs = std::chrono::duration_cast<std::chrono::milliseconds>(now.time_since_epoch()).count();

    long ts = std::stol(timestamp);
    return std::abs(nowMs - ts) < maxWindowMs;
}

bool verify(const std::string& payload, const std::string& timestamp, const std::string& signature) {
    // Verify timestamp
    if (!verifyTimestamp(timestamp)) {
        return false;
    }

    // Calculate signature
    std::string message = payload + "&time=" + timestamp;
    std::string expectedSig = hmacSha256(SECRET, message);

    return expectedSig == signature;
}

int main() {
    std::string payload = "{\"amount\":\"13.41\"}";
    std::string timestamp = "1693212861000";
    std::string signature = "abc123...";

    std::cout << "Valid: " << (verify(payload, timestamp, signature) ? "true" : "false") << std::endl;
    return 0;
}
```
{% endtab %}
{% endtabs %}

## Security Suggestions

{% hint style="warning" %}
#### Why Must We Verify Signature?

Webhook signature verification is a key step to ensure your system security. If you don't verify signatures, attackers can impersonate BlockATM to send forged payment notifications to your system, which may lead to:

* **Fake Delivery**: Shipping to users based on forged "payment successful" notification, but payment actually not received
* **Financial Loss**: Confirming transactions based on forged "payout successful" notification, but no such transaction exists on blockchain
* **Business Chaos**: Forged order status updates may cause your business logic to malfunction

Therefore, **you must always verify signatures**, never skip this security check.
{% endhint %}

### 1. Always Verify Signatures

{% hint style="danger" %}
**Warning**: Never skip signature verification step. Even in development and testing environments, it is recommended to maintain correct implementation of signature verification logic.
{% endhint %}

**Verification Flow**:

1. Extract `BlockATM-Signature-V2` and `BlockATM-Request-Time` from request headers
2. Get raw request body, **do not** get after parsing, because JSON parsing may change content
3. Concatenate signature string: `raw payload + "&time=" + timestamp`
4. Calculate signature using same HMAC-SHA256 algorithm
5. Use constant-time comparison (`hmac.compare_digest`) to prevent timing attacks

**Common Mistakes**:

```python
# ❌ Wrong: Direct string comparison may be susceptible to timing attacks
if expected_signature == received_signature:

# ✅ Correct: Use constant-time comparison
if hmac.compare_digest(expected_signature, received_signature):
```

### 2. Verify Timestamp

Timestamp verification is used to prevent **Replay Attacks**. Attackers may intercept legitimate Webhook notifications and resend them, trying to trigger your business logic repeatedly.

**Recommended Time Window**:

* Recommended time window: **5 minutes** (300000 milliseconds)
* If BlockATM server time differs from your server time, you can appropriately relax this, but it is not recommended to exceed 15 minutes

**Verification Logic**:

```python
def verify_timestamp(timestamp, max_window_ms=300000):
    current_time_ms = int(time.time() * 1000)
    try:
        event_time_ms = int(timestamp)
    except (ValueError, TypeError):
        return False  # Invalid timestamp format
    return abs(current_time_ms - event_time_ms) < max_window_ms
```

### 3. Use HTTPS

{% hint style="danger" %}
**Strongly Recommended**: Production environments must use HTTPS protocol Webhook URL. Using HTTP plaintext transmission may cause requests to be intercepted, tampered, or replayed.
{% endhint %}

**Certificate Requirements**:

* Recommended to use SSL/TLS certificates issued by trusted CAs
* Avoid using self-signed certificates
* Regularly check certificate validity and renew in time

### 4. Idempotent Processing

The same Webhook event may be received multiple times, reasons include:

* BlockATM retry mechanism (see below)
* Your server response timed out but actually processed successfully
* Network jitter causing duplicate requests

**Suggestions for Implementing Idempotency**:

```python
def handle_payment_event(event):
    order_id = event.get('orderNo')

    # Check if order has been processed (using Redis or database)
    if order_processed(order_id):
        return  # Skip already processed orders

    # Execute business logic
    process_payment(event)

    # Mark order as processed
    mark_order_processed(order_id)
```

{% hint style="info" %}
**BlockATM Retry Mechanism**: If Webhook delivery fails (your server does not return HTTP 200), BlockATM will retry according to the following strategy:

* 1st retry: after 1 minute
* 2nd retry: after 5 minutes
* 3rd retry: after 30 minutes
* 4th retry: after 2 hours
* 5th retry: after 24 hours

If all 5 retries fail, delivery will stop.
{% endhint %}

### 5. Log Recording

Complete log recording is crucial for troubleshooting and security audits.

**Recommended logs to record**:

| Log Type | Content | Purpose |
| ---- | ---------------- | ------ |
| Reception log | Time received, event type, order number | Audit trail |
| Verification log | Signature verification result, timestamp verification result | Troubleshooting |
| Processing log | Business processing start/success/failure | Business audit |
| Error log | Exception info, stack trace | Bug location |

**Log Example**:

```python
import logging

logger = logging.getLogger('webhook')

def handle_webhook(request):
    logger.info(f"Received webhook: event={event_type}, order={order_id}")

    # Verify
    if not verify_signature(request):
        logger.warning(f"Signature verification failed: order={order_id}")
        return "Invalid signature", 401

    # Process
    try:
        process_event(event)
        logger.info(f"Successfully processed: order={order_id}")
    except Exception as e:
        logger.error(f"Processing failed: order={order_id}, error={e}")
        raise

    return "OK", 200
```

### 6. Respond Quickly

{% hint style="info" %}
**Important**: Your Webhook processing interface should **immediately return HTTP 200 after receiving the request**, then process business logic asynchronously. This prevents request timeout and BlockATM retries caused by long processing time.
{% endhint %}

**Recommended Architecture**:

```python
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    # 1. Immediately verify and return
    if not verify_webhook(request):
        return "Invalid", 401

    # 2. Get data
    data = request.json

    # 3. Immediately return 200
    return "OK", 200

    # 4. Asynchronously process (using message queue)
    # Note: This will not block the request
    send_to_queue(data)
```

### 7. Error Handling

{% hint style="danger" %}
**Important**: If your Webhook processing fails (returns non-200 status code), BlockATM will consider delivery failed and will resend according to retry strategy.
{% endhint %}

**Correct Error Handling**:

```python
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    try:
        verify_and_process(request)
        return "OK", 200
    except ValidationError as e:
        logger.warning(f"Validation error: {e}")
        return "Bad Request", 400  # This will cause retry
    except BusinessError as e:
        logger.error(f"Business error: {e}")
        return "Server Error", 500  # This will cause retry
    except Exception as e:
        logger.exception(f"Unexpected error: {e}")
        return "Server Error", 500  # This will cause retry
```

{% hint style="info" %}
**Tip**: If your business logic truly cannot process a certain event (e.g., order already revoked), you can return 200 instead of an error code. This way BlockATM will not retry, avoiding infinite loops.
{% endhint %}
