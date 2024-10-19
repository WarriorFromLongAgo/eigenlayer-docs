以下是该文档的中文翻译，以及相关作用的说明：

---
sidebar_position: 3
title: 不可思议的平方

## 什么是不可思议的平方？

[不可思议的平方](https://github.com/Layr-Labs/incredible-squaring-avs)是一个具有完整Eigenlayer集成的最小可行AVS演示。这个演示的目的是说明如何将链下计算的值（在这个例子中是一个数的平方）构建为运营商已签约执行的工作的一部分，以及这个业务逻辑如何与AVS合约相关联。我们进行链下计算，让多个运营商签名，然后聚合运营商的签名，最后在链上验证并写入值。视频演示请参见：
不可思议的平方[整体5分钟演示](https://www.loom.com/share/50314b3ec0f34e2ba386d45724602d76?sid=cf176400-fdbb-4bdc-8563-22a68414985d)
不可思议的平方[TaskManager 5分钟演示](https://www.loom.com/share/5f3f2a447bc54ffa9d37d203c32088de?sid=0f5c2c07-82c5-4640-bc6f-6e4327bb3d81)

在AVS可供使用之前，必须完成以下先决步骤：
1. 运营商在EigenLayer [DelegationManager](https://github.com/Layr-Labs/eigenlayer-middleware/blob/6a7a38593f466b1fefd2b575fb0d4f96520a946d/src/ServiceManagerBase.sol#L24)合约中注册。
2. 部署不可思议的平方AVS并注册到[AVSDirectory合约](https://github.com/Layr-Labs/eigenlayer-middleware/blob/6a7a38593f466b1fefd2b575fb0d4f96520a946d/src/ServiceManagerBase.sol#L24)的实现。
3. 运营商通过其RegistryCoordinator在AVS中注册。

## 不可思议的平方AVS生命周期（流程）

每个要求平方的数字请求都经过以下生命周期（流程）：
1. 任务生成器实体将要平方的数字发送到AVS合约（IncredibleSquaringTaskManager.sol）。
2. AVS合约发出事件（NewTaskCreated）表示新的要平方的数字。
3. 运营商监听AVS合约的事件，对数字进行平方，用[BLS签名](https://eth2book.info/capella/part2/building_blocks/signatures/)签名结果，并将其签名发送给聚合器实体。
4. 聚合器使用[BLS签名聚合](https://eth2book.info/capella/part2/building_blocks/signatures/#aggregation)将每个签名组合成一个单一的聚合签名。一旦达到法定人数阈值，聚合器将聚合签名发送回AVS合约。
5. AVS合约验证是否达到法定人数阈值以及聚合签名是否有效。如果是，则接受平方后的数字。

注意[不可思议的平方仓库](https://github.com/Layr-Labs/incredible-squaring-avs)包含了指向源代码文件的具体链接，以获取更多信息。

## 不可思议的平方架构独特特征

不可思议的平方架构包括以下独特于其架构的组件，但并非所有AVS都需要：
1. 使用BLS签名聚合来聚合运营商响应（签名）。还提供了[ECDSA版本](https://github.com/Layr-Labs/incredible-squaring-avs/pull/20)用于测试（测试版）。
2. 任务生成器：不可思议的平方使用链上任务生成，但其他AVS可能选择实现链下任务生成。请参见[EigenDA仓库](https://github.com/Layr-Labs/eigenda?tab=readme-ov-file#overview)作为链下AVS的示例。
3. 聚合器：创建用于收集运营商签名并使用BLS聚合对其进行聚合的实体。
4. 中心化实体：在不可思议的平方演示中，聚合器和任务生成器实体是中心化的。然而，可以探索随时间逐步去中心化架构的每个组件。
5. RegistryCoordinator合约实现：[BLSRegistryCoordinatorWithIndices](https://github.com/Layr-Labs/eigenlayer-middleware/blob/master/src/BLSRegistryCoordinatorWithIndices.sol)的独特实现，允许任何至少有1个委托mockerc20代币的EigenLayer运营商选择加入。

## EigenLayer AVS中BLS vs ECDSA的使用案例

[BLS (Boneh-Lynn-Shacham)](https://en.wikipedia.org/wiki/BLS_digital_signature)是默认使用的签名方案，被认为是最安全的选项。如果AVS设计需要聚合AVS任务，则用于潜在的AVS逻辑。
- 如果AVS设计需要聚合AVS任务，则用于潜在的AVS逻辑。
- 对于注册到AVS是可选的。

[ECDSA (椭圆曲线数字签名算法)](https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm)是AVS开发者可能选择的替代签名方案，以减少链上验证成本，尽管它不如BLS安全。
- 对于运营商注册到EigenLayer核心协议是必需的。
- 对于注册到AVS是可选的。

注意：请不要在不同的AVS之间共享（重用）BLS和ECDSA密钥。

## 在本地部署不可思议的平方演示

访问[不可思议的平方仓库](https://github.com/Layr-Labs/incredible-squaring-avs?tab=readme-ov-file#incredible-squaring-avs)并运行本地演示。

作用说明：

1. 这个文档详细介绍了"不可思议的平方"AVS，它是一个用于演示EigenLayer集成的示例项目。

2. 它解释了AVS的基本概念、工作流程和架构特点，这对于理解EigenLayer生态系统中AVS的运作方式非常有帮助。

3. 文档描述了AVS的生命周期，包括任务生成、运营商响应、签名聚合和验证等步骤，这为开发者提供了一个清晰的AVS运作模型。

4. 它比较了BLS和ECDSA两种签名方案在EigenLayer AVS中的应用，帮助开发者理解不同签名方案的优缺点和适用场景。

5. 文档还提供了本地部署演示的指导，这对于想要实际体验和测试AVS的开发者来说非常有价值。

总的来说，这个文档为开发者提供了一个全面的"不可思议的平方"AVS概述，既包含理论解释，也包含实践指导，有助于开发者更好地理解和开发基于EigenLayer的AVS。