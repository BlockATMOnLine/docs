# 技术问题

关于 API 和集成的常见技术问题。

## API 调用频率限制是多少？

| 限流维度 | 限制值 |
|---------|--------|
| 每 API Key | 1000 请求/分钟 |
| 每 IP | 2000 请求/分钟 |
| 单个接口 | 100 请求/分钟 |

[查看限流详情 →](../integration/open-api/rate-limit.md)

## Webhook 收不到怎么办？

{% stepper %}
{% step %}
## 检查配置

确认 Webhook URL 正确配置且可公网访问。
{% endstep %}

{% step %}
## 检查返回

确保您的服务器返回 HTTP 200。
{% endstep %}

{% step %}
## 查看日志

检查服务器日志，确认请求是否到达。
{% endstep %}

{% step %}
## 验证签名

确认 Webhook 签名验证通过。
{% endstep %}

{% step %}
## 联系支持

如仍有问题，联系技术支持排查。
{% endstep %}
{% endstepper %}

{% hint style="info" %}
**重试机制**：BlockATM 会在 24 小时内重试多次，请确保处理逻辑幂等。
{% endhint %}

## 签名验证失败怎么解决？

常见原因：

| 原因 | 解决方法 |
|------|---------|
| 排序错误 | 参数按 ASCII 升序排列 |
| 时间戳错误 | 使用毫秒时间戳 |
| 密钥错误 | 检查 Secret Key |
| 编码错误 | 使用 UTF-8 编码 |

[查看签名生成示例 →](../integration/open-api/authentication.md)

## 如何处理并发请求？

**幂等性**：同一订单号不能重复创建。

**建议**：
1. 使用唯一订单号（如 UUID）
2. 前端防止重复提交
3. 后端做幂等检查

## 返回码 429 怎么处理？

表示请求超出限流。

**解决方法**：
1. 降低请求频率
2. 实现请求队列
3. 使用缓存减少重复请求

## 如何调试 API 请求？

{% tabs %}
{% tab title="使用 cURL" %}
```bash
curl -X POST https://test-open.blockatm.net/order/api/v2/payout/order \
  -H "Content-Type: application/json" \
  -H "BlockATM-api-Key: YOUR_API_KEY" \
  -H "BlockATM-Request-Time: 1742725435000" \
  -H "BlockATM-Signature-V2: YOUR_SIGNATURE" \
  -d '{"contractId":"xxx","orderNo":"xxx",...}'
```
{% endtab %}

{% tab title="使用 Postman" %}
1. 新建请求
2. 设置 Headers（apiKey, timestamp, signature）
3. 设置 Body（JSON）
4. 发送请求
{% endtab %}
{% endtabs %}

## 区块链网络拥堵怎么办？

| 方案 | 说明 |
|------|------|
| 等待 | 大部分拥堵会在几分钟内恢复 |
| 提高 Gas | （Ethereum）提高 Gas Price |
| 换网络 | 切换到 TRON 或 Arbitrum |

{% hint style="info" %}
**建议**：对于时间敏感的业务，建议使用 TRON 网络，确认速度快。
{% endhint %}
