# Batch Payout

## 1.Product Overview

Crypto enterprises often face high demand for user withdrawals. Currently, the standard method involves manual wallet transfers, which are cumbersome, error-prone during batch processing, and often hindered by insufficient gas fees. To address this, the Batch Payout System provides a flexible and secure fund distribution solution. Built on a "Batch Contract Architecture," users can select the most suitable contract type based on their specific business scenario (e.g., high-frequency payroll or large-scale settlement). The system features a unified fee settlement mechanism, enabling users to complete batch payments without the need to hold native gas tokens directly.

## 2.Payout Contracts & Modes

Every batch payout process is executed via a dedicated payout contract. When deploying a payout contract, a Payout Mode must be selected. Once deployed, the mode is immutable.

#### 2.1 Deposit Mode (Contract Balance)

In this mode, funds are custodied by the payout contract.&#x20;

_Fund Flow: Wallet -> Contract -> Customer_

* Users must top up funds into the payout contract address in advance.
* The contract balance represents the available payout amount.
* Batch payouts are deducted directly from the contract balance.
* Suitable for: Daily high-frequency, fixed-amount payout scenarios, such as payroll and event rewards.

#### 2.2 Allowance Mode (Direct Payout)

In this mode, the payout contract is non-custodial (does not hold actual funds).&#x20;

_Fund Flow: Wallet -(Approve)-> Customer_

* The wallet address approves the contract to spend funds within a specified limit.
* Funds remain in the user's wallet (self-custody).
* The contract executes payouts only within the approved allowance scope.
* Suitable for: Business scenarios requiring high capital efficiency and security segregation, such as clearing, settlement, and vendor payments.

The system performs a dual check on liquidity:

1. Approved Allowance (on-chain limit)
2. Wallet Balance

The actual spendable amount is automatically calculated based on blockchain logic.

<figure><img src="../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>
