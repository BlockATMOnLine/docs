# Payout How It Works

Understand the complete workflow of Batch Payout.

## Fund Flow

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
    A["🏦 Merchant Wallet"] -->|"① Deposit"| B["📦 Payout Contract"]
    B -->|"② On-chain Custody"| C["💎 Contract Balance"]
    C -->|"③ Batch Distribution"| D["👤 User Wallet"]

    style A fill:#D1FAE5,stroke:#10B981,color:#065F46
    style B fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style C fill:#DBEAFE,stroke:#3B82F6,color:#1E40AF
    style D fill:#EEF2FF,stroke:#6366F1,color:#4338CA
```

## Balance Mode Flow

### Complete Sequence Diagram

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

    participant M as 🏦 Merchant
    participant P as 🔷 BlockATM Platform
    participant C as 📦 Payout Contract
    participant T as 🪙 Token Contract
    participant U as 👤 User Wallet

    rect rgb(209, 250, 229)
        Note over M,U: Phase 1: Fund Deposit
        M->>P: Submit deposit request
        P->>T: Transfer to contract address
        T-->>P: Deposit successful
        Note over C: Funds arrived at contract
    end

    rect rgb(238, 242, 255)
        Note over M,U: Phase 2: Payout Execution
        M->>P: Submit payout order
        P->>P: Review & create order
        M->>P: Trigger execution
        P->>C: Call payoutWithBalance()
        C->>T: transfer() transfer
        T-->>U: Arrived at user wallet
        T-->>C: Deduct handling fee
    end

    rect rgb(219, 234, 254)
        Note over M,U: Phase 3: Confirmation Complete
        C-->>P: Payout event
        P-->>M: Webhook callback
    end
```

## Allowance Mode Flow

### Complete Sequence Diagram

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

    participant U as 👤 Authorized User
    participant T as 🪙 Token Contract
    participant P as 🔷 BlockATM Platform
    participant C as 📦 Payout Contract
    participant M as 🏦 Merchant

    rect rgb(254, 243, 199)
        Note over U,P: Phase 1: User Authorization
        U->>T: approve(contract address, amount)
        T-->>U: Authorization successful
        T-->>P: Approval event
        P->>P: Monitor and record authorization
    end

    rect rgb(238, 242, 255)
        Note over U,P: Phase 2: Payout Execution
        M->>P: Submit payout order
        P->>P: Verify authorization amount
        P->>C: Call payoutWithAllowance()
        C->>T: transferFrom(user → recipient)
        T->>T: Verify authorization & deduct
        T-->>C: Transfer successful
    end

    rect rgb(209, 250, 229)
        Note over U,M: Phase 3: Confirmation Complete
        C-->>P: Payout event
        P-->>M: Webhook callback
    end
```

{% hint style="info" %}
**V5.8.0 Feature**: In allowance mode, whitelist is no longer mandatory, any address can be used as authorization source.
{% endhint %}

## Balance vs Allowance Comparison

| Dimension | Balance Mode | Allowance Mode |
|:---|:---|:---|
| **Fund Location** | Contract balance | User wallet balance |
| **Authorization Required** | None | Pre-authorization required |
| **Fund Risk** | Funds locked in contract | Funds under user control |
| **Flexibility** | Need to pre-deposit funds | More flexible and immediate |
| **Recommended Scenario** | High-frequency fixed-amount payouts | Large-scale scattered fund payouts |

## Payment Method Selection Rules

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
    START[🚀 Start Payout] --> Q1{"Contract Balance<br/>≥ Order Amount?"}

    Q1 -->|Yes| A["✅ Balance Payment"]
    Q1 -->|No| Q2{"Allowance Amount<br/>≥ Order Amount?"}

    Q2 -->|Yes| B["✅ Allowance Payment"]
    Q2 -->|No| Q3{"Balance + Allowance<br/>≥ Order Amount?"}

    Q3 -->|Yes| C["❌ Mixed Not Supported"]
    Q3 -->|No| D["❌ Insufficient Funds"]

    style Q1 fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style Q2 fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style Q3 fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style A fill:#D1FAE5,stroke:#10B981,color:#065F46
    style B fill:#D1FAE5,stroke:#10B981,color:#065F46
    style C fill:#FEE2E2,stroke:#EF4444,color:#991B1B
    style D fill:#FEE2E2,stroke:#EF4444,color:#991B1B
```

{% hint style="warning" %}
**Important**: Balance payment and allowance payment are ** mutually exclusive**, mixed payment not supported.
{% endhint %}

## Contract Roles

| Role | Description | Permissions |
|------|------|------|
| **Owner** | Contract creator | Manage contract configuration |
| **Packer** | Packing executor | Execute payout transactions |
| **Finance** | Finance address | Authorized to withdraw funds from contract |
| **ColdWallet** | Cold wallet address | V5.8.0 can be dynamically specified |

## Exception Handling

| Situation | Handling Method |
|------|---------|
| Insufficient balance | Prompt to deposit or switch to allowance mode |
| Insufficient authorization | Prompt to increase authorization amount |
| Chain congestion | Wait for confirmation, can query on-chain status |
| Signature verification failed | Check signature address and signature content |

## Next Steps

- [Balance mode details →](balance-mode.md)
- [Allowance mode details →](allowance-mode.md)
- [View contract interface →](contract.md)
