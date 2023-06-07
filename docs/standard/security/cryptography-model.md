---
title: .NET 加密模型
description: 查看 .NET 中的常用加密算法实现。 了解对象继承、流设计、& 配置的可扩展加密模型。
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [.NET], model
- encryption [.NET], model
ms.assetid: 12fecad4-fbab-432a-bade-2f05976a2971
ms.openlocfilehash: f9ec08992cb8db8f81f11de661612e1b7d15131c
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94831112"
---
# <a name="net-cryptography-model"></a>.NET 加密模型

.NET 提供许多标准加密算法的实现，.NET 加密模型是可扩展的。

## <a name="object-inheritance"></a>对象继承

.NET 加密系统实现了派生类继承的可扩展模式。 层次结构如下所示：

- 算法类型类，如 <xref:System.Security.Cryptography.SymmetricAlgorithm> 、  <xref:System.Security.Cryptography.AsymmetricAlgorithm> 或 <xref:System.Security.Cryptography.HashAlgorithm> 。 此级别是抽象的。

- 从算法类型类继承的算法类；例如，<xref:System.Security.Cryptography.Aes>、<xref:System.Security.Cryptography.RSA> 或 <xref:System.Security.Cryptography.ECDiffieHellman>。 此级别是抽象的。

- 从算法类继承的算法类实现；例如，<xref:System.Security.Cryptography.AesManaged>、<xref:System.Security.Cryptography.RC2CryptoServiceProvider> 或 <xref:System.Security.Cryptography.ECDiffieHellmanCng>。 此级别已完全实现。

这种派生类模式允许您添加新算法或现有算法的新实现。 例如，要创建新的公共密钥算法，你会继承 <xref:System.Security.Cryptography.AsymmetricAlgorithm> 类。 要创建特定算法的新实现，将需要创建该算法的非抽象派生类。

## <a name="how-algorithms-are-implemented-in-net"></a>如何在 .NET 中实现算法

作为可用于一种算法的不同实现的示例，请考虑对称算法。 所有对称算法的基数都是 <xref:System.Security.Cryptography.SymmetricAlgorithm> ，它由 <xref:System.Security.Cryptography.Aes> 、和继承， <xref:System.Security.Cryptography.TripleDES> 并且不再推荐使用。

<xref:System.Security.Cryptography.Aes> 由 <xref:System.Security.Cryptography.AesCryptoServiceProvider> 、和继承 <xref:System.Security.Cryptography.AesCng> <xref:System.Security.Cryptography.AesManaged> 。

在 Windows .NET Framework 中：

* `*CryptoServiceProvider` 算法类（如 <xref:System.Security.Cryptography.AesCryptoServiceProvider> ）是 (CAPI) 实现算法的 Windows 加密 API 的包装器。
* `*Cng` 算法类（如 <xref:System.Security.Cryptography.ECDiffieHellmanCng> ）是围绕 Windows 加密下一代 (CNG) 实现的包装。
* `*Managed` 类（如 <xref:System.Security.Cryptography.AesManaged> ）完全用托管代码编写而成。 `*Managed` 实现不是由 (FIPS) 的联邦信息处理标准认证的，可能比 `*CryptoServiceProvider` 和 `*Cng` 包装类更慢。

在 .NET Core 和 .NET 5 及更高版本中，所有实现类 (`*CryptoServiceProvider` 、 `*Managed` 和 `*Cng`) 都是操作系统 (OS) 算法的包装器。 如果 OS 算法经过 FIPS 认证，则 .NET 将使用 FIPS 认证的算法。 有关详细信息，请参阅 [跨平台加密](cross-platform-cryptography.md)。

大多数情况下，不需要直接引用算法实现类，如 `AesCryptoServiceProvider` 。 通常需要的方法和属性在基本算法类上，如 `Aes` 。 使用基算法类的工厂方法创建默认实现类的实例，并引用基本算法类。 例如，请参阅以下示例中突出显示的代码行：

:::code language="csharp" source="snippets/encrypting-data/csharp/aes-encrypt.cs" highlight="16":::
:::code language="vb" source="snippets/encrypting-data/vb/aes-encrypt.vb" highlight="12":::

## <a name="cryptographic-configuration"></a>加密配置

加密配置允许你将算法的特定实现解析到算法名称，从而允许 .NET 加密类的可扩展性。 可以添加自己算法的硬件或软件实现，并将该实现映射到所选择的算法名称。 如果配置文件中未指定算法，则使用默认设置。

## <a name="choosing-an-algorithm"></a>选择算法

可以出于各种原因选择一种算法：例如，为了保护数据完整性，为了保护数据隐私或为了生成密钥。 为了保护完整性（防止更改）或保护隐私（防止查看），对称算法和哈希算法用于保护数据。 哈希算法主要用于保护数据完整性。

下面是按应用程序分类的建议算法列表：

- 数据隐私：
  - <xref:System.Security.Cryptography.Aes>
- 数据完整性：
  - <xref:System.Security.Cryptography.HMACSHA256>
  - <xref:System.Security.Cryptography.HMACSHA512>
- 数字签名：
  - <xref:System.Security.Cryptography.ECDsa>
  - <xref:System.Security.Cryptography.RSA>
- 密钥交换：
  - <xref:System.Security.Cryptography.ECDiffieHellman>
  - <xref:System.Security.Cryptography.RSA>
- 随机数生成：
  - <xref:System.Security.Cryptography.RandomNumberGenerator.Create%2A?displayProperty=nameWithType>
- 从密码生成密钥：
  - <xref:System.Security.Cryptography.Rfc2898DeriveBytes>

## <a name="see-also"></a>另请参阅

- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
