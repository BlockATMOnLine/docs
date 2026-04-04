# Collection Contract Interface

Collection contract core interface description.

## Contract Types

### Web3 Collection Contract

Supports user to complete payment via wallet connection.

### Scan2Pay Contract

Supports user to complete payment by scanning collection QR code. Scan2Pay contract must first associate with a Web3 collection contract.

## Core Interfaces

### deposit()

User deposits to contract.

```solidity
function deposit(
    address token,    // Token address
    uint256 amount,    // Deposit amount
    string calldata orderNo  // Order number
) external returns (bool)
```

| Parameter | Description |
|------|------|
| token | Token contract address (such as USDT) |
| amount | Deposit amount (must match order) |
| orderNo | Associated order number |

{% hint style="info" %}
**Auto Recording**: deposit() records deposit count for fee calculation and triggers Deposit event.
{% endhint %}

### withdraw()

Signer withdraws funds from contract.

```solidity
function withdraw(
    bool isSafeTransfer,
    Withdraw[] calldata withdrawRequests,
    address recipientAddress
) external onlyFinance returns (bool)
```

| Parameter | Description |
|------|------|
| isSafeTransfer | Whether to use safe transfer |
| withdrawRequests | Withdrawal request list (can be batched) |
| recipientAddress | Recipient address (must match preset) |

{% hint style="warning" %}
**Security Restriction**: recipientAddress must be the fixed address preset when contract was created, cannot be changed.
{% endhint %}

## Events

### Deposit Event

```solidity
event Deposit(
    address indexed user,       // User address
    address indexed contract,    // Contract address
    address indexed token,       // Token address
    uint256 amount,             // Deposit amount
    string orderNo              // Order number
);
```

### Withdraw Event

```solidity
event Withdraw(
    address indexed operator,    // Operator address
    address indexed recipient,   // Recipient address
    Withdraw[] requests,         // Withdrawal requests
    uint256[] fees,             // Fees
    address indexed feeCollector // Fee recipient
);
```

## Access Control

| Function | Permission Requirement |
|------|---------|
| deposit() | Anyone can call |
| withdraw() | Only Finance role (Signer) can call |

## Contract Version History

| Version | Date | Changes |
|------|------|------|
| V2 | 2025-04-17 | Decoupled cashier and contract, supports independent configuration |
| V1 | 2023-07-10 | Initial version, fixed signer address and recipient address |

## Next Steps

- [View fee description →](fees.md)
- [Integrate collection →](../../integration/guides/collect-guide.md)
