---
title: 如何：访问硬件加密设备
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- encryption
- key card
- cryptography
- hardware encryption
- CspParameters
ms.assetid: b0e734df-6eb4-4b16-b48c-6f0fe82d5f17
ms.openlocfilehash: c4f0b77985dea1818729af41b69a4a7d30427415
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829890"
---
# <a name="how-to-access-hardware-encryption-devices"></a>如何：访问硬件加密设备

> [!NOTE]
> 本文适用于 Windows。

可使用 <xref:System.Security.Cryptography.CspParameters> 类来访问硬件加密设备。 例如，可以使用此类将你的应用程序与智能卡、硬件随机数字生成器或特定加密算法的硬件实现进行集成。  

<xref:System.Security.Cryptography.CspParameters> 类会创建一个加密服务提供程序 (CSP)，该程序可访问正确安装的硬件加密设备。  你可以通过使用注册表编辑器 (Regedit.exe) 检查下列注册表项来验证 CSP 的可用性：HKEY_LOCAL_MACHINE\Software\Microsoft\Cryptography\Defaults\Provider。  
  
### <a name="to-sign-data-using-a-key-card"></a>使用密钥卡对数据进行签名  
  
1. 通过将整数提供程序类型和提供程序名称传递给构造函数，创建 <xref:System.Security.Cryptography.CspParameters> 类的新实例。  
  
2. 将适当的标志传递给新创建的 <xref:System.Security.Cryptography.CspParameters> 对象的 <xref:System.Security.Cryptography.CspParameters.Flags%2A> 属性。  例如，传递 <xref:System.Security.Cryptography.CspProviderFlags.UseDefaultKeyContainer> 标志。  
  
3. 通过将 <xref:System.Security.Cryptography.CspParameters> 对象传递给构造函数，创建 <xref:System.Security.Cryptography.AsymmetricAlgorithm> 类（如 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 类）的新实例。  
  
4. 使用其中一种 `Sign` 方法对你的数据进行签名，并使用其中一种 `Verify` 方法来验证你的数据。  
  
### <a name="to-generate-a-random-number-using-a-hardware-random-number-generator"></a>使用硬件随机数字生成器生成随机数  
  
1. 通过将整数提供程序类型和提供程序名称传递给构造函数，创建 <xref:System.Security.Cryptography.CspParameters> 类的新实例。  
  
2. 通过将 <xref:System.Security.Cryptography.CspParameters> 对象传递给构造函数，创建 <xref:System.Security.Cryptography.RNGCryptoServiceProvider> 的新实例。  
  
3. 使用 <xref:System.Security.Cryptography.RNGCryptoServiceProvider.GetBytes%2A> 或 <xref:System.Security.Cryptography.RNGCryptoServiceProvider.GetNonZeroBytes%2A> 方法创建一个随机值。  
  
## <a name="example"></a>示例

下列代码示例演示如何使用智能卡对数据进行签名。  此代码示例创建一个公开智能卡的 <xref:System.Security.Cryptography.CspParameters> 对象，然后使用 CSP 初始化 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 对象。  然后此代码示例会对某些数据进行签名和验证。  

由于 SHA1 出现冲突，我们建议 SHA256 或更好。
  
[!code-cpp[Cryptography.SmartCardCSP#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Cryptography.SmartCardCSP/CPP/Cryptography.SmartCardCSP.cpp#1)]
[!code-csharp[Cryptography.SmartCardCSP#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Cryptography.SmartCardCSP/CS/example.cs#1)]
[!code-vb[Cryptography.SmartCardCSP#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Cryptography.SmartCardCSP/VB/example.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 包括 <xref:System> 和 <xref:System.Security.Cryptography> 命名空间。  
  
- 计算机上必须安装有智能卡读卡器和驱动程序。  
  
- 必须使用特定于读卡器的信息来初始化 <xref:System.Security.Cryptography.CspParameters> 对象。  有关详细信息，请参阅读卡器的文档。

## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
