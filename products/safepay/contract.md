# 收币合约接口

收币合约的核心接口说明。

## 合约类型

### Web3 收款合约

支持用户通过钱包连接方式完成支付。

### Scan2Pay 合约

支持用户通过扫描收款二维码完成支付。Scan2Pay 合约必须先关联一个 Web3 收款合约。

## 核心接口

### deposit()

用户向合约存款。

```solidity
function deposit(
    address token,    // 代币地址
    uint256 amount,    // 存款金额
    string calldata orderNo  // 订单号
) external returns (bool)
```

| 参数 | 说明 |
|------|------|
| token | 代币合约地址（如 USDT） |
| amount | 存款金额（需与订单一致） |
| orderNo | 关联的订单号 |

{% hint style="info" %}
**自动记录**：deposit() 会记录存款次数用于费用计算，并触发 Deposit 事件。
{% endhint %}

### withdraw()

签名者从合约提取资金。

```solidity
function withdraw(
    bool isSafeTransfer,
    Withdraw[] calldata withdrawRequests,
    address recipientAddress
) external onlyFinance returns (bool)
```

| 参数 | 说明 |
|------|------|
| isSafeTransfer | 是否使用安全转账 |
| withdrawRequests | 提款请求列表（可批量） |
| recipientAddress | 收款地址（必须与预设一致） |

{% hint style="warning" %}
**安全限制**：recipientAddress 必须是合约创建时预设的固定地址，不可更改。
{% endhint %}

## 事件

### Deposit 事件

```solidity
event Deposit(
    address indexed user,       // 用户地址
    address indexed contract,    // 合约地址
    address indexed token,       // 代币地址
    uint256 amount,             // 存款金额
    string orderNo              // 订单号
);
```

### Withdraw 事件

```solidity
event Withdraw(
    address indexed operator,    // 操作者地址
    address indexed recipient,   // 收款地址
    Withdraw[] requests,         // 提款请求
    uint256[] fees,             // 手续费
    address indexed feeCollector // 手续费收款方
);
```

## 权限控制

| 函数 | 权限要求 |
|------|---------|
| deposit() | 任何人可调用 |
| withdraw() | 仅 Finance 角色（Signer）可调用 |

## 合约版本历史

| 版本 | 日期 | 变更 |
|------|------|------|
| V2 | 2025-04-17 | 解耦收银台与合约，支持独立配置 |
| V1 | 2023-07-10 | 初始版本，固定签名地址和收款地址 |

## 下一步

- [查看费用说明 →](fees.md)
- [集成收币 →](../../integration/guides/collect-guide.md)
