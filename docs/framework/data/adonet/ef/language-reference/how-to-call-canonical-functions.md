---
title: 如何：调用规范函数
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b3d84873-7403-4957-8e20-b4ae39f50214
ms.openlocfilehash: acfbdbaf21fe1d454b68dfef5bf4f88d8020ea65
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204445"
---
# <a name="how-to-call-canonical-functions"></a>如何：调用规范函数

<xref:System.Data.Objects.EntityFunctions> 类包含公开要在 LINQ to Entities 查询中使用的规范函数的方法。 有关规范函数的信息，请参阅[规范函数](canonical-functions.md)。  
  
> [!NOTE]
> <xref:System.Data.Objects.EntityFunctions.AsUnicode%2A> 类中的 <xref:System.Data.Objects.EntityFunctions.AsNonUnicode%2A> 和 <xref:System.Data.Objects.EntityFunctions> 方法不具有规范函数等效性。  
  
 可以直接调用对一组值执行计算并返回单个值的规范函数（也成为聚合规范函数）。 其他规范函数只能作为 LINQ to Entities 查询的一部分调用。 若要直接调用聚合函数，必须将 <xref:System.Data.Objects.ObjectQuery%601> 传递到此函数。 有关更多信息，请参见下面的第二个示例。  
  
 可以在 LINQ to Entities 查询中使用公共语言运行时 (CLR) 方法调用某些规范函数。 有关映射到规范函数的 CLR 方法的列表，请参阅 [Clr 方法到规范函数的映射](clr-method-to-canonical-function-mapping.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 [AdventureWorks 销售模型](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)。 此示例执行一个 LINQ to Entities 查询，该查询使用 <xref:System.Data.Objects.EntityFunctions.DiffDays%2A> 方法返回 `SellEndDate` 与 `SellStartDate` 之间相差小于 365 天的所有产品：  
  
 [!code-csharp[DP L2E CanonicalAndStoreFunctions#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp l2e canonicalandstorefunctions/cs/program.cs#1)]
 [!code-vb[DP L2E CanonicalAndStoreFunctions#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp l2e canonicalandstorefunctions/vb/module1.vb#1)]  
  
## <a name="example"></a>示例  

 下面的示例使用 [AdventureWorks 销售模型](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)。 此示例直接调用聚合 <xref:System.Data.Objects.EntityFunctions.StandardDeviation%2A> 方法，以返回 `SalesOrderHeader` 小计的标准偏差。 请注意，应将 <xref:System.Data.Objects.ObjectQuery%601> 传递给此函数，这样，就可以调用它而不需要使它成为 LINQ to Entities 查询的一部分。  
  
 [!code-csharp[DP L2E CanonicalAndStoreFunctions#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp l2e canonicalandstorefunctions/cs/program.cs#2)]
 [!code-vb[DP L2E CanonicalAndStoreFunctions#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp l2e canonicalandstorefunctions/vb/module1.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [在 LINQ to Entities 查询中调用函数](calling-functions-in-linq-to-entities-queries.md)
- [LINQ to Entities 中的查询](queries-in-linq-to-entities.md)
