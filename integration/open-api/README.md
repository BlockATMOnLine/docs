# Open API

BlockATM Open API 提供完整的支付接口，支持收币、付币、订单查询等功能。

## 特点

- ✅ **功能完整**：覆盖收币、付币、配置、查询全部场景
- ✅ **签名安全**：HMAC-SHA256 请求签名
- ✅ **多语言支持**：Java、Python、Go、PHP、C++ 示例
- ✅ **Webhook 支持**：实时接收支付事件通知

## API 环境

| 环境 | Base URL |
|------|----------|
| 生产环境 | `https://open.blockatm.net` |
| 测试环境 | `https://test-open.blockatm.net` |

## 核心端点

### 收币

| 接口 | 方法 | 说明 |
|------|------|------|
| `/order/api/v2/pay/order` | POST | 创建收币订单 |
| `/order/api/v2/payorder/list` | GET | 查询收币订单列表 |
| `/order/api/v2/payorder/detail` | GET | 查询收币订单详情 |

### 付币

| 接口 | 方法 | 说明 |
|------|------|------|
| `/order/api/v2/payout/order` | POST | 创建付币订单 |
| `/order/api/v2/payout/detail` | GET | 查询付币订单详情 |

### 配置

| 接口 | 方法 | 说明 |
|------|------|------|
| `/admin/api/v2/pub/cashier/info` | GET | 查询收银台配置 |
| `/admin/api/v2/pub/coin/list` | GET | 查询代币列表 |
| `/admin/api/v2/pub/network/list` | GET | 查询网络列表 |

## 快速开始

1. [了解签名认证 →](authentication.md)
2. [查看请求格式 →](request-format.md)
3. [查询配置信息 →](config-api.md)
4. [查看收币 API →](payment-api.md)
5. [查看付币 API →](payout-api.md)

## 费用说明

| 服务 | 费用 |
|------|------|
| 创建收币订单 | 免费 |
| 创建付币订单 | 免费 |
| 收币成功 | 2 USD 或 0.4% |
| 付币成功 | 1 USD |
