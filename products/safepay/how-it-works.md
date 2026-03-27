# 收币工作原理

了解 Safepay 收币的完整工作流程。

## 资金流向

```mermaid
graph LR
    User[用户钱包] -->|1. 支付| Contract[收币合约]
    Contract -->|2. 保管| Balance[合约余额]
    Balance -->|3. 提取| Merchant[商户钱包]
    
    style Contract fill:#ffe1e1
    style Balance fill:#e1f5ff
```

## Web3 收款流程

### 完整时序图

```mermaid
sequenceDiagram
    participant User as 用户
    participant DApp as 您的应用
    participant Cashier as BlockATM 收银台
    participant Contract as 收币合约
    participant Token as Token 合约
    
    User->>DApp: 选择商品发起支付
    DApp->>Cashier: 获取支付会话
    Cashier-->>DApp: 返回支付页面
    DApp-->>User: 展示收银台
    
    User->>Cashier: 选择网络和代币
    User->>Cashier: 点击支付
    Cashier->>Contract: 生成订单
    
    alt 钱包连接方式
        User->>Cashier: 连接钱包
        Cashier->>Token: 触发授权（如需）
        User->>Token: 授权合约支配代币
        User->>Contract: 调用 deposit()
        Contract->>Token: transferFrom()
    end
    
    Token-->>Contract: 转账成功
    Contract-->>Cashier: 触发 Deposit 事件
    Cashier-->>DApp: 支付成功回调
    
    Note over User,DApp: 区块链确认后最终完成
```

### 关键步骤说明

1. **创建订单**：收银台生成订单，包含金额、代币类型等
2. **用户授权**：首次支付时，用户授权合约支配其代币
3. **链上转账**：用户确认后，代币从用户钱包转入合约
4. **事件通知**：合约触发事件，BlockATM 更新订单状态
5. **回调通知**：您的系统收到 Webhook 通知

## Scan2Pay 流程

### 完整时序图

```mermaid
sequenceDiagram
    participant User as 用户
    participant DApp as 您的应用
    participant Cashier as BlockATM 收银台
    participant Contract as 收币合约
    participant Wallet as 用户钱包 App
    
    User->>DApp: 进入支付页面
    DApp->>Cashier: 获取收款二维码
    Cashier-->>DApp: 返回收款地址 + 二维码
    DApp-->>User: 展示收款二维码
    
    User->>Wallet: 扫描二维码
    Wallet-->>User: 解析收款信息
    User->>Wallet: 确认支付金额
    User->>Wallet: 确认发送
    Wallet->>Contract: 转账到合约地址
    
    Contract-->>Wallet: 转账成功
    Wallet-->>User: 显示成功
    
    Contract-->>Cashier: 监听入账
    Cashier-->>DApp: 支付成功回调
```

{% hint style="warning" %}
**重要**：用户在使用 Scan2Pay 时，必须确保支付金额与订单金额完全一致，否则可能导致订单无法匹配。
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
