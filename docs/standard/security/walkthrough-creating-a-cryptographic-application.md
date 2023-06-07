---
title: 演练：创建加密应用程序
description: 演练如何创建加密应用程序。 了解如何对 Windows 窗体应用程序中的内容进行加密和解密。
ms.date: 07/14/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cryptography [NET], example
- cryptography [NET], cryptographic application example
- cryptography [NET], application example
ms.assetid: abf48c11-1e72-431d-9562-39cf23e1a8ff
ms.openlocfilehash: 70218d60abb336cdb35fc2e89e62a50b6bd79c67
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830553"
---
# <a name="walkthrough-creating-a-cryptographic-application"></a>演练：创建加密应用程序

> [!NOTE]
> 本文适用于 Windows。
>
> 有关 ASP.NET Core 的信息，请参阅 [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)。

本演练演示如何对内容进行加密和解密。 下面的代码示例是特为 Windows 窗体应用程序设计的。 此应用程序不演示实际方案，例如使用智能卡。 而演示加密和解密的基础知识。  
  
本演练使用以下加密原则：  
  
- 使用 <xref:System.Security.Cryptography.Aes> 类（一种对称算法），用于通过使用其自动生成的 <xref:System.Security.Cryptography.SymmetricAlgorithm.Key%2A> 和 <xref:System.Security.Cryptography.SymmetricAlgorithm.IV%2A> 来加密和解密数据。  
  
- 使用 <xref:System.Security.Cryptography.RSA> 非对称算法对加密的数据进行加密和解密 <xref:System.Security.Cryptography.Aes> 。 非对称算法最适用于较少的数据，如密钥。  
  
    > [!NOTE]
    > 如果要保护计算机上的数据而不是与其他人交换加密的内容，请考虑使用 <xref:System.Security.Cryptography.ProtectedData> 类。  
  
 下表总结了本主题中的加密任务。  
  
|任务|说明|  
|----------|-----------------|  
|创建 Windows 窗体应用程序|列出运行该应用程序所需的控件。|  
|声明全局对象|声明字符串路径变量 <xref:System.Security.Cryptography.CspParameters> 和 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 具有 <xref:System.Windows.Forms.Form> 类的全局上下文。|  
|创建非对称密钥|创建非对称公钥和私钥值对，并为其分配一个密钥容器名称。|  
|加密文件|显示对话框，以便选择供加密的文件，并对该文件进行加密。|  
|解密文件|显示对话框，以便选择供解密的已加密文件，并对该文件进行解密。|  
|获取私钥|获取使用密钥容器名称的完整密钥对。|  
|导出公钥|将密钥保存到只带有公共参数的 XML 文件中。|  
|导入公钥|将密钥从 XML 文件加载到密钥容器中。|  
|测试应用程序|列出用于测试此应用程序的步骤。|  
  
## <a name="prerequisites"></a>先决条件  

您需要满足以下条件才能完成本演练：  
  
- 对 <xref:System.IO> 和 <xref:System.Security.Cryptography> 命名空间的引用。  
  
## <a name="creating-a-windows-forms-application"></a>创建 Windows 窗体应用程序  

本演练中的大多数代码示例均设计为按钮控件的事件处理程序。 下表列出了示例应用程序所需的控件及其匹配代码示例所需的名称。  
  
|控件|名称|文本属性（根据需要）|  
|-------------|----------|---------------------------------|  
|<xref:System.Windows.Forms.Button>|`buttonEncryptFile`|加密文件|  
|<xref:System.Windows.Forms.Button>|`buttonDecryptFile`|解密文件|  
|<xref:System.Windows.Forms.Button>|`buttonCreateAsmKeys`|创建密钥|  
|<xref:System.Windows.Forms.Button>|`buttonExportPublicKey`|导出公钥|  
|<xref:System.Windows.Forms.Button>|`buttonImportPublicKey`|导入公钥|  
|<xref:System.Windows.Forms.Button>|`buttonGetPrivateKey`|获取私钥|  
|<xref:System.Windows.Forms.Label>|`label1`|未设置键|  
|<xref:System.Windows.Forms.OpenFileDialog>|`openFileDialog1`||  
|<xref:System.Windows.Forms.OpenFileDialog>|`openFileDialog2`||  
  
 双击 Visual Studio 设计器中的按钮以创建其事件处理程序。
  
## <a name="declaring-global-objects"></a>声明全局对象  

将以下代码添加到窗体的构造函数中。 编辑环境和首选项的字符串变量。  
  
[!code-csharp[CryptoWalkThru#1](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#1)]
[!code-vb[CryptoWalkThru#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#1)]  
  
## <a name="creating-an-asymmetric-key"></a>创建非对称密钥  

此任务创建可对 <xref:System.Security.Cryptography.Aes> 密钥进行加密和解密的非对称密钥。 此密钥用于加密内容，并且它将显示标签控件上的密钥容器名称。  
  
 将以下代码添加为 `Create Keys` 按钮 (`buttonCreateAsmKeys_Click`) 的 `Click` 事件处理程序。  
  
 [!code-csharp[CryptoWalkThru#2](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#2)]
 [!code-vb[CryptoWalkThru#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#2)]  
  
## <a name="encrypting-a-file"></a>加密文件  

此任务涉及两种方法：按钮的事件处理程序方法 `Encrypt File` (`buttonEncryptFile_Click`) 和 `EncryptFile` 方法。 第一种方法显示一个用于选择文件的对话框，并将文件名传递给第二种方法，后者将执行加密。  
  
加密的内容、密钥和 IV 全都保存到一个 <xref:System.IO.FileStream> 中，这被称为加密包。  
  
`EncryptFile` 方法执行以下操作：  
  
1. 创建 <xref:System.Security.Cryptography.Aes> 对称算法，以便对内容进行加密。  
  
2. 创建 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 对象，以便对 <xref:System.Security.Cryptography.Aes> 密钥进行加密。  
  
3. 使用 <xref:System.Security.Cryptography.CryptoStream> 对象读取源文件的 <xref:System.IO.FileStream>，并将其加密到已加密文件的目标 <xref:System.IO.FileStream> 对象中（以字节块为单位）。  
  
4. 确定加密密钥和 IV 的长度并创建其长度值的字节数组。  
  
5. 将密钥、IV 及其长度值写入已加密的包。  
  
 加密包使用以下格式：  
  
- 密钥长度，字节数 0 - 3  
  
- IV 长度，字节数 4 - 7  
  
- 已加密的密钥  
  
- IV  
  
- 密码文本  
  
 可使用密钥和 IV 的长度来确定起点和加密包所有部件的长度，然后可使用它们来解密文件。  
  
 将以下代码添加为 `Encrypt File` 按钮 (`buttonEncryptFile_Click`) 的 `Click` 事件处理程序。  
  
 [!code-csharp[CryptoWalkThru#3](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#3)]
 [!code-vb[CryptoWalkThru#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#3)]  
  
 将下面的 `EncryptFile` 方法添加到窗体中。  
  
 [!code-csharp[CryptoWalkThru#5](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#5)]
 [!code-vb[CryptoWalkThru#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#5)]  
  
## <a name="decrypting-a-file"></a>解密文件  

此任务涉及两种方法，即 `Decrypt File` 按钮 (`buttonDecryptFile_Click`) 的事件处理程序方法和 `DecryptFile` 方法。 第一种方法显示一个用于选择文件的对话框，并将其文件名传递给第二种方法，后者将执行解密。  
  
`Decrypt` 方法执行以下操作：  
  
1. 创建 <xref:System.Security.Cryptography.Aes> 用于解密内容的对称算法。  
  
2. 将已加密包的 <xref:System.IO.FileStream> 的前八个字节读取到字节数组中，以便获取已加密的密钥和 IV 的长度。  
  
3. 将密钥和 IV 从加密包提取到字节数组中。  
  
4. 创建 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 对象，以便对 <xref:System.Security.Cryptography.Aes> 密钥进行解密。  
  
5. 使用 <xref:System.Security.Cryptography.CryptoStream> 对象来读取 <xref:System.IO.FileStream> 加密包的密码文本部分，并将其解密到已解密文件的 <xref:System.IO.FileStream> 对象中（以字节块为单位）。 完成后，解密即完成。  
  
 将以下代码添加为 `Decrypt File` 按钮的 `Click` 事件处理程序。  
  
 [!code-csharp[CryptoWalkThru#4](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#4)]
 [!code-vb[CryptoWalkThru#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#4)]  
  
 将下面的 `DecryptFile` 方法添加到窗体中。  
  
 [!code-csharp[CryptoWalkThru#6](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#6)]
 [!code-vb[CryptoWalkThru#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#6)]  
  
## <a name="exporting-a-public-key"></a>导出公钥

此任务将 `Create Keys` 按钮创建的密钥保存到文件。 它仅导出公共参数。  
  
此任务模拟以下场景：Alice 把她的公钥给 Bob，从而 Bob 可为她加密文件。 他和其他具有公钥的人将不能对文件进行解密，因为他们没有带专用参数的完整密钥对。  
  
将以下代码添加为 `Export Public Key` 按钮 (`buttonExportPublicKey_Click`) 的 `Click` 事件处理程序。  
  
[!code-csharp[CryptoWalkThru#8](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#8)]
[!code-vb[CryptoWalkThru#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#8)]  
  
## <a name="importing-a-public-key"></a>导入公钥

此任务将加载只有公共参数的密钥（由 `Export Public Key` 按钮创建），并将其设置为密钥容器名称。  
  
此任务模拟以下场景：Bob 加载 Alice 的只含公共参数的密钥，从而可以为她加密文件。  
  
将以下代码添加为 `Import Public Key` 按钮 (`buttonImportPublicKey_Click`) 的 `Click` 事件处理程序。  
  
[!code-csharp[CryptoWalkThru#9](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#9)]
[!code-vb[CryptoWalkThru#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#9)]  
  
## <a name="getting-a-private-key"></a>获取私钥  

此任务将密钥容器名称设置为使用 `Create Keys` 按钮创建的密钥的名称。 密钥容器将包含带专用参数的完整密钥对。  
  
此任务模拟以下场景：Alice 使用她的私钥对由 Bob 加密的文件进行解密。  
  
将以下代码添加为 `Get Private Key` 按钮 (`buttonGetPrivateKey_Click`) 的 `Click` 事件处理程序。  
  
[!code-csharp[CryptoWalkThru#7](../../../samples/snippets/csharp/VS_Snippets_CLR/CryptoWalkThru/cs/Form1.cs#7)]
[!code-vb[CryptoWalkThru#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/CryptoWalkThru/vb/Form1.vb#7)]  
  
## <a name="testing-the-application"></a>测试应用程序

生成应用程序后，执行以下测试方案。  
  
#### <a name="to-create-keys-encrypt-and-decrypt"></a>创建密钥、加密和解密  
  
1. 单击“`Create Keys`”按钮。 标签显示密钥名称，并显示它是完整密钥对。  
  
2. 单击“`Export Public Key`”按钮。 请注意，导出公钥参数并不会更改当前密钥。  
  
3. 单击 `Encrypt File` 按钮并选择文件。  
  
4. 单击 `Decrypt File` 按钮，然后选择刚刚加密的文件。  
  
5. 检查刚刚解密的文件。  
  
6. 关闭应用程序并重启，以便测试检索下一个方案中保留的密钥容器。  
  
#### <a name="to-encrypt-using-the-public-key"></a>使用公钥进行加密  
  
1. 单击“`Import Public Key`”按钮。 标签显示密钥名称，并显示它仅是公共的。  
  
2. 单击 `Encrypt File` 按钮并选择文件。  
  
3. 单击 `Decrypt File` 按钮，然后选择刚刚加密的文件。 这将失败，因为必须使用私钥进行解密。  
  
 此方案演示了仅使用公钥为他人加密文件。 通常这个人只会为你提供公钥，而保留供解密的私钥。  
  
#### <a name="to-decrypt-using-the-private-key"></a>使用私钥进行解密  
  
1. 单击“`Get Private Key`”按钮。 标签显示密钥名称，并显示它是否是完整密钥对。  
  
2. 单击 `Decrypt File` 按钮，然后选择刚刚加密的文件。 这将会成功，因为你具有用于解密的完整密钥对。  
  
## <a name="see-also"></a>另请参阅

- [加密模型](cryptography-model.md) -介绍如何在基类库中实现加密。
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
