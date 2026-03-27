# 连接 MetaMask 到 BlockATM

本指南介绍如何将 MetaMask 钱包连接到 BlockATM 管理后台。

## 前置条件

- ✅ 已安装 MetaMask 钱包
- ✅ 已创建钱包并备份助记词
- ✅ BlockATM 管理后台账号

## 连接步骤

### 步骤 1：登录管理后台

1. 访问 BlockATM 管理后台
   - 生产环境：[app.blockatm.net](https://app.blockatm.net)
   - 测试环境：[backstage-b2b-pre.ufcfan.org](https://backstage-b2b-pre.ufcfan.org)
2. 使用账号密码登录

### 步骤 2：进入钱包管理

1. 登录后，点击右上角的用户头像
2. 选择"钱包管理"或"账户设置"
3. 找到"连接钱包"选项

### 步骤 3：选择 MetaMask

1. 在钱包列表中，选择"MetaMask"
2. 系统会弹出 MetaMask 连接请求

### 步骤 4：授权连接

1. MetaMask 会弹出确认窗口
2. 检查连接的网站是否为 BlockATM
3. 点击"连接"（Connect）
4. 选择要连接的账户
5. 点击"下一步" → "连接"

{% hint style="info" %}
**提示**：首次连接后，下次访问时会自动连接，无需重复授权。
{% endhint %}

### 步骤 5：验证连接成功

连接成功后，您应该看到：
- 钱包地址显示在页面右上角
- 账户余额信息（如已关联）
- 可以进行收币/付币操作

## 切换网络

BlockATM 支持多个网络，您可能需要在不同网络间切换：

### 切换到 Ethereum

1. 点击 MetaMask 网络选择器（顶部）
2. 选择"Ethereum Mainnet"
3. 等待网络切换完成

### 切换到 Arbitrum

1. 点击 MetaMask 网络选择器
2. 如果未看到 Arbitrum，需要手动添加：
   - 网络名称：Arbitrum One
   - RPC URL：https://arb1.arbitrum.io/rpc
   - 链 ID：42161
   - 代币符号：ETH
   - 区块浏览器：https://arbiscan.io

{% hint style="warning" %}
**重要**：TRON 网络需要使用 TronLink 钱包，MetaMask 不支持 TRON 网络。
{% endhint %}

## 断开连接

如需断开钱包连接：

1. 点击 BlockATM 右上角的钱包地址
2. 选择"断开连接"
3. 或在 MetaMask 中移除 BlockATM 的授权

## 常见问题

### 连接失败怎么办？

**可能原因**：
- MetaMask 未安装或未解锁
- 浏览器插件被禁用
- 网络连接问题

**解决方法**：
1. 刷新页面重试
2. 检查 MetaMask 插件是否启用
3. 确保 MetaMask 已解锁（输入密码）

### 可以连接多个钱包吗？
是的。您可以为不同的网络或业务连接不同的钱包。

### 连接后能看到我的资产吗？
BlockATM 只能看到您授权的信息，无法控制您的资产。您的资产始终由您自己控制。

## 安全提示

{% hint style="warning" %}
**安全提醒**：
- 只在官方 BlockATM 网站连接钱包
- 定期检查已连接的 DApp
- 不使用时可以断开连接
- 不要授权不明的权限请求
{% endhint %}

## 下一步

- [创建收币合约 →](../../integration/guides/collect-guide.md)
- [获取测试币 →](../../getting-started/supported-networks.md)
- [开始收币集成 →](../../integration/guides/collect-guide.md)
