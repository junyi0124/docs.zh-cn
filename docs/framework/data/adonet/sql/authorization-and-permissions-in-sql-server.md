---
title: SQL Server 中的授权和权限
description: 了解如何显式授予权限，使用户可以在 SQL Server 中通过 ADO.NET 访问你创建的数据库对象。
ms.date: 03/30/2017
ms.assetid: d340405c-91f4-4837-a3cc-a238ee89888a
ms.openlocfilehash: 748d40ff2c64ae59f43e6296ef591efcb5ff9831
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197490"
---
# <a name="authorization-and-permissions-in-sql-server"></a>SQL Server 中的授权和权限

在创建数据库对象时，必须显式授予权限以使用户可以访问它们。 每个可保护对象都具有可使用权限语句授予主体的权限。  
  
## <a name="the-principle-of-least-privilege"></a>最低特权原则  

 使用最低特权用户帐户 (LUA) 方法来开发应用程序是抵御安全威胁的全面防御策略的重要组成部分。 LUA 方法确保用户遵循最低特权原则并始终使用受限制的用户帐户登录。 使用不同的固定服务器角色分别执行不同的管理任务，`sysadmin` 固定服务器角色的使用受到严格限制。  
  
 向数据库用户授予权限时，应始终遵循最低特权原则。 向用户或角色授予完成给定任务所必需的最低权限。  
  
> [!IMPORTANT]
> 使用 LUA 方法开发和测试应用程序会给开发过程增加一定的难度。 以系统管理员或数据库所有者身份登录时，创建对象和编写代码要比使用 LUA 帐户登录更容易。 不过，在最低特权用户试图运行需要提升权限才能正常工作的应用程序时，使用高特权帐户开发的应用程序会使功能减少所带来的影响变得混淆。 授予用户过多的权限以重新获得失去的功能会使您的应用程序容易受到攻击。 在使用 LUA 帐户登录时设计、开发和测试应用程序会强制使用规范的方法进行安全规划，以消除异常和以授予提升特权作为快速解决问题的倾向做法。 即使你的应用程序旨在使用 Windows 身份验证来进行部署，你也可以使用 SQL Server 登录名进行测试。  
  
## <a name="role-based-permissions"></a>基于角色的权限  

 向角色而不是用户授予权限可简化安全管理。 分配给角色的权限集由该角色的所有成员继承。 在角色中添加或移除用户要比为单个用户重新创建单独的权限集更为简便。 角色可以嵌套；但是，嵌套的级别太多会降低性能。 您也可以将用户添加到固定数据库角色来简化权限的分配。  
  
 您可以授予架构级别的权限。 对于在架构中创建的所有新对象，用户可以自动继承权限；您无需在创建新对象时授予权限。  
  
## <a name="permissions-through-procedural-code"></a>通过过程代码授予权限  

 通过模块（如存储过程和用户定义函数）包装数据访问可在应用程序周围提供额外的保护层。 通过仅对存储过程或函数授予权限，同时对基础对象（如表）拒绝权限，您可以防止用户直接与数据库对象进行交互。 SQL Server 通过所有权链接实现此目的。  
  
## <a name="permission-statements"></a>权限语句  

 下表说明了三个 Transact-SQL 权限语句。  
  
|权限语句|描述|  
|--------------------------|-----------------|  
|GRANT|授予权限。|  
|REVOKE|撤消权限。 这是新对象的默认状态。 从用户或角色撤消的权限仍可以从主体分配到的其他组或角色继承。|  
|DENY|DENY 撤消一个权限，使其不能被继承。 DENY 优先于所有权限，只是 DENY 不适用于对象所有者或 `sysadmin` 的成员。 如果您针对 `public` 角色对某个对象执行 DENY 权限语句，则会拒绝该对象的所有者和 `sysadmin` 成员以外的所有用户和角色访问该对象。|  
  
- GRANT 语句可以为能够由数据库用户继承的组或角色分配权限。 但是，DENY 语句优先于所有其他权限语句。 因此，已被拒绝某一权限的用户无法从其他角色继承该权限。  
  
> [!NOTE]
> 不能对 `sysadmin` 固定服务器角色和成员对象所有者拒绝权限。  
  
## <a name="ownership-chains"></a>所有权链  

 SQL Server 确保只有已被授予权限的主体才能访问对象。 当多个数据库对象互相访问时，该序列称为链。 当 SQL Server 遍历链中的链接时，它计算权限的方式不同于单独访问各项时计算权限的方式。 当通过链访问一个对象时，SQL Server 首选将对象的所有者与调用对象的所有者（链中的上一个链接）进行比较。 如果这两个对象具有相同所有者，则不检查被引用对象的权限。 每当一个对象访问另一个具有不同所有者的对象时，所有权链就会断开，因此 SQL Server 必须检查调用方的安全上下文。  
  
## <a name="procedural-code-and-ownership-chaining"></a>过程代码和所有权链接  

 假设为某一用户授予了对从表中选择数据的存储过程的执行权限。 如果存储过程和表具有相同的所有者，则无需向用户授予对表的任何权限，甚至可以拒绝权限。 但是，如果存储过程和表具有不同的所有者，则 SQL Server 在允许访问数据前必须检查用户对表的权限。  
  
> [!NOTE]
> 所有权链接不适用于动态 SQL 语句的情况。 若要调用执行 SQL 语句的过程，必须向调用方授予对基础表的权限，这会使您的应用程序容易受到 SQL 注入攻击。 SQL Server 提供了新的机制（如模拟和使用证书对模块进行签名），这些机制无需授予基础表的权限。 它们还可与 CLR 存储过程一起使用。  
  
## <a name="external-resources"></a>外部资源  

 有关详细信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[权限](/sql/relational-databases/security/permissions-database-engine)|包含说明权限层次结构、目录视图以及固定服务器角色权限和数据库角色权限的主题。|
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [SQL Server 中的身份验证](authentication-in-sql-server.md)
- [SQL Server 中的服务器和数据库角色](server-and-database-roles-in-sql-server.md)
- [SQL Server 中的所有权和用户架构分离](ownership-and-user-schema-separation-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
