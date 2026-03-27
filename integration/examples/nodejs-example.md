# Node.js 后端集成示例

本示例展示如何在 Node.js 后端集成 BlockATM API，包括签名生成、订单创建和 Webhook 处理。

## 项目结构

```
server/
├── src/
│   ├── routes/
│   │   └── blockatm.js      # 路由
│   ├── services/
│   │   └── blockatm.js     # 服务
│   ├── middleware/
│   │   └── signature.js     # 签名中间件
│   ├── utils/
│   │   └── crypto.js       # 加密工具
│   └── app.js
├── .env
└── package.json
```

## 依赖安装

```bash
npm install axios dotenv express body-parser cors
```

## 环境配置

```bash
# .env
BLOCKATM_API_KEY=your_api_key
BLOCKATM_SECRET_KEY=your_secret_key
BLOCKATM_WEBHOOK_KEY=your_webhook_key
BLOCKATM_API_URL=https://open.blockatm.net
BLOCKATM_API_URL_TEST=https://test-open.blockatm.net
```

## 签名工具

```javascript
// utils/crypto.js
const crypto = require('crypto');

/**
 * 生成 BlockATM 签名
 * @param {Object} params - 请求参数对象
 * @param {number} timestamp - 毫秒时间戳
 * @param {string} secretKey - Secret Key
 * @returns {string} HMAC-SHA256 签名
 */
function generateSignature(params, timestamp, secretKey) {
  // 1. 按 ASCII 排序参数 key
  const sortedKeys = Object.keys(params).sort();
  
  // 2. 拼接 key=value 格式字符串
  const paramString = sortedKeys
    .map(key => `${key}=${params[key]}`)
    .join('&');
  
  // 3. 拼接时间戳
  const payload = `${paramString}&time=${timestamp}`;
  
  // 4. 计算 HMAC-SHA256
  const signature = crypto
    .createHmac('sha256', secretKey)
    .update(payload)
    .digest('hex');
  
  return signature;
}

/**
 * 验证 Webhook 签名
 * @param {Object} payload - 请求体
 * @param {string} signature - 签名
 * @param {string} webhookKey - Webhook Key
 * @returns {boolean} 是否验证通过
 */
function verifyWebhookSignature(payload, signature, webhookKey) {
  const expected = crypto
    .createHmac('sha256', webhookKey)
    .update(JSON.stringify(payload))
    .digest('hex');
  
  return expected === signature;
}

module.exports = {
  generateSignature,
  verifyWebhookSignature
};
```

## BlockATM 服务

```javascript
// services/blockatm.js
const axios = require('axios');
const { generateSignature } = require('../utils/crypto');

class BlockATMService {
  constructor(apiKey, secretKey, baseUrl = 'https://open.blockatm.net') {
    this.apiKey = apiKey;
    this.secretKey = secretKey;
    this.baseUrl = baseUrl;
    this.client = axios.create({
      baseURL: baseUrl,
      timeout: 30000
    });
  }

  /**
   * 创建收币订单
   */
  async createPaymentOrder(params) {
    const timestamp = Date.now();
    const signature = generateSignature(params, timestamp, this.secretKey);

    const response = await this.client.post('/order/api/v2/pay/order', params, {
      headers: {
        'Content-Type': 'application/json',
        'BlockATM-API-Key': this.apiKey,
        'BlockATM-Request-Time': timestamp.toString(),
        'BlockATM-Signature-V2': signature
      }
    });

    return response.data;
  }

  /**
   * 查询收币订单
   */
  async getPaymentOrder(orderNo) {
    const timestamp = Date.now();
    const params = { orderNo };
    const signature = generateSignature(params, timestamp, this.secretKey);

    const response = await this.client.get('/order/api/v2/payorder/detail', {
      params,
      headers: {
        'BlockATM-API-Key': this.apiKey,
        'BlockATM-Request-Time': timestamp.toString(),
        'BlockATM-Signature-V2': signature
      }
    });

    return response.data;
  }

  /**
   * 创建付币订单
   */
  async createPayoutOrder(params) {
    const timestamp = Date.now();
    const signature = generateSignature(params, timestamp, this.secretKey);

    const response = await this.client.post('/order/api/v2/payout/order', params, {
      headers: {
        'Content-Type': 'application/json',
        'BlockATM-API-Key': this.apiKey,
        'BlockATM-Request-Time': timestamp.toString(),
        'BlockATM-Signature-V2': signature
      }
    });

    return response.data;
  }

  /**
   * 查询付币订单
   */
  async getPayoutOrder(orderNo) {
    const timestamp = Date.now();
    const params = { orderNo };
    const signature = generateSignature(params, timestamp, this.secretKey);

    const response = await this.client.get('/order/api/v2/payout/detail', {
      params,
      headers: {
        'BlockATM-API-Key': this.apiKey,
        'BlockATM-Request-Time': timestamp.toString(),
        'BlockATM-Signature-V2': signature
      }
    });

    return response.data;
  }

  /**
   * 获取收银台配置
   */
  async getCashierInfo(cashierId) {
    const timestamp = Date.now();
    const params = { cashierId };
    const signature = generateSignature(params, timestamp, this.secretKey);

    const response = await this.client.get('/admin/api/v2/pub/cashier/info', {
      params,
      headers: {
        'BlockATM-API-Key': this.apiKey,
        'BlockATM-Request-Time': timestamp.toString(),
        'BlockATM-Signature-V2': signature
      }
    });

    return response.data;
  }
}

module.exports = BlockATMService;
```

## 路由处理

```javascript
// routes/blockatm.js
const express = require('express');
const router = express.Router();
const BlockATMService = require('../services/blockatm');
const { verifyWebhookSignature } = require('../utils/crypto');

// 初始化服务
const blockatm = new BlockATMService(
  process.env.BLOCKATM_API_KEY,
  process.env.BLOCKATM_SECRET_KEY
);

/**
 * POST /api/blockatm/create-order
 * 创建收币订单
 */
router.post('/create-order', async (req, res) => {
  try {
    const { cashierId, orderNo, amount, symbol, chainId, custNo } = req.body;

    const result = await blockatm.createPaymentOrder({
      cashierId,
      orderNo,
      amount,
      symbol,
      chainId,
      custNo
    });

    res.json(result);
  } catch (error) {
    console.error('创建订单失败:', error.message);
    res.status(500).json({ 
      code: 'ERROR_000500', 
      message: '创建订单失败' 
    });
  }
});

/**
 * GET /api/blockatm/order/:orderNo
 * 查询订单状态
 */
router.get('/order/:orderNo', async (req, res) => {
  try {
    const { orderNo } = req.params;
    const result = await blockatm.getPaymentOrder(orderNo);
    res.json(result);
  } catch (error) {
    console.error('查询订单失败:', error.message);
    res.status(500).json({ 
      code: 'ERROR_000500', 
      message: '查询订单失败' 
    });
  }
});

/**
 * POST /api/blockatm/create-payout
 * 创建付币订单
 */
router.post('/create-payout', async (req, res) => {
  try {
    const { contractId, orderNo, recipient, amount, symbol, chainId } = req.body;

    const result = await blockatm.createPayoutOrder({
      contractId,
      orderNo,
      recipient,
      amount,
      symbol,
      chainId
    });

    res.json(result);
  } catch (error) {
    console.error('创建付币订单失败:', error.message);
    res.status(500).json({ 
      code: 'ERROR_000500', 
      message: '创建付币订单失败' 
    });
  }
});

/**
 * POST /api/blockatm/webhook
 * 接收 BlockATM Webhook 通知
 */
router.post('/webhook', (req, res) => {
  try {
    const signature = req.headers['blockatm-signature'];
    const payload = req.body;

    // 验证签名
    if (!verifyWebhookSignature(payload, signature, process.env.BLOCKATM_WEBHOOK_KEY)) {
      console.error('Webhook 签名验证失败');
      return res.status(401).json({ error: 'Invalid signature' });
    }

    // 处理支付成功通知
    if (payload.eventType === 'payment.success') {
      const { orderNo, txHash, amount, symbol } = payload;
      console.log(`支付成功: 订单 ${orderNo}, 金额 ${amount} ${symbol}`);
      
      // TODO: 更新您的数据库
      // await updateOrderStatus(orderNo, 'COMPLETED', txHash);
    }

    // 处理付币成功通知
    if (payload.eventType === 'payout.success') {
      const { orderNo, txHash, amount, symbol } = payload;
      console.log(`付币成功: 订单 ${orderNo}, 金额 ${amount} ${symbol}`);
      
      // TODO: 更新您的数据库
    }

    res.status(200).json({ message: 'OK' });
  } catch (error) {
    console.error('Webhook 处理失败:', error.message);
    res.status(500).json({ error: 'Internal error' });
  }
});

/**
 * GET /api/blockatm/cashier/:cashierId
 * 获取收银台配置
 */
router.get('/cashier/:cashierId', async (req, res) => {
  try {
    const { cashierId } = req.params;
    const result = await blockatm.getCashierInfo(cashierId);
    res.json(result);
  } catch (error) {
    console.error('获取收银台信息失败:', error.message);
    res.status(500).json({ 
      code: 'ERROR_000500', 
      message: '获取收银台信息失败' 
    });
  }
});

module.exports = router;
```

## 主应用

```javascript
// app.js
require('dotenv').config();
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const blockatmRoutes = require('./routes/blockatm');

const app = express();

// 中间件
app.use(cors());
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));

// 路由
app.use('/api/blockatm', blockatmRoutes);

// 健康检查
app.get('/health', (req, res) => {
  res.json({ status: 'ok' });
});

// 错误处理
app.use((err, req, res, next) => {
  console.error('Server error:', err);
  res.status(500).json({ error: 'Internal server error' });
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

## Webhook 处理示例（带数据库）

```javascript
// services/orderService.js
const db = require('../db'); // 假设的数据库模块

class OrderService {
  async handlePaymentSuccess(payload) {
    const { id, orderNo, fromAddress, amount, symbol, status, blockTime } = payload;
    
    // 查询订单
    const order = await db.orders.findOne({ orderNo });
    if (!order) {
      console.warn(`订单不存在: ${orderNo}`);
      return;
    }

    // 防止重复处理（幂等性）
    if (order.status === 'COMPLETED') {
      console.log(`订单已处理: ${orderNo}`);
      return;
    }

    // 更新订单状态
    await db.orders.updateOne(
      { orderNo },
      {
        $set: {
          status: 'COMPLETED',
          txHash: payload.txHash,
          fromAddress,
          completedAt: new Date(blockTime)
        },
        $push: {
          history: {
            type: 'PAYMENT_RECEIVED',
            data: payload,
            createdAt: new Date()
          }
        }
      }
    );

    // 发送通知（如有）
    await sendNotification(order.userId, 'PAYMENT_SUCCESS', {
      orderNo,
      amount,
      symbol
    });

    console.log(`订单支付成功: ${orderNo}`);
  }

  async handlePaymentFailed(payload) {
    const { orderNo, reason } = payload;
    
    await db.orders.updateOne(
      { orderNo },
      {
        $set: {
          status: 'FAILED',
          failReason: reason
        }
      }
    );

    console.log(`订单支付失败: ${orderNo}, 原因: ${reason}`);
  }
}

module.exports = new OrderService();
```

## 测试脚本

```javascript
// scripts/test-api.js
const BlockATMService = require('../services/blockatm');

async function test() {
  const client = new BlockATMService(
    process.env.BLOCKATM_API_KEY,
    process.env.BLOCKATM_SECRET_KEY,
    process.env.BLOCKATM_API_URL_TEST
  );

  try {
    // 创建测试订单
    const orderResult = await client.createPaymentOrder({
      cashierId: 'YOUR_CASHIER_ID',
      orderNo: `TEST_${Date.now()}`,
      amount: '10',
      symbol: 'USDT',
      chainId: 'TRON',
      custNo: 'TEST_USER_001'
    });
    console.log('创建订单结果:', orderResult);

    // 查询订单
    const queryResult = await client.getPaymentOrder(orderResult.data?.orderNo);
    console.log('查询订单结果:', queryResult);

    // 获取收银台信息
    const cashierInfo = await client.getCashierInfo('YOUR_CASHIER_ID');
    console.log('收银台信息:', cashierInfo);

    console.log('✅ 所有测试通过！');
  } catch (error) {
    console.error('❌ 测试失败:', error.message);
    if (error.response) {
      console.error('响应数据:', error.response.data);
    }
  }
}

test();
```

## 部署建议

### 使用 PM2

```bash
# 安装 PM2
npm install -g pm2

# 启动服务
pm2 start app.js --name blockatm-api

# 查看日志
pm2 logs blockatm-api

# 重启
pm2 restart blockatm-api
```

### Nginx 配置

```nginx
server {
    listen 443 ssl;
    server_name your-domain.com;

    ssl_certificate /path/to/cert.pem;
    ssl_certificate_key /path/to/key.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

---

## 下一步

- [API 文档](../open-api/README.md)
- [Webhook 配置](../webhooks/README.md)
- [签名认证](../open-api/authentication.md)
