以下是该文档的中文翻译，以及相关作用的说明：

---
sidebar_position: 9
title: 无许可代币策略
---

# 无许可代币策略

无许可代币支持使任何ERC-20代币都可以无需许可地被添加为可重新质押的资产，这显著扩大了可以为去中心化网络安全做出贡献的资产范围，并解锁了ERC-20代币在EigenLayer上的加密经济安全性。

有了这种能力，AVS可以选择接受任何ERC-20代币作为重新质押的资产，为其AVS提供加密经济安全性。这允许AVS评估所有可用代币的供应和效用，创建跨生态系统的合作伙伴关系，同时确保其服务的安全性。这增加了整个生态系统的一致性和连通性，使我们更接近开放创新的最终目标。

# 添加新策略

要向EigenLayer协议添加新策略：

* 调用StrategyFactory.deployNewStrategy()。
* 您的策略现在可以与您的AVS关联。

更多详细信息，请参阅[此处](https://github.com/Layr-Labs/eigenlayer-contracts/blob/dev/docs/core/StrategyManager.md#strategyfactorydeploynewstrategy)的合约文档。

作用说明：

1. 这个文档介绍了EigenLayer的无许可代币策略，这是一个重要的功能，允许更多类型的代币参与到EigenLayer的生态系统中。

2. 它解释了这个功能如何扩大可用于网络安全的资产范围，从而增强整个生态系统的安全性和互操作性。

3. 文档提供了如何添加新策略的简单步骤，这对于想要将自己的代币集成到EigenLayer中的开发者来说非常有用。

4. 通过引用相关的合约文档，它为开发者提供了更深入了解技术细节的途径。

5. 整体而言，这个文档强调了EigenLayer的灵活性和可扩展性，鼓励更广泛的生态系统参与和创新。

关于代码引用：


```417:448:eigenda/common/abis/EigenDAServiceManager.json
    {
        "type": "function",
        "name": "getOperatorRestakedStrategies",
        "inputs": [
            {
                "name": "operator",
                "type": "address",
                "internalType": "address"
            }
        ],
        "outputs": [
            {
                "name": "",
                "type": "address[]",
                "internalType": "address[]"
            }
        ],
        "stateMutability": "view"
    },
    {
        "type": "function",
        "name": "getRestakeableStrategies",
        "inputs": [],
        "outputs": [
            {
                "name": "",
                "type": "address[]",
                "internalType": "address[]"
            }
        ],
        "stateMutability": "view"
    },
```


这部分代码定义了两个重要的函数接口：`getOperatorRestakedStrategies`和`getRestakeableStrategies`。这些函数与文档中描述的无许可代币策略直接相关，允许查询运营商的重新质押策略和可重新质押的策略。