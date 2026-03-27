---
hidden: true
---

# Widget URL 签名

Widget URL 签名用于限制未授权第三方的访问，通过提供有限的权限和时间来发起请求。

## 何时使用 URL 签名

当您使用 URL 方式集成收银台时（而非 SDK 方式），需要对 URL 进行签名。

## 签名步骤

### 1. 生成签名

将 URL 参数发送到您的后端服务器，使用 Secret Key 生成 HMAC-SHA256 签名：

1. 使用 SHA-256 哈希函数创建 HMAC
2. 使用您的 Secret API Key 作为密钥
3. 使用原始 URL 的查询字符串作为消息

### 2. URL 编码

确保所有查询参数值在创建签名之前进行 **URL 编码**。

### 3. 签名算法

```
HMAC-SHA256(secretKey, URL-encoded query string)
```

## 多语言示例

{% tabs %}
{% tab title="JavaScript" %}
```javascript
import crypto from 'crypto';
import { URL } from 'url';

// 配置
const originalUrl = 'https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225';
const secretKey = 'sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0';

// 处理 URL 和参数
const urlObj = new URL(originalUrl);
const params = urlObj.searchParams;

// URL 编码所有参数值
params.forEach((v, k) => params.set(k, encodeURIComponent(v)));

// 使用 HMAC-SHA256 生成签名
const signature = crypto.createHmac('sha256', secretKey)
    .update(params.toString())
    .digest('hex');

// 将签名追加到 URL
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
        // 配置
        String originalUrl = "https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225";
        String secretKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";

        // 处理 URL 和参数
        String query = new URI(originalUrl).getQuery();
        Map<String, String> params = new LinkedHashMap<>();

        // URL 编码参数
        for (String param : query.split("&")) {
            String[] kv = param.split("=", 2);
            params.put(kv[0], kv.length > 1 ? URLEncoder.encode(kv[1], StandardCharsets.UTF_8.name()) : "");
        }

        // 构建查询字符串
        String queryString = String.join("&", params.entrySet().stream()
                .map(e -> e.getKey() + "=" + e.getValue())
                .toArray(String[]::new));

        // 生成并追加签名
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
    # 配置
    original_url = "https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225"
    secret_key = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0"

    # 解析 URL 和参数
    parsed = urlparse(original_url)
    params = parse_qs(parsed.query, keep_blank_values=True)

    # URL 编码参数值
    encoded_params = {k: quote(v[0], safe='') for k, v in params.items()}

    # 生成查询字符串
    query_string = urlencode(encoded_params)

    # 计算 HMAC-SHA256 签名
    signature = hmac.new(
        secret_key.encode('utf-8'),
        query_string.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

    # 构建签名 URL
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
    // 配置
    $originalUrl = "https://test-pay.blockatm.com?apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225";
    $secretKey = "sck_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0";

    // 解析 URL 和参数
    $parsedUrl = parse_url($originalUrl);
    parse_str($parsedUrl['query'], $params);

    // URL 编码参数值
    $encodedParams = array_map('urlencode', $params);

    // 生成查询字符串
    $queryString = http_build_query($encodedParams);

    // 计算 HMAC-SHA256 签名
    $signature = hash_hmac('sha256', $queryString, $secretKey);

    // 构建签名 URL
    $signedUrl = $originalUrl . '&signature=' . $signature;
    echo "Signed URL:\n" . $signedUrl . "\n";
}

signUrl();
?>
```
{% endtab %}
{% endtabs %}

## 签名示例

**Secret Key:**
```
sk_ci_QOoPSlHDSsgXYeNyTP2i0ug1HKLRjHw9Ug7mCc1Q0
```

**URL 参数:**
```
apiKey=pk_payment_my3T68cbuIXf1x3QOEbWtFEfcJPxeBr8wTewDVM&custNo=C86002201&orderNo=C202503225
```

**签名结果:**
```
5b2419abcb925389c3f6cb42f35eed85ec36b95578a9d25ee500f9fafdeb08dc
```

## 集成步骤

1. 将 Widget URL 参数发送到您的后端服务器
2. 使用 BlockATM 管理后台的 Secret Key 生成签名
3. 返回签名或完整的签名 URL
4. 使用 SDK 或 URL 方式展示收银台
