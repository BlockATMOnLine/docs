# Signature Authentication

BlockATM API uses HMAC-SHA256 for request signatures to ensure authenticity and integrity of requests.

## Authentication Parameters

| Header | Description | Required |
|--------|------|------|
| BlockATM-API-Key | API Key | Yes |
| BlockATM-Request-Time | Request timestamp (milliseconds) | Required for signed APIs |
| BlockATM-Signature-V2 | HMAC-SHA256 signature | Required for signed APIs |
| BlockATM-Rec_Window | Request validity window (milliseconds) | No (default 30000) |

{% hint style="info" %}
**Get Keys**: Get API Key and Secret Key in BlockATM admin dashboard "Cashier" → "Integration".
{% endhint %}

## Signature Algorithm

### Step 1: Concatenate Parameter String

Sort request parameters (JSON body) by key in ASCII ascending order, concatenate as `key=value` format, separated by `&`.

**Original Request Example:**
```json
{
  "custNo": "86000123",
  "orderNo": "202504001399",
  "lang": "zh-CN"
}
```

**After Processing:**
```
custNo=86000123&lang=zh-CN&orderNo=202504001399
```

### Step 2: Concatenate Timestamp

Append timestamp to the end of parameter string:

```
custNo=86000123&lang=zh-CN&orderNo=202504001399&time=1742723373000
```

### Step 3: Calculate HMAC-SHA256

Use Secret Key to calculate HMAC-SHA256 signature on the concatenated string:

```
HMAC-SHA256(secretKey, payload)
```

## Multi-language Examples

{% tabs %}
{% tab title="Java" %}
```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.util.*;

public class BlockATMSigner {

    public static String generateSignature(Map<String, String> params, long timestamp, String secretKey)
        throws NoSuchAlgorithmException, InvalidKeyException {

        // Step 1: Sort parameters by ASCII
        Map<String, String> sortedMap = new TreeMap<>(params);
        List<String> paramList = new ArrayList<>();
        for (Map.Entry<String, String> entry : sortedMap.entrySet()) {
            paramList.add(entry.getKey() + "=" + entry.getValue());
        }
        String sortedParams = String.join("&", paramList);

        // Step 2: Concatenate timestamp
        String payload = sortedParams + "&time=" + timestamp;

        // Step 3: Calculate HMAC-SHA256
        Mac sha256Hmac = Mac.getInstance("HmacSHA256");
        SecretKeySpec secretKeySpec = new SecretKeySpec(
            secretKey.getBytes(StandardCharsets.UTF_8),
            "HmacSHA256"
        );
        sha256Hmac.init(secretKeySpec);
        byte[] hash = sha256Hmac.doFinal(payload.getBytes(StandardCharsets.UTF_8));

        // Convert to hex string
        StringBuilder signature = new StringBuilder();
        for (byte b : hash) {
            signature.append(String.format("%02x", b));
        }
        return signature.toString();
    }
}
```
{% endtab %}

{% tab title="Python" %}
```python
import hmac
import hashlib
from urllib.parse import urlencode

def generate_signature(params: dict, timestamp: int, secret_key: str) -> str:
    # Step 1: Sort by key
    sorted_params = sorted(params.items())
    query_string = urlencode(sorted_params)

    # Step 2: Concatenate timestamp
    payload = f"{query_string}&time={timestamp}"

    # Step 3: Calculate HMAC-SHA256
    signature = hmac.new(
        secret_key.encode('utf-8'),
        payload.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    return signature
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
    "sort"
    "strings"
)

func generateSignature(params map[string]string, timestamp int64, secretKey string) string {
    // Step 1: Sort by key
    keys := make([]string, 0, len(params))
    for k := range params {
        keys = append(keys, k)
    }
    sort.Strings(keys)

    // Concatenate
    var queryParts []string
    for _, k := range keys {
        queryParts = append(queryParts, fmt.Sprintf("%s=%s", k, params[k]))
    }
    queryString := strings.Join(queryParts, "&")

    // Step 2: Concatenate timestamp
    payload := fmt.Sprintf("%s&time=%d", queryString, timestamp)

    // Step 3: Calculate HMAC-SHA256
    h := hmac.New(sha256.New, []byte(secretKey))
    h.Write([]byte(payload))

    return hex.EncodeToString(h.Sum(nil))
}
```
{% endtab %}

{% tab title="PHP" %}
```php
function generateSignature(array $params, int $timestamp, string $secretKey): string {
    // Step 1: Sort by key
    ksort($params);
    $queryString = http_build_query($params);

    // Step 2: Concatenate timestamp
    $payload = $queryString . "&time=" . $timestamp;

    // Step 3: Calculate HMAC-SHA256
    $signature = hash_hmac('sha256', $payload, $secretKey);

    return $signature;
}
```
{% endtab %}
{% endtabs %}

## Signature Verification

{% hint style="warning" %}
**Important**: Signature verification failure is usually caused by:
- Incorrect parameter concatenation order (not sorted by ASCII)
- Incorrect timestamp format (should be milliseconds)
- Incorrect Secret Key
- Missing or extra parameters
{% endhint %}

## Timestamp Requirements

| Requirement | Description |
|------|------|
| Format | Unix millisecond timestamp |
| Validity | Default 30 seconds (can be modified via Rec_Window) |
| Timezone | UTC recommended |

**Example**: `1742723373000` represents `2025-03-23 10:29:33 UTC`
