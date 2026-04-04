# Payout (Batch Payout)

Batch Payout is BlockATM's cryptocurrency batch payment solution, supporting balance payment and allowance payment two modes.

## Product Features

- ✅ **Batch Processing**: Support initiating multiple payouts at once
- ✅ **Dual Mode**: Balance payment + Allowance payment, flexible selection
- ✅ **Self-custody**: Funds completely under your control
- ✅ **Multi-chain Support**: TRON, Ethereum, Arbitrum
- ✅ **Transparent Fees**: Fixed fees, no hidden charges

## Payment Modes

### Balance Mode

Funds pre-deposited to contract, deducted from contract balance when paying out.

**Applicable Scenarios**:
- Daily high-frequency fixed-amount payouts (such as salary distribution)
- Merchant already has large funds in contract

### Allowance Mode — V5.8.0

User authorizes contract to manage their funds, no need to pre-deposit to contract.

**Applicable Scenarios**:
- Need to improve capital efficiency
- Don't want funds pre-deposited in contract
- Large-scale batch payouts

[Learn about allowance mode →](allowance-mode.md)

## Fees

| Type | Fee |
|------|------|
| Create contract | 200 USD/contract |
| Payout | 1 USD/order |

[View detailed fees →](fees.md)

## Quick Start

1. [Learn about payout principles →](how-it-works.md)
2. [Choose payment mode →](balance-mode.md)
3. [Integrate payout →](../../integration/guides/payout-guide.md)
