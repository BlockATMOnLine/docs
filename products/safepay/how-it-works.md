# 收币工作原理

了解 Safepay 收币的完整工作流程。

## 资金流向

```mermaid
%%{
  init: {
    'theme': 'neutral',
    'themeVariables': {
      'primaryColor': '#6366F1',
      'lineColor': '#64748B'
    }
  }
}%%
graph LR
    A["👤 用户钱包"] -->|"① 转账"| B["📦 收币合约"]
    B -->|"② 链上托管"| C["💎 合约余额"]
    C -->|"③ 按需提取"| D["🏦 商户钱包"]

    style A fill:#EEF2FF,stroke:#6366F1,color:#4338CA
    style B fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style C fill:#DBEAFE,stroke:#3B82F6,color:#1E40AF
    style D fill:#D1FAE5,stroke:#10B981,color:#065F46
```

## Web3 收款流程

### 完整时序图

```mermaid
%%{
  init: {
    'theme': 'neutral',
    'themeVariables': {
      'primaryColor': '#6366F1',
      'actorBkg': '#EEF2FF',
      'actorBorder': '#6366F1',
      'actorTextColor': '#1E293B',
      'noteBkgColor': '#F8FAFC',
      'noteTextColor': '#1E293B',
      'signalColor': '#64748B',
      'signalTextColor': '#1E293B'
    }
  }
}%%
sequenceDiagram
    autonumber

    participant U as 👤 用户
    participant A as 📱 商户应用
    participant C as 🏪 BlockATM 收银台
    participant S as ⚙️ 智能合约
    participant T as 🪙 Token 合约
    participant B as ⛓️ 区块链

    rect rgb(238, 242, 255)
        Note over U,C: 阶段一：订单初始化
        A->>C: 获取支付会话
        C-->>A: 返回收银台页面
        A-->>U: 展示收银台界面
        U->>C: 选择网络 & 代币
        U->>C: 点击支付
        C->>S: 生成链上订单
    end

    rect rgb(219, 234, 254)
        Note over U,B: 阶段二：链上执行
        C->>U: 唤起钱包签名
        U->>T: 授权合约支配代币（如需）
        U->>S: 调用 deposit()
        S->>T: transferFrom() 转账
        T-->>S: 转账成功
        S->>B: 广播交易
    end

    rect rgb(209, 250, 229)
        Note over U,A: 阶段三：确认完成
        B->>S: 区块确认
        S-->>C: Deposit 事件触发
        C-->>A: Webhook 回调
        A-->>U: 支付成功展示
    end
```

### 关键步骤说明

1. **创建订单**：收银台生成链上订单，记录金额、代币类型、商户信息
2. **钱包授权**：首次支付时，用户授权合约支配其代币资产
3. **链上转账**：用户签名确认后，代币从用户钱包原子化转移至合约
4. **事件监听**：合约触发 `Deposit` 事件，BlockATM 链上监控服务捕获
5. **状态同步**：订单状态更新，Webhook 通知商户系统

## Scan2Pay 流程

### 完整时序图

```mermaid
%%{
  init: {
    'theme': 'neutral',
    'themeVariables': {
      'primaryColor': '#6366F1',
      'actorBkg': '#EEF2FF',
      'noteBkgColor': '#FEF3C7',
      'noteBorderColor': '#F59E0B'
    }
  }
}%%
sequenceDiagram
    autonumber

    participant U as 👤 用户
    participant A as 📱 商户应用
    participant C as 🏪 BlockATM 收银台
    participant S as 📦 收币合约
    participant W as 👛 用户钱包 App
    participant B as ⛓️ 区块链

    rect rgb(238, 242, 255)
        Note over U,B: 阶段一：订单生成
        A->>C: 请求收款二维码
        C->>S: 生成临时收款地址
        S-->>C: 返回收款信息
        C-->>A: 返回二维码 + 地址
        A-->>U: 展示收款码
    end

    rect rgb(254, 243, 199)
        Note over U,B: 阶段二：用户支付
        U->>W: 扫描二维码
        W-->>U: 解析收款详情
        U->>W: 确认支付金额
        U->>W: 授权转账
        W->>S: 转账至合约地址
        S-->>W: 转账成功
        W-->>U: 显示成功
    end

    rect rgb(219, 234, 254)
        Note over U,A: 阶段三：订单匹配
        S->>B: 交易上链
        B->>S: 区块确认
        S-->>C: 入账事件
        C-->>A: 订单匹配成功
        A-->>U: 支付完成
    end
```

{% hint style="warning" %}
**重要**：用户在使用 Scan2Pay 时，必须确保支付金额与订单金额完全一致，否则可能导致订单无法自动关联。
{% endhint %}

## 合约地址角色

| 角色 | 说明 | 设置时机 |
|------|------|---------|
| **Owner** | 合约创建者，可管理合约配置 | 创建合约时 |
| **Signer** | 有权从合约提取资金的地址 | 创建合约时 |
| **Receiver** | 资金最终到达的地址 | 创建合约时 |

## 异常处理

| 情况 | 处理方式 |
|------|---------|
| 用户未授权 | 提示用户完成授权后重试 |
| 区块链拥堵 | 等待确认，可查询链上状态 |
| 金额不匹配 | Scan2Pay 订单可能无法自动关联 |
| 订单超时 | 收银台显示超时，用户可重新发起支付 |

## 下一步

- [查看合约接口 →](contract.md)
- [查看费用说明 →](fees.md)
- [集成收币 →](../../integration/guides/collect-guide.md)
