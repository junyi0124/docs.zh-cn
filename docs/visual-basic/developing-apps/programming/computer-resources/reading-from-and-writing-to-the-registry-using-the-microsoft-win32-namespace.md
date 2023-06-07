---
title: 使用 Microsoft.Win32 命名空间读取和写入注册表
ms.date: 07/20/2015
helpviewer_keywords:
- registry [Visual Basic]
ms.assetid: 4a0dcce0-c27b-4199-baa8-ee4528da6a56
ms.openlocfilehash: 58c3a92067cd0be5db02231c5fc1a13b429a60a0
ms.sourcegitcommit: 74d05613d6c57106f83f82ce8ee71176874ea3f0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2020
ms.locfileid: "93282237"
---
# <a name="reading-from-and-writing-to-the-registry-using-the-microsoftwin32-namespace-visual-basic"></a>使用 Microsoft.Win32 命名空间读取和写入注册表 (Visual Basic)

虽然在针对注册表进行编程时，`My.Computer.Registry` 应涵盖你的基本需求，不过你还可以使用 .NET 的 <xref:Microsoft.Win32> 命名空间中的 <xref:Microsoft.Win32.Registry> 和 <xref:Microsoft.Win32.RegistryKey> 类。
  
## <a name="keys-in-the-registry-class"></a>注册表类中的项  

 <xref:Microsoft.Win32.Registry> 类提供可以用于访问子项及其值的注册表基项。 这些基项本身是只读的。 下表列出并介绍了 <xref:Microsoft.Win32.Registry> 类公开的七个项。  
  
|**Key**|**说明**|  
|-------------|---------------------|  
|<xref:Microsoft.Win32.Registry.ClassesRoot>|定义文档的类型以及与这些类型关联的属性。|  
|<xref:Microsoft.Win32.Registry.CurrentConfig>|包含不是特定于用户的硬件配置信息。|  
|<xref:Microsoft.Win32.Registry.CurrentUser>|包含有关当前用户首选项的信息，如环境变量。|  
|<xref:Microsoft.Win32.Registry.DynData>|包含动态注册表数据，如虚拟设备驱动程序使用的数据。|  
|<xref:Microsoft.Win32.Registry.LocalMachine>|包含保存本地计算机的配置数据的五个子项（硬件、SAM、安全性、软件和系统）。|  
|<xref:Microsoft.Win32.Registry.PerformanceData>|包含软件组件的性能信息。|  
|<xref:Microsoft.Win32.Registry.Users>|包含有关默认用户首选项的信息。|  
  
> [!IMPORTANT]
> 将数据写入当前用户 (<xref:Microsoft.Win32.Registry.CurrentUser>) 比写入本地计算机 (<xref:Microsoft.Win32.Registry.LocalMachine>) 更安全。 当你创建的项以前已由其他进程（可能是恶意的）进行了创建时，会发生通常称为“强占”的情况。 若要防止此情况发生，请使用在项尚未存在时返回 `Nothing` 的方法（如 <xref:Microsoft.Win32.RegistryKey.GetValue%2A>）。  
  
## <a name="reading-a-value-from-the-registry"></a>从注册表中读取值  

 下面的代码演示如何从 HKEY_CURRENT_USER 中读取字符串。  
  
 [!code-vb[VbResourceTasks#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#20)]  
  
 下面的代码读取一个字符串，使它递增，然后将它写入 HKEY_CURRENT_USER。  
  
 [!code-vb[VbResourceTasks#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#21)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.SystemException>
- <xref:System.ApplicationException>
- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- [Try...Catch...Finally 语句](../../../language-reference/statements/try-catch-finally-statement.md)
- [读取和写入注册表](reading-from-and-writing-to-the-registry.md)
- [安全性与注册表](security-and-the-registry.md)
