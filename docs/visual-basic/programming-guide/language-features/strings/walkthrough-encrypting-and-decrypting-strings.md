---
title: 加密和解密字符串
ms.date: 07/20/2015
helpviewer_keywords:
- encryption [Visual Basic], strings
- strings [Visual Basic], encrypting
- decryption [Visual Basic], strings
- strings [Visual Basic], decrypting
ms.assetid: 1f51e40a-2f88-43e2-a83e-28a0b5c0d6fd
ms.openlocfilehash: e0e3fc332bf9430b1fa56dbb7630f849d3a29c2e
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072400"
---
# <a name="walkthrough-encrypting-and-decrypting-strings-in-visual-basic"></a>演练：在 Visual Basic 中对字符串进行加密和解密

本演练演示了如何使用类， <xref:System.Security.Cryptography.DESCryptoServiceProvider> 通过使用加密服务提供程序 (CSP) 三级数据加密标准 () 算法的版本来对字符串进行加密和解密 <xref:System.Security.Cryptography.TripleDES> 。 第一步是创建一个封装3DES 算法的简单包装器类，并将加密数据存储为64编码的字符串。 然后，使用该包装将私有用户数据安全地存储在可公开访问的文本文件中。  
  
 你可以使用加密来保护用户机密 (例如，密码) 和，使未经授权的用户无法读取凭据。 这可以防止授权用户的身份被盗，从而保护用户的资产并提供不可否认性。 加密还可以保护用户的数据，防止未经授权的用户对其进行访问。  
  
 有关更多信息，请参阅[加密服务](../../../../standard/security/cryptographic-services.md)。  
  
> [!IMPORTANT]
> Rijndael (现在称为高级加密标准 [AES] ) ，而三次数据加密标准 (3DES) 算法提供比 DES 更高的安全性，因为它们的计算工作量更高。 有关详细信息，请参阅 <xref:System.Security.Cryptography.DES> 和 <xref:System.Security.Cryptography.Rijndael>。  
  
### <a name="to-create-the-encryption-wrapper"></a>创建加密包装器  
  
1. 创建 `Simple3Des` 用于封装加密和解密方法的类。  
  
     [!code-vb[VbVbalrStrings#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#38)]  
  
2. 将加密命名空间的导入添加到包含类的文件的开头 `Simple3Des` 。  
  
     [!code-vb[VbVbalrStrings#77](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#77)]  
  
3. 在 `Simple3Des` 类中，添加私有字段以存储3des 加密服务提供程序。  
  
     [!code-vb[VbVbalrStrings#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#39)]  
  
4. 添加一个私有方法，该方法根据指定键的哈希创建指定长度的字节数组。  
  
     [!code-vb[VbVbalrStrings#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#41)]  
  
5. 添加构造函数来初始化3DES 加密服务提供程序。  
  
     `key`参数控制 `EncryptData` 和 `DecryptData` 方法。  
  
     [!code-vb[VbVbalrStrings#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#40)]  
  
6. 添加加密字符串的公共方法。  
  
     [!code-vb[VbVbalrStrings#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#42)]  
  
7. 添加解密字符串的公共方法。  
  
     [!code-vb[VbVbalrStrings#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#43)]  
  
     现在可以使用包装类来保护用户资产。 在此示例中，它用于安全地将私有用户数据存储在可公开访问的文本文件中。  
  
### <a name="to-test-the-encryption-wrapper"></a>测试加密包装器  
  
1. 在单独的类中，添加一个方法，该方法使用包装器的 `EncryptData` 方法对字符串进行加密并将其写入用户的 "我的文档" 文件夹。  
  
     [!code-vb[VbVbalrStrings#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#78)]  
  
2. 添加一个方法，该方法读取用户的 "我的文档" 文件夹中的加密字符串，并使用包装器的方法对字符串进行解密 `DecryptData` 。  
  
     [!code-vb[VbVbalrStrings#79](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class3.vb#79)]  
  
3. 添加用户界面代码以调用 `TestEncoding` 和 `TestDecoding` 方法。  
  
4. 运行应用程序。  
  
     在测试应用程序时，请注意，如果提供了错误的密码，它不会对数据进行解密。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Security.Cryptography>
- <xref:System.Security.Cryptography.DESCryptoServiceProvider>
- <xref:System.Security.Cryptography.DES>
- <xref:System.Security.Cryptography.TripleDES>
- <xref:System.Security.Cryptography.Rijndael>
- [加密服务](../../../../standard/security/cryptographic-services.md)
