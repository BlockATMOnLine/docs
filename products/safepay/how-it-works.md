# Collection How It Works

Understand the complete workflow of Safepay collection.

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
    A["👤 User Wallet"] -->|"① Transfer"| B["📦 Collection Contract"]
    B -->|"② On-chain Custody"| C["💎 Contract Balance"]
    C -->|"③ Withdraw as Needed"| D["🏦 Merchant Wallet"]

    style A fill:#EEF2FF,stroke:#6366F1,color:#4338CA
    style B fill:#FEF3C7,stroke:#F59E0B,color:#92400E
    style C fill:#DBEAFE,stroke:#3B82F6,color:#1E40AF
    style D fill:#D1FAE5,stroke:#10B981,color:#065F46
```

## Web3 Collection Flow

### Complete Sequence Diagram

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

    participant U as 👤 User
    participant A as 📱 Merchant App
    participant C as 🏪 BlockATM Cashier
    participant S as ⚙️ Smart Contract
    participant T as 🪙 Token Contract
    participant B as ⛓️ Blockchain

    rect rgb(238, 242, 255)
        Note over U,C: Phase 1: Order Initialization
        A->>C: Get payment session
        C-->>A: Return cashier page
        A-->>U: Display cashier interface
        U->>C: Select network & token
        U->>C: Click pay
        C->>S: Generate on-chain order
    end

    rect rgb(219, 234, 254)
        Note over U,B: Phase 2: On-chain Execution
        C->>U: Invoke wallet signature
        U->>T: Authorize contract to manage tokens (if needed)
        U->>S: Call deposit()
        S->>T: transferFrom() transfer
        T-->>S: Transfer successful
        S->>B: Broadcast transaction
    end

    rect rgb(209, 250, 229)
        Note over U,A: Phase 3: Confirmation Complete
        B->>S: Block confirmation
        S-->>C: Deposit event triggered
        C-->>A: Webhook callback
        A-->>U: Payment success display
    end
```

### Key Steps Description

1. **Create Order**: Cashier generates on-chain order, records amount, token type, merchant info
2. **Wallet Authorization**: On first payment, user authorizes contract to manage their tokens
3. **On-chain Transfer**: After user signs and confirms, tokens atomically transferred from user wallet to contract
4. **Event Monitoring**: Contract triggers `Deposit` event, BlockATM on-chain monitoring service captures it
5. **Status Sync**: Order status updated, Webhook notifies merchant system

## Scan2Pay Flow

### Complete Sequence Diagram

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

    participant U as 👤 User
    participant A as 📱 Merchant App
    participant C as 🏪 BlockATM Cashier
    participant S as 📦 Collection Contract
    participant W as 👛 User Wallet App
    participant B as ⛓️ Blockchain

    rect rgb(238, 242, 255)
        Note over U,B: Phase 1: Order Generation
        A->>C: Request collection QR code
        C->>S: Generate temporary collection address
        S-->>C: Return collection info
        C-->>A: Return QR code + address
        A-->>U: Display collection code
    end

    rect rgb(254, 243, 199)
        Note over U,B: Phase 2: User Payment
        U->>W: Scan QR code
        W-->>U: Parse collection details
        U->>W: Confirm payment amount
        U->>W: Authorize transfer
        W->>S: Transfer to contract address
        S-->>W: Transfer successful
        W-->>U: Display success
    end

    rect rgb(219, 234, 254)
        Note over U,A: Phase 3: Order Matching
        S->>B: Transaction on-chain
        B->>S: Block confirmation
        S-->>C: Account entry event
        C-->>A: Order matching successful
        A-->>U: Payment complete
    end
```

{% hint style="warning" %}
**Important**: When using Scan2Pay, user must ensure payment amount exactly matches order amount, otherwise order may fail to automatically match.
{% endhint %}

## Contract Address Roles

| Role | Description | Setting Time |
|------|------|---------|
| **Owner** | Contract creator, can manage contract configuration | When creating contract |
| **Signer** | Address authorized to withdraw funds from contract | When creating contract |
| **Receiver** | Address where funds ultimately arrive | When creating contract |

## Exception Handling

| Situation | Handling Method |
|------|---------|
| User not authorized | Prompt user to complete authorization and retry |
| Blockchain congestion | Wait for confirmation, can query on-chain status |
| Amount mismatch | Scan2Pay order may fail to auto-match |
| Order timeout | Cashier displays timeout, user can re-initiate payment |

## Next Steps

- [View contract interface →](contract.md)
- [View fee description →](fees.md)
- [Integrate collection →](../../integration/guides/collect-guide.md)
