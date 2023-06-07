---
title: 远程查询执行与本地执行
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ee50e943-9349-4c84-ab1c-c35d3ada1a9c
ms.openlocfilehash: c99e726902192fc8324e77441b80aa4519c55ddc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91196944"
---
# <a name="remote-vs-local-execution"></a>远程查询执行与本地执行

您可以决定以远程方式（即数据库引擎对数据库执行查询）或在本地（[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 对本地缓存执行查询）执行您的查询。  
  
## <a name="remote-execution"></a>远程执行  

 请考虑下列查询：  
  
 [!code-csharp[DLinqQueryConcepts#7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#7)]
 [!code-vb[DLinqQueryConcepts#7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#7)]  
  
 如果数据库有数千行订单，则在处理其中很小一部分时您不需要将它们全都检索出来。 在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中，<xref:System.Data.Linq.EntitySet%601> 类实现了 <xref:System.Linq.IQueryable> 接口。 这种方式确保了可以以远程方式执行此类查询。 利用此技术有两大优点：  
  
- 不会检索到不需要的数据。  
  
- 由于利用了数据库索引，由数据库引擎执行的查询通常更为高效。  
  
## <a name="local-execution"></a>本地执行  

 在其他一些情况下，您可能需要在本地缓存中保留完整的相关实体集。 为此，<xref:System.Data.Linq.EntitySet%601> 提供了 <xref:System.Data.Linq.EntitySet%601.Load%2A> 方法，用于显式加载 <xref:System.Data.Linq.EntitySet%601> 的所有成员。  
  
 如果 <xref:System.Data.Linq.EntitySet%601> 已经加载，则后续查询将在本地执行。 这种方式在两个方面起到帮助作用：  
  
- 如果此完整集必须在本地使用或使用多次，则您可以避免远程查询和与之相关的延迟。  
  
- 实体可以序列化为完整的实体。  
  
 下面的代码段演示了如何实现本地执行：  
  
 [!code-csharp[DLinqQueryConcepts#8](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#8)]
 [!code-vb[DLinqQueryConcepts#8](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#8)]  
  
## <a name="comparison"></a>比较  

 这两项功能提供了强大的选项组合：大型集合采用以远程方式执行，小型集合或在需要完整集的情况下在本地执行。 您需要通过 <xref:System.Linq.IQueryable> 进行远程执行，对于本地执行，则需要对内存中的 <xref:System.Collections.Generic.IEnumerable%601> 集合执行。 若要强制执行本地执行 (即 <xref:System.Collections.Generic.IEnumerable%601>) ，请参阅 [将类型转换为泛型 IEnumerable](convert-a-type-to-a-generic-ienumerable.md)。  
  
### <a name="queries-against-unordered-sets"></a>针对无序集的查询  

 请注意实现的本地集合 <xref:System.Collections.Generic.List%601> 与提供对关系数据库中 *无序集* 执行的远程查询的集合之间的重要区别。 <xref:System.Collections.Generic.List%601> 方法（如使用索引值的那些方法）需要列表语义，列表语义通常无法通过针对无序集的远程查询获得。 因此，此类方法隐式加载 <xref:System.Data.Linq.EntitySet%601>，以允许本地执行。  
  
## <a name="see-also"></a>请参阅

- [查询概念](query-concepts.md)
