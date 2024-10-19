以下是该文档的中文翻译，以及相关作用的说明：

---
sidebar_position: 2
title: 构建AVS
---

## 步骤1：学习EigenLayer基础知识

在深入了解AVS是什么以及如何设计和构建之前，请查看[EigenLayer简介](https://docs.eigenlayer.xyz/eigenlayer/overview/)概述，快速熟悉质押者和运营商的概念。

查看[EigenLayer学习资源](/docs/eigenlayer/resources/learning-resources.md)下的材料，包括：
- 通过[你可以发明EigenLayer](https://www.blog.eigenlayer.xyz/ycie/)了解EigenLayer的工作原理。
- 通过[可编程信任的三大支柱：EigenLayer的终极目标](https://www.blog.eigenlayer.xyz/the-three-dimensions-of-programmable-trust/)了解您需要的信任类型。

## 步骤2：从想法到代码：本地测试和部署您的AVS

以下部分涵盖了AVS需要构建的最小智能合约集成和部署脚本集，以便：
1. 被视为用于演示和概念验证目的的功能完整的AVS。
2. 准备您的AVS以集成即将发布的惩罚功能。

:::info
要开始以下过程，请fork这个示例仓库[hello-world-avs](https://github.com/Layr-Labs/hello-world-avs)。
:::

### 智能合约要求

**1: 与EigenLayer核心（AVS目录）集成**  
实现ECDSAServiceManagerBase或ServiceManagerBase（BLS）的实例。  
请参见hello-world-avs的[示例](https://github.com/Layr-Labs/hello-world-avs/blob/master/contracts/src/HelloWorldServiceManager.sol)和incredible-squaring-avs的[示例](https://github.com/Layr-Labs/incredible-squaring-avs/blob/master/contracts/src/IncredibleSquaringServiceManager.sol)。

**2: 链上验证**  
实现至少一个链上可证明的事件。最常见的方法是在链上写入ECDSA或BLS聚合签名（APK）。
请参见incredible-squaring-avs的[示例](https://github.com/Layr-Labs/incredible-squaring-avs/blob/8bd0ac663dcc2289cad02af4a7f0002ea07bc1d8/contracts/src/IncredibleSquaringTaskManager.sol#L102)和hello-world-avs的[示例](https://github.com/Layr-Labs/hello-world-avs/blob/84ae1974c212c193a3992467f7d431bad39f74a3/src/index.ts#L130)。

### 合约部署要求

实现合约部署脚本，以部署到您的[本地anvil节点](https://book.getfoundry.sh/reference/anvil/)。

**1: 部署EigenLayer合约和状态**  
请参见hello-world-avs的[示例](https://github.com/Layr-Labs/hello-world-avs/blob/master/utils/anvil/deploy-eigenlayer-save-anvil-state.sh)。

**2: 部署您的AVS合约**  
请参见hello-world-avs的forge部署脚本[示例](https://github.com/Layr-Labs/hello-world-avs/blob/master/contracts/script/HelloWorldDeployer.s.sol)和bash部署脚本[示例](https://github.com/Layr-Labs/hello-world-avs/blob/master/utils/anvil/deploy-eigenlayer-save-anvil-state.sh)。

### 运营商（链下）要求

**1: 运营商注册到AVS**  
提供一种机制让运营商注册到AVS。

请参见hello-world-avs的[示例](https://github.com/Layr-Labs/hello-world-avs/blob/84ae1974c212c193a3992467f7d431bad39f74a3/src/index.ts#L41)。

**2: 至少一个事件写入您的AVS链上合约**  
运营商二进制文件（或链下聚合服务代码）必须至少向AVS链上合约写入一个事件，以用于未来的链上验证、奖励和惩罚目的。

请参见hello-world-avs的[示例](https://github.com/Layr-Labs/hello-world-avs/blob/84ae1974c212c193a3992467f7d431bad39f74a3/src/index.ts#L25)。

## 步骤3：准备和部署到测试网

1. 以便于运营商启动的方式打包运营商的长期运行可执行文件（通过二进制文件、docker容器或类似方式）。

2. 编写测试网用户和运营商文档，包括：
   - 信任建模：向用户澄清您架构中的任何信任假设。识别受信任（中心化）和不受信任（去中心化，无需信任）的组件。
   - 运营商安装、注册、注销说明。
   - 最终用户（即"消费者"）使用您的AVS服务的说明。
   - 将用于AVS升级的通信渠道。
   - 描述可用的运营商监控工具，如GraFana仪表板、日志文件或类似工具。

3. 遵循[AVS开发者安全最佳实践](./avs-developer-best-practices.md)。

4. 遵循[开发者密钥管理注意事项](./key-management/developers.md)中的指导。

5. 为您的运营商可执行包实现[节点规范](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/spec/intro)。

6. 遵循[测试网仪表板入门说明](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-dashboard-onboarding)。

7. 按照[这里](./rewards.md)的说明实现奖励分配。

## 步骤4：准备和部署到主网

1. 智能合约审计：让至少2-3家信誉良好的审计公司审计您的代码库。
2. 完成用户和运营商文档。
3. 遵循[主网仪表板入门说明](https://docs.eigenlayer.xyz/eigenlayer/avs-guides/avs-dashboard-onboarding)。

## 联系我们

一旦您有了想在EigenLayer上构建的想法，请提交一份[AVS问卷](https://bit.ly/avsquestions)并与我们联系。

作用说明：

1. 这个文档为开发者提供了一个全面的指南，介绍了如何在EigenLayer上构建和部署AVS（主动验证服务）。

2. 它分为四个主要步骤，从学习基础知识到最终部署到主网，每个步骤都提供了详细的说明和资源链接。

3. 文档强调了智能合约集成、本地测试、链上验证等关键技术要求，这对于确保AVS的功能性和安全性至关重要。

4. 它还提供了许多实际的代码示例和参考，这对开发者实际实现AVS非常有帮助。

5. 文档还涵盖了文档编写、安全最佳实践、密钥管理等非技术但同样重要的方面。

6. 最后，它鼓励开发者与EigenLayer团队联系，这有助于生态系统的发展和创新。

总的来说，这个文档为开发者提供了一个结构化的路线图，指导他们从概念到实际部署的整个AVS开发过程。




