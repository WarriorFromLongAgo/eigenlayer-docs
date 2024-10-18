以下是该文档的中文翻译，以及相关作用的说明：

---
sidebar_position: 1
title: AVS 概述
---

在深入了解AVS是什么以及如何设计和构建之前，请查看[EigenLayer简介](https://docs.eigenlayer.xyz/eigenlayer/overview/)概述，快速熟悉质押者和运营商的概念。

此外，请查看以下资源：

<div class="button1-container">
    <Button label="AVS 手册" link="https://eigenlabs.gitbook.io/avs-book" />
    <Button label="GO SDK" link="https://github.com/Layr-Labs/eigensdk-go" />
    <Button label="Rust SDK" link="https://github.com/Layr-Labs/eigensdk-rs" />
    <Button label="Awesome AVS" link="https://github.com/Layr-Labs/awesome-avs" />
</div>

<p className="button1-container-heading">或与我们的团队联系：</p>

<div class="button1-container">
    <Button label="BuildOnEigen 群聊" link="https://t.me/+LsjfhgFoHJEyN2Rh" />
    <Button label="安排通话" link="https://share.hsforms.com/1BksFoaPjSk2l3pQ5J4EVCAein6l" />
</div>

## 什么是AVS？

AVS是任何需要自己的分布式验证语义进行验证的系统，如侧链、数据可用性层、新的虚拟机、keeper网络、预言机网络、桥接器、门限加密方案、可信执行环境等。

每个AVS都有自己的一组合约，用于保存与服务功能相关的状态，如哪些运营商正在运行服务以及有多少质押保证金在保护服务。

以下是EigenLayer核心合约的高级概述，以及AVS如何构建在其上并被使用。

![AVS架构概述](/img/avs/avs-architecture-v1.png)

让我们澄清上图所示的一些交互。

- 质押者通过将资产存入`StrategyManager`与EigenLayer交互。要了解更多关于这是如何工作的，[这个文档](https://github.com/Layr-Labs/eigenlayer-contracts/blob/master/docs/core/StrategyManager.md)提供了对`StrategyManager`的深入解释。
- 质押者还通过选择要委托的运营商与EigenLayer交互。这种委托由`DelegationManager`处理，你可以在[这里](https://github.com/Layr-Labs/eigenlayer-contracts/blob/master/docs/core/DelegationManager.md)找到深入的解释。
- 运营商是运行特定于他们选择服务的AVS的链下客户端软件的参与者。这个客户端软件独立于核心EigenLayer协议。运营商必须通过EigenLayer的`DelegationManager`合约进行注册/注销过程才能成为EigenLayer运营商。运营商注册是这些运营商选择加入AVS并为其服务的先决条件。关于注册如何工作，请参考[这个文档](https://docs.eigenlayer.xyz/eigenlayer/operator-guides/operator-introduction)。
- 虚线框表示的组件是可选的，取决于接口设计。
- 每个AVS开发者可以根据需要设计和实现自己的合约，只要他们的入口点（通常称为`ServiceManager`）实现了EigenLayer协议期望的接口。关于这方面的具体细节将很快公布。

## 额外资源

访问[awesome-avs](https://github.com/Layr-Labs/awesome-avs)查看构建AVS的学习资源和最新的社区贡献。

## 联系我们

一旦你有了想在EigenLayer上构建的想法，请提交一份[AVS问卷](https://bit.ly/avsquestions)并与我们联系。

作用说明：

1. 本文档为AVS（主动验证服务）开发者提供了一个全面的概述，介绍了AVS的概念、架构和开发流程。

2. 它提供了多个重要资源的链接，包括SDK、文档和社区资源，帮助开发者快速入门和深入学习。

3. 文档解释了AVS在EigenLayer生态系统中的角色和重要性，包括与质押者和运营商的交互。

4. 通过图表和详细说明，文档清晰地展示了AVS的架构和与EigenLayer核心合约的关系。

5. 它强调了AVS开发的灵活性，同时也指出了需要遵循的关键接口要求。

6. 文档鼓励开发者与EigenLayer团队联系，提供了多个沟通渠道，促进了生态系统的发展和创新。

总的来说，这个文档为有意开发AVS的开发者提供了一个全面的起点，涵盖了从概念理解到实际开发的各个方面，有助于推动EigenLayer生态系统的扩展和多样化。