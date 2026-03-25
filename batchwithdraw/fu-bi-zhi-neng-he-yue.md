# Payout Contract

The payout contract is used for bulk payouts, with the payout request initiated and signed by the "Authorized Signature Address" specified in the contract. The payout operation is then executed by the BlockATM payout proxy contract.

### Contract Permissions Explanation

<table><thead><tr><th width="131.3125">Role</th><th>Address Type</th><th width="244.0625">Description</th><th>Key Permissions</th></tr></thead><tbody><tr><td><p>Owner</p><p></p></td><td>Admin Wallet</td><td>Creator and highest authority holder of the payout contract. Responsible for initialization and risk configuration.</td><td><p>• Deploy Payout Contracts</p><p></p><p>• Configure Risk Rules (Whitelist, Limits, Time-locks)</p></td></tr><tr><td>Finance</td><td>Signer Address</td><td>Responsible for signing on-chain transactions for audited orders to ensure fund security.</td><td><p>• Sign and Broadcast Transactions</p><p></p><p>• Maintain Whitelist (requires Admin approval)</p></td></tr><tr><td>Operator</td><td>Operator Wallet</td><td>Daily operations staff responsible for creating and initially reviewing orders.</td><td><p>• Create Payout Orders</p><p></p><p>• Audit Orders (requires Admin approval)</p></td></tr><tr><td>Approval Wallet</td><td>Funding Address</td><td>Used in Approval Mode. The wallet bound to the contract that grants spending limits (Approval).</td><td>• Set and Adjust Contract Approval Limits</td></tr></tbody></table>



{% hint style="warning" %}
#### **When creating a payout contract, a "Payout Signer Address" must be specified. Once specified and the contract is deployed, this address becomes immutable (cannot be changed) to guarantee the security of contract assets.**
{% endhint %}

### Payout Smart Contract Code

{% tabs %}
{% tab title="Constructor Function" %}
```solidity
/**
* Function: payout constructor
* Purpose: Initializes merchant contract with critical parameters including finance addresses and proxy address during deployment.
* @param newFinanceList List of finance addresses for initializing financial permissions
* @param newProxyPayoutAddress Proxy address that has exclusive batch payout execution rights
**/
constructor(
    bool safe,
    uint256 id,
    address[] memory newFinanceList,
    address newProxyPayoutAddress,
    address newFeeGateway
) {
    // Parameter safety checks
    ...
    
    // Set proxy contract address
    proxyPayoutAddress = newProxyPayoutAddress;

    // Initialize finance addresses
    processList(newFinanceList, financeMap);
    financeList = newFinanceList;

    // Set contract owner
    owner = msg.sender;
    
    // Other initialization parameters
    ...
}
```
{% endtab %}

{% tab title="Payout Function" %}
```solidity
/**
* Function: payoutByContract
* Purpose: Handles batch payment operations
* Restriction: onlyFinancials(payoutAddress) Ensures only BlockATM-authorized financial addresses or proxy contracts can call this function
* @param orderNo Array of batch payment order numbers, uploaded by merchant finance via API or Excel
* @param array Array of recipient addresses, uploaded by merchant finance via API or Excel
* @param amount Array of payment amounts, uploaded by merchant finance via API or Excel
* @return bool Returns true indicating successful payment
**/
function payoutByContract(
    bool safe, 
    address tokenAddress, 
    uint256 total, 
    address payoutAddress, 
    string[] calldata orderNo, 
    address[] calldata array, 
    uint256[] calldata amount
) public onlyFinancials(payoutAddress) returns (bool) {
    // Calls internal payoutToken function to execute payment
    payoutToken(safe, payoutAddress, tokenAddress, total, 0, 1, orderNo, array, amount, 0);
    // Returns true indicating successful payment
    return true;
}

/**
* Function: payoutToken
* Purpose: Executes the payment process flow
*/
function payoutToken(
    bool safe, 
    address from, 
    address tokenAddress, 
    uint256 total, 
    uint256 gasAmount, 
    uint256 payType, 
    string[] calldata orderNo, 
    address[] calldata array, 
    uint256[] calldata amount, 
    uint256 id
) internal {
    // Parameter safety checks
    ...
   
    // Executes batch token transfers
    _transferTokens(safe, from, tokenAddress, total, gasAmount, payType, feeAmount);
    _processBatchPayments(safe, tokenAddress, array, amount, length);
    
    // Calculates and deducts processing fees
    super.withdrawCommon(safe, tokenAddress, IBlockFee(feeGateway).feeAddress(), feeAmount + gasAmount);
    
    // Emits payment completion event for BlockATM payment monitoring
    emit PayoutToken(from, tokenAddress, payType, feeAmount, gasAmount, orderNo, array, amount, msg.sender, id);
}
```
{% endtab %}
{% endtabs %}

***

### Token Approval&#x20;

Approval Management is a dedicated portal provided by BlockATM for Approval Payout Contracts. It is deeply integrated with BlockATM's payout workflow and risk control mechanisms.

By integrating the "Approve" action with actual payout operations, BlockATM ensures operational continuity while maintaining on-chain security, avoiding risks associated with disconnected workflows.

<figure><img src="../.gitbook/assets/image (46).png" alt=""><figcaption></figcaption></figure>

#### Why not use generic third-party approval tools?

Compared to generic approval tools (like block explorers), BlockATM's Approval Management offers additional security designs tailored for payout scenarios:

* **Environment Security Check** The system detects the security of the user's current operating environment to minimize the possibility of granting approvals in risky environments.
* **Identity Verification** Only the **Funding Wallet** bound during the contract creation can pass verification to access the management page. This prevents unauthorized or incorrect addresses from manipulating critical fund permissions.
* **Smart Approval Suggestions** The system calculates the required approval amount based on pending and created orders, providing **suggested approval limits.** This prevents risks associated with over-approval (infinite approval) or insufficient funds.

***

### Historical Contract Versions

### V3&#x20;

January 13, 2026

* Dual-Mode Architecture: Introduced support for running both Balance Mode and Approval Mode in parallel.
* Approval Payout Contract: Enables batch payouts by directly accessing wallet funds within a limited approval scope.
* Approval Management Portal: Unified display and management of Approved Limits, Available Limits, and Pending Payout Amounts.

### V2

April 17, 2025

* Upgrade to the Web3 self-hosted framework payout contract.
* Provides two methods for uploading payout orders: automated upload via API and manual upload via Excel.

### V1

October 22, 2023

* Implement self-service withdrawals based on the payout client.
* Self-service contract interaction based on Web3 SDK.





