---
title: 比较 GUID 和 uniqueidentifier 值
description: 了解如何创建和比较 SQL Server 的 .NET Framework 数据提供程序中的 GUID 值，这些值由 uniqueidentifier 数据类型表示。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: aababd75-2335-43e3-ace8-4b7ae84191a8
ms.openlocfilehash: 649093259747d045945c55e39edb1391708940cf
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173570"
---
# <a name="comparing-guid-and-uniqueidentifier-values"></a>比较 GUID 和 uniqueidentifier 值

SQL Server 中的全局唯一标识符 (GUID) 数据类型由 `uniqueidentifier` 数据类型表示，该数据类型存储 16 字节的二进制值。 GUID 是一个二进制数，它主要用作一个标识符，此标识符必须在一个在多个站点上具有多台计算机的网络中是惟一的。 可以通过调用 Transact-SQL NEWID 函数来生成 GUID，并保证其在全球是唯一的。 有关详细信息，请参阅 [uniqueidentifier (Transact-SQL)](/sql/t-sql/data-types/uniqueidentifier-transact-sql)。  
  
## <a name="working-with-sqlguid-values"></a>使用 SqlGuid 值  

 由于 GUID 值很长且不明确，因此它们对用户没有意义。 如果随机生成的 GUID 用于键值，并且插入大量的行，则会在索引中获得随机 I/O，这会对性能产生负面影响。 与其他数据类型相比，GUID 也相对较大。 通常，我们建议仅将 GUID 用于任何其他数据类型都不适合的极窄方案。  
  
### <a name="comparing-guid-values"></a>比较 GUID 值  

 比较运算符可与 `uniqueidentifier` 值一起使用。 不过，排序不是通过比较两个值的位模式来实现的。 允许对值进行的唯一操作 `uniqueidentifier` 是比较 (=、 <>、 \<, > 、 \<=, > =) 和检查 NULL (为 null，并且不是 null) 。 不允许使用任何其他算术运算符。  
  
 <xref:System.Guid> 和 <xref:System.Data.SqlTypes.SqlGuid> 都具有用于比较不同 GUID 值的 `CompareTo` 方法。 但 `System.Guid.CompareTo` 和 `SqlTypes.SqlGuid.CompareTo` 的实现方式不同。 <xref:System.Data.SqlTypes.SqlGuid> 使用 SQL Server 行为实现 `CompareTo`，一个值的最后 6 个字节是最重要的。 <xref:System.Guid> 计算全部 16 个字节。 下面的示例对这种行为差异进行了演示。 代码的第一部分显示未排序的 <xref:System.Guid> 值，第二部分显示已排序的 <xref:System.Guid> 值。 第三部分显示已排序的 <xref:System.Data.SqlTypes.SqlGuid> 值。 输出显示在代码清单下方。  
  
 [!code-csharp[DataWorks SqlTypes.Guid#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlTypes.Guid/CS/source.cs#1)]
 [!code-vb[DataWorks SqlTypes.Guid#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlTypes.Guid/VB/source.vb#1)]  
  
 此示例生成以下结果。  
  
```output  
Unsorted Guids:  
3aaaaaaa-bbbb-cccc-dddd-2eeeeeeeeeee  
2aaaaaaa-bbbb-cccc-dddd-1eeeeeeeeeee  
1aaaaaaa-bbbb-cccc-dddd-3eeeeeeeeeee  
  
Sorted Guids:  
1aaaaaaa-bbbb-cccc-dddd-3eeeeeeeeeee  
2aaaaaaa-bbbb-cccc-dddd-1eeeeeeeeeee  
3aaaaaaa-bbbb-cccc-dddd-2eeeeeeeeeee  
  
Sorted SqlGuids:  
2aaaaaaa-bbbb-cccc-dddd-1eeeeeeeeeee  
3aaaaaaa-bbbb-cccc-dddd-2eeeeeeeeeee  
1aaaaaaa-bbbb-cccc-dddd-3eeeeeeeeeee  
```  
  
## <a name="see-also"></a>请参阅

- [SQL Server 数据类型和 ADO.NET](sql-server-data-types.md)
- [ADO.NET 概述](../ado-net-overview.md)
