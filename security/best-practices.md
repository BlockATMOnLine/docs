# Security Best Practices

This guide introduces security best practices when using BlockATM, helping you protect funds and API security.

## Table of Contents

1. [API Key Management](#api-key-管理)
2. [Signature Address Management](#签名地址管理)
3. [Withdrawal Address Security](#提币地址安全)
4. [Monitoring Alert Configuration](#监控告警配置)
5. [Emergency Response](#应急响应)

---

## API Key Management

### Risk Levels

| Operation | Risk Level | Impact |
|------|---------|------|
| API Key leaked | 🔴 High | Can create orders, query data |
| Secret Key leaked | 🔴 High | Can forge signatures, malicious calls |
| Webhook Key leaked | 🟡 Medium | Can forge Webhook notifications |

### Best Practices

#### 1. Secure Storage

✅ **Recommended Approach**:

```bash
# Use environment variables
export BLOCKATM_API_KEY="pck_xxxxxx"
export BLOCKATM_SECRET_KEY="sck_xxxxxx"

# Use secret management service
AWS Secrets Manager
Azure Key Vault
HashiCorp Vault
```

```javascript
// Node.js example
const apiKey = process.env.BLOCKATM_API_KEY;
const secretKey = process.env.BLOCKATM_SECRET_KEY;

// Do not hardcode in code
// ❌ Wrong example
const apiKey = "pck_xxxxxx"; // Do not do this!
```

❌ **Dangerous Approaches**:

- Commit Key to Git repository
- Expose Secret Key in frontend code
- Send Key via email/chat tools
- Write Key in documents

#### 2. Permission Separation

Create different API Keys for different environments:

| Environment | Purpose | Permission |
|------|------|------|
| Development | Local development testing | Test network only |
| Production | Online business | Full permission |
| Read-only Key | Data query | Query permission only |

#### 3. Regular Rotation

Recommended to rotate API Key every 90 days:

```bash
# Rotation process
1. Generate new Key in admin dashboard
2. Update application configuration
3. Test new Key
4. Disable old Key
5. Record rotation log
```

#### 4. Monitor Usage

Set up usage alerts:

```javascript
// Monitor API call frequency
const callCount = await getApiCallCount(last24Hours);
if (callCount > threshold) {
  sendAlert('API call anomaly', { callCount });
}

// Monitor failure rate
const failRate = await getFailRate(last1Hour);
if (failRate > 0.1) { // Over 10%
  sendAlert('API failure rate too high', { failRate });
}
```

---

## Signature Address Management

### What is Signature Address?

Signature address (Signer) is the address authorized to withdraw funds from smart contract. This is the most important security control point.

### Risk Levels

| Configuration | Risk Level | Consequence |
|------|---------|------|
| Using exchange address | 🔴 Extremely High | May not be able to withdraw |
| Using online wallet | 🔴 High | Private key may be leaked |
| Using hot wallet | 🟡 Medium | Internet risk |
| Using hardware wallet | 🟢 Low | Best choice |

### Best Practices

#### 1. Use Hardware Wallet

**Recommended Devices**:

| Device | Price | Security | Purchase Link |
|------|------|--------|---------|
| Ledger Nano X | $149 | ⭐⭐⭐⭐⭐ | [ledger.com](https://www.ledger.com) |
| Ledger Nano S Plus | $79 | ⭐⭐⭐⭐⭐ | [ledger.com](https://www.ledger.com) |
| Trezor Model T | $219 | ⭐⭐⭐⭐⭐ | [trezor.io](https://trezor.io) |

**Setup Steps**:

```
1. Purchase hardware wallet from official channel
2. Initialize device and backup recovery phrase
3. Connect hardware wallet in MetaMask
4. Copy hardware wallet address as signature address
5. Store hardware wallet in secure location
```

#### 2. Multi-sign Configuration (Advanced)

For large amounts, recommended to use multi-sign wallet:

| Configuration | Applicable Scenario | Security |
|------|---------|--------|
| 2/3 Multi-sign | Small/medium enterprise | ⭐⭐⭐⭐ |
| 3/5 Multi-sign | Large enterprise | ⭐⭐⭐⭐⭐ |
| 4/7 Multi-sign | Institutional level | ⭐⭐⭐⭐⭐ |

**Multi-sign Services**:
- [Gnosis Safe](https://safe.global) - EVM network
- [BitGo](https://www.bitgo.com) - Multi-chain support

#### 3. Address Verification

Before setting signature address:

```bash
# 1. Small amount test (recommended)
1. Set signature address
2. Create small amount collection order (such as 1 USDT)
3. Complete payment
4. Try to withdraw to signature address
5. After confirming arrival, formally use

# 2. Address format check
TRON address: starts with T, 34 characters
Ethereum address: starts with 0x, 42 characters
```

#### 4. Recovery Phrase Storage

✅ **Correct Approach**:
- Write down by hand
- Store in safe
- Can store in bank safe deposit box
- Consider fireproof waterproof storage

❌ **Wrong Approach**:
- Do not save screenshots
- Do not store in computer/phone
- Do not send via WeChat/email
- Do not tell anyone

---

## Withdrawal Address Security

### Whitelist Mechanism

Recommended to enable withdrawal address whitelist:

```
1. Add common withdrawal addresses in admin dashboard
2. Enable whitelist verification
3. Only whitelist addresses can withdraw
4. New addresses require review period (such as 24 hours)
```

### Address Verification

Verify address before each withdrawal:

```javascript
function validateWithdrawAddress(address, network) {
  // TRON address
  if (network === 'TRON') {
    if (!address.startsWith('T')) return false;
    if (address.length !== 34) return false;
  }

  // Ethereum address
  if (network === 'ETHEREUM' || network === 'ARBITRUM') {
    if (!address.startsWith('0x')) return false;
    if (address.length !== 42) return false;
  }

  return true;
}

// Manual verification for first withdrawal
if (isNewAddress(address)) {
  requireManualApproval();
}
```

### Withdrawal Limits

Set reasonable withdrawal limits:

| Scenario | Single Limit | Daily Limit | Review Requirement |
|------|---------|---------|---------|
| Small withdrawal | < 1000 USD | < 5000 USD | Auto approval |
| Medium withdrawal | 1000-10000 USD | < 50000 USD | Single person review |
| Large withdrawal | > 10000 USD | > 50000 USD | Dual review |

---

## Monitoring Alert Configuration

### Key Metrics

Monitor the following metrics and alert in time:

#### 1. Order Monitoring

```javascript
// Abnormal order alerts
const abnormalOrders = await getAbnormalOrders();
if (abnormalOrders.length > 0) {
  sendAlert('Abnormal orders found', {
    count: abnormalOrders.length,
    orders: abnormalOrders
  });
}

// Large order alerts
const largeOrders = await getOrders({ amount: { $gt: 10000 } });
if (largeOrders.length > 0) {
  sendAlert('Large order notification', { orders: largeOrders });
}
```

#### 2. Balance Monitoring

```javascript
// Contract balance insufficient alerts
const contractBalance = await getContractBalance();
const threshold = 1000; // USD

if (contractBalance < threshold) {
  sendAlert('Contract balance insufficient', {
    balance: contractBalance,
    threshold: threshold
  });
}
```

#### 3. Webhook Monitoring

```javascript
// Webhook failure alerts
const webhookFailures = await getWebhookFailures(last1Hour);
if (webhookFailures > 5) {
  sendAlert('Webhook consecutive failures', {
    count: webhookFailures
  });
}
```

### Alert Channels

Configure multiple alert channels:

| Channel | Priority | Response Time |
|------|-------|---------|
| Phone | 🔴 P0 | Immediately |
| SMS | 🔴 P0 | Within 1 minute |
| Email | 🟡 P1 | Within 15 minutes |
| Slack/DingTalk | 🟡 P1 | Within 30 minutes |

### Alert Levels

```javascript
// P0 - Critical alert (Phone + SMS)
- Fund anomaly
- Large theft risk
- System unavailable

// P1 - Important alert (Email + IM)
- API failure rate too high
- Webhook consecutive failures
- Contract balance insufficient

// P2 - General alert (Email)
- Single order anomaly
- Configuration change
```

---

## Emergency Response

### When Suspicious Activity is Discovered

#### 1. Immediately Stop Loss

```bash
# Step 1: Pause cashier
Admin dashboard → Cashier → Pause

# Step 2: Disable API Key
Admin dashboard → API Settings → Disable current Key

# Step 3: Transfer funds
If at risk, immediately transfer funds to secure address
```

#### 2. Investigate Cause

Collect the following information:

- [ ] Abnormal order list
- [ ] API call logs
- [ ] Webhook logs
- [ ] Blockchain transaction records
- [ ] Affected time range

#### 3. Contact BlockATM

```
Telegram: Passto_john
Email: john.feng@chixi88.com
Subject: 【Urgent】Security Incident Report - {Merchant ID}

Content:
1. Event description
2. Discovery time
3. Impact scope
4. Measures already taken
5. Assistance needed
```

### Key Leakage Emergency

#### Secret Key Leaked

```
1. Immediately disable leaked API Key
2. Generate new API Key
3. Update application configuration
4. Check for abnormal calls
5. Notify users (if user impact)
```

#### Recovery Phrase Leaked

```
1. Immediately transfer all funds to new address
2. Create new smart contract
3. Update all configurations
4. Notify relevant parties
```

---

## Security Checklist

### Pre-launch Check

- [ ] API Key stored in environment variables
- [ ] Secret Key not committed to code repository
- [ ] Signature address uses hardware wallet
- [ ] Recovery phrase backed up securely
- [ ] Withdrawal address whitelist enabled
- [ ] Monitoring alerts configured
- [ ] Small amount test completed
- [ ] Emergency response process written

### Regular Review

- [ ] Review API call logs monthly
- [ ] Rotate API Key quarterly
- [ ] Review security configuration semi-annually
- [ ] Conduct security audit annually

---

## Security Resources

### Learning Materials

- [Ethereum Security Best Practices](https://ethereum.org/zh/developers/docs/security/)
- [Smart Contract Security](https://github.com/slowmist/Knowledge-Base)
- [OWASP Cryptographic Storage](https://cheatsheetseries.owasp.org/cheatsheets/Cryptographic_Storage_Cheat_Sheet.html)

### Security Tools

| Tool | Purpose | Link |
|------|------|------|
| Ledger | Hardware wallet | [ledger.com](https://www.ledger.com) |
| Gnosis Safe | Multi-sign wallet | [safe.global](https://safe.global) |
| Revoke.cash | Revoke authorization | [revoke.cash](https://revoke.cash) |

---

## Next Steps

- [Contract audit report →](audit-report.md)
- [Self-custody description →](self-custody.md)
- [Exception handling guide →](../integration/guides/exception-handling.md)
