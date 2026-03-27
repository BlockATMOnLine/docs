# Java 后端集成示例

本示例展示如何在 Java Spring Boot 后端集成 BlockATM API。

## 项目结构

```
src/main/java/com/example/blockatm/
├── config/
│   └── BlockATMConfig.java      # 配置类
├── controller/
│   └── BlockATMController.java  # 控制器
├── service/
│   ├── BlockATMService.java     # 服务接口
│   └── impl/
│       └── BlockATMServiceImpl.java  # 服务实现
├── dto/
│   ├── CreateOrderRequest.java  # 请求 DTO
│   └── WebhookRequest.java      # Webhook DTO
├── util/
│   └── SignatureUtil.java      # 签名工具
└── BlockATMApplication.java
```

## 依赖配置

```xml
<!-- pom.xml -->
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-validation</artifactId>
    </dependency>
    <dependency>
        <groupId>com.squareup.okhttp3</groupId>
        <artifactId>okhttp</artifactId>
        <version>4.12.0</version>
    </dependency>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
    </dependency>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
    </dependency>
</dependencies>
```

## 配置类

```java
// config/BlockATMConfig.java
package com.example.blockatm.config;

import lombok.Data;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Data
@Configuration
@ConfigurationProperties(prefix = "blockatm")
public class BlockATMConfig {
    
    private String apiKey;
    private String secretKey;
    private String webhookKey;
    private String baseUrl = "https://open.blockatm.net";
    private String testUrl = "https://test-open.blockatm.net";
    
    private Boolean testMode = false;
    
    public String getActiveUrl() {
        return testMode ? testUrl : baseUrl;
    }
}
```

## 签名工具

```java
// util/SignatureUtil.java
package com.example.blockatm.util;

import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;
import java.security.InvalidKeyException;
import java.security.NoSuchAlgorithmException;
import java.util.Map;
import java.util.TreeMap;
import java.util.stream.Collectors;

public class SignatureUtil {

    private static final String ALGORITHM = "HmacSHA256";

    /**
     * 生成 BlockATM 签名
     * @param params 请求参数
     * @param timestamp 毫秒时间戳
     * @param secretKey Secret Key
     * @return HMAC-SHA256 签名
     */
    public static String generateSignature(Map<String, String> params, 
                                           long timestamp, 
                                           String secretKey) {
        // 1. 按 ASCII 排序参数 key（使用 TreeMap）
        TreeMap<String, String> sortedMap = new TreeMap<>(params);
        
        // 2. 拼接 key=value 格式字符串
        String paramString = sortedMap.entrySet().stream()
                .map(entry -> entry.getKey() + "=" + entry.getValue())
                .collect(Collectors.joining("&"));
        
        // 3. 拼接时间戳
        String payload = paramString + "&time=" + timestamp;
        
        // 4. 计算 HMAC-SHA256
        return hmacSha256(payload, secretKey);
    }

    /**
     * HMAC-SHA256 计算
     */
    public static String hmacSha256(String data, String key) {
        try {
            Mac mac = Mac.getInstance(ALGORITHM);
            SecretKeySpec secretKeySpec = new SecretKeySpec(
                    key.getBytes(StandardCharsets.UTF_8), 
                    ALGORITHM
            );
            mac.init(secretKeySpec);
            byte[] hash = mac.doFinal(data.getBytes(StandardCharsets.UTF_8));
            
            // 转换为十六进制字符串
            StringBuilder signature = new StringBuilder();
            for (byte b : hash) {
                signature.append(String.format("%02x", b));
            }
            return signature.toString();
        } catch (NoSuchAlgorithmException | InvalidKeyException e) {
            throw new RuntimeException("签名计算失败", e);
        }
    }

    /**
     * 验证 Webhook 签名
     */
    public static boolean verifyWebhookSignature(String payload, 
                                                  String signature, 
                                                  String webhookKey) {
        String expected = hmacSha256(payload, webhookKey);
        return expected.equalsIgnoreCase(signature);
    }
}
```

## DTO 类

```java
// dto/CreateOrderRequest.java
package com.example.blockatm.dto;

import lombok.Data;
import javax.validation.constraints.NotBlank;
import javax.validation.constraints.NotNull;
import java.math.BigDecimal;

@Data
public class CreateOrderRequest {
    
    @NotBlank(message = "收银台 ID 不能为空")
    private String cashierId;
    
    @NotBlank(message = "订单号不能为空")
    private String orderNo;
    
    @NotNull(message = "金额不能为空")
    private BigDecimal amount;
    
    @NotBlank(message = "代币符号不能为空")
    private String symbol;
    
    @NotBlank(message = "链 ID 不能为空")
    private String chainId;
    
    private String custNo;  // 客户编号（可选）
}
```

```java
// dto/WebhookRequest.java
package com.example.blockatm.dto;

import lombok.Data;
import java.math.BigDecimal;

@Data
public class WebhookRequest {
    private String eventType;
    private String orderNo;
    private String txHash;
    private BigDecimal amount;
    private String symbol;
    private String status;
    private Long blockTime;
    private String fromAddress;
    private String id;
}
```

## 服务接口

```java
// service/BlockATMService.java
package com.example.blockatm.service;

import com.example.blockatm.dto.CreateOrderRequest;
import java.util.Map;

public interface BlockATMService {
    
    /**
     * 创建收币订单
     */
    Map<String, Object> createPaymentOrder(CreateOrderRequest request);
    
    /**
     * 查询收币订单
     */
    Map<String, Object> getPaymentOrder(String orderNo);
    
    /**
     * 创建付币订单
     */
    Map<String, Object> createPayoutOrder(Map<String, String> params);
    
    /**
     * 查询付币订单
     */
    Map<String, Object> getPayoutOrder(String orderNo);
    
    /**
     * 获取收银台配置
     */
    Map<String, Object> getCashierInfo(String cashierId);
}
```

## 服务实现

```java
// service/impl/BlockATMServiceImpl.java
package com.example.blockatm.service.impl;

import com.example.blockatm.config.BlockATMConfig;
import com.example.blockatm.dto.CreateOrderRequest;
import com.example.blockatm.service.BlockATMService;
import com.example.blockatm.util.SignatureUtil;
import com.fasterxml.jackson.databind.ObjectMapper;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import okhttp3.*;
import org.springframework.stereotype.Service;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.TreeMap;
import java.util.concurrent.TimeUnit;

@Slf4j
@Service
@RequiredArgsConstructor
public class BlockATMServiceImpl implements BlockATMService {

    private final BlockATMConfig config;
    private final ObjectMapper objectMapper = new ObjectMapper();
    
    private final MediaType JSON = MediaType.parse("application/json; charset=utf-8");
    private OkHttpClient client = new OkHttpClient.Builder()
            .connectTimeout(30, TimeUnit.SECONDS)
            .readTimeout(30, TimeUnit.SECONDS)
            .build();

    @Override
    public Map<String, Object> createPaymentOrder(CreateOrderRequest request) {
        Map<String, String> params = new HashMap<>();
        params.put("cashierId", request.getCashierId());
        params.put("orderNo", request.getOrderNo());
        params.put("amount", request.getAmount().toString());
        params.put("symbol", request.getSymbol());
        params.put("chainId", request.getChainId());
        if (request.getCustNo() != null) {
            params.put("custNo", request.getCustNo());
        }
        
        return post("/order/api/v2/pay/order", params);
    }

    @Override
    public Map<String, Object> getPaymentOrder(String orderNo) {
        Map<String, String> params = new HashMap<>();
        params.put("orderNo", orderNo);
        return get("/order/api/v2/payorder/detail", params);
    }

    @Override
    public Map<String, Object> createPayoutOrder(Map<String, String> params) {
        return post("/order/api/v2/payout/order", params);
    }

    @Override
    public Map<String, Object> getPayoutOrder(String orderNo) {
        Map<String, String> params = new HashMap<>();
        params.put("orderNo", orderNo);
        return get("/order/api/v2/payout/detail", params);
    }

    @Override
    public Map<String, Object> getCashierInfo(String cashierId) {
        Map<String, String> params = new HashMap<>();
        params.put("cashierId", cashierId);
        return get("/admin/api/v2/pub/cashier/info", params);
    }

    private Map<String, Object> get(String path, Map<String, String> params) {
        try {
            // 构建查询字符串
            TreeMap<String, String> sortedParams = new TreeMap<>(params);
            String queryString = sortedParams.entrySet().stream()
                    .map(e -> e.getKey() + "=" + e.getValue())
                    .collect(Collectors.joining("&"));
            
            String url = config.getActiveUrl() + path + "?" + queryString;
            long timestamp = System.currentTimeMillis();
            String signature = SignatureUtil.generateSignature(params, timestamp, config.getSecretKey());
            
            Request request = new Request.Builder()
                    .url(url)
                    .get()
                    .addHeader("BlockATM-API-Key", config.getApiKey())
                    .addHeader("BlockATM-Request-Time", String.valueOf(timestamp))
                    .addHeader("BlockATM-Signature-V2", signature)
                    .build();
            
            return executeRequest(request);
        } catch (Exception e) {
            log.error("GET 请求失败: {}", path, e);
            throw new RuntimeException("API 请求失败", e);
        }
    }

    private Map<String, Object> post(String path, Map<String, String> params) {
        try {
            long timestamp = System.currentTimeMillis();
            String signature = SignatureUtil.generateSignature(params, timestamp, config.getSecretKey());
            
            String jsonBody = objectMapper.writeValueAsString(params);
            
            Request request = new Request.Builder()
                    .url(config.getActiveUrl() + path)
                    .post(RequestBody.create(jsonBody, JSON))
                    .addHeader("Content-Type", "application/json")
                    .addHeader("BlockATM-API-Key", config.getApiKey())
                    .addHeader("BlockATM-Request-Time", String.valueOf(timestamp))
                    .addHeader("BlockATM-Signature-V2", signature)
                    .build();
            
            return executeRequest(request);
        } catch (Exception e) {
            log.error("POST 请求失败: {}", path, e);
            throw new RuntimeException("API 请求失败", e);
        }
    }

    private Map<String, Object> executeRequest(Request request) throws IOException {
        try (Response response = client.newCall(request).execute()) {
            String responseBody = response.body().string();
            
            if (!response.isSuccessful()) {
                log.error("请求失败: {} - {}", response.code(), responseBody);
                throw new RuntimeException("API 请求失败: " + response.code());
            }
            
            return objectMapper.readValue(responseBody, Map.class);
        }
    }
}
```

## 控制器

```java
// controller/BlockATMController.java
package com.example.blockatm.controller;

import com.example.blockatm.dto.CreateOrderRequest;
import com.example.blockatm.service.BlockATMService;
import com.example.blockatm.util.SignatureUtil;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;

import java.util.Map;

@Slf4j
@RestController
@RequestMapping("/api/blockatm")
@RequiredArgsConstructor
public class BlockATMController {

    private final BlockATMService blockATMService;

    /**
     * 创建收币订单
     */
    @PostMapping("/create-order")
    public Map<String, Object> createOrder(@Validated @RequestBody CreateOrderRequest request) {
        log.info("创建订单: {}", request);
        return blockATMService.createPaymentOrder(request);
    }

    /**
     * 查询收币订单
     */
    @GetMapping("/order/{orderNo}")
    public Map<String, Object> getOrder(@PathVariable String orderNo) {
        log.info("查询订单: {}", orderNo);
        return blockATMService.getPaymentOrder(orderNo);
    }

    /**
     * 创建付币订单
     */
    @PostMapping("/create-payout")
    public Map<String, Object> createPayout(@RequestBody Map<String, String> params) {
        log.info("创建付币订单: {}", params);
        return blockATMService.createPayoutOrder(params);
    }

    /**
     * 查询付币订单
     */
    @GetMapping("/payout/{orderNo}")
    public Map<String, Object> getPayoutOrder(@PathVariable String orderNo) {
        log.info("查询付币订单: {}", orderNo);
        return blockATMService.getPayoutOrder(orderNo);
    }

    /**
     * Webhook 回调
     */
    @PostMapping("/webhook")
    public String handleWebhook(@RequestBody String payload,
                                 @RequestHeader("BlockATM-Signature") String signature) {
        log.info("收到 Webhook: {}", payload);
        
        // 验证签名
        if (!SignatureUtil.verifyWebhookSignature(payload, signature, 
                blockATMService.getConfig().getWebhookKey())) {
            log.error("Webhook 签名验证失败");
            return "{\"error\": \"Invalid signature\"}";
        }
        
        try {
            // 处理通知（根据 eventType）
            // payment.success / payment.failed / payout.success / payout.failed
            // TODO: 更新订单状态
            
            return "{\"message\": \"OK\"}";
        } catch (Exception e) {
            log.error("处理 Webhook 失败", e);
            return "{\"error\": \"Internal error\"}";
        }
    }

    /**
     * 获取收银台配置
     */
    @GetMapping("/cashier/{cashierId}")
    public Map<String, Object> getCashierInfo(@PathVariable String cashierId) {
        log.info("获取收银台信息: {}", cashierId);
        return blockATMService.getCashierInfo(cashierId);
    }
}
```

## 配置示例

```yaml
# application.yml
blockatm:
  api-key: ${BLOCKATM_API_KEY:your_api_key}
  secret-key: ${BLOCKATM_SECRET_KEY:your_secret_key}
  webhook-key: ${BLOCKATM_WEBHOOK_KEY:your_webhook_key}
  base-url: https://open.blockatm.net
  test-url: https://test-open.blockatm.net
  test-mode: true  # 开发环境设为 true

server:
  port: 8080
```

## 单元测试

```java
// test/BlockATMServiceTest.java
package com.example.blockatm;

import com.example.blockatm.util.SignatureUtil;
import org.junit.jupiter.api.Test;

import java.util.HashMap;
import java.util.Map;

import static org.junit.jupiter.api.Assertions.*;

class SignatureUtilTest {

    @Test
    void testGenerateSignature() {
        Map<String, String> params = new HashMap<>();
        params.put("custNo", "86000123");
        params.put("orderNo", "202504001399");
        params.put("lang", "zh-CN");
        
        long timestamp = 1742723373000L;
        String secretKey = "your_secret_key";
        
        String signature = SignatureUtil.generateSignature(params, timestamp, secretKey);
        
        assertNotNull(signature);
        assertEquals(64, signature.length()); // SHA256 hex = 64 chars
    }

    @Test
    void testVerifyWebhookSignature() {
        String payload = "{\"orderNo\":\"123\",\"amount\":\"100\"}";
        String webhookKey = "your_webhook_key";
        
        String expectedSignature = SignatureUtil.hmacSha256(payload, webhookKey);
        
        assertTrue(SignatureUtil.verifyWebhookSignature(payload, expectedSignature, webhookKey));
        assertFalse(SignatureUtil.verifyWebhookSignature(payload, "wrong_signature", webhookKey));
    }

    @Test
    void testSignatureConsistency() {
        // 相同的输入应该产生相同的签名
        Map<String, String> params = new HashMap<>();
        params.put("orderNo", "TEST123");
        params.put("amount", "100");
        
        long timestamp = 1742723373000L;
        String secretKey = "test_secret";
        
        String sig1 = SignatureUtil.generateSignature(params, timestamp, secretKey);
        String sig2 = SignatureUtil.generateSignature(params, timestamp, secretKey);
        
        assertEquals(sig1, sig2);
    }
}
```

## 错误处理

```java
// exception/BlockATMException.java
package com.example.blockatm.exception;

public class BlockATMException extends RuntimeException {
    
    private final String errorCode;
    
    public BlockATMException(String errorCode, String message) {
        super(message);
        this.errorCode = errorCode;
    }
    
    public String getErrorCode() {
        return errorCode;
    }
}

// 全局异常处理
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(BlockATMException.class)
    public Map<String, Object> handleBlockATMException(BlockATMException e) {
        log.error("BlockATM 异常: {} - {}", e.getErrorCode(), e.getMessage());
        return Map.of(
            "code", e.getErrorCode(),
            "message", e.getMessage()
        );
    }
    
    @ExceptionHandler(Exception.class)
    public Map<String, Object> handleGeneralException(Exception e) {
        log.error("系统异常", e);
        return Map.of(
            "code", "ERROR_000500",
            "message", "系统错误"
        );
    }
}
```

---

## 下一步

- [API 文档](../open-api/README.md)
- [Webhook 配置](../webhooks/README.md)
- [签名认证](../open-api/authentication.md)
