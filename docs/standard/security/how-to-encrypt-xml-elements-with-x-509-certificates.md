---
title: 如何：使用 X.509 证书加密 XML 元素
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- encryption [.NET], X.509 certificates
- cryptography [.NET], X.509 certificates
- System.Security.Cryptography.EncryptedXml class
- XML encryption
- System.Security.Cryptography.X509Certificate2 class
- X.509 certificates
- certificates, X.509 certificates
ms.assetid: 761f1c66-631c-47af-aa86-ad9c50cfa453
ms.openlocfilehash: a336997a8cb81f1838d9c5b1ec82bbb04326ed0c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729325"
---
# <a name="how-to-encrypt-xml-elements-with-x509-certificates"></a>如何：使用 X.509 证书加密 XML 元素

可以使用 <xref:System.Security.Cryptography.Xml> 命名空间中的类加密 XML 文档内的元素。  XML 加密是交换或存储加密的 XML 数据的一种标准方式，使用后就无需担心数据被轻易读取。  有关 XML 加密标准的详细信息，请参阅位于万维网联合会 (W3C) 规范 for XML Encryption <https://www.w3.org/TR/xmldsig-core/> 。  
  
 可以使用 XML 加密将任何 XML 元素或文档替换为包含加密 XML 数据的 <`EncryptedData`> 元素。 <`EncryptedData`> 元素可以包含一些子元素来收入关于加密期间使用的密钥和进程的信息。  XML 加密允许文档包含多个加密元素，并允许对一个元素进行多次加密。  此过程中的代码示例演示了如何创建一个 <`EncryptedData`> 元素和几个其他子元素，以便以后在解密过程中使用。  
  
此示例使用两个密钥对 XML 元素进行加密。 该示例以编程方式检索证书，并使用它通过方法对 XML 元素进行加密 <xref:System.Security.Cryptography.Xml.EncryptedXml.Encrypt%2A> 。 在内部，<xref:System.Security.Cryptography.Xml.EncryptedXml.Encrypt%2A> 方法创建一个单独的会话密钥，并将其用于加密 XML 文档。 此方法对会话密钥进行加密，并将它与加密的 XML 一起保存在一个新的 <`EncryptedData`> 元素中。  

若要对 XML 元素进行解密，请调用 <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> 方法，该方法会自动从存储中检索 x.509 证书并执行必要的解密。  有关如何对按照此过程加密的 XML 元素进行解密的详细信息，请参阅[如何：用 X.509 证书对 XML 元素进行解密](how-to-decrypt-xml-elements-with-x-509-certificates.md)。  
  
此示例适用于以下情况：多个应用程序需要共享加密数据，或应用程序需要保存它各次运行之间的加密数据。  
  
### <a name="to-encrypt-an-xml-element-with-an-x509-certificate"></a>使用 X.509 证书对 XML 元素进行加密  

若要运行此示例，需要创建测试证书，并将其保存到证书存储中。 仅为 Windows 证书创建工具提供该任务的说明 [ ( # A0) ](/windows/desktop/SecCrypto/makecert)。

1. 使用 [Makecert.exe](/windows/desktop/SecCrypto/makecert) 生成 x.509 证书，并将其置于本地用户存储中。 必须生成一个交换密钥，且该密钥必须可导出。 运行下面的命令：  
  
    ```console  
    makecert -r -pe -n "CN=XML_ENC_TEST_CERT" -b 01/01/2020 -e 01/01/2025 -sky exchange -ss my  
    ```  
  
2. 创建 <xref:System.Security.Cryptography.X509Certificates.X509Store> 对象，并进行初始化，以便打开当前用户存储区。  
  
     [!code-csharp[HowToEncryptXMLElementX509#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#2)]
     [!code-vb[HowToEncryptXMLElementX509#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#2)]  
  
3. 在只读模式下打开存储区。  
  
     [!code-csharp[HowToEncryptXMLElementX509#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#3)]
     [!code-vb[HowToEncryptXMLElementX509#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#3)]  
  
4. 使用存储区中的所有证书初始化 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2Collection>。  
  
     [!code-csharp[HowToEncryptXMLElementX509#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#4)]
     [!code-vb[HowToEncryptXMLElementX509#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#4)]  
  
5. 枚举存储区中的证书，找到具有相应名称的证书。  在此示例中，证书名为 `"CN=XML_ENC_TEST_CERT"`。  
  
     [!code-csharp[HowToEncryptXMLElementX509#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#5)]
     [!code-vb[HowToEncryptXMLElementX509#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#5)]  
  
6. 找到该证书后，关闭存储区。  
  
     [!code-csharp[HowToEncryptXMLElementX509#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#6)]
     [!code-vb[HowToEncryptXMLElementX509#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#6)]  
  
7. 通过从磁盘加载 XML 文件来创建 <xref:System.Xml.XmlDocument> 对象。  <xref:System.Xml.XmlDocument> 对象包含要加密的 XML 元素。  
  
     [!code-csharp[HowToEncryptXMLElementX509#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#7)]
     [!code-vb[HowToEncryptXMLElementX509#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#7)]  
  
8. 在 <xref:System.Xml.XmlDocument> 对象中查找指定元素，并创建一个新的 <xref:System.Xml.XmlElement> 对象来表示想要加密的元素。  在此示例中，加密了 `"creditcard"` 元素。  
  
     [!code-csharp[HowToEncryptXMLElementX509#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#8)]
     [!code-vb[HowToEncryptXMLElementX509#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#8)]  
  
9. 创建 <xref:System.Security.Cryptography.Xml.EncryptedXml> 类的新实例，并通过它使用 X.509 证书对指定元素进行加密。  <xref:System.Security.Cryptography.Xml.EncryptedXml.Encrypt%2A> 方法以 <xref:System.Security.Cryptography.Xml.EncryptedData> 对象的形式返回加密元素。  
  
     [!code-csharp[HowToEncryptXMLElementX509#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#9)]
     [!code-vb[HowToEncryptXMLElementX509#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#9)]  
  
10. 将原始 <xref:System.Xml.XmlDocument> 对象中的元素替换为 <xref:System.Security.Cryptography.Xml.EncryptedData> 元素。  
  
     [!code-csharp[HowToEncryptXMLElementX509#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#10)]
     [!code-vb[HowToEncryptXMLElementX509#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#10)]  
  
11. 保存 <xref:System.Xml.XmlDocument> 对象。  
  
     [!code-csharp[HowToEncryptXMLElementX509#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#11)]
     [!code-vb[HowToEncryptXMLElementX509#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#11)]  
  
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
  
 [!code-csharp[HowToEncryptXMLElementX509#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToEncryptXMLElementX509/cs/sample.cs#1)]
 [!code-vb[HowToEncryptXMLElementX509#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToEncryptXMLElementX509/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 在面向 .NET Framework 的项目中，包含对的引用 `System.Security.dll` 。

- 在面向 .NET Core 或 .NET 5 的项目中，安装 NuGet 包 [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml)。
  
- 包括以下命名空间：<xref:System.Xml>、<xref:System.Security.Cryptography> 和 <xref:System.Security.Cryptography.Xml>。  
  
## <a name="net-security"></a>.NET 安全性
  
此示例中使用的 X.509 证书仅用于测试目的。  应用程序应使用由受信任的证书颁发机构生成的 x.509 证书。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.Xml>
- [如何：使用 X.509 证书解密 XML 元素](how-to-decrypt-xml-elements-with-x-509-certificates.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
