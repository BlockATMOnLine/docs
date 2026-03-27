# React 集成示例

本示例展示如何在 React 应用中集成 BlockATM Widget SDK。

## 项目结构

```
src/
├── components/
│   └── BlockATMPayment.jsx    # 支付组件
├── hooks/
│   └── useBlockATM.js         # Hook
├── App.jsx
└── index.js
```

## 基础集成

### 1. 创建支付组件

```jsx
// components/BlockATMPayment.jsx
import React, { useEffect, useRef } from 'react';

const BlockATMPayment = ({ 
  apiKey, 
  cashierId, 
  orderNo, 
  amount, 
  symbol, 
  chainId,
  onSuccess,
  onCancel 
}) => {
  const containerRef = useRef(null);

  useEffect(() => {
    // 动态加载 SDK
    const script = document.createElement('script');
    script.src = `https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=${apiKey}`;
    script.async = true;
    document.body.appendChild(script);

    script.onload = () => {
      // 初始化 BlockATM
      if (window.BlockATM && containerRef.current) {
        window.BlockATM.init(containerRef.current, {
          cashierId,
          orderNo,
          amount,
          symbol,
          chainId,
          lang: 'zh-CN',
          callback: (result) => {
            if (result.type === 'finish') {
              onSuccess?.(result.data);
            } else if (result.type === 'cancel') {
              onCancel?.();
            }
          }
        });
      }
    };

    return () => {
      document.body.removeChild(script);
    };
  }, [apiKey, cashierId, orderNo, amount, symbol, chainId]);

  return (
    <div 
      ref={containerRef} 
      style={{ minHeight: '500px', width: '100%' }} 
    />
  );
};

export default BlockATMPayment;
```

### 2. 使用组件

```jsx
// App.jsx
import React, { useState } from 'react';
import BlockATMPayment from './components/BlockATMPayment';

function App() {
  const [orderNo] = useState(`ORDER_${Date.now()}`);
  const [paymentStatus, setPaymentStatus] = useState(null);

  const handleSuccess = (data) => {
    console.log('支付成功:', data);
    setPaymentStatus('success');
  };

  const handleCancel = () => {
    console.log('用户取消');
    setPaymentStatus('cancelled');
  };

  return (
    <div className="App">
      <h1>商品订单支付</h1>
      
      {paymentStatus === 'success' ? (
        <div className="success">
          <h2>✅ 支付成功！</h2>
          <p>感谢您的购买</p>
        </div>
      ) : paymentStatus === 'cancelled' ? (
        <div className="cancelled">
          <h2>支付已取消</h2>
          <button onClick={() => setPaymentStatus(null)}>
            重新支付
          </button>
        </div>
      ) : (
        <BlockATMPayment
          apiKey="YOUR_API_KEY"
          cashierId="YOUR_CASHIER_ID"
          orderNo={orderNo}
          amount="100"
          symbol="USDT"
          chainId="TRON"
          onSuccess={handleSuccess}
          onCancel={handleCancel}
        />
      )}
    </div>
  );
}

export default App;
```

---

## 高级集成

### 带签名的集成

如果需要后端签名：

```jsx
// hooks/useBlockATM.js
import { useState, useEffect } from 'react';

export const useBlockATMSignature = (orderNo, amount) => {
  const [signature, setSignature] = useState(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchSignature = async () => {
      setLoading(true);
      try {
        const response = await fetch('/api/blockatm/signature', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ orderNo, amount })
        });
        const data = await response.json();
        setSignature(data.signature);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    if (orderNo && amount) {
      fetchSignature();
    }
  }, [orderNo, amount]);

  return { signature, loading, error };
};
```

### 带错误处理的组件

```jsx
// components/BlockATMPaymentWithError.jsx
import React, { useEffect, useRef, useState } from 'react';

const BlockATMPaymentWithError = ({ config, onPaymentComplete }) => {
  const containerRef = useRef(null);
  const [error, setError] = useState(null);
  const [sdkLoaded, setSdkLoaded] = useState(false);

  useEffect(() => {
    const loadSDK = async () => {
      try {
        // 检查 SDK 是否已加载
        if (window.BlockATM) {
          setSdkLoaded(true);
          return;
        }

        const script = document.createElement('script');
        script.src = `https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=${config.apiKey}`;
        script.async = true;
        
        script.onload = () => setSdkLoaded(true);
        script.onerror = () => setError('SDK 加载失败');
        
        document.body.appendChild(script);
      } catch (err) {
        setError(err.message);
      }
    };

    loadSDK();
  }, [config.apiKey]);

  useEffect(() => {
    if (sdkLoaded && containerRef.current) {
      try {
        window.BlockATM.init(containerRef.current, {
          ...config,
          callback: (result) => {
            if (result.type === 'finish') {
              onPaymentComplete?.({ success: true, data: result.data });
            } else if (result.type === 'cancel') {
              onPaymentComplete?.({ success: false, reason: 'cancelled' });
            }
          }
        });
      } catch (err) {
        setError(err.message);
      }
    }
  }, [sdkLoaded, config, onPaymentComplete]);

  if (error) {
    return (
      <div className="payment-error">
        <h3>支付加载失败</h3>
        <p>{error}</p>
        <button onClick={() => window.location.reload()}>
          重试
        </button>
      </div>
    );
  }

  return (
    <div ref={containerRef} style={{ minHeight: '500px' }} />
  );
};

export default BlockATMPaymentWithError;
```

---

## 完整页面示例

```jsx
// pages/CheckoutPage.jsx
import React, { useState } from 'react';
import BlockATMPayment from '../components/BlockATMPayment';

const CheckoutPage = () => {
  const [step, setStep] = useState('confirm'); // confirm | payment | success
  const [orderDetails, setOrderDetails] = useState({
    orderNo: `ORDER_${Date.now()}`,
    amount: '100.00',
    currency: 'USD',
    symbol: 'USDT',
    chainId: 'TRON'
  });

  const handlePaymentSuccess = (data) => {
    console.log('Payment completed:', data);
    setStep('success');
  };

  return (
    <div className="checkout-page">
      {step === 'confirm' && (
        <div className="order-confirm">
          <h2>确认订单</h2>
          <div className="order-summary">
            <p>订单号: {orderDetails.orderNo}</p>
            <p>金额: {orderDetails.amount} {orderDetails.currency}</p>
          </div>
          <button 
            className="btn-primary"
            onClick={() => setStep('payment')}
          >
            去支付
          </button>
        </div>
      )}

      {step === 'payment' && (
        <div className="payment-container">
          <h2>选择支付方式</h2>
          <BlockATMPayment
            apiKey={process.env.REACT_APP_BLOCKATM_API_KEY}
            cashierId={process.env.REACT_APP_BLOCKATM_CASHIER_ID}
            orderNo={orderDetails.orderNo}
            amount={orderDetails.amount}
            symbol={orderDetails.symbol}
            chainId={orderDetails.chainId}
            onSuccess={handlePaymentSuccess}
            onCancel={() => setStep('confirm')}
          />
        </div>
      )}

      {step === 'success' && (
        <div className="payment-success">
          <h2>🎉 支付成功！</h2>
          <p>感谢您的购买</p>
          <button onClick={() => window.location.href = '/orders'}>
            查看订单
          </button>
        </div>
      )}
    </div>
  );
};

export default CheckoutPage;
```

---

## 环境配置

```bash
# .env.production
REACT_APP_BLOCKATM_API_KEY=your_api_key
REACT_APP_BLOCKATM_CASHIER_ID=your_cashier_id
REACT_APP_BLOCKATM_API_URL=https://pay.blockatm.net

# .env.development  
REACT_APP_BLOCKATM_API_KEY=your_test_api_key
REACT_APP_BLOCKATM_CASHIER_ID=your_test_cashier_id
REACT_APP_BLOCKATM_API_URL=https://test-pay.blockatm.net
```

---

## 样式示例

```css
/* BlockATM 容器样式 */
#blockatm-container {
  min-height: 500px;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  padding: 20px;
  background: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

/* 加载状态 */
.payment-loading {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  min-height: 300px;
  color: #666;
}

.payment-loading::after {
  content: '';
  width: 40px;
  height: 40px;
  border: 3px solid #f3f3f3;
  border-top: 3px solid #007bff;
  border-radius: 50%;
  animation: spin 1s linear infinite;
  margin-top: 16px;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}
```

---

## 下一步

- [Widget SDK 完整参数](./widget-sdk/parameters.md)
- [API 文档](./open-api/README.md)
- [Webhook 配置](./webhooks/README.md)
