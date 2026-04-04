# Rate Limit Rules

BlockATM API call frequency limit description.

## Rate Limit Strategy

| Limit Dimension | Limit | Description |
|---------|--------|------|
| API Key | 1000 requests/minute | Per API Key limit |
| IP | 2000 requests/minute | Per IP limit |
| API | 100 requests/minute | Per API limit |

{% hint style="info" %}
**Over Limit**: If rate limit threshold is exceeded, 429 Too Many Requests error will be returned.
{% endhint %}

## Response Headers

Rate limit information is returned in response headers:

| Header | Description |
|--------|------|
| X-RateLimit-Limit | Request limit |
| X-RateLimit-Remaining | Remaining available calls |
| X-RateLimit-Reset | Reset time (Unix seconds) |

## Best Practices

### 1. Implement Retry Mechanism

```python
import time

def call_api_with_retry(api_func, max_retries=3):
    for i in range(max_retries):
        response = api_func()
        if response.status_code == 429:
            # Calculate wait time
            retry_after = int(response.headers.get('X-RateLimit-Reset', 60))
            wait_time = max(retry_after - time.time(), 1)
            time.sleep(wait_time)
            continue
        return response
    raise Exception("Max retries exceeded")
```

### 2. Batch Processing

For large volumes of requests, recommended:
- Use batch APIs (if available)
- Control request frequency
- Stagger calls during off-peak hours

### 3. Cache Hot Data

```python
# Cache token list, network list and other config data
cache = {
    'coin_list': {'data': None, 'expire': 0},
    'network_list': {'data': None, 'expire': 0}
}

def get_coin_list():
    if time.time() > cache['coin_list']['expire']:
        cache['coin_list']['data'] = api.fetch_coin_list()
        cache['coin_list']['expire'] = time.time() + 3600  # 1 hour cache
    return cache['coin_list']['data']
```

## FAQ

**Q: How long to wait before retry after rate limit?**

A: Check response header `X-RateLimit-Reset`, wait until that timestamp before retrying.

**Q: Can I apply for higher rate limits?**

A: Yes, contact BlockATM technical support for enterprise rate limit quota.
