---
title: 如何：用非对称密钥对 XML 元素进行解密
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- System.Security.Cryptography.RSA class
- asymmetric keys
- System.Security.Cryptography.EncryptedXml class
- XML encryption
- decryption
ms.assetid: dd5de491-dafe-4b94-966d-99714b2e754a
ms.openlocfilehash: 0456c89987b37840daa1c84342528d11c6da73a4
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822225"
---
# <a name="how-to-decrypt-xml-elements-with-asymmetric-keys"></a>如何：用非对称密钥对 XML 元素进行解密

可以使用 <xref:System.Security.Cryptography.Xml> 命名空间中的类对 XML 文档内的元素进行加密和解密。  XML 加密是交换或存储加密的 XML 数据的一种标准方式，使用后就无需担心数据被轻易读取。  有关 XML 加密标准的详细信息，请参阅万维网联合会 (W3C) 推荐 [XML 签名语法和处理](https://www.w3.org/TR/xmldsig-core/)。  

> [!NOTE]
> 本文中的代码适用于 Windows。

此过程中的示例对使用 [如何：用非对称密钥加密 XML 元素](how-to-encrypt-xml-elements-with-asymmetric-keys.md)中所述的方法进行加密的 XML 元素进行解密。  它将查找 <`EncryptedData`> 元素，对元素进行解密，然后将元素替换为原始纯文本 XML 元素。  
  
此示例使用两个密钥对 XML 元素进行解密。  它从密钥容器中检索以前生成的 RSA 私钥，然后使用 RSA 密钥对存储在 <> 元素的 <> 元素中的会话密钥进行解密 `EncryptedKey` `EncryptedData` 。  然后此示例使用会话密钥对 XML 元素进行解密。  
  
此示例适用于以下情况：多个应用程序必须共享加密数据，或应用程序必须保存其每次运行之间的加密数据。  
  
### <a name="to-decrypt-an-xml-element-with-an-asymmetric-key"></a>用非对称密钥对 XML 元素进行解密  
  
1. 创建 <xref:System.Security.Cryptography.CspParameters> 对象，并指定密钥容器的名称。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#2)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#2)]  
  
2. 使用 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 对象从容器中检索以前生成的非对称密钥。  当将 <xref:System.Security.Cryptography.CspParameters> 对象传递到 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 构造函数中时，将自动从密钥容器中检索到密钥。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#3)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#3)]  
  
3. 创建新 <xref:System.Security.Cryptography.Xml.EncryptedXml> 对象以对文档进行解密。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#5)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#5)]  
  
4. 添加一个密钥/名称映射，以便 RSA 密钥与应该进行解密的文档中的元素相关联。  必须使用与加密文档时所用密钥名称相同的名称。  请注意，此名称独立于用来标识在步骤 1 中指定的密钥容器中的密钥名称。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#6)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#6)]  
  
5. 调用 <xref:System.Security.Cryptography.Xml.EncryptedXml.DecryptDocument%2A> 方法以解密 `EncryptedData`> 元素的 <。  此方法使用 RSA 密钥来解密会话密钥，并自动使用会话密钥来解密 XML 元素。  它还会自动将 <`EncryptedData`> 元素替换为原始纯文本。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#7)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#7)]  
  
6. 保存 XML 文档。  
  
     [!code-csharp[HowToDecryptXMLElementAsymmetric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#8)]
     [!code-vb[HowToDecryptXMLElementAsymmetric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#8)]  
  
## <a name="example"></a>示例

此示例假定名为 `test.xml` 的文件与已编译程序存在于同一目录中。  它还假定 `test.xml` 包含使用 [如何：使用非对称密钥对 XML 元素进行加密](how-to-encrypt-xml-elements-with-asymmetric-keys.md)中描述的方法进行加密的 XML 元素。  
  
[!code-csharp[HowToDecryptXMLElementAsymmetric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/cs/sample.cs#1)]
[!code-vb[HowToDecryptXMLElementAsymmetric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToDecryptXMLElementAsymmetric/vb/sample.vb#1)]  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 在面向 .NET Framework 的项目中，包含对的引用 `System.Security.dll` 。

- 在面向 .NET Core 或 .NET 5 的项目中，安装 NuGet 包 [System.Security.Cryptography.Xml](https://www.nuget.org/packages/System.Security.Cryptography.Xml)。
  
- 包括以下命名空间：<xref:System.Xml>、<xref:System.Security.Cryptography> 和 <xref:System.Security.Cryptography.Xml>。  
  
## <a name="net-security"></a>.NET 安全性  

永远不要以纯文本形式存储对称加密密钥，也不要以纯文本形式在计算机之间传输对称密钥。  此外，绝不存储或传输纯文本形式的非对称密钥的私钥。  有关对称和非对称加密密钥的详细信息，请参阅 [生成加密和解密密钥](generating-keys-for-encryption-and-decryption.md)。  
  
 绝不将密钥直接嵌入源代码。  通过使用 [Ildasm.exe (IL 拆装器) ](../../framework/tools/ildasm-exe-il-disassembler.md) 或在文本编辑器（如记事本）中打开程序集，可以轻松地从程序集中读取嵌入的密钥。  
  
 当你使用加密密钥执行操作后，通过将每个字节设置为零或通过调用托管加密类的 <xref:System.Security.Cryptography.SymmetricAlgorithm.Clear%2A> 方法来将它从内存中清除。  加密密钥有时可从内存由调试器读取，或从硬盘读取（如果内存位置分页到磁盘）。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- <xref:System.Security.Cryptography.Xml>
- [如何：使用非对称密钥加密 XML 元素](how-to-encrypt-xml-elements-with-asymmetric-keys.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
