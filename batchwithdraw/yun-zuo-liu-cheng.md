# Operation Process

BlockATM payout process consists of multiple stages, incorporating risk control and manual intervention mechanisms at critical checkpoints.

### **1. Initialization: Deployment & Configuration**

Before using the batch payout function, the following configurations must be completed:

* Deploy Payout Contract: Create an exclusive payout contract.
* Select Payout Mode: Choose between Deposit Mode (Contract Balance) or Allowance Mode during deployment.
* Set Signer Address: Configure the payout signer address (Approval Mode requires specifying the Approval Wallet Address).
* Immutable Settings: The Payout Mode and Signer Address are fixed upon deployment and cannot be changed.

Once these steps are complete, the payout contract enters the Operational State.

### **2. Runtime: Order Processing Flow**

1. Order Creation
2. Whitelist & Risk Check
3. Large Amount Verification (If applicable)
4. Financial Signing
5. Time-lock & Final Control
6. On-chain Batch Execution & Settlement

### **3. Payout Order Mechanism**

#### **3.1 Order Creation**&#x20;

Users can create batch payout orders via Forms, File Uploads, or API. Order details include:

* Recipient Address
* Payout Amount
* Token Information
* Business Order ID and Memo (based on requirements)

The system automatically aggregates amounts and generates a corresponding payout order. A single order supports batch execution for up to 100 addresses.

### **4. Whitelist & Risk Control**

#### **4.1 Whitelist Mechanism**&#x20;

BlockATM provides an address whitelist function to restrict the scope of eligible recipient addresses. Based on the Contract Owner's configuration and order conditions, the risk control flow includes:

* Auto-pass: Addresses in the whitelist may pass risk checks automatically.
* Manual Review: Non-whitelisted addresses require a manual review process.
* First-time Transfer: First-time interactions may mandatorily require manual review.

This mechanism minimizes the risk of mispayments and abnormal addresses. Orders that fail review will be blocked from the subsequent payout process.

### **5. Large Amount Verification**

The system supports a verification process specifically for large-value payout orders.

#### **5.1 Micro-payment Verification**&#x20;

When the large amount rule is triggered:

* The original order is split into a Micro-test Order and the corresponding Large Order.
* The Micro-test Order is executed first to verify the address and process validity.

#### **5.2 Large Amount Unlock Only**&#x20;

After the Micro-test Order is successfully executed on-chain:

* The corresponding Large Order enters the Executable State.
* This prevents exposing large funds to immediate risk.

### **6. Signing, Delay & Execution Control**

#### **6.1 Financial Signing**&#x20;

Orders that pass risk control and verification are pushed to the designated Finance Wallet for signing.

* Signing signifies approval to execute the payout.
* Unsigned orders cannot be broadcast on-chain.

#### **6.2 Time-lock & Final Control**&#x20;

Payouts are not necessarily executed immediately after signing.

* Delayed Execution: The Contract Owner can configure a delay period (Time-lock).
* Intervention: During the countdown, administrators can Cancel or Accelerate the execution.
* This ensures the ability for manual intervention up until the final moment.

{% hint style="info" %}
The ownership of the payout contract belongs to the merchant/business, while the ownership of the payout proxy contract belongs to BlockATM. The payout proxy contract primarily provides API-based secure order uploads and Gas Fee payment on behalf of the merchant to improve payout efficiency. For more details, see:[Payout Contract](fu-bi-zhi-neng-he-yue.md)
{% endhint %}

