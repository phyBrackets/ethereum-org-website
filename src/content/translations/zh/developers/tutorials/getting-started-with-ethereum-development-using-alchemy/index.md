---
title: 使用Alchemy开始以太坊开发
description: "这是初学者的以太坊Alchemy开发指南。我们将带您从注册Alchemy开始，进行命令行请求，写下您的第一个web3脚本！ 无需区块链的开发经验！"
author: "Elan Halpern"
tags:
  - "入门指南"
  - "javascript"
  - "ethers.js"
  - "节点"
  - "查询"
  - "Alchemy"
skill: 初学者
lang: zh
sidebar: true
published: 2020-10-30
source: Medium
sourceUrl: https://medium.com/alchemy-api/getting-started-with-ethereum-development-using-alchemy-c3d6a45c567f
---

![以太坊和Alchemy徽标](./ethereum-alchemy.png)

这是介绍如何使用 Alchemy 进行以太坊开发的初学者入门指南，[Alchemy](https://alchemyapi.io/)是领先的区块链开发人员平台。它为数以百万计的用户提供支持，这些用户来自 70%的顶级区块链应用，其中包括 Maker、0x、MyEtherWallet、Dharma 和 Kyber。

我们将带您注册 Alchemy 来编写您的第一个 web3 脚本！ 无需区块链的开发经验！

## 1\. 注册免费 Alchemy 帐户 {#sign-up-for-a-free-alchemy-account}

创建一个 Alchemy 帐户很容易。[在此免费注册](https://dashboard.alchemyapi.io/signup/)。

## 2\. 创建一个 Alchemy 应用程序 {#create-an-alchemy-app}

要使用 Alchemy 产品，您需要一个 API 密钥来对您的请求进行身份验证。

您可以通过[仪表板](http://dashboard.alchemyapi.io/)创建 API 密钥。 要创建一个新密钥，导航到如下所示的“Create App”：

特别感谢[_ShapeShift_](https://shapeshift.com/)_让我们展示他们的仪表板！_

![Alchemy仪表板](./alchemy-dashboard.png)

填写“Create App”下的详细信息以获取您的新密钥。 在此处还可以看到您以前创建的应用以及您的团队创建的应用。 通过点击任何应用的“View Key”来查看现有密钥。

![使用Alchemy创建应用程序的截图](./create-app.png)

您也可以通过将鼠标悬停在“Apps”上并选择一个来获取现有 API 密钥。 您可以在这里“查看密钥”，以及“编辑应用程序”来特定域名加入白名单、查看几个开发者工具，并查看分析。

![显示用户如何获取API密钥的GIF图](./pull-api-keys.gif)

## 3\. 在命令行中发送请求 {#make-a-request-from-the-command-line}

使用 JSON-RPC 和 curl 通过 Alchemy 与以太坊区块链交互。

对于手动请求，我们建议通过`JSON RPC`发送`POST`请求来进行交互。 只需传入`Content-Type: application/json`标头和查询作为`POST`主体，具有以下字段：

- `jsonrpc`: JSON-RPC 版本，目前只支持`2.0`。
- `method`：ETH API 方法。 [请参阅 API 参考。](https://docs.alchemyapi.io/documentation/alchemy-api-reference/json-rpc)
- `params`：要传递到方法的参数列表。
- `id`：请求的 ID。 将通过响应返回，这样就可以跟踪一个响应属于哪个请求。

这是一个可通过命令行运行的示例，用于查询当前燃气价格：

```bash
curl https://eth-mainnet.alchemyapi.io/v2/demo \
-X POST \
-H "Content-Type: application/json" \
-d '{"jsonrpc":"2.0","method":"eth_gasPrice","params":[],"id":73}'
```

_**注意**： 使用您自己的 API 密钥 https://eth-mainnet.alchemyapio/v2/**your-api-key** 替换 [https://eth-mainnet.alchemyapi.o/v2/demo](https://eth-mainnet.alchemyapi.io/jsonrpc/demo)。_

**结果：**

```json
{ "id": 73,"jsonrpc": "2.0","result": "0x09184e72a000" // 10000000000000 }
```

## 4. 设置 Web3 客户端 {#set-up-your-web3-client}

**如果您已有客户端，** 将您当前的节点提供商的 URL 更改为您的 API 密钥的 Alchemy URL： `“https://eth-mainnet.alchemyapi.io/v2/your-api-key”`

**_注意：_**下面的脚本需要在一个**节点环境**中运行或**保存到一个文件运行**，而不是通过命令行运行。 如果您尚未安装节点或 npm ，请查看此[适用于 mac 的快速设置指南](https://app.gitbook.com/@alchemyapi/s/alchemy/guides/alchemy-for-macs)。

有大量可与与 Alchemy 集成的[Web3 库](https://docs.alchemyapi.io/guides/getting-started#other-web3-libraries)。但是，我们建议使用[Alchemy Web3](https://docs.alchemyapi.io/documentation/alchemy-web3)，它是 web3.js 的替代插件，可以与 Alchemy 无缝配合工作。 这个库有很多优点，例如自动重试和可靠的 WebSocket 支持。

要安装 AlchemyWeb3.js，请**导航到项目目录**并运行：

**使用 yarn：**

```
yarn add @alch/alchemy-web3
```

**使用 NPM：**

```
npm install @alch/alchemy-web3
```

要与 Alchemy 的节点基础设施交互，请在 NodeJS 中运行或将其添加到 JavaScript 文件：

```js
const { createAlchemyWeb3 } = require("@alch/alchemy-web3")
const web3 = createAlchemyWeb3(
  "https://eth-mainnet.alchemyapi.io/v2/your-api-key"
)
```

## 5. 编写您的第一个 Web3 脚本！ {#write-your-first-web3-script}

现在用一个小的 web3 编程来练习，我们将编写一个简单的脚本，用于打印出以太坊主网中最新的区块高度。

1.  **在终端中创建一个新的项目目录并通过 cd 命令进入该目录（如果尚未这样做）：**

```
mkdir web3-example
cd web3-example
```

**2. 在项目中安装 Alchemy web3（或任何 web3）依赖项（如果尚未这样做）：**

```
npm install @alch/alchemy-web3
```

**‌3. 创建一个名为 `index.js` **的文件并添加以下内容：\*\*

> 最终应将`demo`替换为您的 Alchemy HTTP API 密钥 。

```js
async function main() {
  const { createAlchemyWeb3 } = require("@alch/alchemy-web3")
  const web3 = createAlchemyWeb3("https://eth-   mainnet.alchemyapi.io/v2/demo")
  const blockNumber = await web3.eth.getBlockNumber()
  console.log("The latest block number is " + blockNumber)
}
main()
```

不熟悉 async stuff？ 来看看这篇[Medium 文章](https://medium.com/better-programming/understanding-async-await-in-javascript-1d81bb079b2c)。

**4. 使用 node 在终端中运行程序**

```
node index.js
```

**‌5. 现在应该会在控制台中看到最新的区块数量输出结果！**

```
The latest block number is 11043912
```

‌**哇！ 恭喜！ 您刚刚使用 Alchemy 编写了您的第一个 web3 脚本 🎉 **

不知道下一步怎么办？ 尝试部署您的第一个智能合约，并在我们的[_Hello World_](https://docs.alchemyapi.io/tutorials/hello-world-smart-contract)_智能合约指南中练习。 或通过_[_Dashboard Demo App_](https://docs.alchemyapi.io/tutorials/demo-app)测试您的仪表板知识！

[免费注册 Alchemy](https://dashboard.alchemyapi.io/signup/)，查看我们的[文档](https://docs.alchemyapi.io/)，关注我们的[Twitter](https://twitter.com/AlchemyPlatform)获取最新消息。
