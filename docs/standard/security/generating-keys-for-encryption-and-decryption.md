---
title: 生成加密和解密的密钥
description: 了解如何创建和管理用于 .NET 中的加密和解密的对称和非对称密钥。
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- keys, encryption and decryption
- keys, asymmetric
- keys, symmetric
- encryption [.NET], keys
- symmetric keys
- asymmetric keys [.NET]
- cryptography [.NET], keys
ms.assetid: c197dfc9-a453-4226-898d-37a16638056e
ms.openlocfilehash: 2af54cef4f233b7bcae5c476f1aa49fdbf7ef2fc
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95689714"
---
# <a name="generating-keys-for-encryption-and-decryption"></a>生成加密和解密的密钥

创建和管理密钥是加密过程的一个重要部分。 对称算法要求创建密钥和初始化向量 (IV)。 密钥必须对不应解密数据的任何人保密。 IV 并不是一定要保密，但应定期更改。 非对称算法要求创建公钥和私钥。 公钥可以公开给任何人，而私钥必须只有将解密用公钥加密的数据的参与方知道。 本节介绍如何生成和管理对称和非对称算法的密钥。  
  
## <a name="symmetric-keys"></a>对称密钥  

 .NET 提供的对称加密类需要一个密钥和新的初始化向量 (IV) 来加密和解密数据。 每当使用无参数方法创建一个托管对称加密类的新实例时，都会 `Create()` 自动创建新的密钥和 IV。 获许解密你的数据的任何人都必须具有相同的密钥和 IV 并使用相同的算法。 通常情况下，应为每个会话创建一个新的密钥和 IV，且不应为了在稍后会话中使用而存储该密钥和 IV。  
  
 为了将对称密钥和 IV 传送给远程方，通常使用不对称加密来加密对称密钥。 通过不安全的网络发送密钥而不对其进行加密会很不安全，这是因为截获密钥和 IV 的任何人都能够解密您的数据。  
  
 下面的示例演示如何创建算法的默认实现类的新实例 <xref:System.Security.Cryptography.Aes> 。  
  
```vb  
Dim aes As Aes = Aes.Create()  
```  
  
```csharp  
Aes aes = Aes.Create();  
```  
  
 执行前面的代码后，将生成新的密钥和 IV，并分别置于 **Key** 和 **IV** 属性中。  
  
 有时可能需要生成多个密钥。 在这种情况下，你可以创建会实现对称算法的类的新实例，然后通过调用 **GenerateKey** 和 **GenerateIV** 方法来创建新的密钥和 IV。 下面的代码示例演示了如何在创建了对称加密类的新实例后创建新的密钥和 IVs。  
  
```vb  
Dim aes As Aes = Aes.Create()  
aes.GenerateIV()  
aes.GenerateKey()  
```  
  
```csharp  
Aes aes = Aes.Create();  
aes.GenerateIV();  
aes.GenerateKey();  
```  
  
 执行前面的代码时，在新的 **Aes** 实例发出时，将生成一个密钥和 IV。 调用 **GenerateKey** 和 **GenerateIV** 方法时会创建另一个密钥和 IV。
  
## <a name="asymmetric-keys"></a>非对称密钥

 .NET <xref:System.Security.Cryptography.RSA> 为非对称加密提供了类。 使用无参数 `Create()` 方法创建新的实例时，此类将创建一个公钥/私钥对。 可以存储非对称密钥以用于多个会话，也可以生成它以仅用于一个会话。 虽然公钥可以普遍可用，但私钥应受到严密保护。  
  
 每次创建非对称算法类的新实例时，都将生成一个公钥/私钥对。 创建类的新实例后，可以使用方法提取密钥信息 <xref:System.Security.Cryptography.RSA.ExportParameters%2A> ，该方法将返回 <xref:System.Security.Cryptography.RSAParameters> 存储密钥信息的结构。 方法接受一个布尔值，该值指示是只返回公钥信息还是同时返回公钥和私钥信息。

还有其他方法可让你提取关键信息，例如：

* <xref:System.Security.Cryptography.RSA.ExportRSAPublicKey%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.RSA.ExportRSAPrivateKey%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.AsymmetricAlgorithm.ExportSubjectPublicKeyInfo%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.AsymmetricAlgorithm.ExportPkcs8PrivateKey%2A?displayProperty=nameWithType>
* <xref:System.Security.Cryptography.AsymmetricAlgorithm.ExportEncryptedPkcs8PrivateKey%2A?displayProperty=nameWithType>

可以通过使用方法，将 **RSA** 实例初始化为 **RSAParameters** 结构的值 <xref:System.Security.Cryptography.RSA.ImportParameters%2A> 。 或使用方法创建一个新的实例 <xref:System.Security.Cryptography.RSA.Create(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType> 。  
  
 非对称私钥永远不应以原义或纯文本形式存储在本地计算机上。 如果需要存储私钥，则应使用密钥容器。 有关如何在密钥容器中存储私钥的详细信息，请参阅 [如何：将非对称密钥存储在密钥容器中](how-to-store-asymmetric-keys-in-a-key-container.md)。  
  
 下面的代码示例创建 **RSA** 类的新实例，创建公钥/私钥对，并将公钥信息保存到 **RSAParameters** 结构。  
  
```vb  
'Generate a public/private key pair.  
Dim rsa as RSA = RSA.Create()  
'Save the public key information to an RSAParameters structure.  
Dim rsaKeyInfo As RSAParameters = rsa.ExportParameters(false)  
```  
  
```csharp  
//Generate a public/private key pair.  
RSA rsa = RSA.Create();  
//Save the public key information to an RSAParameters structure.  
RSAParameters rsaKeyInfo = rsa.ExportParameters(false);  
```  
  
## <a name="see-also"></a>另请参阅

- [加密数据](encrypting-data.md)
- [解密数据](decrypting-data.md)
- [加密服务](cryptographic-services.md)
- [如何：将非对称密钥存储在密钥容器中](how-to-store-asymmetric-keys-in-a-key-container.md)
- [跨平台加密](cross-platform-cryptography.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
