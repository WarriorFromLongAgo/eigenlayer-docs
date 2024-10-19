这个文件是EigenLayer运营商的常见问题解答(FAQ)。以下是文件内容的分析和翻译：

分析：
1. 文件回答了关于元数据URL和logo的托管要求。
2. 解释了如何处理密钥丢失的情况。
3. 说明了如何找到运营商地址。
4. 提供了更改加密密钥密码的方法。
5. 解释了如何停用/注销运营商。
6. 回答了关于运营商可以选择加入的AVS数量限制问题。
7. 提到目前不支持轮换现有运营商的密钥。

翻译：

---
sidebar_position: 4
title: 运营商常见问题
---

#### 我是否需要公开托管元数据URL？

是的。您需要公开托管元数据URL。`元数据`URL应始终可用，并应返回正确的json响应，如[这个例子](https://holesky-operator-metadata.s3.amazonaws.com/metadata.json)。

#### 我是否需要在元数据json中公开托管logo？

是的。您需要公开托管logo，如[这个例子](https://holesky-operator-metadata.s3.amazonaws.com/eigenlayer.png)。

#### logo图片有任何限制吗？

是的。我们只支持`.png`格式，并且我们严格检查图片内容。如果您的图片不满足要求，EigenLayer应用将不会显示您运营商的logo。

#### 如果我丢失了密钥访问权限怎么办？

当您首次[创建/导入](../operator-guides/operator-installation#create-and-list-keys)密钥时，系统会要求输入密码来加密密钥，创建后还会显示明文私钥。请确保备份私钥和密码。如果两者都丢失，您将无法找回密钥。如果您丢失了明文私钥但仍有密码，可以运行[导出](../operator-guides/operator-installation.md#export-keys)命令获取明文私钥。

#### 我的运营商地址是什么？

在[创建/导入](../operator-guides/operator-installation#create-and-list-keys)ecdsa密钥后，您将看到以下日志消息：

```
startLine: 24
endLine: 31
```

您的运营商地址是日志中的`Ethereum Address`。

#### 如果我想更改加密密钥的密码怎么办？

如果您想更改加密密钥的密码，根据您可用的信息，有两个选择：

1. 如果您知道私钥，可以直接[重新导入](../operator-guides/operator-installation.md#import-keys)，导入时选择不同的名称和新密码。
2. 如果您不知道私钥，可以使用[导出](../operator-guides/operator-installation.md#export-keys)获取。获得私钥后，可以使用选项1重新导入。

#### 如果我想停用/注销我在EigenLayer的运营商怎么办？

目前没有办法注销您的运营商，但您可以将元数据URL中的运营商名称更新为"已停用"或类似内容。这将有助于在网页应用中显示您的运营商为非活动状态。

#### 运营商可以选择加入的AVS数量有限制吗？

运营商可以选择加入的AVS数量没有限制。但是，运营商需要确保他们有足够的基础设施容量来支持他们选择加入的AVS。

#### 为现有运营商轮换密钥的过程是什么？我如何重新注册并将质押转移到新密钥？

目前不支持此操作。