---
title: 在 SQL Server 中启用跨数据库访问
ms.date: 03/30/2017
ms.assetid: 10663fb6-434c-4c81-8178-ec894b9cf895
ms.openlocfilehash: 6ea1ed9a6faa39df0a4f9a4a353bf34c7f5ba601
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156259"
---
# <a name="enabling-cross-database-access-in-sql-server"></a>在 SQL Server 中启用跨数据库访问

当某个数据库中的某一过程依赖另一个数据库中的对象时，会发生跨数据库所有权链接。 跨数据库所有权链与单个数据库中的所有权链接的工作方式相同，不同之处在于完整的所有权链要求将所有对象拥有者映射为同一登录帐户。 如果同一登录帐户拥有源数据库中的源对象和目标数据库中的目标对象，则 SQL Server 不会检查对目标对象的权限。  
  
## <a name="off-by-default"></a>默认情况下为关  

 默认情况下，跨数据库的所有权链接已关闭。 Microsoft 建议禁用跨数据库所有权链接，因为它会使您面临以下安全风险：  
  
- 数据库所有者和 `db_ddladmin` 成员或 `db_owners` 数据库角色可创建其他用户所拥有的对象。 这些对象的目标可能是其他数据库中的对象。 这表示如果启用跨数据库所有权链接，您必须完全信任在所有数据库中具有数据的这些用户。  
  
- 具有 CREATE DATABASE 权限的用户可创建新数据库以及附加现有数据库。 如果启用了跨数据库所有权链接，则这些用户可以从新创建的或他们创建的附加数据库中访问他们在其中没有权限的其他数据库中的对象。  
  
## <a name="enabling-cross-database-ownership-chaining"></a>启用跨数据库所有权链接  

 只能在完全信任高级权限用户的环境中启用跨数据库所有权链接。 可在设置所有数据库，或使用 Transact-SQL 命令 `sp_configure` 和 `ALTER DATABASE` 选择性地设置特定数据库期间，配置跨数据库所有权链接。  
  
 若要有选择地配置跨数据库所有权链接，请使用 `sp_configure` 将服务器的所有权链接关闭。 然后，使用包含 SET DB_CHAINING ON 的 ALTER DATABASE 命令仅为需要跨数据库所有权链接的数据库配置跨数据库所有权链接。  
  
 下面的示例针对所有数据库启用跨数据库所有权链接：  
  
```sql
EXECUTE sp_configure 'show advanced', 1;  
RECONFIGURE;  
EXECUTE sp_configure 'cross db ownership chaining', 1;  
RECONFIGURE;  
```  
  
 下面的示例针对特定数据库启用跨数据库所有权链接：  
  
```sql
ALTER DATABASE Database1 SET DB_CHAINING ON;  
ALTER DATABASE Database2 SET DB_CHAINING ON;  
```  
  
### <a name="dynamic-sql"></a>动态 SQL  

 在执行了动态创建的 SQL 语句的情况下跨数据库所有权链接将不起作用，除非同一用户同时存在于两个数据库中。 在 SQL Server 中，可以通过创建一个可访问另一个数据库中数据的存储过程并用两个数据库中都存在的证书为此过程签名来解决这个问题。 这样，用户就可以访问该过程使用的数据库资源，而无需授予他们数据库访问或权限。  
  
## <a name="external-resources"></a>外部资源  

 有关详细信息，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[使用 EXECUTE AS](/previous-versions/sql/sql-server-2008-r2/ms188304(v=sql.105)) 和 [跨数据库所有权链接选项](/sql/database-engine/configure-windows/cross-db-ownership-chaining-server-configuration-option)扩展数据库模拟。|本文介绍如何为 SQL Server 实例配置跨数据库所有权链接。|  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 安全性概述](overview-of-sql-server-security.md)
- [在 SQL Server 中使用存储过程管理权限](managing-permissions-with-stored-procedures-in-sql-server.md)
- [在 SQL Server 中编写安全动态 SQL](writing-secure-dynamic-sql-in-sql-server.md)
- [SQL Server 中的签名存储过程](signing-stored-procedures-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
