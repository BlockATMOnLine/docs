# Connect MetaMask to BlockATM

This guide explains how to connect MetaMask wallet to BlockATM admin dashboard.

## Prerequisites

- ✅ MetaMask wallet installed
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

### Step 3: Select MetaMask

1. Select "MetaMask" from wallet list
2. System will popup MetaMask connection request

### Step 4: Authorize Connection

1. MetaMask will popup confirmation window
2. Check if connected website is BlockATM
3. Click "Connect"
4. Select account to connect
5. Click "Next" → "Connect"

{% hint style="info" %}
**Note**: After first connection, it will auto-connect on next visit, no need to re-authorize.
{% endhint %}

### Step 5: Verify Successful Connection

After successful connection, you should see:
- Wallet address displayed in upper right corner of page
- Account balance information (if linked)
- Can perform collection/payout operations

## Switch Network

BlockATM supports multiple networks. You may need to switch between different networks:

### Switch to Ethereum

1. Click MetaMask network selector (top)
2. Select "Ethereum Mainnet"
3. Wait for network switch to complete

### Switch to Arbitrum

1. Click MetaMask network selector
2. If Arbitrum is not visible, manually add:
   - Network Name: Arbitrum One
   - RPC URL: https://arb1.arbitrum.io/rpc
   - Chain ID: 42161
   - Currency Symbol: ETH
   - Block Explorer: https://arbiscan.io

{% hint style="warning" %}
**Important**: TRON network requires TronLink wallet. MetaMask does not support TRON network.
{% endhint %}

## Disconnect

To disconnect wallet:

1. Click wallet address in upper right corner of BlockATM
2. Select "Disconnect"
3. Or remove BlockATM authorization in MetaMask

## FAQ

### What to do if connection fails?

**Possible Causes**:
- MetaMask not installed or not unlocked
- Browser extension disabled
- Network connection issue

**Solutions**:
1. Refresh page and retry
2. Check if MetaMask extension is enabled
3. Ensure MetaMask is unlocked (enter password)

### Can I connect multiple wallets?

Yes. You can connect different wallets for different networks or businesses.

### Can I see my assets after connecting?

BlockATM can only see information you authorize. It cannot control your assets. Your assets are always under your control.

## Security Tips

{% hint style="warning" %}
**Security Reminder**:
- Only connect wallet on official BlockATM website
- Regularly check connected DApps
- Disconnect when not in use
- Don't authorize unknown permission requests
{% endhint %}

## Next Steps

- [Create Collection Contract →](../../integration/guides/collect-guide.md)
- [Get Test Tokens →](../../getting-started/supported-networks.md)
- [Start Collection Integration →](../../integration/guides/collect-guide.md)
