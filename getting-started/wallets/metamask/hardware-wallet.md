# MetaMask Hardware Wallet Integration

Combine Ledger or Trezor hardware wallet with MetaMask for the highest level of security.

## What is a Hardware Wallet?

Hardware wallet is a physical device for offline private key storage. Even if your computer is hacked, your assets remain safe.

| Brand | Price | Supported Tokens | Purchase Link |
|------|------|---------|---------|
| Ledger Nano S Plus | $79 | 5500+ | [ledger.com](https://www.ledger.com) |
| Ledger Nano X | $149 | 5500+ | [ledger.com](https://www.ledger.com) |
| Trezor Model T | $219 | 1000+ | [trezor.io](https://trezor.io) |

## Prerequisites

- ✅ Hardware wallet device (Ledger or Trezor)
- ✅ USB cable
- ✅ MetaMask browser extension
- ✅ Hardware wallet official software (Ledger Live / Trezor Suite)

## Step 1: Setup Hardware Wallet

### Ledger Setup

1. Purchase Ledger device from official channel
2. Download and install [Ledger Live](https://www.ledger.com/ledger-live)
3. Initialize device following screen prompts
4. Set PIN code
5. Backup recovery phrase (different from MetaMask, keep separately)
6. Install Ethereum app in Ledger Live

{% hint style="danger" %}
**Warning**: Never purchase hardware wallet from non-official channels. Second-hand devices may be tampered with.
{% endhint %}

### Trezor Setup

1. Purchase Trezor device from official channel
2. Visit [trezor.io/start](https://trezor.io/start)
3. Download and install Trezor Suite
4. Initialize device and set PIN code
5. Backup recovery phrase

## Step 2: Connect Hardware Wallet to MetaMask

### Connect Ledger

1. Connect Ledger to computer via USB
2. Enter PIN code to unlock device
3. Open Ethereum app on Ledger device
4. Open MetaMask
5. Click account name in upper right corner → "Add Account"
6. Select "Hardware Wallet"
7. Select "Ledger"
8. Select account to connect
9. Click "Connect"

### Connect Trezor

1. Connect Trezor to computer via USB
2. Open MetaMask
3. Click account name in upper right corner → "Add Account"
4. Select "Hardware Wallet"
5. Select "Trezor"
6. Follow prompts to connect device
7. Confirm connection on Trezor device
8. Select account to connect

## Step 3: Using on BlockATM

After connecting hardware wallet, using BlockATM is the same as with software wallet:

1. Visit BlockATM admin dashboard
2. Click "Connect Wallet"
3. Select MetaMask
4. MetaMask will prompt you to confirm connection
5. Confirm transaction on hardware device

{% hint style="info" %}
**Note**: Each transaction requires physical confirmation on hardware device, which adds security.
{% endhint %}

## Advantages and Limitations

### ✅ Advantages

- **Highest Security**: Private key never leaves device
- **Virus Proof**: Assets safe even if computer is infected
- **Physical Confirmation**: Each transaction requires physical button confirmation
- **Multi-account Support**: One device can manage multiple accounts

### ⚠️ Limitations

- **Cost**: Hardware wallet purchase required
- **Portability**: Device needs to be carried
- **Backup**: Need recovery phrase to recover if device is lost

## Security Best Practices

### During Purchase

- ✅ Only purchase from official website
- ✅ Check if packaging is complete
- ✅ Verify device anti-counterfeiting label
- ❌ Don't purchase second-hand devices

### During Use

- ✅ Always update firmware through official software
- ✅ Verify receiving address before use
- ✅ Regularly backup recovery phrase
- ❌ Don't digitally store recovery phrase

### During Storage

- ✅ Store in safe location (safe deposit box)
- ✅ Store separately from recovery phrase
- ✅ Consider fireproof waterproof storage
- ❌ Don't place in obvious location

## FAQ

### What to do if device is lost?

Use backed up recovery phrase to restore wallet on new device or in MetaMask. As long as recovery phrase is safe, assets are safe.

### Can I connect multiple devices?

Yes. You can connect multiple hardware wallet accounts in MetaMask.

### Do I need to keep device connected always?

No. Only connect device when signing transactions. Viewing balance doesn't require connection.

### Is firmware update safe?

Yes, but only update through official software (Ledger Live / Trezor Suite).

## Next Steps

- [Connect BlockATM →](connect-blockatm.md)
- [Create Collection Contract →](../../integration/guides/collect-guide.md)
- [Security Best Practices →](../../security/best-practices.md)
