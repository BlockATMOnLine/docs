# 付币合约接口

付币合约的核心接口说明。

## 合约类型

| 合约 | 网络 | 说明 |
|------|------|------|
| BlockATMEthPayout | Ethereum/Arbitrum | ETH 系代币付币合约 |
| BlockATMTronPayout | TRON | TRX/TRC20 代币付币合约 |

## 核心接口

### payoutWithBalance()

使用合约余额进行付币。

```solidity
function payoutWithBalance(
    address token,           // 代币地址
    address to,              // 收款地址（需在白名单）
    uint256 amount,          // 付币金额
    uint256 fee,            // 手续费
    bytes memory signature,  // 签名
    uint256 nonce,          // 随机数
    uint256 timestamp       // 时间戳
) external onlyPacker returns (bool)
```

{% hint style="info" %}
**白名单要求**：余额支付方式，收款地址必须在白名单中。
{% endhint %}

### payoutWithAllowance() — V5.8.0

使用授权额度进行付币。

```solidity
function payoutWithAllowance(
    address token,           // 代币地址
    address from,            // 授权地址（V5.8.0 新增）
    address[] memory recipients,  // 收款地址数组
    uint256[] memory amounts,     // 金额数组
    uint256 totalFee,         // 总手续费
    bytes memory signature,   // 签名
    uint256 nonce,          // 随机数
    uint256 timestamp       // 时间戳
) external onlyPacker returns (bool)
```

{% hint style="warning" %}
**V5.8.0 变更**：
- 新增 `from` 参数，支持任意授权地址
- 不再强制要求白名单
{% endhint %}

## 事件

### BatchPayoutWithBalance

```solidity
event BatchPayoutWithBalance(
    address indexed token,
    uint256 totalAmount,
    uint256 totalFee,
    uint256 recipientCount,
    uint256 nonce,
    uint256 timestamp,
    address indexed operator
);
```

### BatchPayoutWithAllowance — V5.8.0

```solidity
event BatchPayoutWithAllowance(
    address indexed token,
    address indexed from,        // 授权地址（新增 indexed）
    uint256 totalAmount,
    uint256 totalFee,
    uint256 recipientCount,
    uint256 nonce,
    uint256 timestamp,
    address indexed operator
);
```

## 权限控制

| 函数 | 权限要求 |
|------|---------|
| payoutWithBalance() | 仅 Packer 角色 |
| payoutWithAllowance() | 仅 Packer 角色 |

## V5.8.0 合约变更

| 变更项 | 说明 |
|--------|------|
| 构造函数 | 移除 `newColdWalletAddress` 参数 |
| 方法签名 | 增加 `from` 参数 |
| 白名单 | 授权支付不强制白名单 |

## 下一步

- [查看费用说明 →](fees.md)
- [集成付币 →](../../integration/guides/payout-guide.md)
