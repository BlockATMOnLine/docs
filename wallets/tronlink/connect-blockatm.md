# 连接 TronLink 到 BlockATM

本指南介绍如何将 TronLink 钱包连接到 BlockATM 管理后台。

## 前置条件

- ✅ 已安装 TronLink 钱包
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

### 步骤 3：选择 TronLink

1. 在钱包列表中，选择"TronLink"
2. TronLink 会弹出连接请求窗口

{% hint style="info" %}
**提示**：如果 TronLink 没有弹出，请检查：
- TronLink 插件是否已安装
- TronLink 是否已解锁（输入密码）
- 浏览器是否允许弹出窗口
{% endhint %}

### 步骤 4：授权连接

1. TronLink 会显示连接请求
2. 检查连接的网站是否为 BlockATM
3. 点击"连接"（Connect）
4. 选择要连接的账户
5. 点击"确认"

### 步骤 5：验证连接成功

连接成功后，您应该看到：
- 钱包地址显示在页面右上角（T 开头的地址）
- TRX 和 TRC20 代币余额
- 可以进行收币/付币操作

## 切换网络

TronLink 支持 TRON 主网和测试网：

### 切换到 TRON 主网

1. 点击 TronLink 顶部的网络选择器
2. 选择"Mainnet"
3. 等待网络切换完成

### 切换到 Nile 测试网

1. 点击 TronLink 顶部的网络选择器
2. 选择"Nile"
3. 在测试网进行测试操作

{% hint style="warning" %}
**重要**：生产环境请使用 Mainnet，测试环境使用 Nile。不要混淆！
{% endhint %}

## 断开连接

如需断开钱包连接：

1. 点击 BlockATM 右上角的钱包地址
2. 选择"断开连接"
3. 或在 TronLink 中移除 BlockATM 的授权

## 常见问题

### 连接失败怎么办？

**可能原因**：
- TronLink 未安装或未解锁
- 浏览器插件被禁用
- 网络连接问题

**解决方法**：
1. 刷新页面重试
2. 检查 TronLink 插件是否启用
3. 确保 TronLink 已解锁

### 提示"请切换到正确网络"？
BlockATM 会检测当前网络。如果网络不匹配，请按提示切换到正确的网络。

### 可以连接多个钱包吗？
是的。您可以为不同的网络或业务连接不同的钱包。

## 安全提示

{% hint style="warning" %}
**安全提醒**：
- 只在官方 BlockATM 网站连接钱包
- 定期检查已连接的 DApp
- 不使用时可以断开连接
- 不要授权不明的权限请求
{% endhint %}

## TRON 网络特点

了解 TRON 网络的特点有助于更好地使用 BlockATM：

### 费用结构

| 操作 | 费用 | 说明 |
|------|------|------|
| TRX 转账 | ~0.1 TRX | 消耗带宽 |
| TRC20 转账 | ~1-5 USDT | 消耗能量 |
| 合约交互 | 可变 | 根据复杂度 |

### 如何降低费用

1. **持有 TRX**：账户持有 TRX 可获得带宽
2. **资源租赁**：从市场租赁能量
3. **批量操作**：合并多笔交易

## 下一步

- [创建收币合约 →](../../integration/guides/collect-guide.md)
- [获取测试 TRX →](../../getting-started/supported-networks.md)
- [开始收币集成 →](../../integration/guides/collect-guide.md)
