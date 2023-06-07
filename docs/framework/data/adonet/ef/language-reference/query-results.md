---
title: 查询结果
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: bcd7b699-4e50-4523-8c33-2f54a103d94e
ms.openlocfilehash: 5eb23525f685c4ebf22ac24d16aa3ee66297e172
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202222"
---
# <a name="query-results"></a>查询结果

将 LINQ to Entities 查询转换为命令目录树并执行后，查询结果通常作为以下之一返回：  
  
- 概念模型中零个或多个类型化实体对象的集合或复杂类型的投影。  
  
- 概念模型支持的 CLR 类型。  
  
- 内联集合。  
  
- 匿名类型。  
  
 对数据源执行查询时，结果将具体化为 CLR 类型并返回客户端。 所有对象具体化均由实体框架执行。 由于无法在实体框架与 CLR 之间进行映射而导致的任何错误都将导致在对象具体化过程中引发异常。
  
 如果查询执行返回基元概念模型类型，则结果由独立的 CLR 类型和与实体框架断开连接的 CLR 类型组成。 但如果查询返回 <xref:System.Data.Objects.ObjectQuery%601> 所表示的类型化实体对象的集合，则这些类型由对象上下文进行跟踪。 所有对象行为 (如实体框架中定义的子/父集合、更改跟踪、多态性等) 。 此功能可用于其容量，如实体框架中所定义。 有关详细信息，请参阅使用 [对象](../working-with-objects.md)。
  
 从查询返回的结构类型（如匿名类型和可以为 null 的复杂类型）可以是 `null` 值。 返回的实体的 <xref:System.Data.Objects.DataClasses.EntityCollection%601> 属性也可以是 `null` 值。 投影值为 `null` 的实体的集合属性会导致这种情况，例如，调用没有任何元素的 <xref:System.Linq.Queryable.FirstOrDefault%2A> 的 <xref:System.Data.Objects.ObjectQuery%601>。  
  
 在某些情况下，某个查询在其执行期间看似生成具体化结果，但该查询将在服务器上执行，实体对象绝不会在 CLR 中具体化。 如果对象具体化的副作用对您有很大影响，这可能会导致问题。  
  
 下面的示例包含一个自定义类 `MyContact`，该类有一个 `LastName` 属性。 如果设置 `LastName` 属性，`count` 变量就会递增。 如果执行以下两个查询，则第一个查询将使 `count` 递增，而第二个查询不会。 这是因为：在第二个查询中，将从结果中投影 `LastName` 属性，而绝不会创建 `MyContact` 类，因为这不是对存储区执行查询所必需的。  
  
 [!code-csharp[DP L2E Materialization Example#MaterializationSideEffects](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Materialization Example/CS/Program.cs#materializationsideeffects)]
 [!code-vb[DP L2E Materialization Example#MaterializationSideEffects](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Materialization Example/VB/Module1.vb#materializationsideeffects)]  
  
 [!code-csharp[DP L2E Materialization Example#MyContactClass](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Materialization Example/CS/Program.cs#mycontactclass)]
 [!code-vb[DP L2E Materialization Example#MyContactClass](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Materialization Example/VB/Module1.vb#mycontactclass)]
