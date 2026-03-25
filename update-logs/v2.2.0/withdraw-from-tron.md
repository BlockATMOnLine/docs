# Withdraw from Tron

### Steps to Operate via Block Explorer

#### 1. Access TronScan

1. Open the TronScan explorer: https://tronscan.org
2. Search for the contract address or navigate directly to the contract page
3. Switch to the "Contract" tab
4. Click the "Write Contract" button

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

#### 2. Connect Wallet

1. Click the "Connect to Web3" button
2. Select and connect your TronLink wallet
3. Ensure the connected wallet address has sufficient balance

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

#### 3. Call the withdrawByFinancial Function

1. Locate the `withdrawByFinancial` function in the contract function list
2. Click to expand this function
3. Fill in the parameters:
   * `safe`: Enter `false`
   * `tokenAddress`: Enter the token contract address. For example, the USDT contract address is: `TR7NHqjeKQxGTCi8q8ZY4pL8otSzgjLj6t`
   * `to`: Enter the destination address for the withdrawal
   * `amount`: Enter the withdrawal amount (**pay attention to the units**). USDT has 6 decimals. To withdraw 1 USDT, enter `1000000`. Please refer to the \[Amount Units Explanation]
4. Click the "Write" button
5. Confirm the transaction in your wallet

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

#### 4. Transaction Confirmation

1. Wait for the transaction to be packaged and confirmed (typically 1-3 seconds)
2. Check the transaction status on TronScan
3. Verify that the transaction was successful



### Important Notes

#### Address Format Verification

* Ensure the correct Base58 format address is used (starts with 'T')
* Verify address validity to avoid fund loss
* Distinguish between Mainnet and Testnet addresses

#### Amount Units Explanation

TRON network token amounts must be calculated in their smallest unit:

**Examples for Common Tokens:**

* **TRX (Native Token)**: 6 decimals
  * 1 TRX = 1,000,000 sun
  * 100 TRX = 100,000,000 sun
  * 0.5 TRX = 500,000 sun
* **USDT-TRC20**: 6 decimals
  * 1 USDT = 1,000,000 units
  * 100 USDT = 100,000,000 units
  * 0.5 USDT = 500,000 units
* **USDC-TRC20**: 6 decimals
  * 1 USDC = 1,000,000 units
  * 100 USDC = 100,000,000 units
