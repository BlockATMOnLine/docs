# 支持的网络

BlockATM 目前支持三条主流区块链网络（Ethereum、Arbitrum、Tron）上的主流稳定币。需要注意的是，Tron 与其他两条网络不兼容，企业用户需要将 Ethereum/Arbitrum 钱包地址与 Tron 钱包地址关联，以管理不同网络上的智能合约和支付网关。

## 网络与代币概览

| 网络 | ChainId | 支持的代币 |
|------|---------|-----------|
| **Ethereum 主网** | 1 | DAI、USDT、USDC、TUSD、FRAX、WETH、WBTC、cbBTC |
| **TRON 主网** | 0x2b6653dc | USDT、USDJ、USDD |
| **Arbitrum 主网** | 42161 | DAI、USDT、USDC、TUSD、FRAX、WETH、WBTC |
| **Ethereum Sepolia 测试网** | 11155111 | DAI、USDT、USDC、TUSD、FRAX、WETH、WBTC、cbBTC |
| **TRON Shasta 测试网** | 0x94a9059e | USDT、USDJ、USDD、WETH、WBTC |
| **Arbitrum Sepolia 测试网** | 421614 | DAI、USDT、USDC、TUSD、FRAX |

{% hint style="info" %}
我们正在持续添加更多网络和代币的支持...
{% endhint %}

## 获取测试币

在测试环境开发时，您需要获取测试网的代币。

### Ethereum Sepolia 测试网

BlockATM 使用 Sepolia 作为 Ethereum 测试网络。如果 ETH 作为交易费用不足，建议通过以下方式获取测试 ETH：

- [pk910.de Sepolia 水龙头](https://sepolia-faucet.pk910.de/)（推荐，无需注册）
- [Alchemy Sepolia 水龙头](https://www.alchemy.com/faucets/sepolia)
- [QuickNode Sepolia 水龙头](https://quicknode.com/faucet/sepolia)

### TRON Shasta 测试网

BlockATM 使用 Shasta 作为 TRON 测试网络。如果 TRX 作为交易费用不足，可以通过以下方式获取：

1. 加入官方 Tron [Discord 社区](https://discord.gg/SCcs9uAc)
2. 发送 `!shasta <您的钱包地址>`
3. 系统会自动发放测试 TRX

### Arbitrum Sepolia 测试网

作为 Ethereum 的 Layer 2 网络，Arbitrum 可以通过[官方跨链桥](https://bridge.arbitrum.io/?destinationChain=arbitrum-sepolia&sourceChain=sepolia)从 Ethereum Sepolia 转移资产到 Arbitrum Sepolia。

### Optimism Sepolia 测试网

BlockATM 也支持 Optimism Sepolia 测试网。获取测试币后可以通过跨链桥或其他方式获取。

## 测试代币合约地址

您可以通过向以下地址转入少量原生代币（如 0.0001 ETH）来获取 BlockATM 所需的全部测试代币资产。

| 网络 | 水龙头地址 |
|------|-----------|
| Ethereum Sepolia 测试网 | `0xd611c098e26b2B6095F22F4bFfEB1Af1DA597b39` |
| Arbitrum Sepolia 测试网 | `0xad05853cc8395fdbbd0b1c72d2eb9491007e8dd0` |
| Optimism Sepolia 测试网 | `0x0d5d82cff0e742ca3dffc0a2311b64f39d6feaa7` |
| TRON Shasta 测试网 | 即将上线... |

### 测试代币合约地址表

#### USDT

| 网络 | 合约地址 |
|------|---------|
| TRON Shasta 测试网 | `TEYKWmKvdCHX2NuX4tVmhxdN4P3PVjyMcu` |
| Arbitrum Sepolia 测试网 | `0x43bCA8Fe12a7888224a7e76ec938eD9a29800cE2` |
| Optimism Sepolia 测试网 | `0x43bCA8Fe12a7888224a7e76ec938eD9a29800cE2` |
| Ethereum Sepolia 测试网 | `0x0C556DFC43A1de7fDaAdC798e7AA0fd90E62f54E` |

#### USDC

| 网络 | 合约地址 |
|------|---------|
| TRON Shasta 测试网 | `TWjJj93GX51rJ8GRFihPVNA15ieLoLheKaj` |
| Arbitrum Sepolia 测试网 | `0x116789307A429dE86F50d9d04a130b6E99a2107B` |
| Optimism Sepolia 测试网 | `0x3b1Cf5438607051231beCAA0243c47C5BD60aeec` |
| Ethereum Sepolia 测试网 | `0x16033f59599c63fdc1de1c8fe569dcbd1f0d9da3` |

#### DAI

| 网络 | 合约地址 |
|------|---------|
| Arbitrum Sepolia 测试网 | `0x6B2576Ab5AAe6E479fb73611BcB2e4E71126FeAf` |
| Optimism Sepolia 测试网 | `0xc70FbcebCAA4c877c18D80aF62f42534bD18eB6D` |
| Ethereum Sepolia 测试网 | `0xf54cc6b8335a967fa932a2cef7859cf911cfc582` |

#### TUSD

| 网络 | 合约地址 |
|------|---------|
| Arbitrum Sepolia 测试网 | `0x58Bd5D31c29Cd0cfa89496640C3009578B98E6b5` |
| Optimism Sepolia 测试网 | `0x9edccc68f41aa94cf78b08b90ea7e8bc899c874f` |
| Ethereum Sepolia 测试网 | `0x834728a523ddb8f367459eafa7bec8b85767714c` |

#### FRAX

| 网络 | 合约地址 |
|------|---------|
| Arbitrum Sepolia 测试网 | `0x9eDcCc68F41aa94cF78B08b90Ea7e8Bc899c874F` |
| Optimism Sepolia 测试网 | `0x76Cf3f571BCB7333E1EC5588FFd6224837D4ed33` |
| Ethereum Sepolia 测试网 | `0x79aFa1A88a0EF3F0Afc39153C8f178F82db51326` |

#### USDJ

| 网络 | 合约地址 |
|------|---------|
| TRON Shasta 测试网 | `TDTNSJAYgQaEVT271PKybvDzJTmYzR9DUm` |

#### USDD

| 网络 | 合约地址 |
|------|---------|
| TRON Shasta 测试网 | `TUrhpa8bD4u6E11ZtKGRxY8uoBVAUNcmco` |

#### WETH

| 网络 | 合约地址 |
|------|---------|
| Ethereum Sepolia 测试网 | `0x2FFC0b711d9EbD3f46D869173Af7B64C510e8384` |
| Arbitrum Sepolia 测试网 | `0x80D85d775ADAA4ED28E6Ab035227F0590C3bFcF7` |

#### WBTC

| 网络 | 合约地址 |
|------|---------|
| Ethereum Sepolia 测试网 | `0x6e6947a4f19b06FE98a2dC1a95529f00594888F4` |
| Arbitrum Sepolia 测试网 | `0x51Ba29A9b49e575b04c31703161410009890f207` |

#### cbBTC

| 网络 | 合约地址 |
|------|---------|
| Ethereum Sepolia 测试网 | `0xD73144ca96B6A5349Ea8a82456017A35132617eD` |

{% hint style="info" %}
**操作截图**（以 MetaMask 为例，其他钱包类似）：
{% endhint %}

## 钱包支持

BlockATM 支持以下主流钱包：

| 钱包 | 类型 | 支持网络 | 最低版本 | 支持状态 |
|------|------|---------|---------|---------|
| **MetaMask** | 浏览器插件 / 手机 App | Ethereum, Arbitrum | 插件 v12.0.0+ / 手机 v7.4.1+ | ✅ 完全支持 |
| **TronLink** | 浏览器插件 / 手机 App | TRON | 插件 v4.2.4+ / 手机 v4.14.2+ | ✅ 完全支持 |
| **Trust Wallet** | 手机 App | Ethereum, Arbitrum | v8.0.0+ | ✅ 完全支持 |
| **Bitget Wallet** | 手机 App | Ethereum, Arbitrum, TRON | v8.0.0+ | ✅ 完全支持 |
| **OKX Wallet** | 手机 App | Ethereum, Arbitrum | v8.0.0+ | ✅ 完全支持 |
| **OneKey** | 手机 App / 浏览器插件 | Ethereum, Arbitrum, TRON | v5.8.0+ | 🟡 部分支持 |
| **Ledger Live** | 硬件钱包（Nano X） | TRON | Ledger Live 2.107.0+ / app 5.11.0+ | ✅ 完全支持 |

{% hint style="info" %}
**WalletConnect**：支持所有 WalletConnect 兼容钱包，适用于移动端 DApp 连接。
{% endhint %}

### OneKey 特别说明

| 平台 | 支持网络 | 说明 |
|------|---------|------|
| 手机 App / 浏览器插件 | Ethereum, Arbitrum | ✅ 完全支持 |
| 手机 App / 浏览器插件 | TRON | 🟡 部分支持，v5.12+ 才支持合约部署 |
| 硬件 (OneKey Pro) | TRON | 🟡 部分支持，不支持合约部署 |

## 主网与测试网

| 环境 | URL | 用途 |
|------|-----|------|
| **生产环境** | app.blockatm.net | 正式业务 |
| **测试环境** | test-app.blockatm.net | 开发测试 |

{% hint style="warning" %}
**重要**：生产环境和测试环境的数据完全隔离，测试网上的代币没有实际价值。
{% endhint %}

## 网络手续费对比

| 网络 | 典型 Gas 费用 | 确认时间 |
|------|-------------|---------|
| TRON | ~1-5 TRX | 3 秒 |
| Ethereum | $0.1-5 USD | 15-30 秒 |
| Arbitrum | $0.05-0.5 USD | 1-3 分钟 |

{% hint style="info" %}
**建议**：对于大额支付，推荐使用 TRON 网络，手续费低且速度快。对于追求安全性的支付，推荐使用 Ethereum 主网。
{% endhint %}