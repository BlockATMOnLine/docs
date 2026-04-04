# 付币工作原理

了解 Batch Payout 付币的完整工作流程。

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
    A["🏦 商户钱包"] -->|"① 充值"| B["📦 付币合约"]
    B -->|"② 链上托管"| C["💎 合约余额"]
    C -->|"③ 批量分发"| D["👤 用户钱包"]

    style A fill:#D1FAE5,stroke:#10B981,color:#065F46
    style B fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style C fill:#DBEAFE,stroke:#3B82F6,color:#1E40AF
    style D fill:#EEF2FF,stroke:#6366F1,color:#4338CA
```

## 余额模式流程

### 完整时序图

```mermaid
%%{
  init: {
    'theme': 'neutral',
    'themeVariables': {
      'primaryColor': '#6366F1',
      'actorBkg': '#EEF2FF'
    }
  }
}%%
sequenceDiagram
    autonumber

    participant M as 🏦 商户
    participant P as 🔷 BlockATM 平台
    participant C as 📦 付币合约
    participant T as 🪙 Token 合约
    participant U as 👤 用户钱包

    rect rgb(209, 250, 229)
        Note over M,U: 阶段一：资金充值
        M->>P: 提交充值请求
        P->>T: 转入合约地址
        T-->>P: 充值成功
        Note over C: 资金到达合约
    end

    rect rgb(238, 242, 255)
        Note over M,U: 阶段二：付币执行
        M->>P: 提交付币订单
        P->>P: 审核 & 创建订单
        M->>P: 触发执行
        P->>C: 调用 payoutWithBalance()
        C->>T: transfer() 转账
        T-->>U: 到达用户钱包
        T-->>C: 扣除手续费
    end

    rect rgb(219, 234, 254)
        Note over M,U: 阶段三：确认完成
        C-->>P: Payout 事件
        P-->>M: Webhook 回调
    end
```

## 授权模式流程

### 完整时序图

```mermaid
%%{
  init: {
    'theme': 'neutral',
    'themeVariables': {
      'primaryColor': '#6366F1',
      'noteBkgColor': '#FEF3C7',
      'noteBorderColor': '#F59E0B'
    }
  }
}%%
sequenceDiagram
    autonumber

    participant U as 👤 授权用户
    participant T as 🪙 Token 合约
    participant P as 🔷 BlockATM 平台
    participant C as 📦 付币合约
    participant M as 🏦 商户

    rect rgb(254, 243, 199)
        Note over U,P: 阶段一：用户授权
        U->>T: approve(合约地址, 额度)
        T-->>U: 授权成功
        T-->>P: Approval 事件
        P->>P: 监听并记录授权
    end

    rect rgb(238, 242, 255)
        Note over U,P: 阶段二：付币执行
        M->>P: 提交付币订单
        P->>P: 校验授权额度
        P->>C: 调用 payoutWithAllowance()
        C->>T: transferFrom(用户→收款人)
        T->>T: 验证授权 & 扣款
        T-->>C: 转账成功
    end

    rect rgb(209, 250, 229)
        Note over U,M: 阶段三：确认完成
        C-->>P: Payout 事件
        P-->>M: Webhook 回调
    end
```



## 余额 vs 授权对比

| 维度 | 余额模式 | 授权模式 |
|:---|:---|:---|
| **资金位置** | 合约余额 | 用户钱包余额 |
| **授权需要** | 无需 | 需预先授权 |
| **资金风险** | 资金锁在合约中 | 资金在用户控制 |
| **灵活性** | 需预存资金 | 更灵活即时 |
| **推荐场景** | 高频固定金额付币 | 大规模分散资金付币 |

## 支付方式选择规则

```mermaid
%%{
  init: {
    'theme': 'neutral',
    'themeVariables': {
      'primaryColor': '#6366F1',
      'fontFamily': 'Inter'
    }
  }
}%%
graph TB
    START[🚀 开始付币] --> Q1{"合约余额<br/>≥ 订单金额?"}

    Q1 -->|是| A["✅ 余额支付"]
    Q1 -->|否| Q2{"授权额度<br/>≥ 订单金额?"}

    Q2 -->|是| B["✅ 授权支付"]
    Q2 -->|否| Q3{"余额 + 授权<br/>≥ 订单金额?"}

    Q3 -->|是| C["❌ 不支持混合"]
    Q3 -->|否| D["❌ 资金不足"]

    style Q1 fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style Q2 fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style Q3 fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style A fill:#D1FAE5,stroke:#10B981,color:#065F46
    style B fill:#D1FAE5,stroke:#10B981,color:#065F46
    style C fill:#FEE2E2,stroke:#EF4444,color:#991B1B
    style D fill:#FEE2E2,stroke:#EF4444,color:#991B1B
```

{% hint style="warning" %}
**重要**：余额支付和授权支付**二选一**，不支持混合支付。
{% endhint %}

## 合约角色

| 角色 | 说明 | 权限 |
|------|------|------|
| **Owner** | 合约创建者 | 管理合约配置 |
| **Packer** | 打包执行者 | 执行付币交易 |
| **Finance** | 财务地址 | 有权从合约提取资金 |
| **ColdWallet** | 冷钱包地址 | 可动态指定 |

## 异常处理

| 情况 | 处理方式 |
|------|---------|
| 余额不足 | 提示充值或切换授权模式 |
| 授权不足 | 提示增加授权额度 |
| 链上拥堵 | 等待确认，可查询链上状态 |
| 签名验证失败 | 检查签名地址和签名内容 |

## 下一步

- [余额模式详解 →](balance-mode.md)

- [查看合约接口 →](contract.md)
