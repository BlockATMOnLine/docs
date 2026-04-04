# Connect TronLink to BlockATM

This guide explains how to connect TronLink wallet to BlockATM admin dashboard.

## Prerequisites

- ✅ TronLink wallet installed
- ✅ Wallet created and recovery phrase backed up
- ✅ BlockATM admin dashboard account

## Connection Steps

### Step 1: Login to Admin Dashboard

1. Visit BlockATM admin dashboard
   - Production: [app.blockatm.net](https://app.blockatm.net)
   - Test: [backstage-b2b-pre.ufcfan.org](https://backstage-b2b-pre.ufcfan.org)
2. Login with account password

### Step 2: Go to Wallet Management

1. After login, click user avatar in upper right corner
2. Select "Wallet Management" or "Account Settings"
3. Find "Connect Wallet" option

### Step 3: Select TronLink

1. Select "TronLink" from wallet list
2. TronLink will popup connection request window

{% hint style="info" %}
**Note**: If TronLink doesn't popup, check:
- TronLink extension installed
- TronLink unlocked (enter password)
- Browser allows popup windows
{% endhint %}

### Step 4: Authorize Connection

1. TronLink will show connection request
2. Check if connected website is BlockATM
3. Click "Connect"
4. Select account to connect
5. Click "Confirm"

### Step 5: Verify Successful Connection

After successful connection, you should see:
- Wallet address displayed in upper right corner (T-address)
- TRX and TRC20 token balances
- Can perform collection/payout operations

## Switch Network

TronLink supports TRON mainnet and testnet:

### Switch to TRON Mainnet

1. Click TronLink network selector at top
2. Select "Mainnet"
3. Wait for network switch to complete

### Switch to Nile Testnet

1. Click TronLink network selector at top
2. Select "Nile"
3. Conduct testing operations in testnet

{% hint style="warning" %}
**Important**: Use Mainnet for production, use Nile for test environment. Don't mix them up!
{% endhint %}

## Disconnect

To disconnect wallet:

1. Click wallet address in upper right corner of BlockATM
2. Select "Disconnect"
3. Or remove BlockATM authorization in TronLink

## FAQ

### What to do if connection fails?

**Possible Causes**:
- TronLink not installed or not unlocked
- Browser extension disabled
- Network connection issue

**Solutions**:
1. Refresh page and retry
2. Check if TronLink extension is enabled
3. Ensure TronLink is unlocked

### Prompt "Please switch to correct network"?

BlockATM will detect current network. If network doesn't match, please switch to correct network as prompted.

### Can I connect multiple wallets?

Yes. You can connect different wallets for different networks or businesses.

## Security Tips

{% hint style="warning" %}
**Security Reminder**:
- Only connect wallet on official BlockATM website
- Regularly check connected DApps
- Disconnect when not in use
- Don't authorize unknown permission requests
{% endhint %}

## TRON Network Characteristics

Understanding TRON network characteristics helps use BlockATM better:

### Fee Structure

| Operation | Fee | Description |
|------|------|------|
| TRX Transfer | ~0.1 TRX | Consumes bandwidth |
| TRC20 Transfer | ~1-5 USDT | Consumes energy |
| Contract Interaction | Variable | Depends on complexity |

### How to Reduce Fees

1. **Hold TRX**: Account holding TRX gets bandwidth
2. **Resource Rental**: Rent energy from market
3. **Batch Operations**: Combine multiple transactions

## Next Steps

- [Create Collection Contract →](../../integration/guides/collect-guide.md)
- [Get Test TRX →](../../getting-started/supported-networks.md)
- [Start Collection Integration →](../../integration/guides/collect-guide.md)
