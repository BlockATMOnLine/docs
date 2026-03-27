# Vue 集成示例

本示例展示如何在 Vue 3 应用中集成 BlockATM Widget SDK。

## 项目结构

```
src/
├── components/
│   └── BlockATMPayment.vue    # 支付组件
├── composables/
│   └── useBlockATM.js         # 组合式函数
├── views/
│   └── Checkout.vue           # 结账页面
└── App.vue
```

## 基础集成

### 1. 创建支付组件

```vue
<!-- components/BlockATMPayment.vue -->
<template>
  <div ref="containerRef" class="blockatm-payment" />
</template>

<script setup>
import { ref, onMounted, onUnmounted, watch } from 'vue';

const props = defineProps({
  apiKey: {
    type: String,
    required: true
  },
  cashierId: {
    type: String,
    required: true
  },
  orderNo: {
    type: String,
    required: true
  },
  amount: {
    type: String,
    required: true
  },
  symbol: {
    type: String,
    default: 'USDT'
  },
  chainId: {
    type: String,
    default: 'TRON'
  }
});

const emit = defineEmits(['success', 'cancel', 'error']);

const containerRef = ref(null);
let blockATMInstance = null;

const loadSDK = () => {
  return new Promise((resolve, reject) => {
    // 检查是否已加载
    if (window.BlockATM) {
      resolve();
      return;
    }

    const script = document.createElement('script');
    script.src = `https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=${props.apiKey}`;
    script.async = true;
    
    script.onload = () => resolve();
    script.onerror = () => reject(new Error('SDK 加载失败'));
    
    document.body.appendChild(script);
  });
};

const initPayment = async () => {
  try {
    await loadSDK();
    
    if (!window.BlockATM || !containerRef.value) {
      return;
    }

    blockATMInstance = window.BlockATM.init(containerRef.value, {
      cashierId: props.cashierId,
      orderNo: props.orderNo,
      amount: props.amount,
      symbol: props.symbol,
      chainId: props.chainId,
      lang: 'zh-CN',
      callback: (result) => {
        if (result.type === 'finish') {
          emit('success', result.data);
        } else if (result.type === 'cancel') {
          emit('cancel');
        }
      }
    });
  } catch (error) {
    emit('error', error.message);
  }
};

onMounted(() => {
  initPayment();
});

onUnmounted(() => {
  // 清理工作（如需要）
});

watch(() => [props.cashierId, props.orderNo], () => {
  initPayment();
});
</script>

<style scoped>
.blockatm-payment {
  min-height: 500px;
  width: 100%;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  overflow: hidden;
}
</style>
```

### 2. 使用组件

```vue
<!-- views/Checkout.vue -->
<template>
  <div class="checkout-page">
    <h1>订单支付</h1>
    
    <div v-if="paymentStatus === 'idle'" class="order-summary">
      <h2>订单摘要</h2>
      <p>订单号: {{ orderNo }}</p>
      <p>金额: {{ amount }} USDT</p>
      <button @click="startPayment" class="btn-primary">
        去支付
      </button>
    </div>

    <div v-else-if="paymentStatus === 'paying'" class="payment-container">
      <h2>请选择支付方式</h2>
      <BlockATMPayment
        :api-key="apiKey"
        :cashier-id="cashierId"
        :order-no="orderNo"
        :amount="amount"
        symbol="USDT"
        chain-id="TRON"
        @success="handleSuccess"
        @cancel="handleCancel"
        @error="handleError"
      />
    </div>

    <div v-else-if="paymentStatus === 'success'" class="success">
      <h2>🎉 支付成功！</h2>
      <p>感谢您的购买</p>
      <button @click="resetPayment" class="btn-secondary">
        返回
      </button>
    </div>

    <div v-else-if="paymentStatus === 'cancelled'" class="cancelled">
      <h2>支付已取消</h2>
      <button @click="resetPayment" class="btn-primary">
        重新支付
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import BlockATMPayment from '../components/BlockATMPayment.vue';

const apiKey = import.meta.env.VITE_BLOCKATM_API_KEY;
const cashierId = import.meta.env.VITE_BLOCKATM_CASHIER_ID;

const paymentStatus = ref('idle');
const orderNo = ref(`ORDER_${Date.now()}`);
const amount = ref('100');

const startPayment = () => {
  paymentStatus.value = 'paying';
};

const handleSuccess = (data) => {
  console.log('Payment success:', data);
  paymentStatus.value = 'success';
};

const handleCancel = () => {
  console.log('Payment cancelled');
  paymentStatus.value = 'cancelled';
};

const handleError = (error) => {
  console.error('Payment error:', error);
  alert('支付加载失败，请重试');
};

const resetPayment = () => {
  orderNo.value = `ORDER_${Date.now()}`;
  paymentStatus.value = 'idle';
};
</script>

<style scoped>
.checkout-page {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}

.order-summary,
.success,
.cancelled {
  text-align: center;
  padding: 40px 20px;
}

.btn-primary,
.btn-secondary {
  padding: 12px 24px;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  cursor: pointer;
  margin: 10px;
}

.btn-primary {
  background: #007bff;
  color: white;
}

.btn-secondary {
  background: #6c757d;
  color: white;
}
</style>
```

---

## 组合式函数

### useBlockATM Hook

```javascript
// composables/useBlockATM.js
import { ref, shallowRef } from 'vue';

export function useBlockATM() {
  const isLoading = ref(false);
  const error = ref(null);
  const paymentStatus = ref('idle'); // idle | pending | success | failed
  const sdk = shallowRef(null);

  const loadSDK = async (apiKey) => {
    if (sdk.value || window.BlockATM) {
      sdk.value = window.BlockATM;
      return;
    }

    isLoading.value = true;
    error.value = null;

    try {
      const script = document.createElement('script');
      script.src = `https://pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=${apiKey}`;
      script.async = true;

      await new Promise((resolve, reject) => {
        script.onload = resolve;
        script.onerror = () => reject(new Error('SDK 加载失败'));
        document.body.appendChild(script);
      });

      sdk.value = window.BlockATM;
    } catch (err) {
      error.value = err.message;
    } finally {
      isLoading.value = false;
    }
  };

  const initPayment = (container, config) => {
    if (!sdk.value) {
      error.value = 'SDK 未加载';
      return null;
    }

    paymentStatus.value = 'pending';
    
    return sdk.value.init(container, {
      ...config,
      callback: (result) => {
        if (result.type === 'finish') {
          paymentStatus.value = 'success';
        } else if (result.type === 'cancel') {
          paymentStatus.value = 'idle';
        }
      }
    });
  };

  const reset = () => {
    paymentStatus.value = 'idle';
    error.value = null;
  };

  return {
    isLoading,
    error,
    paymentStatus,
    loadSDK,
    initPayment,
    reset
  };
}
```

### 使用组合式函数

```vue
<!-- views/AdvancedCheckout.vue -->
<template>
  <div class="advanced-checkout">
    <h1>高级支付示例</h1>
    
    <div v-if="isLoading" class="loading">
      加载中...
    </div>
    
    <div v-else-if="error" class="error">
      {{ error }}
      <button @click="retryLoad">重试</button>
    </div>
    
    <div v-else>
      <div ref="paymentContainer" class="payment-container" />
    </div>

    <div v-if="paymentStatus === 'success'" class="success-message">
      ✅ 支付成功！
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import { useBlockATM } from '../composables/useBlockATM';

const paymentContainer = ref(null);
const { isLoading, error, paymentStatus, loadSDK, initPayment } = useBlockATM();

onMounted(async () => {
  await loadSDK('YOUR_API_KEY');
  if (paymentContainer.value) {
    initPayment(paymentContainer.value, {
      cashierId: 'YOUR_CASHIER_ID',
      orderNo: `ORDER_${Date.now()}`,
      amount: '100',
      symbol: 'USDT',
      chainId: 'TRON'
    });
  }
});

const retryLoad = async () => {
  await loadSDK('YOUR_API_KEY');
  if (paymentContainer.value) {
    initPayment(paymentContainer.value, {
      cashierId: 'YOUR_CASHIER_ID',
      orderNo: `ORDER_${Date.now()}`,
      amount: '100',
      symbol: 'USDT',
      chainId: 'TRON'
    });
  }
};
</script>
```

---

## Pinia Store 集成

```javascript
// stores/payment.js
import { defineStore } from 'pinia';
import { ref } from 'vue';

export const usePaymentStore = defineStore('payment', () => {
  const status = ref('idle');
  const currentOrder = ref(null);
  const paymentHistory = ref([]);

  const createOrder = async (amount, symbol) => {
    const orderNo = `ORDER_${Date.now()}`;
    currentOrder.value = {
      orderNo,
      amount,
      symbol,
      createdAt: Date.now()
    };
    status.value = 'pending';
    return orderNo;
  };

  const setPaymentSuccess = (data) => {
    status.value = 'success';
    paymentHistory.value.unshift({
      ...currentOrder.value,
      ...data,
      completedAt: Date.now()
    });
  };

  const setPaymentCancelled = () => {
    status.value = 'cancelled';
  };

  const resetPayment = () => {
    status.value = 'idle';
    currentOrder.value = null;
  };

  return {
    status,
    currentOrder,
    paymentHistory,
    createOrder,
    setPaymentSuccess,
    setPaymentCancelled,
    resetPayment
  };
});
```

---

## 环境配置

```bash
# .env.production
VITE_BLOCKATM_API_KEY=your_api_key
VITE_BLOCKATM_CASHIER_ID=your_cashier_id
VITE_BLOCKATM_API_URL=https://pay.blockatm.net

# .env.development
VITE_BLOCKATM_API_KEY=your_test_api_key
VITE_BLOCKATM_CASHIER_ID=your_test_cashier_id
VITE_BLOCKATM_API_URL=https://test-pay.blockatm.net
```

---

## TypeScript 支持

```typescript
// types/blockatm.d.ts
interface BlockATMConfig {
  cashierId: string;
  orderNo: string;
  amount: string;
  symbol: string;
  chainId: string;
  lang?: string;
  signature?: string;
  callback: (result: BlockATMResult) => void;
}

interface BlockATMResult {
  type: 'finish' | 'cancel';
  data?: {
    orderNo: string;
    txHash: string;
    amount: string;
    symbol: string;
  };
  message?: string;
}

interface BlockATM {
  init(container: HTMLElement, config: BlockATMConfig): void;
}

declare global {
  interface Window {
    BlockATM: BlockATM;
  }
}

export {};
```

---

## 下一步

- [Widget SDK 完整参数](../widget-sdk/parameters.md)
- [API 文档](../open-api/README.md)
- [Webhook 配置](../webhooks/README.md)
