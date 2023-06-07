---
title: 在 SQL Server 中创建应用程序角色
ms.date: 03/30/2017
ms.assetid: 27442435-dfb2-4062-8c59-e2960833a638
ms.openlocfilehash: 764ae61cba4bf01681d658cc4aacc2aeaecedd3f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173544"
---
# <a name="creating-application-roles-in-sql-server"></a>在 SQL Server 中创建应用程序角色

应用程序角色可提供对应用程序（而不是数据库角色或用户）分配权限的方法。 用户可以连接到数据库、激活应用程序角色以及采用授予应用程序的权限。 授予应用程序角色的权限在连接期间有效。  
  
> [!IMPORTANT]
> 当客户端应用程序在连接字符串中提供应用程序角色名称和密码时，可激活应用程序角色。 因为密码必须存储在客户端计算机上，所以这些角色在二层应用程序上导致一个安全漏洞。 在三层应用程序中，您可以存储密码，以便其他应用程序用户将无法对其进行访问。  
  
## <a name="application-role-features"></a>应用程序角色功能  

 应用程序角色具有以下功能：  
  
- 与数据库角色不同，应用程序角色不包含成员。  
  
- 当客户端应用程序向 `sp_setapprole` 系统存储过程提供应用程序角色名称和密码时，可激活应用程序角色。  
  
- 密码必须存储在客户端计算机上，并且在运行时提供；应用程序角色无法从 SQL Server 内激活。  
  
- 密码不加密。 参数密码作为单向哈希进行存储。  
  
- 一旦激活，通过应用程序角色获取的权限在连接期间保持有效。  
  
- 应用程序角色继承授予 `public` 角色的权限。  
  
- 如果固定服务器角色 `sysadmin` 的成员激活某一应用程序角色，则安全上下文在连接期间切换为应用程序角色的上下文。  
  
- 如果您在具有应用程序角色的数据库中创建 `guest` 帐户，则不必为该应用程序角色或调用它的任何登录名创建数据库用户帐户。 只有当在另一个数据库中存在 `guest` 帐户时，应用程序才能直接访问另一数据库。  
  
- 返回登录名的内置函数（如 SYSTEM_USER）返回调用应用程序角色的登录名。 返回数据库用户名的内置函数将返回应用程序角色的名称。  
  
### <a name="the-principle-of-least-privilege"></a>最低特权原则  

 仅当密码遭泄漏时，应用程序角色才被授予必要的权限。 在使用应用程序角色的任何数据库中，应撤消对 `public` 角色的权限。 在不希望应用程序角色的调用方具有访问权限的任何数据库中，禁用 `guest` 帐户。  
  
### <a name="application-role-enhancements"></a>应用程序角色增强  

 在激活应用程序角色后，可将执行上下文切换回原始调用方，无需禁用连接池。 `sp_setapprole` 过程具有一个创建 Cookie 的新选项，Cookie 包含有关调用方的上下文信息。 您可以通过调用 `sp_unsetapprole` 过程来还原会话，并向其传递 Cookie。  
  
## <a name="application-role-alternatives"></a>应用程序角色替代项  

 应用程序角色依赖于密码的安全性，而密码具有潜在的安全漏洞。 密码被嵌入应用程序代码或保存在磁盘上就有可能泄露。  
  
 可能需要考虑以下替代方法。  
  
- 使用通过 EXECUTE AS 语句及其 NO REVERT 和 WITH COOKIE 字句切换的上下文。 您可以在未映射为登录名的数据库中创建用户帐户。 然后，向此帐户分配权限。 对于很少登录的用户使用 EXECUTE AS 比较安全，因为它基于权限，而不基于密码。 有关详细信息，请参阅 [在 SQL Server 中通过模拟自定义权限](customizing-permissions-with-impersonation-in-sql-server.md)。  
  
- 使用证书对存储过程进行签名，并仅授予执行这些过程的权限。 有关详细信息，请参阅 [SQL Server 中的签名存储过程](signing-stored-procedures-in-sql-server.md)。  
  
## <a name="external-resources"></a>外部资源  

 有关详细信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[应用程序角色](/sql/relational-databases/security/authentication-access/application-roles)|描述如何在 SQL Server 2008 中创建和使用应用程序角色。|  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 安全性概述](overview-of-sql-server-security.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
