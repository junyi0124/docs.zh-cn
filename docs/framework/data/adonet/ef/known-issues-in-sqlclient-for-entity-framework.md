---
title: SqlClient 中的已知问题（实体框架）
ms.date: 03/30/2017
ms.assetid: 48fe4912-4d0f-46b6-be96-3a42c54780f6
ms.openlocfilehash: 707c749e4dff5d1bbc8d372632aae502092db060
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91198101"
---
# <a name="known-issues-in-sqlclient-for-entity-framework"></a>SqlClient 中的已知问题（实体框架）

本节介绍与 SQL Server .NET Framework 数据提供程序 (SqlClient) 有关的已知问题。  
  
## <a name="trailing-spaces-in-string-functions"></a>字符串函数中的尾随空格  

 SQL Server 忽略字符串值中的尾随空格。 因此，传递字符串中的尾随空格可能导致意外的结果，甚至会导致失败。  
  
 如果字符串中必须包含尾随空格，则应考虑在末尾追加一个空格字符，以便 SQL Server 不会剪裁字符串。 如果尾随空格不是必需的，应在传递给查询管道之前进行修整。  
  
## <a name="right-function"></a>RIGHT 函数  

 如果将非 `null` 值和 0 分别作为第一个自变量和第二个自变量传递给 `RIGHT(nvarchar(max)`, 0`)` 或 `RIGHT(varchar(max)`, 0`)`，将返回 `NULL` 值，而不是 `empty` 字符串。  
  
## <a name="cross-and-outer-apply-operators"></a>CROSS 和 OUTER APPLY 运算符  

 SQL Server 2005 中引入了交叉和外部 APPLY 运算符。 在某些情况下，查询管道可能生成包含 CROSS APPLY 和/或 OUTER APPLY 运算符的 Transact-SQL 语句。 因为某些后端提供程序（包括早于 SQL Server 2005 的 SQL Server 版本）不支持这些运算符，所以不能对这些后端提供程序执行此类查询。  
  
 下面是一些可能导致输出查询中出现 CROSS APPLY 和/或 OUTER APPLY 运算符的典型情况：  
  
- 带分页的相关子查询。  
  
- 相关子查询或导航所生成的集合上的 `AnyElement`。  
  
- 使用接受元素选择器的分组方法的 LINQ 查询。  
  
- 显式指定了 CROSS APPLY 或 OUTER APPLY 的查询  
  
- 在 REF 构造上具有 DEREF 构造的查询。  
  
## <a name="skip-operator"></a>SKIP 运算符  

 如果使用的是 SQL Server 2000，则对非键列使用带有 ORDER BY 的 SKIP 可能会返回不正确的结果。 如果非键列中有重复数据，那么跳过的行数可能多于指定的行数。 这是因为如何为 SQL Server 2000 转换 SKIP。 例如，在下面的查询中，如果 `E.NonKeyColumn` 有重复值，则跳过的行可能超过 5 行：  
  
```sql  
SELECT [E] FROM Container.EntitySet AS [E] ORDER BY [E].[NonKeyColumn] DESC SKIP 5L  
```  
  
## <a name="targeting-the-correct-sql-server-version"></a>以正确的 SQL Server 版本为目标  

 实体框架根据 `ProviderManifestToken` 存储模型 ( ssdl) 文件中架构元素的属性中指定的 SQL Server 版本来面向 transact-sql 查询。 您实际连接到的 SQL Server 的版本可能不是这一版本。 例如，如果使用 SQL Server 2005，但 `ProviderManifestToken` 属性设置为2008，则生成的 transact-sql 查询可能不会在服务器上执行。 例如，在 SQL Server 的早期版本上，无法执行使用了 SQL Server 2008 所引入的新日期时间类型的查询。 如果你使用的是 SQL Server 2005，但你 `ProviderManifestToken` 的特性设置为2000，则生成的 transact-sql 查询可能比较少，或者你可能会收到一个异常，指出不支持该查询。 有关更多信息，请参见本主题前面的“CROSS 和 OUTER APPLY 运算符”部分。  
  
 某些数据库行为取决于为数据库设置的兼容级别。 如果 `ProviderManifestToken` 将属性设置为2005，并且 SQL Server 版本为2005，但数据库的兼容级别设置为 "80" (SQL Server 2000) ，则生成的 transact-sql 将面向 SQL Server 2005，但可能无法按预期执行，因为兼容性级别设置。 例如，如果 ORDER BY 列表中的列名与选择器中的列名相同，则可能会丢失排序信息。  
  
## <a name="nested-queries-in-projection"></a>投影中的嵌套查询  

 投影子句中的嵌套查询可在服务器上转换为笛卡尔积查询。 在某些后端服务器（包括 SQL Server）上，这会导致 TempDB 表变得很大。 这样会降低服务器性能。  
  
 下面是投影子句中嵌套查询的示例：  
  
```sql  
SELECT c, (SELECT c, (SELECT c FROM AdventureWorksModel.Vendor AS c  ) As Inner2 FROM AdventureWorksModel.JobCandidate AS c  ) As Inner1 FROM AdventureWorksModel.EmployeeDepartmentHistory AS c  
```  
  
## <a name="server-generated-guid-identity-values"></a>服务器生成的 GUID 标识值  

 实体框架支持服务器生成的 GUID 类型标识值，但提供程序必须支持在插入行后返回服务器生成的标识值。 从 SQL Server 2005 开始，可以通过 [OUTPUT 子句](/sql/t-sql/queries/output-clause-transact-sql)返回 SQL Server 数据库中服务器生成的 GUID 类型。
  
## <a name="see-also"></a>请参阅

- [用于实体框架的 SqlClient](sqlclient-for-the-entity-framework.md)
- [LINQ to Entities 中的已知问题和注意事项](./language-reference/known-issues-and-considerations-in-linq-to-entities.md)
