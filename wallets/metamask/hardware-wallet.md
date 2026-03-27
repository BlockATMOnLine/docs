# MetaMask 硬件钱包集成

将 Ledger 或 Trezor 硬件钱包与 MetaMask 结合使用，获得最高级别的安全保护。

## 什么是硬件钱包？

硬件钱包是物理设备，用于离线存储您的私钥。即使电脑被黑客攻击，您的资产仍然安全。

| 品牌 | 价格 | 支持代币 | 购买链接 |
|------|------|---------|---------|
| Ledger Nano S Plus | $79 | 5500+ | [ledger.com](https://www.ledger.com) |
| Ledger Nano X | $149 | 5500+ | [ledger.com](https://www.ledger.com) |
| Trezor Model T | $219 | 1000+ | [trezor.io](https://trezor.io) |

## 前置准备

- ✅ 硬件钱包设备（Ledger 或 Trezor）
- ✅ USB 数据线
- ✅ MetaMask 浏览器扩展
- ✅ 硬件钱包官方软件（Ledger Live / Trezor Suite）

## 步骤 1：设置硬件钱包

### Ledger 设置

1. 从官方渠道购买 Ledger 设备
2. 下载并安装 [Ledger Live](https://www.ledger.com/ledger-live)
3. 按照屏幕提示初始化设备
4. 设置 PIN 码
5. 备份助记词（与 MetaMask 不同，请分别保管）
6. 在 Ledger Live 中安装 Ethereum 应用

{% hint style="danger" %}
**警告**：永远不要从非官方渠道购买硬件钱包。二手设备可能被篡改。
{% endhint %}

### Trezor 设置

1. 从官方渠道购买 Trezor 设备
2. 访问 [trezor.io/start](https://trezor.io/start)
3. 下载并安装 Trezor Suite
4. 初始化设备并设置 PIN 码
5. 备份助记词

## 步骤 2：连接硬件钱包到 MetaMask

### 连接 Ledger

1. 用 USB 线连接 Ledger 到电脑
2. 输入 PIN 码解锁设备
3. 在 Ledger 设备上打开 Ethereum 应用
4. 打开 MetaMask
5. 点击右上角账户名称 → "添加账户"
6. 选择"硬件钱包"
7. 选择"Ledger"
8. 选择要连接的账户
9. 点击"连接"

### 连接 Trezor

1. 用 USB 线连接 Trezor 到电脑
2. 打开 MetaMask
3. 点击右上角账户名称 → "添加账户"
4. 选择"硬件钱包"
5. 选择"Trezor"
6. 按照提示连接设备
7. 在 Trezor 设备上确认连接
8. 选择要连接的账户

## 步骤 3：在 BlockATM 使用

连接硬件钱包后，在 BlockATM 的使用方式与软件钱包相同：

1. 访问 BlockATM 管理后台
2. 点击"连接钱包"
3. 选择 MetaMask
4. MetaMask 会提示您确认连接
5. 在硬件设备上确认交易

{% hint style="info" %}
**提示**：每笔交易都需要在硬件设备上物理确认，这增加了安全性。
{% endhint %}

## 优势与局限

### ✅ 优势

- **最高安全性**：私钥永不离开设备
- **防病毒**：即使电脑中毒，资产也安全
- **物理确认**：每笔交易需要物理按钮确认
- **支持多账户**：一个设备可管理多个账户

### ⚠️ 局限

- **成本**：需要购买硬件设备
- **便携性**：需要随身携带设备
- **备用方案**：设备丢失时需要助记词恢复

## 安全最佳实践

### 购买时

- ✅ 只从官方网站购买
- ✅ 检查包装是否完整
- ✅ 验证设备防伪标识
- ❌ 不要购买二手设备

### 使用时

- ✅ 始终通过官方软件更新固件
- ✅ 在使用前验证接收地址
- ✅ 定期备份助记词
- ❌ 不要将助记词数字化存储

### 存储时

- ✅ 存放在安全的地方（保险箱）
- ✅ 与助记词分开存放
- ✅ 考虑防火防水存储
- ❌ 不要放在明显位置

## 常见问题

### 设备丢失怎么办？
使用备份的助记词在新设备或 MetaMask 中恢复钱包。只要助记词安全，资产就安全。

### 可以同时连接多个设备吗？
是的。您可以在 MetaMask 中连接多个硬件钱包账户。

### 需要一直连接设备吗？
不需要。仅在需要签名交易时连接设备。查看余额不需要连接。

### 固件更新安全吗？
是的，但只通过官方软件（Ledger Live / Trezor Suite）更新。

## 下一步

- [连接 BlockATM →](connect-blockatm.md)
- [创建收币合约 →](../../integration/guides/collect-guide.md)
- [安全最佳实践 →](../../security/best-practices.md)
