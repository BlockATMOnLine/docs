# Payout Contract Interface

Payout contract core interface description.

## Contract Types

| Contract | Network | Description |
|------|------|------|
| BlockATMEthPayout | Ethereum/Arbitrum | ETH-series token payout contract |
| BlockATMTronPayout | TRON | TRX/TRC20 token payout contract |

## Core Interfaces

### payoutWithBalance()

Use contract balance for payout.

```solidity
function payoutWithBalance(
    address token,           // Token address
    address to,              // Recipient address (must be in whitelist)
    uint256 amount,          // Payout amount
    uint256 fee,            // Handling fee
    bytes memory signature,  // Signature
    uint256 nonce,          // Nonce
    uint256 timestamp       // Timestamp
) external onlyPacker returns (bool)
```

{% hint style="info" %}
**Whitelist Requirement**: For balance payment method, recipient address must be in whitelist.
{% endhint %}

### payoutWithAllowance() — V5.8.0

Use authorization amount for payout.

```solidity
function payoutWithAllowance(
    address token,           // Token address
    address from,            // Authorization address (new in V5.8.0)
    address[] memory recipients,  // Recipient address array
    uint256[] memory amounts,     // Amount array
    uint256 totalFee,         // Total handling fee
    bytes memory signature,   // Signature
    uint256 nonce,          // Nonce
    uint256 timestamp       // Timestamp
) external onlyPacker returns (bool)
```

{% hint style="warning" %}
**V5.8.0 Changes**:
- Added `from` parameter, supports any authorization address
- Whitelist no longer mandatory
{% endhint %}

## Events

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
    address indexed from,        // Authorization address (new indexed)
    uint256 totalAmount,
    uint256 totalFee,
    uint256 recipientCount,
    uint256 nonce,
    uint256 timestamp,
    address indexed operator
);
```

## Access Control

| Function | Permission Requirement |
|------|---------|
| payoutWithBalance() | Only Packer role |
| payoutWithAllowance() | Only Packer role |

## V5.8.0 Contract Changes

| Change | Description |
|--------|------|
| Constructor | Removed `newColdWalletAddress` parameter |
| Method Signature | Added `from` parameter |
| Whitelist | Allowance payment whitelist not mandatory |

## Next Steps

- [View fee description →](fees.md)
- [Integrate payout →](../../integration/guides/payout-guide.md)
