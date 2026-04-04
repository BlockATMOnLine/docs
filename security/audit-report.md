# Contract Audit

BlockATM smart contracts have passed security audits.

## Audit Report

BlockATM smart contracts have been audited by a well-known security audit institution. The audit report can be downloaded from the admin dashboard or obtained by contacting technical support.

## Audit Scope

| Contract | Description |
|------|------|
| Collection Contract | Safepay collection contract |
| Payout Contract | Batch Payout payout contract |

## Security Features

### Access Control

- **Owner Permission**: Contract creator, has management permissions
- **Signer/Packer Permission**: Execute fund operations, requires signature verification
- **Whitelist Mechanism**: Recipients must be in whitelist

### Signature Verification

All contract operations require signature verification:
- HMAC-SHA256 signature algorithm
- Timestamp replay protection
- Nonce to prevent duplication

### Event Monitoring

- All key operations trigger events
- BlockATM server monitors on-chain events in real-time
- Automatic alerts for abnormal situations

## Security Suggestions

{% hint style="warning" %}
**Important Security Suggestions**:

1. **Use Hardware Wallet**: Recommended to use hardware wallet to manage Owner and Signer addresses
2. **Protect Keys**: Do not leak API Key and Secret Key
3. **Verify Addresses**: Verify recipient address is correct when withdrawing
4. **Small Amount Test**: Test with small amount first for first-time use
{% endhint %}

## Report Source

The original audit report file is located in project documentation:

```
docs/5.7.0/audit-report.md
```
