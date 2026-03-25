# Permission Description

### There are three types of role addresses in the BlockATM platform: Owner  Address, Authorized Signer Address, and Operator Address

* Owner Address: The wallet that creates the smart contract serves as the admin, responsible for managing the smart contract and the cashier system.
* Authorized Signer Address: Set by the owner during smart contract creation, this address is responsible for managing contract assets, including withdrawals, deposits, and payouts.
* Operator Address: After creating a cashier, the owner can assign operator addresses to it. These addresses are responsible for managing cashier orders, including handling missing orders and resending Webhook notifications.

| Modules, Features / Roles  | Owner Address | Authorized Signer Address | Operator Address |
| -------------------------- | ------------- | ------------------------- | ---------------- |
| Wallet Account             | ✅             | ✅                         | ✅                |
|     Associated Wallet      | ✅             | ❌                         | ❌                |
|     Change Wallet          | ✅             | ✅                         | ✅                |
|     Disconnect             | ✅             | ✅                         | ✅                |
| Global Settings            | ✅             | ✅                         | ✅                |
| New User Guide             | ✅             | ❌                         | ❌                |
| Collection Contract        | ✅             | ✅                         | ❌                |
|     Create Contract        | ✅             | ❌                         | ❌                |
|     Delete Contract        | ✅             | ❌                         | ❌                |
|     Edit Contract          | ✅             | ❌                         | ❌                |
|     Contract List          | ✅             | ✅                         | ❌                |
| Scan2Pay  Contract         | ✅             | ✅                         | ❌                |
|     Create Contract        | ✅             | ❌                         | ❌                |
|     Delete Contract        | ✅             | ❌                         | ❌                |
|     Edit Contract          | ✅             | ❌                         | ❌                |
|     Contract List          | ✅             | ✅                         | ❌                |
| Cashier Desk               | ✅             | ❌                         | ✅                |
|     Create Cashier Desk    | ✅             | ❌                         | ❌                |
|     Delete Cashier Desk    | ✅             | ❌                         | ❌                |
|     Edit Cashier Desk      | ✅             | ❌                         | ❌                |
|     Enable Cashier Desk    | ✅             | ❌                         | ❌                |
|     Disable Cashier Desk   | ✅             | ❌                         | ❌                |
|     Test Cashier Desk      | ✅             | ❌                         | ❌                |
|     Integrate Cashier Desk | ✅             | ❌                         | ❌                |
|     Cashier Desk List      | ✅             | ❌                         | ✅                |
|     Collection Order       | ✅             | ❌                         | ✅                |
|     Complete Order         | ❌             | ❌                         | ✅                |
|     Resend Notification    | ❌             | ❌                         | ✅                |
|     Operator List          | ✅             | ❌                         | ❌                |
|     Add Operator           | ✅             | ❌                         | ❌                |
|     Remove Operator        | ✅             | ❌                         | ❌                |
|     Edit Operator Nickname | ✅             | ❌                         | ❌                |
| Payout Contract            | ✅             | ✅                         | ❌                |
|     Create Contract        | ✅             | ❌                         | ❌                |
|     Delete Contract        | ✅             | ❌                         | ❌                |
|     Edit Contract          | ✅             | ❌                         | ❌                |
|     Integrate Contract     | ✅             | ❌                         | ❌                |
|     Contract List          | ✅             | ✅                         | ❌                |
| Assets                     | ✅             | ✅                         | ❌                |
| Collection Contract Assets | ✅             | ✅                         | ❌                |
|     Collection Records     | ✅             | ✅                         | ❌                |
|     Withdrawal Records     | ✅             | ✅                         | ❌                |
|     Withdraw               | ❌             | ✅                         | ❌                |
| Scan2Pay Contract Assets   | ✅             | ✅                         | ❌                |
|     Collection Records     | ✅             | ✅                         | ❌                |
|     Withdrawal Records     | ✅             | ✅                         | ❌                |
|     Withdraw               | ❌             | ✅                         | ❌                |
| Payout Contract Assets     | ✅             | ✅                         | ❌                |
|     Payout Records         | ✅             | ✅                         | ❌                |
|     Resend Notification    | ❌             | ✅                         | ❌                |
|     Deposit Records        | ✅             | ✅                         | ❌                |
|     Payout                 | ❌             | ✅                         | ❌                |
|     Deposit                | ❌             | ✅                         | ❌                |
