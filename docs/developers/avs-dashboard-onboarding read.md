以下是该文档的中文翻译：

---
sidebar_position: 7
title: AVS 仪表板接入
---

本文档定义了AVS必须实现的接口，以便我们能够为V1 [AVS市场](https://app.eigenlayer.xyz/avs)索引其数据。

新的AVS列表：为了使AVS的名称、信息和logo被索引，它必须在[AVSDirectory合约](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/src/contracts/core/AVSDirectory.sol)上调用`updateAVSMetadataURI()`。
目前需要大约10分钟才能完成索引，并在仪表板上更新元数据。

更新AVS列表：如果您为AVS的新版本部署了新合约，请务必删除之前的列表。调用更新元数据函数，值为null，例如`updateAVSMetadataURI("")`以删除之前的列表。您的列表将在一小时内从应用程序缓存中删除。

## 接口

```javascript
startLine: 17
endLine: 29
```

### registerOperatorToAVS 和 deregisterOperatorFromAVS
为了在UI上显示其操作员列表，AVS必须通过调用EigenLayer的AVSDirectory上的`registerOperatorToAVS()`和`deregisterOperatorFromAVS()`来处理操作员注册/注销。这些函数主要用于转发调用到`AVSDirectory.sol`合约，以确认操作员在AVS中的注册。

```solidity
startLine: 35
endLine: 45
```

### getOperatorRestakedStrategies
必须实现此函数以提供操作员在AVS中重新质押的策略列表。这允许AVS在UI上显示其总重新质押价值。给定一个操作员，此函数应：
- 从`RegistryCoordinator.sol`合约中检索操作员的仲裁位图。
- 检索仲裁位图中每个仲裁的策略地址。

注意，不保证操作员在仲裁中是否有策略的份额或返回数组中每个元素的唯一性。链下服务应单独进行该验证。

```solidity
startLine: 54
endLine: 80
```

### getRestakeableStrategies
必须实现此函数，以便在UI上正确显示该AVS的所有可能的可重新质押策略。这些是AVS支持重新质押的策略。

```solidity
startLine: 84
endLine: 106
```

参考[ServiceManagerBase.sol](https://github.com/Layr-Labs/eigenlayer-middleware/blob/mainnet/src/ServiceManagerBase.sol)以获取接口的参考实现。

AVSDirectory合约的代理和实现地址可在EigenLayer合约 -> [部署](https://github.com/Layr-Labs/eigenlayer-contracts/?tab=readme-ov-file#deployments)中找到。

查看EigenDA的AVS特定部署 -> [部署](https://github.com/Layr-Labs/eigenlayer-middleware/tree/dev?tab=readme-ov-file#deployments)

## 元数据URI格式

元数据URI应遵循此[示例](https://holesky-operator-metadata.s3.amazonaws.com/avs_1.json)中概述的格式。logo必须为PNG格式。

```json
startLine: 118
endLine: 124
```

注意，为了在UI上正确渲染您的logo，logo必须托管在GitHub上，其引用必须指向原始文件，如上面的示例所示。如果您需要一个公开托管logo的仓库，您可以向`eigendata`仓库提交PR并添加您的logo：https://github.com/Layr-Labs/eigendata。

## Holesky仪表板接入
一旦您完成了上述步骤并调用了`updateAVSMetadataURI`函数，您的AVS将在大约10分钟内反映在Holesky仪表板上。

## 主网仪表板接入
在计划主网接入之前，请先在测试网上进行测试。如果您尚未就任何接入问题与Eigen Labs团队联系，您可以通过填写[此表单](https://forms.gle/8BJSntA3eYUnZZgs8)与团队取得联系。