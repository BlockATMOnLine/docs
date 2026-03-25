# Withdraw from  Ethereum

#### 1. Accessing the Contract Page

1. Open the corresponding block explorer (e.g., Etherscan, BSCScan, etc.): https://etherscan.io/
2. Search for the contract address or navigate directly to the contract page
3. Switch to the "Contract" tab
4. Click the "Write Contract" button

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

#### 2. Connecting Your Wallet

1. Click the "Connect to Web3" button
2. Select and connect your wallet (e.g., MetaMask)
3. Ensure the connected wallet address belongs to the financial operator

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

#### 3. Calling the withdrawByFinancial Function

1. Locate the `withdrawByFinancial` function in the contract function list
2. Click to expand this function
3. Fill in the parameters:
   * `safe`: Enter `true`
   * `tokenAddress`: Enter the token contract address. For example, the USDT contract address is: `0xdAC17F958D2ee523a2206206994597C13D831ec7`
   * `to`: Enter the destination address for the withdrawal
   * `amount`: Enter the withdrawal amount (**pay attention to the units**). USDT has 6 decimals. To withdraw 1 USDT, enter `1000000`. Please refer to the \[Amount Units Explanation]
4. Click the "Write" button
5. Confirm the transaction in your wallet

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

#### 4. Transaction Confirmation

1. Wait for the transaction to be packaged and confirmed
2. Check the transaction status on the block explorer
3. Verify that a `FinancialWithdraw` event was emitted

#### Amount Units Explanation

Token amounts must be calculated in their smallest unit, as different tokens have different numbers of decimal places.

**Examples for Common Tokens:**

* **USDT (Tether)**: 6 decimals
  * 1 USDT = 1,000,000 units
  * 100 USDT = 100,000,000 units
  * 0.5 USDT = 500,000 units
* **USDC (USD Coin)**: 6 decimals
  * 1 USDC = 1,000,000 units
  * 100 USDC = 100,000,000 units
* **DAI**: 18 decimals
  * 1 DAI = 1,000,000,000,000,000,000 units (1e18)
  * 100 DAI = 100,000,000,000,000,000,000 units (100e18)
