---
title: 如何：用对称密钥对 XML 元素进行解密
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- symmetric keys
- System.Security.Cryptography.EncryptedXml class
- System.Security.Cryptography.Aes class
- XML encryption
- decryption
ms.assetid: 6038aff0-f92c-4e29-a618-d793410410d8
ms.openlocfilehash: 67ace547fc539ab0a2d7affb339f908eb9670a29
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729351"
---
# <a name="how-to-decrypt-xml-elements-with-symmetric-keys"></a>如何：用对称密钥对 XML 元素进行解密

可以使用 <xref:System.Security.Cryptography.Xml> 命名空间中的类加密 XML 文档内的元素。  XML 加密可用于存储或传输敏感 XML，而无需担心数据被轻易读取。  此代码示例使用高级加密标准 (AES) 算法对 XML 元素进行解密。
  
 有关如何使用此过程对 XML 元素进行加密的信息，请参阅 [如何：使用对称密钥对 Xml 元素进行加密](how-to-encrypt-xml-elements-with-symmetric-keys.md)。  
  
 当使用对称算法（如 AES）来加密 XML 数据时，必须使用相同的密钥来加密和解密 XML 数据。  此过程中的示例假定加密的 XML 是使用相同的密钥进行加密的，且加密方和解密方对要使用的算法和密钥达成一致。  此示例不对加密的 XML 内的 AES 密钥进行存储或加密。  
  
 此示例适用于以下情况：单个应用程序需要根据内存中存储的会话密钥加密数据，或根据从密码派生而来的加密型强密钥进行加密。  对于两个或多个应用程序需要共享加密的 XML 数据的情况，请考虑使用基于非对称算法或 X.509 证书的加密方案。  
  
### <a name="to-decrypt-an-xml-element-with-a-symmetric-key"></a>若要使用对称密钥解密 XML 元素  
  
1. 使用 [如何：使用对称密钥加密 Xml 元素](how-to-encrypt-xml-elements-with-symmetric-keys.md)中所述的技术，使用以前生成的密钥对 xml 元素进行加密。  
  
2. `EncryptedData`在包含加密的 xml 的对象中查找 XML 加密标准) 定义的 <> 元素 (<xref:System.Xml.XmlDocument> ，并创建新的 <xref:System.Xml.XmlElement> 对象来表示该元素。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#10)]
     [!code-vb[HowToEncryptXMLElementSymmetric#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#10)]  
  
3. 通过从之前创建的 <xref:System.Xml.XmlElement> 对象加载原始 XML 数据来创建 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#11)]
     [!code-vb[HowToEncryptXMLElementSymmetric#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#11)]  
  
4. 创建一个新的 <xref:System.Security.Cryptography.Xml.EncryptedXml> 对象并使用它对使用与加密相同密钥的 XML 数据进行解密。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#12)]
     [!code-vb[HowToEncryptXMLElementSymmetric#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#12)]  
  
5. 将加密的元素替换为 XML 文档内的新解密的纯文本元素。  
  
     [!code-csharp[HowToEncryptXMLElementSymmetric#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/cs/sample.cs#13)]
     [!code-vb[HowToEncryptXMLElementSymmetric#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementSymmetric/vb/sample.vb#13)]  
  
## <a name="example"></a>示例  

 此示例假定名为 `"test.xml"` 的文件与已编译程序存在于同一目录中。  它还假定 `"test.xml"` 包含 `"creditcard"` 元素。  可以将以下 XML 放在名为 `test.xml` 的文件，并将其用于以下示例。  
  
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
  
永远不要以纯文本形式存储加密密钥，也不要以纯文本形式在计算机之间传输密钥。  
  
当你完成使用对称加密密钥后，通过将每个字节设置为零或通过调用托管加密类的 <xref:System.Security.Cryptography.SymmetricAlgorithm.Clear%2A> 方法来将它从内存中清除。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.Xml>
- [如何：用对称密钥对 XML 元素进行加密](how-to-encrypt-xml-elements-with-symmetric-keys.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
