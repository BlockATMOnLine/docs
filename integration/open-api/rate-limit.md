# 限流规则

BlockATM API 的调用频率限制说明。

## 限流策略

| 限流维度 | 限制值 | 说明 |
|---------|--------|------|
| API Key | 1000 请求/分钟 | 每个 API Key 的限制 |
| IP | 2000 请求/分钟 | 每个 IP 的限制 |
| 接口 | 100 请求/分钟 | 单个接口的限制 |

{% hint style="info" %}
**超出限制**：如果超出限流阈值，将返回 429 Too Many Requests 错误。
{% endhint %}

## 响应头

限流信息会在响应头中返回：

| Header | 说明 |
|--------|------|
| X-RateLimit-Limit | 请求上限 |
| X-RateLimit-Remaining | 剩余可用次数 |
| X-RateLimit-Reset | 重置时间（Unix 秒） |

## 最佳实践

### 1. 实现重试机制

```python
import time

def call_api_with_retry(api_func, max_retries=3):
    for i in range(max_retries):
        response = api_func()
        if response.status_code == 429:
            # 计算等待时间
            retry_after = int(response.headers.get('X-RateLimit-Reset', 60))
            wait_time = max(retry_after - time.time(), 1)
            time.sleep(wait_time)
            continue
        return response
    raise Exception("Max retries exceeded")
```

### 2. 批量处理

对于大量请求，建议：
- 使用批量接口（如有）
- 控制请求频率
- 错峰调用

### 3. 缓存热点数据

```python
# 缓存代币列表、网络列表等配置数据
cache = {
    'coin_list': {'data': None, 'expire': 0},
    'network_list': {'data': None, 'expire': 0}
}

def get_coin_list():
    if time.time() > cache['coin_list']['expire']:
        cache['coin_list']['data'] = api.fetch_coin_list()
        cache['coin_list']['expire'] = time.time() + 3600  # 1小时缓存
    return cache['coin_list']['data']
```

## 常见问题

**Q：限流后多久可以重试？**

A：查看响应头 `X-RateLimit-Reset`，等待该时间戳后重试。

**Q：可以申请更高的限流额度吗？**

A：可以，联系 BlockATM 技术支持申请企业版限流配额。
