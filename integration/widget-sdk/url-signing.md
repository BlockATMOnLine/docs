---
hidden: true
---

# Widget URL Signing

Widget URL signing is used to restrict unauthorized third-party access by providing limited permissions and time to initiate requests.

## When to Use URL Signing

When you integrate cashier using URL method (instead of SDK method), you need to sign the URL.

## Signing Steps

### 1. Generate Signature

Send URL parameters to your backend server, use Secret Key to generate HMAC-SHA256 signature:

1. Use SHA-256 hash function to create HMAC
2. Use your Secret API Key as the key
3. Use the original URL query string as the message

### 2. URL Encoding

Ensure all query parameter values are **URL encoded** before creating the signature.

### 3. Signature Algorithm

```
HMAC-SHA256(secretKey, URL-encoded query string)
```

## Multi-language Examples

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import crypto from 'crypto';
import { URL } from 'url';

// Configuration
const originalUrl = 'https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225';
const secretKey = 'sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0';

// Handle URL and parameters
const urlObj = new URL(originalUrl);
const params = urlObj.searchParams;

// URL encode all parameter values
params.forEach((v, k) => params.set(k, encodeURIComponent(v)));

// Generate signature using HMAC-SHA256
const signature = crypto.createHmac('sha256', secretKey)
    .update(params.toString())
    .digest('hex');

// Append signature to URL
urlObj.searchParams.set('signature', signature);

console.log('Signed URL:\n' + urlObj.toString());
```
{% endtab %}

{% tab title="Java" %}
```java
package com.btm.api.service;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.net.URI;
import java.net.URLEncoder;
import java.nio.charset.StandardCharsets;
import java.util.*;

public class UrlSigner {

    public static void main(String[] args) throws Exception {
        // Configuration
        String originalUrl = "https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225";
        String secretKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";

        // Handle URL and parameters
        String query = new URI(originalUrl).getQuery();
        Map<String, String> params = new LinkedHashMap<>();

        // URL encode parameters
        for (String param : query.split("&")) {
            String[] kv = param.split("=", 2);
            params.put(kv[0], kv.length > 1 ? URLEncoder.encode(kv[1], StandardCharsets.UTF_8.name()) : "");
        }

        // Build query string
        String queryString = String.join("&", params.entrySet().stream()
                .map(e -> e.getKey() + "=" + e.getValue())
                .toArray(String[]::new));

        // Generate and append signature
        String signedUrl = originalUrl + "&signature=" + hmacSha256(secretKey, queryString);
        System.out.println("Signed URL:\n" + signedUrl);
    }

    private static String hmacSha256(String secret, String data) throws Exception {
        Mac mac = Mac.getInstance("HmacSHA256");
        mac.init(new SecretKeySpec(secret.getBytes(StandardCharsets.UTF_8), "HmacSHA256"));
        return bytesToHex(mac.doFinal(data.getBytes(StandardCharsets.UTF_8)));
    }

    private static String bytesToHex(byte[] bytes) {
        StringBuilder sb = new StringBuilder();
        for (byte b : bytes) {
            sb.append(String.format("%02x", b));
        }
        return sb.toString();
    }
}
```
{% endtab %}

{% tab title="Python" %}
```python
import hmac
import hashlib
from urllib.parse import urlparse, parse_qs, urlencode, quote

def sign_url():
    # Configuration
    original_url = "https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225"
    secret_key = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"

    # Parse URL and parameters
    parsed = urlparse(original_url)
    params = parse_qs(parsed.query, keep_blank_values=True)

    # URL encode parameter values
    encoded_params = {k: quote(v[0], safe='') for k, v in params.items()}

    # Generate query string
    query_string = urlencode(encoded_params)

    # Calculate HMAC-SHA256 signature
    signature = hmac.new(
        secret_key.encode('utf-8'),
        query_string.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    # Build signed URL
    signed_url = f"{original_url}&signature={signature}"
    print("Signed URL:")
    print(signed_url)

if __name__ == "__main__":
    sign_url()
```
{% endtab %}

{% tab title="PHP" %}
```php
<?php
function signUrl() {
    // Configuration
    $originalUrl = "https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225";
    $secretKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";

    // Parse URL and parameters
    $parsedUrl = parse_url($originalUrl);
    parse_str($parsedUrl['query'], $params);

    // URL encode parameter values
    $encodedParams = array_map('urlencode', $params);

    // Generate query string
    $queryString = http_build_query($encodedParams);

    // Calculate HMAC-SHA256 signature
    $signature = hash_hmac('sha256', $queryString, $secretKey);

    // Build signed URL
    $signedUrl = $originalUrl . '&signature=' . $signature;
    echo "Signed URL:\n" . $signedUrl . "\n";
}

signUrl();
?>
```
{% endtab %}
{% endtabs %}

## Signature Example

**Secret Key:**
```
sk_ci_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0
```

**URL Parameters:**
```
apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225
```

**Signature Result:**
```
5b2419abcb925389c3f6cb42f35eed85ec36b95578a9d25ee500f9fafdeb08dc
```

## Integration Steps

1. Send Widget URL parameters to your backend server
2. Use Secret Key from BlockATM admin dashboard to generate signature
3. Return signature or complete signed URL
4. Display cashier using SDK or URL method
