# 签名认证

BlockATM API 使用 HMAC-SHA256 进行请求签名，确保请求的真实性和完整性。

## 认证参数

| Header | 说明 | 必填 |
|--------|------|------|
| BlockATM-api-Key | API 密钥 | 是 |
| BlockATM-Request-Time | 请求时间戳（毫秒） | 签名接口必填 |
| BlockATM-Signature-V2 | HMAC-SHA256 签名 | 签名接口必填 |
| BlockATM-Rec_Window | 请求有效时间窗口（毫秒） | 否（默认 30000） |

{% hint style="info" %}
**获取密钥**：在 BlockATM 管理后台「收银台」→「集成」中获取 API Key 和 Secret Key。
{% endhint %}

## 签名算法

### 步骤 1：拼接参数字符串

将请求参数（JSON body）按 key 的 ASCII 升序排列，拼接为 `key=value` 格式，用 `&` 分隔。

**示例原始请求：**
```json
{
  "custNo": "86000123",
  "orderNo": "202504001399",
  "lang": "zh-CN"
}
```

**处理后：**
```
custNo=86000123&lang=zh-CN&orderNo=202504001399
```

### 步骤 2：拼接时间戳

在参数字符串末尾拼接时间戳：

```
custNo=86000123&lang=zh-CN&orderNo=202504001399&time=1742723373000
```

### 步骤 3：计算 HMAC-SHA256

使用 Secret Key 对拼接后的字符串进行 HMAC-SHA256 签名：

```
HMAC-SHA256(secretKey, payload)
```

## 多语言示例

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
        
        // 步骤1：按 ASCII 排序参数
        Map<String, String> sortedMap = new TreeMap<>(params);
        List<String> paramList = new ArrayList<>();
        for (Map.Entry<String, String> entry : sortedMap.entrySet()) {
            paramList.add(entry.getKey() + "=" + entry.getValue());
        }
        String sortedParams = String.join("&", paramList);
        
        // 步骤2：拼接时间戳
        String payload = sortedParams + "&time=" + timestamp;
        
        // 步骤3：计算 HMAC-SHA256
        Mac sha256Hmac = Mac.getInstance("HmacSHA256");
        SecretKeySpec secretKeySpec = new SecretKeySpec(
            secretKey.getBytes(StandardCharsets.UTF_8), 
            "HmacSHA256"
        );
        sha256Hmac.init(secretKeySpec);
        byte[] hash = sha256Hmac.doFinal(payload.getBytes(StandardCharsets.UTF_8));
        
        // 转换为十六进制字符串
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
    # 步骤1：按 key 排序
    sorted_params = sorted(params.items())
    query_string = urlencode(sorted_params)
    
    # 步骤2：拼接时间戳
    payload = f"{query_string}&time={timestamp}"
    
    # 步骤3：计算 HMAC-SHA256
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
    // 步骤1：按 key 排序
    keys := make([]string, 0, len(params))
    for k := range params {
        keys = append(keys, k)
    }
    sort.Strings(keys)
    
    // 拼接
    var queryParts []string
    for _, k := range keys {
        queryParts = append(queryParts, fmt.Sprintf("%s=%s", k, params[k]))
    }
    queryString := strings.Join(queryParts, "&")
    
    // 步骤2：拼接时间戳
    payload := fmt.Sprintf("%s&time=%d", queryString, timestamp)
    
    // 步骤3：计算 HMAC-SHA256
    h := hmac.New(sha256.New, []byte(secretKey))
    h.Write([]byte(payload))
    
    return hex.EncodeToString(h.Sum(nil))
}
```
{% endtab %}

{% tab title="PHP" %}
```php
function generateSignature(array $params, int $timestamp, string $secretKey): string {
    // 步骤1：按 key 排序
    ksort($params);
    $queryString = http_build_query($params);
    
    // 步骤2：拼接时间戳
    $payload = $queryString . "&time=" . $timestamp;
    
    // 步骤3：计算 HMAC-SHA256
    $signature = hash_hmac('sha256', $payload, $secretKey);
    
    return $signature;
}
```
{% endtab %}
{% endtabs %}

## 验证签名

{% hint style="warning" %}
**重要**：签名验证失败通常由以下原因导致：
- 参数拼接顺序不正确（未按 ASCII 排序）
- 时间戳格式错误（应为毫秒）
- Secret Key 不正确
- 参数遗漏或多余
{% endhint %}

## 时间戳要求

| 要求 | 说明 |
|------|------|
| 格式 | Unix 毫秒时间戳 |
| 有效期 | 默认 30 秒（可通过 Rec_Window 修改） |
| 时区 | 建议使用 UTC |

**示例**：`1742723373000` 表示 `2025-03-23 10:29:33 UTC`
