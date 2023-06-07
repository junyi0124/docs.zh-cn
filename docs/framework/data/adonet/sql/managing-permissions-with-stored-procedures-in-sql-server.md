---
title: 在 SQL Server 中使用存储过程管理权限
description: 了解如何通过使用存储过程或用户定义函数实现访问权限来限制对数据和数据库对象的访问。
ms.date: 03/30/2017
ms.assetid: 08fa34e8-2ffa-470d-ba62-e511a5f8558e
ms.openlocfilehash: 44f6a0c3ca6b913c8998c4e5ddb60eab2b64e71b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172718"
---
# <a name="managing-permissions-with-stored-procedures-in-sql-server"></a>在 SQL Server 中使用存储过程管理权限

为数据库创建多道防线的一个方法是使用存储过程或用户定义的函数实现所有数据访问。 撤消或拒绝对基础对象（如表）的所有权限，并授予对存储过程的 EXECUTE 权限。 这会为数据和数据库对象有效创建安全外围防线。  
  
## <a name="stored-procedure-benefits"></a>存储过程的优点  

 存储过程具有以下优点：  
  
- 可以包装数据逻辑和业务规则，以便用户可以仅通过开发人员和数据库管理员打算使用的方式访问数据和对象。  
  
- 验证所有用户输入的参数化存储过程可用于阻止 SQL 注入攻击。 如果使用动态 SQL，请确保将命令参数化，并绝对不能将参数值直接包括在查询字符串中。  
  
- 可禁止即席查询和数据修改。 这样将阻止用户恶意或无意中损坏数据或执行查询，以避免降低服务器或网络的性能。  
  
- 可以在过程代码中处理错误，而无需将错误直接传递给客户端应用程序。 这样可防止返回错误消息，以避免其可能有助于探测攻击。 在服务器上记录错误并对其进行处理。  
  
- 存储过程只能编写一次，可由很多应用程序访问。  
  
- 客户端应用程序不需要知道有关基础数据结构的任何信息。 只要更改不影响参数列表或返回的数据类型，就可以更改存储过程代码，而无需在客户端应用程序中进行更改。  
  
- 存储过程可通过将多个操作组合到一个过程调用中来减少网络通讯。  
  
## <a name="stored-procedure-execution"></a>存储过程的执行  

 存储过程利用所有权链接来提供对数据的访问，这样，用户就不必拥有访问数据库对象的显式权限。 如果按顺序相互访问的对象由同一个用户所拥有，就会存在所有权链接。 例如，存储过程可以调用其他存储过程，或者存储过程可访问多个表。 如果执行链中的所有对象的所有者相同，则 SQL Server 只检查调用方的 EXECUTE 权限，而不检查调用方对其他对象的权限。 因此，只需对存储过程授予 EXECUTE 权限；可以撤消或拒绝对基础表的所有权限。  
  
## <a name="best-practices"></a>最佳实践  

 仅编写存储过程不足以保证应用程序的安全， 还应当考虑以下潜在的安全漏洞。  
  
- 为您希望其能够访问数据的数据库角色授予对存储过程的 EXECUTE 权限。  
  
- 撤消或拒绝数据库中所有角色和用户（包括 `public` 角色）对基础表的所有权限。 所有用户会从公共角色中继承权限。 因此，拒绝对 `public` 的权限表示只有所有者和 `sysadmin` 成员具有访问权；所有其他用户将无法从其他角色的成员资格继承权限。  
  
- 请不要将用户或角色添加到 `sysadmin` 或 `db_owner` 角色。 系统管理员和数据库所有者可访问所有数据库对象。  
  
- 禁用 `guest` 帐户。 这样将阻止匿名用户连接到数据库。 默认情况下，会在新数据库中禁用来宾帐户。  
  
- 实现错误处理和记录错误。  
  
- 创建用于验证所有用户输入的参数化存储过程。 将所有用户输入视为不受信任。  
  
- 除非绝对必要，否则应避免使用动态 SQL。 使用 Transact-SQL QUOTENAME() 函数可分隔字符串值，并对输入字符串中的任何分隔符进行转义。  
  
## <a name="external-resources"></a>外部资源  

 有关详细信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[存储过程](/sql/relational-databases/stored-procedures/stored-procedures-database-engine) 和 [SQL 注入](/sql/relational-databases/security/sql-injection)|本文介绍如何创建存储过程和 SQL 注入的工作方式。|  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 安全性概述](overview-of-sql-server-security.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [在 SQL Server 中编写安全动态 SQL](writing-secure-dynamic-sql-in-sql-server.md)
- [SQL Server 中的签名存储过程](signing-stored-procedures-in-sql-server.md)
- [在 SQL Server 中使用模拟自定义权限](customizing-permissions-with-impersonation-in-sql-server.md)
- [使用存储过程修改数据](../modifying-data-with-stored-procedures.md)
- [ADO.NET 概述](../ado-net-overview.md)
