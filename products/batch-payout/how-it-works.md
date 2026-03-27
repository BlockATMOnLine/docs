# 付币工作原理

了解 Batch Payout 付币的完整工作流程。

## 资金流向

```mermaid
graph LR
    Merchant[商户钱包] -->|充值| Contract[付币合约]
    Contract -->|保管| Balance[合约余额]
    Balance -->|提取| User[用户钱包]
    
    style Contract fill:#ffe1e1
    style Balance fill:#e1f5ff
```

## 余额模式流程

### 完整时序图

```mermaid
sequenceDiagram
    participant Merchant as 商户运营
    participant Platform as BlockATM 平台
    participant Contract as 付币合约
    participant Token as Token 合约
    participant User as 用户钱包
    
    Merchant->>Platform: 1. 充值到合约
    Platform->>Token: 2. 转账到合约地址
    Token-->>Platform: 3. 转账成功
    
    Merchant->>Platform: 4. 提交付币订单
    Platform->>Platform: 5. 审核并创建订单
    
    Merchant->>Platform: 6. 执行付币
    Platform->>Contract: 7. 调用 payoutWithBalance()
    Contract->>Token: 8. transfer()
    Token-->>User: 9. 转账到用户
    Token-->>Contract: 10. 扣手续费
    
    Contract-->>Platform: 11. 触发事件
    Platform-->>Merchant: 12. 回调通知
```

## 授权模式流程

### 完整时序图

```mermaid
sequenceDiagram
    participant User as 授权用户
    participant Contract as 付币合约
    participant Token as Token 合约
    participant Merchant as 商户运营
    participant Platform as BlockATM 平台
    
    Note over User,Platform: 阶段1：授权
    User->>Token: approve(合约地址, 额度)
    Token-->>User: 授权成功
    Token-->>Platform: Approval 事件
    Platform->>Platform: 监听并记录授权
    
    Note over User,Platform: 阶段2：付币
    Merchant->>Platform: 提交付币订单
    Platform->>Platform: 查询授权额度
    
    Platform->>Contract: 调用 payoutWithAllowance()
    Contract->>Token: transferFrom(用户, 收款人, 金额)
    Token->>Token: 验证授权额度
    Token-->>User: 扣款
    Token-->>Contract: 转账成功
    Contract-->>Platform: 触发事件
```

{% hint style="info" %}
**V5.8.0 特性**：授权模式下，不再强制要求白名单，任意地址都可以作为授权来源。
{% endhint %}

## 余额 vs 授权对比

| 对比项 | 余额模式 | 授权模式 |
|-------|---------|---------|
| 资金位置 | 合约余额 | 用户钱包余额 |
| 授权需要 | 无 | 需要预先授权 |
| 资金风险 | 资金在合约中 | 资金在用户钱包 |
| 灵活性 | 需预存 | 更灵活 |
| 推荐场景 | 高频固定金额 | 大规模、分散资金 |

## 支付方式选择规则

```mermaid
graph TB
    Start[开始付币] --> Check1{合约余额<br/>>>= 订单金额?}
    
    Check1 -->|是| Balance[✅ 余额支付]
    Check1 -->|否| Check2{授权额度<br/>>>= 订单金额?}
    
    Check2 -->|是| Allowance[✅ 授权支付]
    Check2 -->|否| Check3{余额 + 授权<br/>>>= 订单金额?}
    
    Check3 -->|是| Reject[❌ 拒绝<br/>不支持混合]
    Check3 -->|否| Fail[❌ 资金不足]
    
    style Check1 fill:#fff4e1
    style Check2 fill:#fff4e1
    style Check3 fill:#fff4e1
    style Reject fill:#ffe1e1
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
| **ColdWallet** | 冷钱包地址 | V5.8.0 可动态指定 |

## 异常处理

| 情况 | 处理方式 |
|------|---------|
| 余额不足 | 提示充值或切换授权模式 |
| 授权不足 | 提示增加授权额度 |
| 链上拥堵 | 等待确认，可查询链上状态 |
| 签名验证失败 | 检查签名地址和签名内容 |

## 下一步

- [余额模式详解 →](balance-mode.md)
- [授权模式详解 →](allowance-mode.md)
- [查看合约接口 →](contract.md)
