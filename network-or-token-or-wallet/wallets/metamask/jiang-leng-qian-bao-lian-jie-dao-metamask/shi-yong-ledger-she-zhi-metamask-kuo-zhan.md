# 使用 Ledger 设置 MetaMask 扩展

将 MetaMask 与 Ledger 硬件钱包连接是一个常见的安全增强方法，让你在使用 MetaMask 的便利性的同时，利用 Ledger 的冷存储保护私钥。以下是详细的步骤指南，帮助你完成连接：

### 前提条件

* **硬件设备**
  * Ledger 硬件钱包（例如 Ledger Nano S、Nano X）。
  * 已通过 Ledger Live 更新至最新固件。
* **软件程序**
  * 已安装 MetaMask 浏览器扩展（支持 Chrome、Firefox、Edge 或 Brave）。
  * Ledger Live 软件（用于管理 Ledger 和安装应用）。
* **其他准备**
  * USB 线连接 Ledger 到电脑。
  * Ledger 上已安装“Ethereum”应用（通过 Ledger Live 的“Manager”安装）。

***

### 操作步骤

**1. 准备 Ledger 设备**

1. **连接并解锁 Ledger**：
   * 用 USB 线将 Ledger 连接到电脑。
   * 输入 PIN 码解锁设备。
2. **打开 Ethereum 应用**：
   * 在 Ledger 设备上，导航到“Ethereum”应用并打开。
   * 屏幕应显示“Application is ready”。
3. **启用合约数据**（重要）：
   * 在 Ledger 的 Ethereum 应用设置中，启用“Contract Data”：
     * 进入“Settings” > “Ethereum” > 将“Contract Data”设置为“Yes”。
     * 这允许 Ledger 处理智能合约交易（如 ERC-20 代币转账或 DApp 交互）。

**2. 在 MetaMask 中连接 Ledger**

1. **打开 MetaMask**：
   * 点击浏览器右上角的 MetaMask 图标，输入密码解锁。
2. **进入账户菜单**：
   * 点击右上角的账户头像（圆形图标），打开下拉菜单。
3. **选择“Connect Hardware Wallet”**：
   * 在菜单中找到并点击“Connect Hardware Wallet”（连接硬件钱包）。
4. **选择 Ledger**：
   * 在弹出的窗口中，选择“Ledger”选项，然后点击“Continue”（继续）。
5. **检测设备**：
   * MetaMask 会尝试检测你的 Ledger。
   * 如果未自动识别：
     * 确保 Ledger 已解锁并运行 Ethereum 应用。
     * 检查 USB 连接是否正常。
     * 允许浏览器访问 USB 设备（若弹出权限提示，点击“Allow”）。

**3. 导入 Ledger 账户**

1. **选择账户**：
   * 连接成功后，MetaMask 会显示 Ledger 上的多个以太坊地址（通常 5 个或更多）。
   * 这些地址由 Ledger 的种子短语（Seed Phrase）生成。
   * 勾选你想使用的账户（通常选择第一个），然后点击“Unlock”（解锁）。
2. **完成连接**：
   * MetaMask 主界面将显示新导入的 Ledger 账户，地址以“0x”开头，账户名可能标注为“Ledger 1”。

**4. 测试连接**

1. **查看余额**：
   * 如果 Ledger 账户已有 ETH 或代币，余额会显示在 MetaMask 中。
2. **发送测试交易**：
   * 点击“Send”，输入少量 ETH 到另一个地址。
   * 提交后，Ledger 屏幕会显示交易详情：
     * 检查金额、接收地址等。
     * 按 Ledger 上的两个按钮确认签名。
3. **验证**：
   * 交易成功后，MetaMask 更新余额，可通过 Etherscan 查看记录。

***

#### 切换网络（可选）

* 默认连接到以太坊主网。
* 若需连接其他 EVM 兼容链（如 BSC、Polygon）：
  1. 点击 MetaMask 顶部网络下拉菜单 > “Add Network”。
  2. 输入目标网络的 RPC 详情（可在链官网找到）。
  3. Ledger 的 Ethereum 应用支持所有 EVM 链，无需额外配置。

***

#### 重要注意事项

1. **固件与应用版本**：
   * 确保 Ledger 固件和 Ethereum 应用为最新版本（通过 Ledger Live 更新）。
   * 旧版本可能导致连接失败。
2. **浏览器兼容性**：
   * Chrome 有时需手动允许 USB 访问，若失败，尝试 Firefox 或 Brave。
3. **合约数据**：
   * 若未启用“Contract Data”，与 DApp 或 ERC-20 代币交互会失败。
4. **安全性**：
   * Ledger 的种子短语永不输入电脑，仅用于设备恢复。
   * MetaMask 仅调用 Ledger 签名，不存储私钥。
5. **移动端限制**：
   * MetaMask 移动应用不支持直接连接 Ledger，需使用桌面浏览器。

***

#### 常见问题与解决

* **“Ledger 未检测到”**：
  * 检查 USB 连接、解锁状态和 Ethereum 应用是否运行。
  * 重启浏览器或更换 USB 端口。
* **“交易签名失败”**：
  * 确保“Contract Data”已启用，且账户有足够 ETH 支付 Gas 费。
* **“余额不显示”**：
  * 确认网络正确，或手动添加代币合约地址（在“Assets” > “Add Token”）。

***

#### 优势

* **安全性**：私钥存储在 Ledger，免受电脑病毒或钓鱼攻击。
* **便利性**：通过 MetaMask 界面与 DApp 无缝交互。
* **多用途**：支持以太坊及所有 EVM 链的资产管理。
