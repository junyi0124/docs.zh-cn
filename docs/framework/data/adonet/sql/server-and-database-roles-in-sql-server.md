---
title: SQL Server 中的服务器和数据库角色
description: 了解固定服务器和固定数据库角色，这些角色具有分配给它们的固定权限集。 SQL Server 使用基于角色的安全性。
ms.date: 03/30/2017
ms.assetid: 5482dfdb-e498-4614-8652-b174829eed13
ms.openlocfilehash: 3babf6b249da6e67a6a48bcad647e4674650c348
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177327"
---
# <a name="server-and-database-roles-in-sql-server"></a>SQL Server 中的服务器和数据库角色

所有版本的 SQL Server 均使用基于角色的安全，它允许您为角色、用户组而不是各个用户分配权限。 固定服务器和固定数据库角色具有分配给它们的一组固定的权限。  
  
## <a name="fixed-server-roles"></a>固定服务器角色  

 固定服务器角色具有一组固定的权限，并且适用于整个服务器范围。 它们专门用于管理 SQL Server，且不能更改分配给它们的权限。 可以在数据库中不存在用户帐户的情况下向固定服务器角色分配登录。  
  
> [!IMPORTANT]
> `sysadmin` 固定服务器角色包含所有其他角色并且具有无限范围。 请不要将这些主体添加到此角色，除非它们是高度信任的。 `sysadmin` 角色成员对所有服务器数据库和资源有不可撤消的管理特权。  
  
 将用户添加到固定服务器角色时，请仔细选择。 例如，`bulkadmin` 角色允许用户将任意本地文件的内容插入表中，这样可能会损坏数据的完整性。 有关固定服务器角色和权限的完整列表，请参阅 SQL Server 联机丛书。  
  
## <a name="fixed-database-roles"></a>固定数据库角色  

 固定数据库角色具有一组预定义的权限，这些权限旨在允许您轻松管理权限组。 `db_owner` 角色的成员可对数据库执行所有配置和维护活动。  
  
 有关 SQL Server 预定义角色的更多信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[服务器级角色](/sql/relational-databases/security/authentication-access/server-level-roles)|描述固定服务器角色以及与 SQL Server 相关联的权限。|  
|[数据库级别的角色](/sql/relational-databases/security/authentication-access/database-level-roles)|描述固定数据库角色及与其关联的权限|  
  
## <a name="database-roles-and-users"></a>数据库角色和用户  

 要使用数据库对象，必须将登录映射到数据库用户帐户。 这样就可以将数据库用户添加到数据库角色，从而继承与这些角色关联的任何权限集。 可以授予所有权限。  
  
 当为应用程序设计安全性时，还必须考虑 `public` 角色、`dbo` 用户帐户和 `guest` 帐户。  
  
### <a name="the-public-role"></a>公共角色  

 `public` 角色包含在每个数据库中，包括系统数据库。 无法删除该角色，也无法向其中添加用户或从中删除用户。 授予 `public` 角色的权限由所有其他用户和角色继承，因为默认情况下，它们属于 `public` 角色。 仅为 `public` 角色授予您希望所有用户都具有的权限。  
  
### <a name="the-dbo-user-account"></a>dbo 用户帐户  

 `dbo` 或数据库所有者是具有在数据库中执行所有活动的默示权限的用户帐户。 `sysadmin` 固定服务器角色的成员会自动映射到 `dbo`。  
  
> [!NOTE]
> `dbo` 也是架构的名称，如 [SQL Server 中的所有权和用户架构分离](ownership-and-user-schema-separation-in-sql-server.md)中所述。  
  
 `dbo` 用户帐户经常与 `db_owner` 固定数据库角色相混淆。 `db_owner` 的作用域是一个数据库；`sysadmin` 的作用域是整个服务器。 `db_owner` 角色中的成员无法授予 `dbo` 用户特权。  
  
### <a name="the-guest-user-account"></a>guest 用户帐户  

 用户经过身份验证并允许登录 SQL Server 的实例后，用户需要访问的每个数据库中必须存在一个单独的用户帐户。 要求每个数据库中具有用户帐户会阻止用户连接到 SQL Server 的实例，并且会阻止用户访问服务器上的所有数据库。 通过允许没有数据库用户帐户的登录访问数据库，可在数据库中包含 `guest` 用户帐户时避开此需求。  
  
 在所有版本的 SQL Server 中，`guest` 帐户均为内置帐户。 默认情况下，它在新的数据库中是禁用的。 如果启用此帐户，则可以通过撤消其 CONNECT 权限（方法是执行 Transact-SQL REVOKE CONNECT FROM GUEST 语句）来禁用此帐户。  
  
> [!IMPORTANT]
> 避免使用 `guest` 帐户；没有其自己的数据库权限的所有登录都会获取授予此帐户的数据库权限。 如果必须使用 `guest` 帐户，请为其授予最小权限。  
  
 有关 SQL Server 登录名、用户和角色的更多信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[数据库引擎权限入门](/sql/relational-databases/security/authentication-access/getting-started-with-database-engine-permissions)|包含指向描述主体、角色、凭据、安全对象和权限的主题的链接。|  
|[主体](/sql/relational-databases/security/authentication-access/principals-database-engine)|描述主体并包含指向描述服务器和数据库角色的主题的链接。|  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [SQL Server 中的身份验证](authentication-in-sql-server.md)
- [SQL Server 中的所有权和用户架构分离](ownership-and-user-schema-separation-in-sql-server.md)
- [SQL Server 中的授权和权限](authorization-and-permissions-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
