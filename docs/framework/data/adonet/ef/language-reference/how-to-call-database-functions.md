---
title: 如何：调用数据库函数
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 79038efa-15bf-464a-83e2-35fe145252ce
ms.openlocfilehash: a4cf77000af56ed2a6445beaef2d7856486b85db
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173661"
---
# <a name="how-to-call-database-functions"></a>如何：调用数据库函数

<xref:System.Data.Objects.SqlClient.SqlFunctions> 类包含公开要在 LINQ to Entities 查询中使用的 SQL Server 函数的方法。 当您在 LINQ to Entities 查询中使用 <xref:System.Data.Objects.SqlClient.SqlFunctions> 方法时，将在数据库中执行相应的数据库函数。  
  
> [!NOTE]
> 可以直接调用对一组值执行计算并返回单个值的数据库函数（也称为聚合数据库函数）。 其他规范函数只能作为 LINQ to Entities 查询的一部分调用。 若要直接调用聚合函数，必须将 <xref:System.Data.Objects.ObjectQuery%601> 传递到此函数。 有关更多信息，请参见下面的第二个示例。  
  
> [!NOTE]
> <xref:System.Data.Objects.SqlClient.SqlFunctions> 类中的方法特定于 SQL Server 函数。 通过其他提供程序可能能够提供公开数据库函数的相似类。  
  
## <a name="example"></a>示例  

 下面的示例使用 [AdventureWorks 销售模型](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)。 此示例执行一个 LINQ to Entities 查询，该查询使用 <xref:System.Data.Objects.SqlClient.SqlFunctions.CharIndex%2A> 方法返回其姓氏以“Si”开头的所有联系人：  
  
 [!code-csharp[DP L2E CanonicalAndStoreFunctions#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp l2e canonicalandstorefunctions/cs/program.cs#3)]
 [!code-vb[DP L2E CanonicalAndStoreFunctions#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp l2e canonicalandstorefunctions/vb/module1.vb#3)]  
  
## <a name="example"></a>示例  

 下面的示例使用 [AdventureWorks 销售模型](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)。 此示例直接调用聚合 <xref:System.Data.Objects.SqlClient.SqlFunctions.ChecksumAggregate%2A> 方法。 请注意，应将 <xref:System.Data.Objects.ObjectQuery%601> 传递给此函数，这样，就可以调用它而不需要使它成为 LINQ to Entities 查询的一部分。  
  
 [!code-csharp[DP L2E CanonicalAndStoreFunctions#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp l2e canonicalandstorefunctions/cs/program.cs#4)]
 [!code-vb[DP L2E CanonicalAndStoreFunctions#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp l2e canonicalandstorefunctions/vb/module1.vb#4)]  
  
## <a name="see-also"></a>请参阅

- [在 LINQ to Entities 查询中调用函数](calling-functions-in-linq-to-entities-queries.md)
- [LINQ to Entities 中的查询](queries-in-linq-to-entities.md)
