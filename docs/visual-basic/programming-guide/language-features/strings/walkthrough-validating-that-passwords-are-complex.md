---
title: 验证密码复杂性
ms.date: 07/20/2015
helpviewer_keywords:
- String data type [Visual Basic], validation
ms.assetid: 5d9a918f-6c1f-41a3-a019-b5c2b8ce0381
ms.openlocfilehash: 9597d7a9d6b68b8c91f32d97da3532f181585cf6
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072348"
---
# <a name="walkthrough-validating-that-passwords-are-complex-visual-basic"></a>演练：验证密码是否复杂 (Visual Basic)

此方法检查某些强密码特性，并使用有关检查密码失败的信息更新字符串参数。  
  
 密码可在安全系统中用于向用户授权。 但是，密码必须难以被未经授权的用户猜出。 攻击者可以使用 *字典攻击* 程序，该程序可循环访问字典中的所有单词 (或不同语言的多个字典) 并测试任何词是否作为用户密码使用。 弱密码（如 "Yankees" 或 "Mustang"）很快就会被猜出。 更强的密码，例如 "？您的 " L1N3vaFiNdMeyeP@sSWerd ！" 不太可能被猜出。 受密码保护的系统应确保用户选择强密码。  
  
 强密码 (包含大写字母、小写字母、数字和特殊字符的组合) ，而不是单词。 此示例演示如何验证复杂性。  
  
## <a name="example"></a>示例  
  
### <a name="code"></a>代码  

 [!code-vb[VbVbcnRegEx#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnRegEx/VB/Class1.vb#1)]  
  
## <a name="compile-the-code"></a>编译代码  

 通过传递包含该密码的字符串来调用此方法。  
  
 此示例需要：  
  
- 对 <xref:System.Text.RegularExpressions> 命名空间成员的访问权限。 如果未在代码中完全限定成员名称，则添加 `Imports` 语句。 有关详细信息，请参阅 [Imports 语句（.NET 命名空间和类型）](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)。  
  
## <a name="security"></a>安全性  

 如果要在网络中移动密码，则需要使用安全方法来传输数据。 有关详细信息，请参阅 [ASP.NET Web 应用程序安全性](/previous-versions/aspnet/330a99hc(v=vs.100))。
  
 可以 `ValidatePassword` 通过添加更多的复杂性检查来提高函数的准确性：  
  
- 对照用户的名称、用户标识符和应用程序定义的字典比较密码及其子字符串。 此外，在执行比较时，将视觉上相似的字符视为等效字符。 例如，将字母 "l" 和 "e" 视为等效于数字 "1" 和 "3"。  
  
- 如果只有一个大写字符，请确保它不是密码的第一个字符。  
  
- 请确保密码的最后两个字符是字母字符。  
  
- 不允许从键盘最上面的行输入所有符号的密码。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Text.RegularExpressions.Regex>
- [ASP.NET Web 应用程序安全性](/previous-versions/aspnet/330a99hc(v=vs.100))
