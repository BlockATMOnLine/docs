# 签名验证

BlockATM Webhook 使用 HMAC-SHA256 签名验证请求的真实性。

## 验证原理

```
HMAC-SHA256(secretKey, payload)
```

## 请求头

| Header | 说明 |
|--------|------|
| BlockATM-Signature-V2 | HMAC-SHA256 签名 |
| BlockATM-Request-Time | 请求时间戳（毫秒） |
| BlockATM-Event | 事件类型 |

## 验证步骤

### 步骤 1：提取参数

从请求头中提取：
- `BlockATM-Signature-V2`：签名
- `BlockATM-Request-Time`：时间戳
- 请求体：payload

### 步骤 2：计算签名

使用相同的算法计算签名：

```python
# Python 示例
import hmac
import hashlib

def verify_signature(payload, timestamp, signature, secret_key):
    # 拼接待签名字符串
    message = payload + "&time=" + timestamp
    
    # 计算签名
    expected_signature = hmac.new(
        secret_key.encode('utf-8'),
        message.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()
    
    # 比较签名
    return hmac.compare_digest(expected_signature, signature)
```

### 步骤 3：时间戳验证

验证请求是否在有效时间窗口内：

```python
def verify_timestamp(timestamp, max_window=300000):  # 5分钟
    current_time = int(time.time() * 1000)
    return abs(current_time - int(timestamp)) < max_window
```

## 完整验证示例

{% tabs %}
{% tab title="Python" %}
```python
import hmac
import hashlib
import time

WEBHOOK_SECRET = "your_webhook_secret"

def verify_webhook(request):
    # 获取请求头
    signature = request.headers.get('BlockATM-Signature-V2')
    timestamp = request.headers.get('BlockATM-Request-Time')
    event = request.headers.get('BlockATM-Event')
    
    # 获取请求体
    payload = request.get_data(as_text=True)
    
    # 验证时间戳
    current_time = int(time.time() * 1000)
    if abs(current_time - int(timestamp)) > 300000:
        return False, "Timestamp expired"
    
    # 计算签名
    message = payload + "&time=" + timestamp
    expected_sig = hmac.new(
        WEBHOOK_SECRET.encode('utf-8'),
        message.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()
    
    # 比较签名
    if not hmac.compare_digest(expected_sig, signature):
        return False, "Invalid signature"
    
    return True, "OK"

# 处理 Webhook
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    valid, msg = verify_webhook(request)
    if not valid:
        return msg, 401
    
    data = request.json
    event = data.get('event')
    
    if event == 'payment':
        # 处理收币事件
        pass
    elif event == 'payout':
        # 处理付币事件
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
            // 验证时间戳
            long currentTime = System.currentTimeMillis();
            if (Math.abs(currentTime - Long.parseLong(timestamp)) > 300000) {
                return false;
            }
            
            // 计算签名
            String message = payload + "&time=" + timestamp;
            Mac sha256Hmac = Mac.getInstance("HmacSHA256");
            SecretKeySpec secretKeySpec = new SecretKeySpec(
                SECRET.getBytes(StandardCharsets.UTF_8), "HmacSHA256"
            );
            sha256Hmac.init(secretKeySpec);
            byte[] hash = sha256Hmac.doFinal(message.getBytes(StandardCharsets.UTF_8));
            
            // 转换为十六进制
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
    // 验证时间戳
    currentTime := time.Now().UnixMilli()
    ts, _ := time.ParseDuration("300000ms")
    if currentTime-int64(timestamp) > int64(ts.Milliseconds()) {
        return false, "Timestamp expired"
    }
    
    // 计算签名
    message := payload + "&time=" + timestamp
    h := hmac.New(sha256.New, []byte(SECRET))
    h.Write([]byte(message))
    expectedSig := hex.EncodeToString(h.Sum(nil))
    
    // 比较签名
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
    // 验证时间戳
    if (!verifyTimestamp(timestamp)) {
        return false;
    }
    
    // 计算签名
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

## 安全建议

{% hint style="warning" %}
### 为什么必须验证签名？

Webhook 签名验证是保障您系统安全的关键环节。如果不验证签名，攻击者可以伪装成 BlockATM 向您的系统发送伪造的支付通知，可能导致：

- **虚假发货**：基于伪造的"支付成功"通知向用户发货，但实际未收到款项
- **资金损失**：基于伪造的"付币成功"通知确认交易，但实际区块链上并无此交易
- **业务混乱**：伪造的订单状态更新可能导致您的业务逻辑出现异常

因此，**必须始终验证签名**，切勿跳过此安全检查。
{% endhint %}

### 1. 始终验证签名

{% hint style="danger" %}
**警告**：切勿跳过签名验证步骤。即使在开发测试环境中，也建议保持签名验证逻辑的正确实现。
{% endhint %}

**验证流程**：

1. 从请求头提取 `BlockATM-Signature-V2` 和 `BlockATM-Request-Time`
2. 获取原始请求体（raw body），**不要**解析后再获取，因为 JSON 解析可能改变内容
3. 拼接签名字符串：`原始payload + "&time=" + 时间戳`
4. 使用相同的 HMAC-SHA256 算法计算签名
5. 使用恒定时间比较（`hmac.compare_digest`）防止时序攻击

**常见错误**：

```python
# ❌ 错误：直接比较字符串可能遭受时序攻击
if expected_signature == received_signature:

# ✅ 正确：使用恒定时间比较
if hmac.compare_digest(expected_signature, received_signature):
```

### 2. 验证时间戳

时间戳验证用于防止**重放攻击（Replay Attack）**。攻击者可能截获合法的 Webhook 通知并重新发送，试图重复触发您的业务逻辑。

**时间窗口建议**：

- 推荐时间窗口：**5 分钟**（300000 毫秒）
- 如果 BlockATM 的服务器时间与您的服务器时间存在偏差，可以适当放宽，但不建议超过 15 分钟

**验证逻辑**：

```python
def verify_timestamp(timestamp, max_window_ms=300000):
    current_time_ms = int(time.time() * 1000)
    try:
        event_time_ms = int(timestamp)
    except (ValueError, TypeError):
        return False  # 无效时间戳格式
    return abs(current_time_ms - event_time_ms) < max_window_ms
```

### 3. 使用 HTTPS

{% hint style="danger" %}
**强烈建议**：生产环境中必须使用 HTTPS协议的 Webhook URL。使用 HTTP 明文传输可能导致请求被截获、篡改或重放。
{% endhint %}

**证书要求**：

- 推荐使用受信任 CA 颁发的 SSL/TLS 证书
- 避免使用自签名证书
- 定期检查证书有效期，及时续期

### 4. 幂等处理

同一 Webhook 事件可能会收到多次，原因是：

- BlockATM 的重试机制（见下方说明）
- 您的服务器响应超时但实际处理成功
- 网络抖动导致的重复请求

**实现幂等的建议**：

```python
def handle_payment_event(event):
    order_id = event.get('orderNo')
    
    # 检查订单是否已处理（使用 Redis 或数据库）
    if order_processed(order_id):
        return  # 跳过已处理的订单
    
    # 执行业务逻辑
    process_payment(event)
    
    # 标记订单为已处理
    mark_order_processed(order_id)
```

{% hint style="info" %}
**BlockATM 重试机制**：如果 Webhook 投递失败（您的服务器未返回 HTTP 200），BlockATM 会按以下策略重试：
- 第 1 次重试：1 分钟后
- 第 2 次重试：5 分钟后
- 第 3 次重试：30 分钟后
- 第 4 次重试：2 小时后
- 第 5 次重试：24 小时后

如果 5 次重试均失败，将停止投递。
{% endhint %}

### 5. 记录日志

完善的日志记录对于问题排查和安全审计至关重要。

**建议记录的日志**：

| 日志类型 | 记录内容 | 目的 |
|---------|---------|------|
| 接收日志 | 收到请求的时间、事件类型、订单号 | 审计追踪 |
| 验证日志 | 签名验证结果、时间戳验证结果 | 问题排查 |
| 处理日志 | 业务处理开始/成功/失败 | 业务审计 |
| 错误日志 | 异常信息、堆栈跟踪 | Bug 定位 |

**日志示例**：

```python
import logging

logger = logging.getLogger('webhook')

def handle_webhook(request):
    logger.info(f"Received webhook: event={event_type}, order={order_id}")
    
    # 验证
    if not verify_signature(request):
        logger.warning(f"Signature verification failed: order={order_id}")
        return "Invalid signature", 401
    
    # 处理
    try:
        process_event(event)
        logger.info(f"Successfully processed: order={order_id}")
    except Exception as e:
        logger.error(f"Processing failed: order={order_id}, error={e}")
        raise
    
    return "OK", 200
```

### 6. 快速响应

{% hint style="info" %}
**重要**：您的 Webhook 处理接口应该在**接收到请求后立即返回 HTTP 200**，然后再异步处理业务逻辑。这可以避免因处理耗时过长导致的请求超时和 BlockATM 重试。
{% endhint %}

**推荐架构**：

```python
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    # 1. 立即验证并返回
    if not verify_webhook(request):
        return "Invalid", 401
    
    # 2. 获取数据
    data = request.json
    
    # 3. 立即返回 200
    return "OK", 200
    
    # 4. 异步处理（使用消息队列）
    # 注意：此处不会阻塞请求
    send_to_queue(data)
```

### 7. 错误处理

{% hint style="danger" %}
**重要**：如果您的 Webhook 处理失败（返回非 200 状态码），BlockATM 会认为投递失败并按照重试策略重新发送。
{% endhint %}

**正确的错误处理**：

```python
@app.route('/webhook', methods=['POST'])
def handle_webhook():
    try:
        verify_and_process(request)
        return "OK", 200
    except ValidationError as e:
        logger.warning(f"Validation error: {e}")
        return "Bad Request", 400  # 这会导致重试
    except BusinessError as e:
        logger.error(f"Business error: {e}")
        return "Server Error", 500  # 这会导致重试
    except Exception as e:
        logger.exception(f"Unexpected error: {e}")
        return "Server Error", 500  # 这会导致重试
```

{% hint style="info" %}
**提示**：如果您的业务逻辑确实无法处理某个事件（如订单已撤销），可以返回 200 而不是错误码。这样 BlockATM 就不会重试，避免无限循环。
{% endhint %}
