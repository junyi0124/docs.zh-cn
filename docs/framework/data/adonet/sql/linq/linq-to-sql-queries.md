---
title: LINQ to SQL 查询
ms.date: 03/30/2017
ms.assetid: f4897aaa-7f44-4c20-a471-b948c2971aae
ms.openlocfilehash: c49644a866a6e245c6be1f9a8e8f95d003fd0191
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175221"
---
# <a name="linq-to-sql-queries"></a>LINQ to SQL 查询

使用与 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] LINQ 中相同的语法来定义查询。 唯一的差异是您的查询中引用的对象映射到数据库中的元素。 有关详细信息，请参阅 [LINQ 查询简介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 将您编写的查询转换成等效的 SQL 查询，然后将它们发送至服务器进行处理。 更具体地说，您的应用程序使用 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] API 来请求查询执行。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 提供程序随后会将查询转换成 SQL 文本，并委托 ADO 提供程序执行。 ADO 提供程序将查询结果作为 `DataReader` 返回。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 提供程序将 ADO 结果转换成用户对象的 <xref:System.Linq.IQueryable> 集合。  
  
> [!NOTE]
> .NET Framework 内置类型上的大多数方法和运算符都可以直接翻译到 SQL。 LINQ 无法转换的将生成运行时异常。 有关详细信息，请参阅 [SQL-CLR 类型映射](sql-clr-type-mapping.md)。  
  
 下表显示了 LINQ 和 query 项之间的相似之处和不同之处 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 。  
  
|项|LINQ 查询|[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] Query|  
|----------|----------------|----------------------------------------------------------------------|  
|保存查询的局部变量的返回类型（对于返回序列的查询而言）|泛型 `IEnumerable`|泛型 `IQueryable`|  
|指定数据源|使用 `From` (Visual Basic) 或 `from` (c # ) 子句|相同|  
|Filtering|使用 `Where` / `where` 子句|相同|  
|分组|使用 `Group…By` / `groupby` 子句|相同|  
|选择（投影）|使用 `Select` / `select` 子句|相同|  
|延迟执行与立即执行|请参阅 [c # (的 LINQ 查询简介 ) ](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)|相同|  
|实现联接|使用 `Join` / `join` 子句|可以使用 `Join` / `join` 子句，但更有效地使用 <xref:System.Data.Linq.Mapping.AssociationAttribute> 特性。 有关详细信息，请参阅 [跨关系进行查询](querying-across-relationships.md)。|  
|远程执行与本地执行||有关详细信息，请参阅 [远程执行与本地执行](remote-vs-local-execution.md)。|  
|流式查询与缓存查询|在本地内存情况中不适用||  
  
## <a name="see-also"></a>请参阅

- [LINQ 查询简介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)
- [基本 LINQ 查询操作](../../../../../csharp/programming-guide/concepts/linq/basic-linq-query-operations.md)
- [LINQ 查询操作中的类型关系](../../../../../csharp/programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)
- [查询概念](query-concepts.md)
