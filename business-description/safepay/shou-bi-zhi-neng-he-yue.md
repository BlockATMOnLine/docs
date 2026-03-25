# Collection Contract

Collection contract is divided into two types: Web3 collection contract, Scan2pay contract.

### Web3 collection contract

The Web3 collection contract provides a secure and reliable way to collect coins. Users can connect their wallets and authorize smart contracts to automatically execute transactions for payment. The encrypted currency paid by users is directly transferred to the Web3 collection contract, and only the "authorized signature address" specified in the contract has permission to withdraw funds, which can only be withdrawn to the "asset receiving address" specified in the contract.

{% hint style="info" %}
When creating a web3 collection contract, you need to specify the authorized signature address and asset receiving address. Once specified and the contract is successfully created, it cannot be modified to ensure the security of the contract assets.
{% endhint %}

### Scan2pay contract

The scan2pay contract provides a convenient and fast way to collect coin payments. Users can transfer payments by scanning the QR code of the receiving address with their mobile wallet. Only the "authorized signature address" specified in the contract has permission to withdraw coins, and can only be withdrawn to the "asset receiving address" specified in the contract.

{% hint style="info" %}
When creating a scan2pay contract, you need to specify the authorized signature address and asset receiving address. However, the asset receiving address of the scan code receiving coin contract must be a Web3 collection contract address, which means that the premise of creating a scan2pay contract is to first create a Web3 collection contract.
{% endhint %}

### Contract permissions

<table><thead><tr><th width="250.23046875">Address type</th><th width="349.3359375">Introduction</th><th width="281.12109375">permissions</th></tr></thead><tbody><tr><td>Owner Address</td><td>Address for creating collection contract</td><td>Management Collection Contract</td></tr><tr><td>Authorized Signature Address</td><td>The address with permission to withdraw contract assets</td><td>Extract contract assets</td></tr><tr><td>Asset Receiving Address</td><td>The address that has permission to receive contract assets.</td><td>Receive contract assets</td></tr></tbody></table>

### Collection contract code

{% tabs %}
{% tab title="Constructor Function" %}
```solidity
@param newFinanceList The list of financial personnel addresses, which cannot be modified after being written during contract deployment  
@notice This constructor is used to initialize the contract's key parameters and permission settings.  
@notice The withdrawal address list and financial address list cannot be empty, ensuring necessary permission configurations during contract initialization.  
@notice The fee gateway address is used to handle fee-related logic, ensuring transparency in system revenue and fee deductions.  
*/  
constructor(  
    bool safe,  
    uint256 id,  
    address[] memory newWithdrawList,  
    address[] memory newFinanceList,  
    address newFeeGateway  
) {  
    //Parameter safety check  
    ...  
    // Set withdrawal addresses  
    processList(newWithdrawList, withdrawMap);  
    withdrawList = newWithdrawList;  
    // Set financial addresses  
    processList(newFinanceList, financeMap);  
    financeList = newFinanceList;  
    // Other parameter settings  
    ....  
    // Set contract owner  
    owner = msg.sender;  
}  
```
{% endtab %}

{% tab title="Collection Function " %}
```solidity
/*
 * Function: deposit
 * Purpose: Allows users to transfer tokens to the payment contract and records the transaction.
 * @notice This function records deposit counts, which are used to calculate total fees.
 * @notice This function triggers a `Deposit` event to log transaction details.
 */
function deposit(
    address tokenAddress,
    uint256 amount,
    string calldata orderNo
) 
    public 
    checkTokenAddress(tokenAddress) 
    returns (bool) 
{
    // Required parameter and state checks
    ...

    // Execute ERC20 token transfer
    uint256 finalAmount = super.transferCommon(tokenAddress, address(this), amount);

    // Update deposit count for this token address (used for fee calculation)
    transferMap[tokenAddress] += 1;

    // Emit Deposit event to record transaction details
    emit Deposit(msg.sender, address(this), tokenAddress, finalAmount, orderNo);

    return true;
}
```
{% endtab %}

{% tab title="Withdraw Function" %}
```solidity
/**
 * Function: withdraw
 * Purpose: Allows merchants to batch withdraw assets from the contract
 * Restriction: onlyFinance - This function can only be called by finance roles, ensuring only authorized personnel can execute withdrawals.
 * @param recipientAddress The withdrawal address must be the merchant's fixed withdrawal address (immutable) to prevent funds from being sent to incorrect addresses.
 * @param withdrawRequests Array of withdrawal requests, supporting batch withdrawals of multiple token types simultaneously.
 * @notice Each withdrawal calculates fees and transfers them to the designated fee collection address.
 * @notice Emits a `Withdraw` event to record withdrawal details for auditing and tracking purposes.
 */
function withdraw(
    bool isSafeTransfer,
    Withdraw[] calldata withdrawRequests,
    address recipientAddress
) 
    public 
    onlyFinance 
    returns (bool) 
{
    // Restrict withdrawal address to only the pre-set fixed address
    require(recipientAddress == FIXED_WITHDRAW_ADDRESS, "Withdrawal address must be the immutable fixed address");

    // Additional withdrawal parameter safety checks
    ...

    // Process withdrawal requests in batch
    for (uint256 i = 0; i < requestCount;) {
        
        // Calculate withdrawal amount and fees
        ...
        // Execute fund transfer
        super.withdrawCommon(isSafeTransfer, request.tokenAddress, recipientAddress, userAmount);
        
        // Execute fee transfer
        super.withdrawCommon(isSafeTransfer, request.tokenAddress, feeCollector, fee);
    }

    // Emit Withdraw event for BlockATM event monitoring
    emit Withdraw(msg.sender, recipientAddress, withdrawRequests, feeAmounts, feeCollector);

    // Return true indicating successful withdrawal
    return true;
}
```
{% endtab %}
{% endtabs %}

***

### Historical contract version

### V2

April 17, 2025

* The main body of the independent cashier, decoupling the collection contract from the cashier, allowing enterprises to independently decide on the collection contract associated with the cashier.
* Change the rules of receiving fees: change from deducting from the deposit to collecting from the balance of the receiving contract, reducing business expansion costs.

### V1

July 10, 2023

* The collection contract specifies the authorized signature address and asset receiving address, only the authorized signature address has permission to withdraw contract assets to the asset receiving address.
