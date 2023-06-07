---
title: 查找数值序列中的最大值
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 70d7c058-0280-4815-a008-6f290093591a
ms.openlocfilehash: b70b94338ca7bdbb600bac697d3a36ff117d757e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155999"
---
# <a name="find-the-maximum-value-in-a-numeric-sequence"></a>查找数值序列中的最大值

使用 <xref:System.Linq.Enumerable.Max%2A> 运算符可查找数值序列中的最高值。  
  
## <a name="example"></a>示例  

 下面的示例查找任何员工的最近雇佣日期。  
  
 如果您对 Northwind 示例数据库运行此查询，则输出为：`11/15/1994 12:00:00 AM`。  
  
 [!code-csharp[DLinqQueryExamples#6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#6)]
 [!code-vb[DLinqQueryExamples#6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#6)]  
  
## <a name="example"></a>示例  

 下面的示例查找任何产品的最大库存件数。  
  
 如果您对 Northwind 示例数据库运行此示例，则输出为：`125`。  
  
 [!code-csharp[DLinqQueryExamples#7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#7)]
 [!code-vb[DLinqQueryExamples#7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#7)]  
  
## <a name="example"></a>示例  

 下面的示例使用 Max 查找每个类别中单价最高的 `Products`。 然后，按类别列出输出结果。  
  
 [!code-csharp[DLinqQueryExamples#8](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#8)]
 [!code-vb[DLinqQueryExamples#8](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#8)]  
  
 如果您对 Northwind 示例数据库运行上一个查询，所得到的结果将与如下内容类似：  
  
 `1`  
  
 `Côte de Blaye`  
  
 `2`  
  
 `Vegie-spread`  
  
 `3`  
  
 `Sir Rodney's Marmalade`  
  
 `4`  
  
 `Raclette Courdavault`  
  
 `5`  
  
 `Gnocchi di nonna Alice`  
  
 `6`  
  
 `Thüringer Rostbratwurst`  
  
 `7`  
  
 `Manjimup Dried Apples`  
  
 `8`  
  
 `Carnarvon Tigers`  
  
## <a name="see-also"></a>请参阅

- [聚合查询](aggregate-queries.md)
- [下载示例数据库](downloading-sample-databases.md)
