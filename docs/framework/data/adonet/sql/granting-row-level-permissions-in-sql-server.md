---
title: 在 SQL Server 中授予行级权限
ms.date: 03/30/2017
ms.assetid: a55aaa12-34ab-41cd-9dec-fd255b29258c
ms.openlocfilehash: 0b34eaee4b66a2be82049816f0a98b9f53012303
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90554848"
---
# <a name="granting-row-level-permissions-in-sql-server"></a>在 SQL Server 中授予行级权限

在某些情况下，会要求更为细致地控制数据访问，而不仅仅是授予、撤消或拒绝提供的权限。 例如，医院数据库应用程序可能需要限制各个医生只访问与自己的患者相关的信息。 在很多环境（包括财务、法律、政府和军事应用程序）中都存在类似的要求。 为了满足这些情况，SQL Server 2016 提供了 [行级安全](/sql/relational-databases/security/row-level-security) 功能，可以用于简化并集中化安全策略中的行级访问逻辑。 对于 SQL Server 的早期版本，可以通过使用视图制定行级筛选来实现类似功能。

## <a name="implementing-row-level-filtering"></a>实现行级筛选

行级筛选适用于在类似以上医院示例中的单个表格中存储信息的应用程序。 若要实现行级筛选，每行都有一个定义区别性参数（如用户名、标签或其他标识符）的列。 在表格上创建安全策略或视图，利用该表格可以筛选用户可以访问的行。 然后创建参数化存储的过程，利用该过程可以控制用户可以执行的查询的类型。

以下示例介绍了基于用户名或登录名如何配置行级筛选：

- 创建表格，同时添加要存储名称的列。

- 启用行级筛选：

  - 如果你使用的是 SQL Server 2016 或更高版本，或使用的是 [Azure SQL 数据库](/azure/sql-database/)，则创建在表格中添加谓词的安全策略，该表格将返回的行限制为符合当前数据库用户（使用 CURRENT_USER() 内置函数）或当前登录名（使用 SUSER_SNAME() 内置函数）的行：

      ```sql
      CREATE SCHEMA Security
      GO

      CREATE FUNCTION Security.userAccessPredicate(@UserName sysname)
          RETURNS TABLE
          WITH SCHEMABINDING
      AS
          RETURN SELECT 1 AS accessResult
          WHERE @UserName = SUSER_SNAME()
      GO

      CREATE SECURITY POLICY Security.userAccessPolicy
          ADD FILTER PREDICATE Security.userAccessPredicate(UserName) ON dbo.MyTable,
          ADD BLOCK PREDICATE Security.userAccessPredicate(UserName) ON dbo.MyTable
      GO
      ```

  - 如果你使用的是 2016 之前的 SQL Server 版本，则可以使用视图实现类似功能：

      ```sql
      CREATE VIEW vw_MyTable
      AS
          RETURN SELECT * FROM MyTable
          WHERE UserName = SUSER_SNAME()
      GO
      ```

- 创建用于选择、插入、更新和删除数据的存储过程。 如果由安全策略实施筛选，存储过程则应直接在基表上执行这些操作；否则，如果由视图实施筛选，存储过程则应将执行操作改为运行视图。 安全策略或视图会自动筛选由用户查询返回或修改的行，存储过程会提供更坚固的安全边界，以防止具有直接查询访问的用户成功运行可以推断是否存在筛选的数据的查询。

- 对于插入数据的存储过程，使用安全策略或视图中指定的相同函数捕获用户名称，并将该值插入 UserName 列。

- 拒绝 `public` 角色在表格（和视图，如果适用）上的所有权限。 用户将不能从其他数据库角色继承权限，因为筛选器谓词基于用户或登录名，而不基于角色。

- 为数据库角色授予对存储过程的 EXECUTE 权限。 用户只能通过提供的存储过程访问数据。

## <a name="see-also"></a>请参阅

- [行级安全性](/sql/relational-databases/security/row-level-security)
- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [SQL Server 安全性概述](overview-of-sql-server-security.md)
- [SQL Server 中的应用程序安全方案](application-security-scenarios-in-sql-server.md)
- [在 SQL Server 中使用存储过程管理权限](managing-permissions-with-stored-procedures-in-sql-server.md)
- [在 SQL Server 中编写安全动态 SQL](writing-secure-dynamic-sql-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
