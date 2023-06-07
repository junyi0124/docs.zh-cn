---
title: 安全性与注册表
ms.date: 07/20/2015
helpviewer_keywords:
- security [Visual Basic], registry
- registry [Visual Basic], security issues
ms.assetid: 9980aff7-2f69-492b-8f66-29a9a76d3df5
ms.openlocfilehash: 2eba9d177769b22de3f931bc7af4905a80e316b5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359963"
---
# <a name="security-and-the-registry-visual-basic"></a>安全性与注册表 (Visual Basic)

本页讨论将数据存储在注册表中的安全意义。  
  
## <a name="permissions"></a>权限  

 即使注册表项受 ACL（访问控制列表）保护，在注册表中以纯文本形式存储机密信息（例如密码）也不安全。  
  
 对注册表进行操作时，如果允许对系统资源或受保护的信息进行不适当的访问，则可能会降低安全性。 若要使用这些属性，必须具有来自 <xref:System.Security.Permissions.RegistryPermissionAccess> 枚举（其控制对注册表变量的访问）的读取和写入权限。 通过完全信任运行的任何代码（在默认安全策略下，指安装在用户本地硬盘上的任何代码）都具有访问注册表的必要权限。 有关详细信息，请参阅 <xref:System.Security.Permissions.RegistryPermission> 类。  
  
 不应将注册表变量存储在某些内存位置，在这些位置，不具有 <xref:System.Security.Permissions.RegistryPermission> 的代码可访问这些变量。 同样，授予权限时，授予顺利完成操作所需的最小特权。  
  
 通过 <xref:System.Security.Permissions.RegistryPermissionAccess> 枚举定义注册表权限访问值。 下表详细说明了其成员。  
  
|“值”|对注册表变量的访问|  
|-----------|----------------------------------|  
|`AllAccess`|创建、读取和写入|  
|`Create`|创建|  
|`NoAccess`|无访问权限|  
|`Read`|读取|  
|`Write`|Write|  
  
## <a name="checking-values-in-registry-keys"></a>检查注册表项中的值  

 创建注册表值时，需要确定该值已存在时应执行的操作。 另一进程（可能是恶意进程）可能已创建了该值，并拥有对该值的访问权。 将数据放入注册表值后，其他进程即可使用这些数据。 若要防止出现这种情况，请使用 `GetValue` 方法。 如果项不存在，则该方法返回 `Nothing`。  
  
> [!IMPORTANT]
> 从 Web 应用程序读取注册表时，当前用户的标识依赖于在 Web 应用程序中实现的身份验证和模拟。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.MyServices.RegistryProxy>
- [读取和写入注册表](reading-from-and-writing-to-the-registry.md)
