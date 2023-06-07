---
title: 如何：用对称密钥对 XML 元素进行加密
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- AES algorithm
- cryptography [.NET], symmetric keys
- encryption [.NET], symmetric keys
- symmetric keys
- System.Security.Cryptography.EncryptedXml class
- System.Security.Cryptography.Aes class
- XML encryption
- Advanced Encryption Standard algorithm
ms.assetid: d8461a44-aa2c-4ef4-b3e4-ab7cbaaee1b5
ms.openlocfilehash: cfda1835a1390b025f2ee0509a91c59104a77ae9
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94820223"
---
# <a name="how-to-encrypt-xml-elements-with-symmetric-keys"></a>如何：用对称密钥对 XML 元素进行加密

可以使用 <xref:System.Security.Cryptography.Xml> 命名空间中的类加密 XML 文档内的元素。  XML 加密可用于存储或传输敏感 XML，而无需担心数据被轻易读取。  此过程使用高级加密标准 (AES) 算法对 XML 元素进行加密。  
  
 有关如何解密使用此过程加密的 XML 元素的信息，请参阅 [如何：使用对称密钥对 Xml 元素进行解密](how-to-decrypt-xml-elements-with-symmetric-keys.md)。  
  
 当使用对称算法（如 AES）来加密 XML 数据时，必须使用相同的密钥来加密和解密 XML 数据。  此过程中的示例假定加密的 XML 将使用相同的密钥进行解密，且加密方和解密方就要使用的算法和密钥达成了一致。  此示例不对加密的 XML 内的 AES 密钥进行存储或加密。  
  
 此示例适用于以下情况：单个应用程序需要根据内存中存储的会话密钥加密数据，或根据从密码派生而来的加密型强密钥进行加密。  对于两个或多个应用程序需要共享加密的 XML 数据的情况，请考虑使用基于非对称算法或 X.509 证书的加密方案。  
  
### <a name="to-encrypt-an-xml-element-with-a-symmetric-key"></a>使用对称密钥加密 XML 元素  
  
1. 使用 <xref:System.Security.Cryptography.Aes> 类生成对称密钥。  此密钥将用于加密 XML 元素。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#2)]
     [!code-vb[HowToEncryptXMLElementSymmetric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#2)]  
  
2. 通过从磁盘加载 XML 文件来创建 <xref:System.Xml.XmlDocument> 对象。  <xref:System.Xml.XmlDocument> 对象包含要加密的 XML 元素。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#3)]
     [!code-vb[HowToEncryptXMLElementSymmetric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#3)]  
  
3. 在 <xref:System.Xml.XmlDocument> 对象中查找指定元素，并创建一个新的 <xref:System.Xml.XmlElement> 对象来表示想要加密的元素。  在此示例中，加密了 `"creditcard"` 元素。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#4)]
     [!code-vb[HowToEncryptXMLElementSymmetric#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#4)]  
  
4. 创建 <xref:System.Security.Cryptography.Xml.EncryptedXml> 类的新实例，并将其与对称密钥一起使用来加密 <xref:System.Xml.XmlElement>。  <xref:System.Security.Cryptography.Xml.EncryptedXml.EncryptData%2A> 方法以加密的字节数组的形式返回加密元素。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#5)]
     [!code-vb[HowToEncryptXMLElementSymmetric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#5)]  
  
5. 构造一个 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象并对其填充 XML 加密元素的 URL 标识符。  此 URL 标识符可使解密方知道 XML 包含一个加密元素。  可使用 <xref:System.Security.Cryptography.Xml.EncryptedXml.XmlEncElementUrl> 字段来指定 URL 标识符。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#6)]
     [!code-vb[HowToEncryptXMLElementSymmetric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#6)]  
  
6. 创建一个 <xref:System.Security.Cryptography.Xml.EncryptionMethod> 对象，将它初始化为用于生成密钥的加密算法的 URL 标识符。  将 <xref:System.Security.Cryptography.Xml.EncryptionMethod> 对象传递给 <xref:System.Security.Cryptography.Xml.EncryptedType.EncryptionMethod%2A> 属性。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#7)]
     [!code-vb[HowToEncryptXMLElementSymmetric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#7)]  
  
7. 将加密的元素数据添加到 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#8)]
     [!code-vb[HowToEncryptXMLElementSymmetric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#8)]  
  
8. 将原始 <xref:System.Xml.XmlDocument> 对象中的元素替换为 <xref:System.Security.Cryptography.Xml.EncryptedData> 元素。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#9)]
     [!code-vb[HowToEncryptXMLElementSymmetric#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#9)]  
  
## <a name="example"></a>示例  
  
```xml  
<root>  
    <creditcard>  
        <number>19834209</number>  
        <expiry>02/02/2002</expiry>  
    </creditcard>  
</root>  
```  
  
 [!code-csharp[HowToEncryptXMLElementSymmetric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#1)]
 [!code-vb[HowToEncryptXMLElementSymmetric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 在面向 .NET Framework 的项目中，包含对的引用 `System.Security.dll` 。

- 在面向 .NET Core 或 .NET 5 的项目中，安装 NuGet 包 [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml)。
  
- 包括以下命名空间：<xref:System.Xml>、<xref:System.Security.Cryptography> 和 <xref:System.Security.Cryptography.Xml>。  
  
## <a name="net-security"></a>.NET 安全性

永远不要以纯文本形式存储加密密钥，也不要以纯文本形式在计算机之间传输密钥。  请转而使用安全的密钥容器来存储加密密钥。  
  
当你使用加密密钥执行操作后，通过将每个字节设置为零或通过调用托管加密类的 <xref:System.Security.Cryptography.SymmetricAlgorithm.Clear%2A> 方法来将它从内存中清除。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md) -介绍如何在基类库中实现加密。
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.Xml>
- [如何：用对称密钥对 XML 元素进行解密](how-to-decrypt-xml-elements-with-symmetric-keys.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
