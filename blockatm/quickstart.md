# 快速开始

本指南将帮助您在 15 分钟内完成 BlockATM 的首次集成，从零开始到收到第一笔测试支付。

## 前置准备清单（3 分钟）

在开始之前，请完成以下准备：

### 1. 创建加密货币钱包

选择一个钱包创建：

**推荐新手**：

* [创建 MetaMask 钱包](../wallets/metamask/create-wallet.md)（5 分钟）- 支持 Ethereum/Arbitrum
* [创建 TronLink 钱包](../wallets/tronlink/create-wallet.md)（3 分钟）- 支持 TRON

{% hint style="warning" %}
**重要**：务必备份好助记词！助记词丢失 = 资产丢失，BlockATM 无法帮您恢复。
{% endhint %}

### 2. 获取测试币

根据您选择的网络获取测试币：

| 网络               | 测试币获取                                                          |
| ---------------- | -------------------------------------------------------------- |
| Ethereum Sepolia | [faucets.chainlinklabs.com](https://faucets.chainlinklabs.com) |
| Arbitrum Sepolia | [faucets.chain.link](https://faucets.chain.link)               |
| TRON Nile        | [nileex.io](https://nileex.io/join/getJoinPage)                |

### 3. 注册 BlockATM 账号

1. 访问 [BlockATM 测试环境](https://backstage-b2b-pre.ufcfan.org)
2. 点击"注册"
3. 填写邮箱和密码
4. 验证邮箱

{% hint style="info" %}
**建议**：先在测试环境熟悉流程，再使用生产环境。
{% endhint %}

***

## 步骤 1：创建收币合约（5 分钟）

### 1.1 登录管理后台

访问并登录：

* 测试环境：https://backstage-b2b-pre.ufcfan.org
* 生产环境：https://app.blockatm.net

### 1.2 进入收币合约管理

1. 登录后，点击顶部导航栏的"收币"
2. 在左侧菜单选择"收币合约"
3. 点击"创建合约"按钮

### 1.3 选择合约类型

选择**Web3 收款合约**（适合新手）：

| 类型       | 说明       | 费用      |
| -------- | -------- | ------- |
| Web3 收款  | 用户连接钱包支付 | 2 USD/笔 |
| Scan2Pay | 用户扫码支付   | 0.4%/笔  |

### 1.4 配置合约地址

填写以下信息：

**签名地址（Signer）**：

* 有权从合约提取资金的地址
* 建议使用硬件钱包地址
* 示例：`0x1234...5678`

**收款地址（Receiver）**：

* 资金最终到达的地址
* 可以和签名地址相同
* 示例：`0xabcd...efgh`

{% hint style="warning" %}
**重要**：

* 仔细核对地址，一旦创建无法修改
* 建议先小额测试提现功能
* 使用硬件钱包管理大额资金
{% endhint %}

### 1.5 支付创建费用

1. 确认配置信息
2. 支付 200 USD 合约创建费用
3. 等待合约部署（约 1-2 分钟）
4. 部署成功后，记录合约 ID

***

## 步骤 2：创建收银台（3 分钟）

### 2.1 进入收银台管理

1. 点击顶部导航栏的"收银台"
2. 点击"创建收银台"

### 2.2 填写商户信息

* **商户名称**：您的业务名称
* **商户描述**：简单描述业务
* **联系邮箱**：接收通知的邮箱

### 2.3 选择网络和代币

**支持的网络**：

* ✅ Ethereum (ERC20)
* ✅ Arbitrum (ARB20)
* ✅ TRON (TRC20)

**支持的代币**：

* USDT（推荐）
* USDC
* DAI
* 其他 ERC20/TRC20 代币

### 2.4 启用支付方式

选择至少一种支付方式：

**连接钱包支付**：

* ✅ 用户体验好
* ✅ 支付金额准确
* 适合：桌面端

**扫码支付**：

* ✅ 无需连接钱包
* ✅ 适合移动端
* 适合：手机用户

### 2.5 绑定收币合约

1. 选择刚才创建的收币合约
2. 一个合约只能绑定一个收银台
3. 点击"预览收银台"查看效果
4. 确认后点击"创建"

### 2.6 获取集成信息

创建成功后，点击"集成"按钮，获取：

* **收银台 ID**（Cashier ID）：`cs_xxxxxx`
* **API Key**（公钥）：`pck_xxxxxx`
* **Secret Key**（私钥）：`sck_xxxxxx` ⚠️ 保密
* **Webhook Key**：用于验证通知签名

{% hint style="danger" %}
**重要**：Secret Key 只显示一次！请立即复制并安全保存。
{% endhint %}

***

## 步骤 3：集成收银台（5 分钟）

### 3.1 创建测试页面

在您的电脑上创建一个新的 HTML 文件：

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>BlockATM 支付测试</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 600px;
      margin: 50px auto;
      padding: 20px;
    }
    #blockatm-container {
      min-height: 500px;
      border: 1px solid #ddd;
      border-radius: 8px;
      margin-top: 20px;
    }
    .status {
      padding: 10px;
      margin: 10px 0;
      border-radius: 4px;
    }
    .success { background: #d4edda; color: #155724; }
    .error { background: #f8d7da; color: #721c24; }
  </style>
</head>
<body>
  <h1>🎉 BlockATM 支付测试</h1>
  
  <div id="status"></div>
  <div id="blockatm-container"></div>

  <!-- 引入 BlockATM SDK -->
  <script src="https://test-pay.blockatm.net/libs/v2/BlockATM.umd.js?apiKey=YOUR_API_KEY"></script>
  
  <script>
    // 替换为您的实际值
    const CASHIER_ID = 'YOUR_CASHIER_ID';
    const ORDER_NO = 'ORDER_' + Date.now();
    const AMOUNT = '10'; // 测试金额：10 USDT
    const SYMBOL = 'USDT';
    const CHAIN_ID = 'TRON'; // 或 'ETHEREUM', 'ARBITRUM'

    // 初始化收银台
    window.BlockATM.init(
      document.getElementById('blockatm-container'),
      {
        cashierId: CASHIER_ID,
        orderNo: ORDER_NO,
        amount: AMOUNT,
        symbol: SYMBOL,
        chainId: CHAIN_ID,
        lang: 'zh-CN',
        callback: function(result) {
          const statusDiv = document.getElementById('status');
          
          if (result.type === 'finish') {
            statusDiv.className = 'status success';
            statusDiv.innerHTML = '✅ 支付成功！订单号：' + result.data.orderNo;
            console.log('支付成功:', result.data);
          } else if (result.type === 'cancel') {
            statusDiv.className = 'status error';
            statusDiv.innerHTML = '❌ 用户取消支付';
          } else {
            statusDiv.className = 'status error';
            statusDiv.innerHTML = '❌ 支付失败：' + result.message;
          }
        }
      }
    );
  </script>
</body>
</html>
```

### 3.2 替换配置值

将以下值替换为您在 2.6 获取的信息：

```javascript
const CASHIER_ID = 'cs_xxxxxx';  // 替换为您的收银台 ID
const API_KEY = 'pck_xxxxxx';    // 替换为您的 API Key
```

### 3.3 在浏览器打开

1. 双击 HTML 文件，在浏览器中打开
2.  或使用本地服务器：

    ```bash
    # 使用 Python
    python -m http.server 8000
    # 访问 http://localhost:8000/your-file.html
    ```

### 3.4 测试支付

1. 页面加载后，会显示收银台
2. 点击"连接钱包"
3. 选择您创建的钱包（MetaMask 或 TronLink）
4. 授权支付
5. 确认交易

{% hint style="info" %}
**提示**：测试环境请使用测试币，不要使用真实资金！
{% endhint %}

### 3.5 验证支付结果

支付成功后，您会看到：

1. 页面显示"✅ 支付成功"
2. 浏览器控制台输出订单详情
3. 管理后台订单状态更新
4. 如配置了 Webhook，会收到通知

***

## 验证清单

完成以下检查，确保集成成功：

* [ ] ✅ 收银台正常显示在页面中
* [ ] ✅ 钱包连接成功
* [ ] ✅ 能够看到支持的代币列表
* [ ] ✅ 支付金额显示正确
* [ ] ✅ 钱包授权交易成功
* [ ] ✅ 支付完成后显示成功消息
* [ ] ✅ 管理后台订单状态更新
* [ ] ✅ （可选）Webhook 收到通知

***

## 常见问题排查

### 问题 1：收银台不显示

**可能原因**：

* API Key 不正确
* 收银台未激活
* 浏览器控制台有错误

**解决方法**：

1. 检查 API Key 是否正确
2. 在管理后台确认收银台状态为"active"
3. 打开浏览器控制台（F12）查看错误

### 问题 2：钱包连接失败

**可能原因**：

* 钱包插件未安装
* 钱包未解锁
* 网络不匹配

**解决方法**：

1. 确认已安装 MetaMask 或 TronLink
2. 解锁钱包（输入密码）
3. 切换到正确的网络（TRON/Ethereum/Arbitrum）

### 问题 3：支付失败

**可能原因**：

* 余额不足
* Gas 费用不足
* 网络拥堵

**解决方法**：

1. 检查钱包余额是否足够
2. 确保有足够的 Gas（TRX/ETH）
3. 等待几分钟后重试

***

## 下一步

集成成功后，您可以：

### 深入学习

* [收币完整集成指南](../getting-started/guides/collect-guide.md) - 详细集成流程
* [Open API 文档](../getting-started/open-api/) - 后端集成
* [Webhook 配置](../getting-started/webhooks/) - 接收支付通知

### 生产环境部署

1. 在生产环境创建合约和收银台
2. 更新 SDK URL 为生产环境
3. 使用真实的 API Key
4. 配置 Webhook 接收真实支付通知

### 高级功能

* [付币功能](../getting-started/guides/payout-guide.md) - 批量支付
* [授权模式](../getting-started/products/batch-payout/allowance-mode.md) - 提高资金效率
* [异常处理](../getting-started/guides/exception-handling.md) - 处理失败订单

***

## 需要帮助？

* 💬 Telegram：Passto\_john
* 📧 邮箱：john.feng@chixi88.com
* 📖 [常见问题](../faq/)

{% hint style="success" %}
**恭喜！** 您已完成 BlockATM 的首次集成。继续探索更多功能吧！
{% endhint %}
