这个文件主要介绍了EigenLayer运营商的安装和配置过程。以下是文件内容的分析和翻译：

分析：
1. 列出了软件要求，包括Docker和Linux环境。
2. 提供了CLI安装的多种方法，包括使用二进制文件、Go和源代码安装。
3. 详细说明了如何创建和列出ECDSA和BLS密钥。
4. 解释了运营商配置和注册的步骤，包括创建配置文件、上传logo和元数据、配置RPC节点等。
5. 介绍了如何检查注册状态和更新元数据。
6. 讨论了委托审批者（delegationApprover）的设计模式。

翻译：

---
sidebar_position: 2
---

# 安装

## 节点运营商检查清单

### **软件要求**

- Docker：确保您的系统已安装Docker。要下载Docker，请按照[此处](https://docs.docker.com/get-docker/)列出的说明进行操作。
- Docker Compose：确保也安装并正确配置了Docker Compose。要下载Docker Compose，请按照[此处](https://docs.docker.com/compose/install/)列出的说明进行操作。
- Linux环境：EigenLayer仅支持Linux。确保您有Linux环境，如Docker，用于安装。
  - 如果您选择使用Go编程语言安装eigenlayer-cli，请确保安装了Go 1.21或更高版本。您可以在[这里](https://go.dev/doc/install)找到安装指南。

---

### 检查要求

在原生Linux系统上，您可以使用lsb_release -a命令获取有关Linux发行版的信息。

**检查Docker**
如果您不使用原生Linux系统并想使用EigenLayer，可以检查是否安装了Docker：

- 打开终端或命令提示符。
- 运行以下命令检查Docker是否已安装并运行：

```
startLine: 24
endLine: 26
```

如果Docker已安装并运行，EigenLayer可以在Docker容器内使用，这提供了Linux环境。

通过遵循这些步骤，您可以确定是否有适合EigenLayer安装的Linux环境。

---

## CLI安装

### 使用二进制文件安装CLI

要下载最新版本的二进制文件，请运行：

```
startLine: 42
endLine: 44
```

二进制文件将安装在`~/bin`目录中。

要将二进制文件添加到您的路径中，请运行：

```
startLine: 48
endLine: 50
```

#### 在自定义位置安装CLI

要在自定义位置下载二进制文件，请运行：

```
startLine: 56
endLine: 58
```

---

### 使用Go安装CLI

现在我们将使用Go安装eigenlayer-CLI。以下命令将在您的系统中安装eigenlayer的可执行文件以及库和其依赖项。

```
startLine: 66
endLine: 68
```

要检查GOBIN是否不在您的PATH中，您可以从终端执行`echo $GOBIN`。如果它没有打印任何内容，则它不在您的PATH中。要将GOBIN添加到您的PATH中，请将以下行添加到您的$HOME/.profile：

```
startLine: 72
endLine: 75
```

对配置文件所做的更改可能要到下次登录计算机时才会生效。要立即应用更改，请直接运行shell命令或使用诸如source $HOME/.profile之类的命令从配置文件执行它们。

---

### 从源代码安装CLI

要采用此安装方法，您需要安装Go。请确保您安装了最低版本为1.21的Go，[点击这里](https://go.dev/doc/install)。

使用此方法，您可以手动生成二进制文件，下载并编译源代码。

```
startLine: 87
endLine: 91
```

或者如果您安装了**make**：

```
startLine: 95
endLine: 98
```

可执行文件将位于build文件夹中。

如果您想将二进制文件放入PATH中（或者如果您使用了[Go](https://github.com/Layr-Labs/eigenlayer-cli#install-eigenlayer-cli-using-go)方法，但$GOBIN不在您的PATH中），请将二进制文件复制到/usr/local/bin：

---

## 创建和列出密钥

ECDSA密钥对对应于运营商的以太坊地址和与Eigenlayer交互的密钥。BLS密钥用于EigenLayer协议内的认证目的。当您向EigenLayer注册AVS时，会使用BLS密钥。

### 创建密钥

使用CLI生成加密的ECDSA和BLS密钥：

```
startLine: 114
endLine: 117
```

- `[keyname]` - 这将是创建的密钥文件的名称。它将保存为`<keyname>.ecdsa.key.json`或`<keyname>.bls.key.json`。

这将提示输入密码，您可以使用该密码加密密钥。密钥将存储在本地磁盘上，并在创建密钥后显示一次。它还将仅显示一次私钥，以便您可以在丢失密码或密钥文件的情况下备份它。

您还可以通过将密码传递给此命令来创建密钥。这可以帮助自动创建密钥，并且不会提示输入密码。此支持在[v0.6.2](https://github.com/Layr-Labs/eigenlayer-cli/releases/tag/v0.6.2)中添加
```
startLine: 124
endLine: 126
```

[省略了约131行]

如果您要部署到测试网，请按照[获取测试网ETH](https://docs.eigenlayer.xyz/restaking-guides/restaking-user-guide/testnet/obtaining-testnet-eth-and-liquid-staking-tokens-lsts)中的说明为web3钱包注资HolEth。

---

## 运营商配置和注册

**步骤1：** 使用以下命令创建运营商注册所需的配置文件：

```
startLine: 265
endLine: 267
```

当提示输入运营商地址时，确保您的运营商地址与您在密钥创建步骤中创建/导入的ecdsa密钥地址相同。

该命令将创建两个文件：`operator.yaml`和`metadata.json`。

**步骤2：** 上传Logo图片，配置`metadata.json`，并上传两者

将运营商的logo上传到可公开访问的位置，并在`metadata.json`文件中设置url。运营商注册目前仅支持`.png`图像，且大小必须小于1MB。

`name`和`description`应符合[此处](https://github.com/Layr-Labs/eigensdk-go/blob/master/utils/utils.go#L29)提到的正则表达式。您可以使用诸如https://regex101.com/之类的服务来验证您的字段。

完成`metadata.json`中的详细信息。`metadata.json`文件大小必须小于4KB。将文件上传到可公开访问的位置，并在`operator.yaml`中设置该url。请注意，成功注册需要**可公开访问**的元数据url。这里为您提供了一个示例operator.yaml文件供参考：[operator.yaml](https://github.com/Layr-Labs/eigenlayer-cli/blob/master/pkg/operator/config/operator-config-example.yaml)。

:::info
对于主网运营商 - `metadata.json`和运营商logo .png文件必须专门通过github.com存储库托管。注意：不允许使用**gist.github.com**托管的文件。
这些要求不适用于测试网运营商。
:::

:::warning
使用Github进行托管时，请确保链接到原始文件（[示例](https://raw.githubusercontent.com/Layr-Labs/eigenlayer-cli/master/pkg/operator/config/metadata-example.json)），而不是github存储库URL（[示例](https://github.com/Layr-Labs/eigenlayer-cli/blob/master/pkg/operator/config/metadata-example.json)）。
:::

**步骤3：** 配置RPC节点：

EigenLayer CLI需要访问以太坊RPC节点才能发布注册。请计划利用RPC节点提供商或运行您自己的本地RPC节点，以在operator.yaml中引用。

请在此处找到RPC节点提供商的示例列表：
- https://chainlist.org/
- https://www.alchemy.com/list-of/rpc-node-providers-on-ethereum

此时确保您的运营商服务器可以访问您的RPC提供商。您可以从运营商服务器运行以下命令：
`curl -I [your_server_url]`

**步骤4：** DelegationManager合约地址

您必须为您的环境配置正确的DelegationManager合约地址。
- 导航到[EigenLayer合约：部署](https://github.com/Layr-Labs/eigenlayer-contracts?tab=readme-ov-file#deployments)并找到您环境（主网、测试网）的`DelegationManager`的代理地址。
- 在您的运营商配置文件中将`el_delegation_manager_address`的值设置为您环境的地址。

**可选：** 设置委托审批者

运营商可以选择在注册时设置[delegationApprover](https://github.com/Layr-Labs/eigenlayer-contracts/blob/mainnet/src/contracts/interfaces/IDelegationManager.sol#L30)。如果将`delegationApprover`设置为非零值，则`delegationApprover`地址将需要签署其对质押者向该运营商新委托的批准。如果保留默认值为零地址（0x000...），则所有新委托将自动批准，无需任何签名。请参阅下面的[委托审批者设计模式](#delegationapprover-design-patterns)以获取更多详细信息。

EigenLayer Web应用程序模拟交易以检查合约是否会回滚。如果委托调用因任何原因回滚，按钮将被禁用。

**步骤5：** 注册命令

这是您可以用来注册运营商的命令。

```
startLine: 321
endLine: 323
```

> _注意：运营商注册需要ECDSA密钥。您可以选择使用EigenLayer CLI_[_创建_](https://github.com/Layr-Labs/eigenlayer-cli/blob/master/README.md#create-keys)_自己的密钥集（建议首次用户使用）或_[_导入_](https://github.com/Layr-Labs/eigenlayer-cli/blob/master/README.md#import-keys)_您现有的密钥（建议已经创建密钥的高级用户使用），如前一节所述。_

---

## 检查注册状态

这是您可以用来查询运营商注册状态的命令。

```
startLine: 333
endLine: 335
```

---

## 元数据更新
### 一般元数据更新
这是您可以用来对运营商的元数据进行更改或更新的命令。在v0.9.0之后，此命令不会更新元数据uri。请使用[下面](#update-metadata-uri-post-v090)的命令来更新它。

```
startLine: 342
endLine: 344
```

### 更新元数据URI（v0.9.0之后）
在[v0.9.0](https://github.com/Layr-Labs/eigenlayer-cli/releases/tag/v0.9.0)中，我们引入了一个新命令来更新元数据uri。

```
startLine: 349
endLine: 351
```

## 委托审批者设计模式

委托审批者功能可以以多种方式使用，为运营商提供额外的程序化控制，以决定接受哪些再质押者的委托。

### 将签名从委托审批者传递给质押者

一系列设计涉及将唯一签名从运营商传递给请求批准的再质押者。唯一签名将有一个相应的"盐"（一次性使用的唯一值）和"过期时间"。再质押者将签名（盐和过期时间）传递到`DelegationManager.delegateTo`函数（[源代码在此](https://github.com/Layr-Labs/eigenlayer-contracts/blob/mainnet/src/contracts/core/DelegationManager.sol#L135-L155)）。该函数使用EIP1271检查签名，因此：
- A) 运营商将EOA设置为其`delegationApprover`，DelegationManager只检查签名是否是来自EOA的有效ECDSA签名。
- 或者 B) 运营商将智能合约设置为其`delegationApprover`，DelegationManager调用`delegationApprover`上的isValidSignature函数，并检查合约是否返回`0x1626ba7e`（如[EIP-1271规范](https://eips.ethereum.org/EIPS/eip-1271#specification)中定义）。

如果委托审批者自己调用DelegationManager.delegateToBySignature函数，则他们需要提供[来自再质押者的签名](https://github.com/Layr-Labs/eigenlayer-contracts/blob/mainnet/src/contracts/core/DelegationManager.sol#L157-L204)。如果调用者本身是委托审批者，则忽略approverSignatureAndExpiry输入。这种方法的一个潜在缺点是委托审批者将支付交易的gas费用。

**委托的再质押者白名单和黑名单**

如果运营商使用上述选项B，即智能合约作为其`delegationApprover`，他们还可以维护一个批准的白名单。合约可以存储批准的签名哈希的Merkle根，并在委托时为每个再质押者提供Merkle证明。[这个分支](https://github.com/Layr-Labs/eigenlayer-contracts/blob/feat-example-operator-delegation-whitelist/src/contracts/examples/DelegationApproverWhitelist.sol)提供了这样一个智能合约的概念证明。

上面的示例可以修改为使用非包含的Merkle证明而不是包含的Merkle证明，从而作为"黑名单"。