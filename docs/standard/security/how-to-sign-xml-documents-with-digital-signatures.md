---
title: 如何：使用数字签名为 XML 文档签名
description: 了解如何用数字签名对 XML 文档进行签名。 使用 .NET 中的 System.Security.Cryptography.Xml 命名空间中的类。
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- signatures, XML signing
- System.Security.Cryptography.SignedXml class
- digital signatures, XML signing
- System.Security.Cryptography.RSA class
- XML digital signatures
- XML signing
- signing XML
ms.assetid: 99692ac1-d8c9-42d7-b1bf-2737b01037e4
ms.openlocfilehash: 2cb63fd91b1aeb51c762975103ea665e0d8539b1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726673"
---
# <a name="how-to-sign-xml-documents-with-digital-signatures"></a>如何：使用数字签名为 XML 文档签名

可以使用 <xref:System.Security.Cryptography.Xml> 命名空间中的类通过数字签名对 XML 文档或部分 XML 文档进行签名。  使用 XML 数字签名 (XMLDSIG)，你可以验证签名后的数据没有被更改。  有关 XMLDSIG 标准的详细信息，请参阅万维网联合会 (W3C) 推荐 [XML 签名语法和处理](https://www.w3.org/TR/xmldsig-core/)。  
  
> [!NOTE]
> 本文中的代码适用于 Windows。

此过程中的代码示例演示了如何对整个 XML 文档进行数字签名，以及如何将签名附加到 <> 元素中的文档 `Signature` 。  该示例创建一个 RSA 签名密钥，并将该密钥添加到安全密钥容器，然后使用该密钥对 XML 文档进行数字签名。  然后可以检索该密码来验证 XML 数字签名，或使用它对另一个 XML 文档进行签名。  
  
有关如何验证使用此过程创建的 XML 数字签名的信息，请参阅 [如何：验证 Xml 文档的数字签名](how-to-verify-the-digital-signatures-of-xml-documents.md)。  
  
### <a name="to-digitally-sign-an-xml-document"></a>对 XML 文档进行数字签名  
  
1. 创建 <xref:System.Security.Cryptography.CspParameters> 对象，并指定密钥容器的名称。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#2)]
     [!code-vb[HowToSignXMLDocumentRSA#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#2)]  
  
2. 使用 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 类生成一个非对称密钥。  当将 <xref:System.Security.Cryptography.CspParameters> 对象传递 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 类的构造函数时，密钥将自动保存在密钥容器中。  此密钥将用于对 XML 文档签名。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#3)]
     [!code-vb[HowToSignXMLDocumentRSA#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#3)]  
  
3. 通过从磁盘加载 XML 文件来创建 <xref:System.Xml.XmlDocument> 对象。  <xref:System.Xml.XmlDocument> 对象包含要加密的 XML 元素。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#4)]
     [!code-vb[HowToSignXMLDocumentRSA#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#4)]  
  
4. 创建一个新的 <xref:System.Security.Cryptography.Xml.SignedXml> 对象，并向其传递 <xref:System.Xml.XmlDocument> 对象。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#5)]
     [!code-vb[HowToSignXMLDocumentRSA#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#5)]  
  
5. 将签名 RSA 密钥添加到 <xref:System.Security.Cryptography.Xml.SignedXml> 对象。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#6)]
     [!code-vb[HowToSignXMLDocumentRSA#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#6)]  
  
6. 创建说明签名内容的 <xref:System.Security.Cryptography.Xml.Reference> 对象。  若要对整个文档进行签名，请将 <xref:System.Security.Cryptography.Xml.Reference.Uri%2A> 属性设置为 `""`。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#7)]
     [!code-vb[HowToSignXMLDocumentRSA#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#7)]  
  
7. 将 <xref:System.Security.Cryptography.Xml.XmlDsigEnvelopedSignatureTransform> 对象添加到 <xref:System.Security.Cryptography.Xml.Reference> 对象中。  变换使验证程序可以使用与签名工具所用的相同方式表示 XML 数据。  可以采用不同的方式表示 XML 数据，因此这一步对于验证至关重要。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#8)]
     [!code-vb[HowToSignXMLDocumentRSA#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#8)]  
  
8. 将 <xref:System.Security.Cryptography.Xml.Reference> 对象添加到 <xref:System.Security.Cryptography.Xml.SignedXml> 对象中。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#9)]
     [!code-vb[HowToSignXMLDocumentRSA#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#9)]  
  
9. 通过调用 <xref:System.Security.Cryptography.Xml.SignedXml.ComputeSignature%2A> 方法计算签名。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#10)]
     [!code-vb[HowToSignXMLDocumentRSA#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#10)]  
  
10. 检索 (<>) 元素的签名的 XML 表示形式 `Signature` ，并将其保存到新的 <xref:System.Xml.XmlElement> 对象。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#11)]
     [!code-vb[HowToSignXMLDocumentRSA#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#11)]  
  
11. 将元素追加到 <xref:System.Xml.XmlDocument> 对象中。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#12)]
     [!code-vb[HowToSignXMLDocumentRSA#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#12)]  
  
12. 保存文档。  
  
     [!code-csharp[HowToSignXMLDocumentRSA#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#13)]
     [!code-vb[HowToSignXMLDocumentRSA#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#13)]  
  
## <a name="example"></a>示例  

 此示例假定名为 `test.xml` 的文件与已编译程序存在于同一目录中。  可以将以下 XML 放在名为 `test.xml` 的文件，并将其用于以下示例。  
  
```xml  
<root>  
    <creditcard>  
        <number>19834209</number>  
        <expiry>02/02/2002</expiry>  
    </creditcard>  
</root>  
```  
  
 [!code-csharp[HowToSignXMLDocumentRSA#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToSignXMLDocumentRSA/cs/sample.cs#1)]
 [!code-vb[HowToSignXMLDocumentRSA#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToSignXMLDocumentRSA/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 在面向 .NET Framework 的项目中，包含对的引用 `System.Security.dll` 。

- 在面向 .NET Core 或 .NET 5 的项目中，安装 NuGet 包 [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml)。
  
- 包括以下命名空间：<xref:System.Xml>、<xref:System.Security.Cryptography> 和 <xref:System.Security.Cryptography.Xml>。  
  
## <a name="net-security"></a>.NET 安全性

切勿用纯文本存储或传输非对称密钥对的私钥。  有关对称和非对称加密密钥的详细信息，请参阅 [生成加密和解密密钥](generating-keys-for-encryption-and-decryption.md)。  
  
切勿将私钥直接嵌入到源代码中。  [Ildasm.exe 使用 (IL 拆装器) ](../../framework/tools/ildasm-exe-il-disassembler.md)或通过在文本编辑器（例如记事本）中打开程序集，可以轻松地从程序集中读取嵌入的密钥。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.Xml>
- [如何：验证 XML 文档的数字签名](how-to-verify-the-digital-signatures-of-xml-documents.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
