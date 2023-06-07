---
title: 如何：用非对称密钥对 XML 元素进行加密
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [.NET], asymmetric keys
- AES algorithm
- System.Security.Cryptography.RSA class
- asymmetric keys [.NET]
- System.Security.Cryptography.EncryptedXml class
- XML encryption
- key containers
- Advanced Encryption Standard algorithm
- encryption [.NET], asymmetric keys
ms.assetid: a164ba4f-e596-4bbe-a9ca-f214fe89ed48
ms.openlocfilehash: 29fe7c229664b08c9e1563413525818f2e883198
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729338"
---
# <a name="how-to-encrypt-xml-elements-with-asymmetric-keys"></a>如何：用非对称密钥对 XML 元素进行加密

可以使用 <xref:System.Security.Cryptography.Xml> 命名空间中的类加密 XML 文档内的元素。  XML 加密是交换或存储加密的 XML 数据的一种标准方式，使用后就无需担心数据被轻易读取。  有关 XML 加密标准的详细信息，请参阅位于万维网联合会 (W3C) 规范 for XML Encryption <https://www.w3.org/TR/xmldsig-core/> 。  
  
 可以使用 XML 加密将任何 XML 元素或文档替换为包含加密 XML 数据的 <`EncryptedData`> 元素。  <`EncryptedData`> 元素还可以包含子元素，这些子元素包含有关加密期间使用的密钥和进程的信息。  XML 加密允许文档包含多个加密元素，并允许对一个元素进行多次加密。  此过程中的代码示例演示如何创建一个 <`EncryptedData`> 元素以及其他一些子元素，这些子元素可在稍后解密过程中使用。  
  
 此示例使用两个密钥对 XML 元素进行加密。  它生成 RSA 公钥/私钥对，并将密钥对保存到安全的密钥容器中。  然后，该示例使用高级加密标准 (AES) 算法创建一个单独的会话密钥。  使用 AES 会话密钥对 XML 文档进行加密，再使用 RSA 公钥对 AES 会话密钥进行加密。  最后，该示例会将加密的 AES 会话密钥和加密的 XML 数据保存到新的 <> 元素中的 XML 文档 `EncryptedData` 。  
  
 若要解密 XML 元素，可检索密钥容器中的 RSA 私钥，用其来解密会话密钥，然后使用会话密钥来解密文档。  有关如何解密使用此过程加密的 XML 元素的详细信息，请参阅 [如何：使用非对称密钥对 Xml 元素进行解密](how-to-decrypt-xml-elements-with-asymmetric-keys.md)。  
  
 此示例适用于以下情况：多个应用程序需要共享加密数据，或应用程序需要保存它各次运行之间的加密数据。
  
### <a name="to-encrypt-an-xml-element-with-an-asymmetric-key"></a>使用非对称密钥加密 XML 元素  
  
1. 创建 <xref:System.Security.Cryptography.CspParameters> 对象，并指定密钥容器的名称。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#2)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#2)]  
  
2. 使用 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 类生成对称密钥。  当将 <xref:System.Security.Cryptography.CspParameters> 对象传递 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 类的构造函数时，密钥将自动保存在密钥容器中。  该密钥将用于加密 AES 会话密钥，并且稍后可检索该密钥以便对其进行解密。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#3)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#3)]  
  
3. 通过从磁盘加载 XML 文件来创建 <xref:System.Xml.XmlDocument> 对象。  <xref:System.Xml.XmlDocument> 对象包含要加密的 XML 元素。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#4)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#4)]  
  
4. 在 <xref:System.Xml.XmlDocument> 对象中查找指定元素，并创建一个新的 <xref:System.Xml.XmlElement> 对象来表示想要加密的元素。 在此示例中，加密了 `"creditcard"` 元素。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#5)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#5)]  
  
5. 使用 <xref:System.Security.Cryptography.Aes> 类创建新的会话密钥。  此密钥将加密 XML 元素，然后其自身将被加密并被放置在 XML 文档中。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#6)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#6)]  
  
6. 创建 <xref:System.Security.Cryptography.Xml.EncryptedXml> 类的新实例，并通过它使用会话密钥对指定元素进行加密。  <xref:System.Security.Cryptography.Xml.EncryptedXml.EncryptData%2A> 方法以加密的字节数组的形式返回加密元素。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#7)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#7)]  
  
7. 构造一个 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象并对其填充加密 XML 元素的 URL 标识符。  此 URL 标识符可使解密方知道 XML 包含一个加密元素。  可使用 <xref:System.Security.Cryptography.Xml.EncryptedXml.XmlEncElementUrl> 字段来指定 URL 标识符。  纯文本 XML 元素将被 `EncryptedData` 此对象封装的 <> 元素替换 <xref:System.Security.Cryptography.Xml.EncryptedData> 。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#8)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#8)]  
  
8. 创建一个 <xref:System.Security.Cryptography.Xml.EncryptionMethod> 对象，将它初始化为用于生成会话密钥的加密算法的 URL 标识符。  将 <xref:System.Security.Cryptography.Xml.EncryptionMethod> 对象传递给 <xref:System.Security.Cryptography.Xml.EncryptedType.EncryptionMethod%2A> 属性。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#9)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#9)]  
  
9. 创建一个 <xref:System.Security.Cryptography.Xml.EncryptedKey> 对象，以便包含加密的会话密钥。  加密会话密钥，将其添加到 <xref:System.Security.Cryptography.Xml.EncryptedKey> 对象，并输入会话密钥名称和密钥标识符 URL。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#10)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#10)]  
  
10. 创建新的 <xref:System.Security.Cryptography.Xml.DataReference> 对象，它将加密数据映射到特定的会话密钥。  此可选步骤使你能够轻松地指定 XML 文档的多个部件由单个密钥进行加密。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#11)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#11)]  
  
11. 将加密的密钥添加到 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#12)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#12)]  
  
12. 创建新的 <xref:System.Security.Cryptography.Xml.KeyInfo> 对象以指定 RSA 密钥的名称。  将其添加到 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象。 这有助于解密方确定解密会话密钥时要使用的正确的非对称密钥。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#13)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#13)]  
  
13. 将加密的元素数据添加到 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#14](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#14)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#14)]  
  
14. 将原始 <xref:System.Xml.XmlDocument> 对象中的元素替换为 <xref:System.Security.Cryptography.Xml.EncryptedData> 元素。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#15](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#15)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#15)]  
  
15. 保存 <xref:System.Xml.XmlDocument> 对象。  
  
     [!code-csharp[HowToEncryptXMLElementAsymmetric#16](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#16)]
     [!code-vb[HowToEncryptXMLElementAsymmetric#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#16)]  
  
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
  
 [!code-csharp[HowToEncryptXMLElementAsymmetric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/cs/sample.cs#1)]
 [!code-vb[HowToEncryptXMLElementAsymmetric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementAsymmetric/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 在面向 .NET Framework 的项目中，包含对的引用 `System.Security.dll` 。

- 在面向 .NET Core 或 .NET 5 的项目中，安装 NuGet 包 [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml)。
  
- 包括以下命名空间：<xref:System.Xml>、<xref:System.Security.Cryptography> 和 <xref:System.Security.Cryptography.Xml>。  
  
## <a name="net-security"></a>.NET 安全性

永远不要以纯文本形式存储对称加密密钥，也不要以纯文本形式在计算机之间传输对称密钥。  此外，绝不存储或传输纯文本形式的非对称密钥的私钥。  有关对称和非对称加密密钥的详细信息，请参阅 [生成加密和解密密钥](generating-keys-for-encryption-and-decryption.md)。  
  
绝不将密钥直接嵌入源代码。  [Ildasm.exe 使用 (IL 拆装器) ](../../framework/tools/ildasm-exe-il-disassembler.md)或通过在文本编辑器（例如记事本）中打开程序集，可以轻松地从程序集中读取嵌入的密钥。  
  
当你使用加密密钥执行操作后，通过将每个字节设置为零或通过调用托管加密类的 <xref:System.Security.Cryptography.SymmetricAlgorithm.Clear%2A> 方法来将它从内存中清除。  加密密钥有时可从内存由调试器读取，或从硬盘读取（如果内存位置分页到磁盘）。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)- <xref:System.Security.Cryptography.Xml>
- [如何：使用非对称密钥解密 XML 元素](how-to-decrypt-xml-elements-with-asymmetric-keys.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
