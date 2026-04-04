# Supported Networks

BlockATM currently supports mainstream stablecoins on three mainstream blockchain networks (Ethereum, Arbitrum, Tron). Note that Tron is incompatible with the other two networks. Enterprise users need to associate Ethereum/Arbitrum wallet addresses with Tron wallet addresses to manage smart contracts and payment gateways on different networks.

## Network and Token Overview

| Network | ChainId | Supported Tokens |
|------|---------|-----------|
| **Ethereum Mainnet** | 1 | DAI, USDT, USDC, TUSD, FRAX, WETH, WBTC, cbBTC |
| **TRON Mainnet** | 0x2b6653dc | USDT, USDJ, USDD |
| **Arbitrum Mainnet** | 42161 | DAI, USDT, USDC, TUSD, FRAX, WETH, WBTC |
| **Ethereum Sepolia Testnet** | 11155111 | DAI, USDT, USDC, TUSD, FRAX, WETH, WBTC, cbBTC |
| **TRON Shasta Testnet** | 0x94a9059e | USDT, USDJ, USDD, WETH, WBTC |
| **Arbitrum Sepolia Testnet** | 421614 | DAI, USDT, USDC, TUSD, FRAX |

{% hint style="info" %}
We are continuously adding support for more networks and tokens...
{% endhint %}

## Getting Test Tokens

When developing in the test environment, you need to get testnet tokens.

### Ethereum Sepolia Testnet

BlockATM uses Sepolia as the Ethereum test network. If ETH for transaction fees is insufficient, get test ETH through:

- [pk910.de Sepolia Faucet](https://sepolia-faucet.pk910.de/) (Recommended, no registration required)
- [Alchemy Sepolia Faucet](https://www.alchemy.com/faucets/sepolia)
- [QuickNode Sepolia Faucet](https://quicknode.com/faucet/sepolia)

### TRON Shasta Testnet

BlockATM uses Shasta as the TRON test network. If TRX for transaction fees is insufficient:

1. Join official Tron [Discord community](https://discord.gg/SCcs9uAc)
2. Send `!shasta <your wallet address>`
3. System will automatically send test TRX

### Arbitrum Sepolia Testnet

As an Ethereum Layer 2 network, Arbitrum can transfer assets from Ethereum Sepolia to Arbitrum Sepolia via the [official bridge](https://bridge.arbitrum.io/?destinationChain=arbitrum-sepolia&sourceChain=sepolia).

### Optimism Sepolia Testnet

BlockATM also supports Optimism Sepolia testnet. After getting test tokens, you can get them through bridge or other methods.

## Test Token Contract Addresses

You can get all test token assets required by BlockATM by transferring a small amount of native tokens (such as 0.0001 ETH) to the following addresses.

| Network | Faucet Address |
|------|-----------|
| Ethereum Sepolia Testnet | `0xd611c098e26b2B6095F22F4bFfEB1Af1DA597b39` |
| Arbitrum Sepolia Testnet | `0xad05853cc8395fdbbd0b1c72d2eb9491007e8dd0` |
| Optimism Sepolia Testnet | `0x0d5d82cff0e742ca3dffc0a2311b64f39d6feaa7` |
| TRON Shasta Testnet | Coming soon... |

### Test Token Contract Address Table

#### USDT

| Network | Contract Address |
|------|---------|
| TRON Shasta Testnet | `TEYKWmKvdCHX2NuX4tVmhxdN4P3PVjyMcu` |
| Arbitrum Sepolia Testnet | `0x43bCA8Fe12a7888224a7e76ec938eD9a29800cE2` |
| Optimism Sepolia Testnet | `0x43bCA8Fe12a7888224a7e76ec938eD9a29800cE2` |
| Ethereum Sepolia Testnet | `0x0C556DFC43A1de7fDaAdC798e7AA0fd90E62f54E` |

#### USDC

| Network | Contract Address |
|------|---------|
| TRON Shasta Testnet | `TWjJj93GX51rJ8GRFihPVNA15ieLoLheKaj` |
| Arbitrum Sepolia Testnet | `0x116789307A429dE86F50d9d04a130b6E99a2107B` |
| Optimism Sepolia Testnet | `0x3b1Cf5438607051231beCAA0243c47C5BD60aeec` |
| Ethereum Sepolia Testnet | `0x16033f59599c63fdc1de1c8fe569dcbd1f0d9da3` |

#### DAI

| Network | Contract Address |
|------|---------|
| Arbitrum Sepolia Testnet | `0x6B2576Ab5AAe6E479fb73611BcB2e4E71126FeAf` |
| Optimism Sepolia Testnet | `0xc70FbcebCAA4c877c18D80aF62f42534bD18eB6D` |
| Ethereum Sepolia Testnet | `0xf54cc6b8335a967fa932a2cef7859cf911cfc582` |

#### TUSD

| Network | Contract Address |
|------|---------|
| Arbitrum Sepolia Testnet | `0x58Bd5D31c29Cd0cfa89496640C3009578B98E6b5` |
| Optimism Sepolia Testnet | `0x9edccc68f41aa94cf78b08b90ea7e8bc899c874f` |
| Ethereum Sepolia Testnet | `0x834728a523ddb8f367459eafa7bec8b85767714c` |

#### FRAX

| Network | Contract Address |
|------|---------|
| Arbitrum Sepolia Testnet | `0x9eDcCc68F41aa94cF78B08b90Ea7e8Bc899c874F` |
| Optimism Sepolia Testnet | `0x76Cf3f571BCB7333E1EC5588FFd6224837D4ed33` |
| Ethereum Sepolia Testnet | `0x79aFa1A88a0EF3F0Afc39153C8f178F82db51326` |

#### USDJ

| Network | Contract Address |
|------|---------|
| TRON Shasta Testnet | `TDTNSJAYgQaEVT271PKybvDzJTmYzR9DUm` |

#### USDD

| Network | Contract Address |
|------|---------|
| TRON Shasta Testnet | `TUrhpa8bD4u6E11ZtKGRxY8uoBVAUNcmco` |

#### WETH

| Network | Contract Address |
|------|---------|
| Ethereum Sepolia Testnet | `0x2FFC0b711d9EbD3f46D869173Af7B64C510e8384` |
| Arbitrum Sepolia Testnet | `0x80D85d775ADAA4ED28E6Ab035227F0590C3bFcF7` |

#### WBTC

| Network | Contract Address |
|------|---------|
| Ethereum Sepolia Testnet | `0x6e6947a4f19b06FE98a2dC1a95529f00594888F4` |
| Arbitrum Sepolia Testnet | `0x51Ba29A9b49e575b04c31703161410009890f207` |

#### cbBTC

| Network | Contract Address |
|------|---------|
| Ethereum Sepolia Testnet | `0xD73144ca96B6A5349Ea8a82456017A35132617eD` |

{% hint style="info" %}
**Operation Screenshots** (using MetaMask as example, similar for other wallets):
{% endhint %}

## Wallet Support

BlockATM supports the following mainstream wallets:

| Wallet | Type | Supported Networks | Min Version | Support Status |
|------|------|---------|---------|---------|
| **MetaMask** | Browser Extension / Mobile App | Ethereum, Arbitrum | Extension v12.0.0+ / Mobile v7.4.1+ | ✅ Full Support |
| **TronLink** | Browser Extension / Mobile App | TRON | Extension v4.2.4+ / Mobile v4.14.2+ | ✅ Full Support |
| **Trust Wallet** | Mobile App | Ethereum, Arbitrum | v8.0.0+ | ✅ Full Support |
| **Bitget Wallet** | Mobile App | Ethereum, Arbitrum, TRON | v8.0.0+ | ✅ Full Support |
| **OKX Wallet** | Mobile App | Ethereum, Arbitrum | v8.0.0+ | ✅ Full Support |
| **OneKey** | Mobile App / Browser Extension | Ethereum, Arbitrum, TRON | v5.8.0+ | 🟡 Partial Support |
| **Ledger Live** | Hardware Wallet (Nano X) | TRON | Ledger Live 2.107.0+ / app 5.11.0+ | ✅ Full Support |

{% hint style="info" %}
**WalletConnect**: Supports all WalletConnect compatible wallets, suitable for mobile DApp connections.
{% endhint %}

### OneKey Special Notes

| Platform | Supported Networks | Description |
|------|---------|------|
| Mobile App / Browser Extension | Ethereum, Arbitrum | ✅ Full Support |
| Mobile App / Browser Extension | TRON | 🟡 Partial Support, v5.12+ required for contract deployment |
| Hardware (OneKey Pro) | TRON | 🟡 Partial Support, does not support contract deployment |

## Mainnet vs Testnet

| Environment | URL | Purpose |
|------|-----|------|
| **Production** | app.blockatm.net | Production business |
| **Test** | test-app.blockatm.net | Development testing |

{% hint style="warning" %}
**Important**: Production and test environments have completely isolated data. Testnet tokens have no actual value.
{% endhint %}

## Network Fee Comparison

| Network | Typical Gas Fee | Confirmation Time |
|------|-------------|---------|
| TRON | ~1-5 TRX | 3 sec |
| Ethereum | $0.1-5 USD | 15-30 sec |
| Arbitrum | $0.05-0.5 USD | 1-3 min |

{% hint style="info" %}
**Recommendation**: For large payments, recommend using TRON network with low fees and fast speed. For payments requiring more security, recommend using Ethereum mainnet.
{% endhint %}
