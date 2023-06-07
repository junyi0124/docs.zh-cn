---
title: 如何：使用数据保护
description: 了解如何通过访问 .NET 中的数据保护 API (DPAPI) 来使用数据保护。
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DPAPI
- encryption [.NET], data protection API
- data [.NET], decryption
- ProtectedMemory class, about ProtectedMemory class
- ProtectedData class, about ProtectedData class
- cryptography [.NET], data protection API
- data protection API [.NET]
- decryption
- data [.NET], encryption
ms.assetid: 606698b0-cb1a-42ca-beeb-0bea34205d20
ms.openlocfilehash: ed1b18e2c6456b53559e8fb7e989f148fefd35c7
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94820093"
---
# <a name="how-to-use-data-protection"></a>如何：使用数据保护

> [!NOTE]
> 本文适用于 Windows。
>
> 有关 ASP.NET Core 的信息，请参阅 [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)。

.NET 提供对数据保护 API 的访问 (DPAPI) ，这允许你使用来自当前用户帐户或计算机的信息加密数据。  当使用 DPAPI 时，你会使显式生成和存储加密密钥的困难问题得到缓解。  
  
使用 <xref:System.Security.Cryptography.ProtectedData> 类来加密字节数组的副本。 此功能在 .NET Framework、.NET Core 和 .NET 5 中可用。  你可以指定由当前用户帐户加密的数据仅能通过相同的用户帐户解密，也可以指定由当前用户帐户加密的数据可以通过计算机上的任何帐户进行解密。  请参阅 <xref:System.Security.Cryptography.DataProtectionScope> 枚举以获取 <xref:System.Security.Cryptography.ProtectedData> 选项的详细说明。  
  
## <a name="encrypt-data-to-a-file-or-stream-using-data-protection"></a>使用数据保护将数据加密到文件或流  
  
1. 创建随机平均信息量。  
  
2. 调用静态 <xref:System.Security.Cryptography.ProtectedData.Protect%2A> 方法，同时传递要加密的字节数组、平均信息量和数据保护范围。  
  
3. 将加密的数据写入文件或流。  
  
### <a name="to-decrypt-data-from-a-file-or-stream-using-data-protection"></a>若要使用数据保护从文件或流解密数据  
  
1. 从文件或流中读取加密的数据。  
  
2. 调用静态 <xref:System.Security.Cryptography.ProtectedData.Unprotect%2A> 方法，同时传递要解密的字节数组和数据保护范围。  
  
## <a name="example"></a>示例

下面的代码示例演示加密和解密的两种形式。  首先，该代码示例对内存中的字节数组进行加密，然后将其解密。  接下来，该代码示例对字节数组的副本进行加密、将其保存到文件、从文件加载回数据，然后对数据进行解密。  此示例显示原始数据、加密的数据和已解密的数据。

[!code-csharp[DPAPI-HowTO#1](../../../samples/snippets/csharp/VS_Snippets_CLR/DPAPI-HowTO/cs/sample.cs#1)]
[!code-vb[DPAPI-HowTO#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/DPAPI-HowTO/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  

此示例仅在面向 .NET Framework 并在 Windows 上运行时才会编译和运行。

- 包括对 `System.Security.dll` 的引用。  
  
- 包括 <xref:System>、<xref:System.IO>、<xref:System.Security.Cryptography> 和 <xref:System.Text> 命名空间。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.ProtectedMemory>
- <xref:System.Security.Cryptography.ProtectedData>
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
