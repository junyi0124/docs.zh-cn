---
title: 计算数值序列中值的和
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 24e335b0-984e-4825-8721-0a91b533b7c3
ms.openlocfilehash: 3d160e2cce5f3e0a7eea305657260b6fa4ded7fe
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164436"
---
# <a name="compute-the-sum-of-values-in-a-numeric-sequence"></a>计算数值序列中值的和

使用 <xref:System.Linq.Enumerable.Sum%2A> 运算符可以计算序列中数值的和。  
  
 请注意 `Sum` 中 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 运算符的以下特征：  
  
- 使用标准查询运算符中的聚合运算符 `Sum` 计算空序列或只包含 null 的序列时，所得结果为零。 在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中，SQL 的语义保持不变。 因此，使用 `Sum` 计算空序列或只包含 null 的序列时，所得结果为 null 而非零。  
  
- 针对中间结果的 SQL 限制适用于 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中的聚合。 32 位整型量之和不是使用 64 位结果计算的，且在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 转换 `Sum` 时可能会发生溢出。 即使对于内存中的对应序列，标准查询运算符的实现不会造成溢出，仍存在这种可能性。  
  
## <a name="example"></a>示例  

 下面的示例查找 `Order` 表中所有订单的总运费。  
  
 如果您对 Northwind 示例数据库运行此查询，则输出为：`64942.6900`。  
  
 [!code-csharp[DLinqQueryExamples#12](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#12)]
 [!code-vb[DLinqQueryExamples#12](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#12)]  
  
## <a name="example"></a>示例  

 下面的示例查找所有产品总的订购件数。  
  
 如果您对 Northwind 示例数据库运行此查询，则输出为：`780`。  
  
 请注意，您必须对 `short` 类型（例如，`UnitsOnOrder`）进行强制转换，因为 `Sum` 没有 short 类型的重载。  
  
 [!code-csharp[DLinqQueryExamples#13](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#13)]
 [!code-vb[DLinqQueryExamples#13](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#13)]  
  
## <a name="see-also"></a>请参阅

- [聚合查询](aggregate-queries.md)
- [下载示例数据库](downloading-sample-databases.md)
