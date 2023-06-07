---
title: 如何：删除注册表项
ms.date: 07/20/2015
f1_keywords:
- vb.DeleteSetting
helpviewer_keywords:
- GetSetting function [Visual Basic]
- registry [Visual Basic], deleting values
- GetAllSettings function
- registry keys [Visual Basic], deleting
- registry [Visual Basic], deleting keys
- examples [Visual Basic], registry
ms.assetid: ab9aca0e-42b0-4ff7-8ff9-845a4bfdf9f2
ms.openlocfilehash: ea537d302f64933176f1a44fec2e27b804ff5809
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363314"
---
# <a name="how-to-delete-a-registry-key-in-visual-basic"></a>如何：在 Visual Basic 中删除注册表项

<xref:Microsoft.Win32.RegistryKey.DeleteSubKey%28System.String%29> 和 <xref:Microsoft.Win32.RegistryKey.DeleteSubKey%28System.String%2CSystem.Boolean%29> 方法可以用于删除注册表项。  
  
## <a name="procedure"></a>过程  
  
#### <a name="to-delete-a-registry-key"></a>删除注册表项  
  
- 使用 `DeleteSubKey` 方法删除注册表项。 此示例将删除 CurrentUser 配置单元中的软件/TestApp 项。 可在代码中更改为相应的字符串，或使它依赖于用户提供的信息。  
  
     [!code-vb[VbResourceTasks#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#19)]  
  
## <a name="robust-programming"></a>可靠编程  

 如果键/值对不存在，`DeleteSubKey` 方法将返回一个空字符串。  
  
 以下情况可能会导致异常：  
  
- 密钥名称是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- 用户没有删除注册表项的权限 (<xref:System.Security.SecurityException>)。  
  
- 项名称超过 255 个字符的限制 (<xref:System.ArgumentException>)。  
  
- 注册表项为只读 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 如果未授予足够的运行时权限 (<xref:System.Security.Permissions.RegistryPermission>) 或用户没有用于创建或写入设置的适当访问权限（由 ACL 确定），则注册表调用将失败。 例如，具有代码访问安全性权限的本地应用程序可能没有操作系统权限。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.Win32.RegistryKey.DeleteSubKey%2A>
- <xref:Microsoft.Win32.RegistryKey>
- [安全性与注册表](security-and-the-registry.md)
- [读取和写入注册表](reading-from-and-writing-to-the-registry.md)
