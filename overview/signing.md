---
description: Protecting your sensitive data
---

# Signing

## Checking a Webhook Signature

BlockATM signs the webhook events and requests we send to your endpoints. We do so by including a signature in each event’s BlockATM-Signature-V2 header.\
This allows you to validate that the events and requests were sent by BlockATM , not by a third party.

Before you can verify BlockATM-Signature-V2 signatures for webhook events, you need to retrieve your webhook API key from the Developers page on the BlockATM dashboard.

### Step 1

Concatenate all parameters from the JSON object in ascending order of their keys according to the ASCII character order,\
using the format "key=value" separated by "&"。\
Concatenate the 'BlockATM-Request-Time' from the request header in the format '\&time=' at the end.

Here is an example of the signature parameters and the resulting signature:

request Data:

```json
{
  "amount": 999,
  "cashierId": 91,
  "chainId": "11155111",
  "custNo": "cust00001",
  "fromAddress": "0xa9e358e33a57e67c9b84618a52f0194c345c8e35",
  "id": 8210003764,
  "network": "Ethereum",
  "status": 9,
  "symbol": "USDT",
  "txId": "0x1da59f33aa6f6b435514126e26d5622c3e377e4762579aa0ac0130139625853d"
}
```

Concatenate the sorted parameters and time result

```java

// you can get the time from request header BlockATM-Request-Time,example of time: 1696946592054
amount=13.410037&chainId=5&custNo=OrderNO_123456&fee=2&network=TRON&platOrderNo=8210000374&status=1&symbol=USDT&txId=1t&type=1&time=1696947336603
```

### Step 2

Compute a HMAC with the SHA-256 hash function. Use your account's webhook **Secret Key** as the key, and use the signed\_payload string as the message in both cases.

```javascript
// you can get the signature from request header BlockATM-Signature-V1
    MEYCIQDHxQ0IhgUNbRqTKbU71fBkp+lAJlMXEQYt6mDQfWRY7gIhAMWIpVoG6qBhgIPi30x30wLlAaxyhptZfm6nMRz75VxA
```

Compare the signature in the header to the expected signature.

## Demo

See language-specific code examples below for implementation.

{% tabs %}
{% tab title="java" %}
```java
package com.btm.api.service;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.util.Map;
import java.util.TreeMap;

public class SignatureGenerator {

    public static void main(String[] args) {
        // 1. Prepare test parameters
        Map<String, String> params = createTestParams();
        String requestTime = "1743060268000"; // From request header
        String apiKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"; // Replace with actual Secret key

        // 2. Build payload string
        String payload = buildSignPayload(params, requestTime);
        System.out.println("[Debug] Payload String:\n" + payload + "\n");

        // 3. Generate HEX signature
        String signature = calculateHmacSha256Hex(apiKey, payload);
        System.out.println("[Result] Final Signature:\n" + signature);
    }

    /**
     * Creates test parameters matching documentation example
     */
    private static Map<String, String> createTestParams() {
        Map<String, String> params = new TreeMap<>();
        params.put("amount", "999");
        params.put("cashierId", "91");
        params.put("chainId", "11155111");
        params.put("custNo", "cust00001");
        params.put("fromAddress", "0xa9e358e33a57e67c9b84618a52f0194c345c8e35");
        params.put("id", "8210003764");
        params.put("network", "Ethereum");
        params.put("status", "9");
        params.put("symbol", "USDT");
        params.put("txId", "0x1da59f33aa6f6b435514126e26d5622c3e377e4762579aa0ac0130139625853d");
        return params;
    }

    /**
     * Builds the payload string for signing
     * @param params Request parameters map
     * @param time Request timestamp from header
     * @return Sorted parameter string with timestamp appended
     */
    private static String buildSignPayload(Map<String, String> params, String time) {
        StringBuilder payload = new StringBuilder();

        // Sort parameters using TreeMap's natural ordering
        new TreeMap<>(params).forEach((key, value) -> {
            if (payload.length() > 0) payload.append("&");
            payload.append(key).append("=").append(value);
        });

        // Append timestamp parameter
        payload.append("&time=").append(time);

        return payload.toString();
    }

    /**
     * Calculates HMAC-SHA256 signature in HEX format
     * @param secret API key
     * @param message Payload string
     * @return Hexadecimal signature string
     */
    private static String calculateHmacSha256Hex(String secret, String message) {
        try {
            // Initialize HMAC-SHA256 algorithm
            Mac hmac = Mac.getInstance("HmacSHA256");
            SecretKeySpec keySpec = new SecretKeySpec(
                    secret.getBytes(StandardCharsets.UTF_8),
                    "HmacSHA256"
            );

            // Calculate signature
            hmac.init(keySpec);
            byte[] signatureBytes = hmac.doFinal(message.getBytes(StandardCharsets.UTF_8));

            // Convert to HEX string
            return bytesToHex(signatureBytes);
        } catch (Exception e) {
            throw new RuntimeException("HMAC calculation failed", e);
        }
    }

    /**
     * Converts byte array to lowercase HEX string
     * @param bytes Byte array to convert
     * @return Hexadecimal representation
     */
    private static String bytesToHex(byte[] bytes) {
        StringBuilder hexString = new StringBuilder();
        for (byte b : bytes) {
            String hex = Integer.toHexString(0xff & b);
            if (hex.length() == 1) hexString.append('0');
            hexString.append(hex);
        }
        return hexString.toString();
    }
}
```
{% endtab %}

{% tab title="python" %}
```
import hmac
import hashlib

def create_test_params():
    return {
        "amount": "999",
        "cashierId": "91",
        "chainId": "11155111",
        "custNo": "cust00001",
        "fromAddress": "0xa9e358e33a57e67c9b84618a52f0194c345c8e35",
        "id": "8210003764",
        "network": "Ethereum",
        "status": "9",
        "symbol": "USDT",
        "txId": "0x1da59f33aa6f6b435514126e26d5622c3e377e4762579aa0ac0130139625853d"
    }

def build_sign_payload(params, time):
    sorted_params = sorted(params.items())
    payload = "&".join([f"{k}={v}" for k, v in sorted_params])
    return f"{payload}&time={time}"

def calculate_hmac_sha256_hex(secret, message):
    return hmac.new(
        secret.encode('utf-8'),
        message.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

if __name__ == "__main__":
    params = create_test_params()
    request_time = "1743060268000"
    api_key = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"
    
    payload = build_sign_payload(params, request_time)
    print(f"[Debug] Payload String:\n{payload}\n")
    
    signature = calculate_hmac_sha256_hex(api_key, payload)
    print(f"[Result] Final Signature:\n{signature}")
```
{% endtab %}

{% tab title="c++" %}
```cpp
#include <iostream>
#include <map>
#include <string>
#include <algorithm>
#include <openssl/hmac.h>
#include <iomanip>
#include <sstream>

std::map<std::string, std::string> create_test_params() {
    return {
        {"amount", "999"},
        {"cashierId", "91"},
        {"chainId", "11155111"},
        {"custNo", "cust00001"},
        {"fromAddress", "0xa9e358e33a57e67c9b84618a52f0194c345c8e35"},
        {"id", "8210003764"},
        {"network", "Ethereum"},
        {"status", "9"},
        {"symbol", "USDT"},
        {"txId", "0x1da59f33aa6f6b435514126e26d5622c3e377e4762579aa0ac0130139625853d"}
    };
}

std::string build_sign_payload(std::map<std::string, std::string> params, std::string time) {
    std::string payload;
    for (const auto& [key, value] : params) {
        if (!payload.empty()) payload += "&";
        payload += key + "=" + value;
    }
    return payload + "&time=" + time;
}

std::string calculate_hmac_sha256_hex(const std::string& secret, const std::string& message) {
    unsigned char hash[EVP_MAX_MD_SIZE];
    unsigned int len = 0;
    
    HMAC_CTX* ctx = HMAC_CTX_new();
    HMAC_Init_ex(ctx, secret.c_str(), secret.length(), EVP_sha256(), NULL);
    HMAC_Update(ctx, (unsigned char*)message.c_str(), message.length());
    HMAC_Final(ctx, hash, &len);
    HMAC_CTX_free(ctx);

    std::stringstream ss;
    for(unsigned int i = 0; i < len; i++) {
        ss << std::hex << std::setw(2) << std::setfill('0') << (int)hash[i];
    }
    return ss.str();
}

int main() {
    auto params = create_test_params();
    std::string request_time = "1743060268000";
    std::string api_key = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";
    
    std::string payload = build_sign_payload(params, request_time);
    std::cout << "[Debug] Payload String:\n" << payload << "\n\n";
    
    std::string signature = calculate_hmac_sha256_hex(api_key, payload);
    std::cout << "[Result] Final Signature:\n" << signature << std::endl;
    
    return 0;
}
```
{% endtab %}

{% tab title="go" %}
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

func createTestParams() map[string]string {
	return map[string]string{
		"amount":      "999",
		"cashierId":   "91",
		"chainId":     "11155111",
		"custNo":      "cust00001",
		"fromAddress": "0xa9e358e33a57e67c9b84618a52f0194c345c8e35",
		"id":          "8210003764",
		"network":     "Ethereum",
		"status":      "9",
		"symbol":      "USDT",
		"txId":        "0x1da59f33aa6f6b435514126e26d5622c3e377e4762579aa0ac0130139625853d",
	}
}

func buildSignPayload(params map[string]string, time string) string {
	keys := make([]string, 0, len(params))
	for k := range params {
		keys = append(keys, k)
	}
	sort.Strings(keys)

	var parts []string
	for _, k := range keys {
		parts = append(parts, fmt.Sprintf("%s=%s", k, params[k]))
	}
	return strings.Join(parts, "&") + "&time=" + time
}

func calculateHmacSha256Hex(secret, message string) string {
	h := hmac.New(sha256.New, []byte(secret))
	h.Write([]byte(message))
	return hex.EncodeToString(h.Sum(nil))
}

func main() {
	params := createTestParams()
	requestTime := "1743060268000"
	apiKey := "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"

	payload := buildSignPayload(params, requestTime)
	fmt.Printf("[Debug] Payload String:\n%s\n\n", payload)

	signature := calculateHmacSha256Hex(apiKey, payload)
	fmt.Printf("[Result] Final Signature:\n%s\n", signature)
}
```
{% endtab %}
{% endtabs %}
