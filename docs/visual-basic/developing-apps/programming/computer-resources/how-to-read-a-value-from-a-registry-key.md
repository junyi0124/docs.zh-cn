---
title: 如何：从注册表项读取值
ms.date: 07/20/2015
helpviewer_keywords:
- registry keys [Visual Basic], determining if a value exists in
- My.Computer.Registry object, examples
- registry [Visual Basic], determining if values exist
- registry keys [Visual Basic], reading from
- registry [Visual Basic], reading
ms.assetid: 775d0a57-68c9-464e-8949-9a39bd29cc64
ms.openlocfilehash: 74069b111f4316eb81c74f5e62c1fbff6ab8f4b8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401844"
---
# <a name="how-to-read-a-value-from-a-registry-key-in-visual-basic"></a>如何：在 Visual Basic 中从注册表项中读取值

`My.Computer.Registry` 对象的 `GetValue` 方法可以用于读取 Windows 注册表中的值。  
  
 如果在下面的示例中，“Software\MyApp”项不存在，则引发异常。 如果在下面的示例中，`ValueName`“Name”不存在，则返回 `Nothing`。  
  
 `GetValue` 方法还可以用于确定特定注册表项中是否存在给定值。  
  
 代码从 Web 应用程序读取注册表时，当前用户取决于在 Web 应用程序中实现的身份验证和模拟。  
  
### <a name="to-read-a-value-from-a-registry-key"></a>从注册表项读取值  
  
- 使用 `GetValue` 方法（指定路径和名称）可从注册表项读取值。 下面的示例从 `HKEY_CURRENT_USER\Software\MyApp` 读取值 `Name` 并将它显示在消息框中。  
  
     [!code-vb[VbResourceTasks#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#4)]  
  
 此代码示例也可作为 IntelliSense 代码片段。 在代码片段选取器中，它位于“Windows 操作系统”>“注册表”中。  有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
### <a name="to-determine-whether-a-value-exists-in-a-registry-key"></a>确定值是否存在于注册表项中  
  
- 使用 `GetValue` 方法可检索值。 下面的代码检查值是否存在并在它不存在时返回消息。  
  
     [!code-vb[VbResourceTasks#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#12)]  
  
## <a name="robust-programming"></a>可靠编程  

 注册表包含用于存储数据的顶层（或根）项。 例如，HKEY_LOCAL_MACHINE 根项用于存储所有用户使用的计算机级别设置，而 HKEY_CURRENT_USER 用于存储特定于单个用户的数据。  
  
 以下情况可能会导致异常：  
  
- 密钥名称是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- 用户没有从注册表项 (<xref:System.Security.SecurityException>) 进行读取的权限。  
  
- 项名称超过 255 个字符的限制 (<xref:System.ArgumentException>)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 若要运行此进程，程序集需要 <xref:System.Security.Permissions.RegistryPermission> 类授予的特权等级。 如果在部分信任上下文中运行，该进程可能会因特权不足而引发异常。 同样，用户必须具有用于创建或写入设置的正确 ACL。 例如，具有代码访问安全性权限的本地应用程序可能没有操作系统权限。 有关详细信息，请参阅[代码访问安全性基础知识](../../../../framework/misc/code-access-security-basics.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- <xref:Microsoft.Win32.RegistryHive>
- [读取和写入注册表](reading-from-and-writing-to-the-registry.md)
