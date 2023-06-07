---
title: 查找数值序列中的最小值
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 78203093-f242-4572-9b31-9495b10926aa
ms.openlocfilehash: 2ffff8b69839d5c1e70e81f9fc6f3a97f57ac6c6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155973"
---
# <a name="find-the-minimum-value-in-a-numeric-sequence"></a>查找数值序列中的最小值

使用 <xref:System.Linq.Enumerable.Min%2A> 运算符可返回数值序列中的最小值。  
  
## <a name="example"></a>示例  

 下面的示例查找所有产品的最低单价。  
  
 如果您对 Northwind 示例数据库运行此查询，则输出为：`2.5000`。  
  
 [!code-csharp[DLinqQueryExamples#9](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#9)]
 [!code-vb[DLinqQueryExamples#9](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#9)]  
  
## <a name="example"></a>示例  

 下面的示例查找所有订单的最低运费额。  
  
 如果您对 Northwind 示例数据库运行此查询，则输出为：`0.0200`。  
  
 [!code-csharp[DLinqQueryExamples#10](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#10)]
 [!code-vb[DLinqQueryExamples#10](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#10)]  
  
## <a name="example"></a>示例  

 下面的示例使用 Min 查找每个类别中单价最低的 `Products`。 输出按类别排列。  
  
 [!code-csharp[DLinqQueryExamples#11](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#11)]
 [!code-vb[DLinqQueryExamples#11](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#11)]  
  
 如果您对 Northwind 示例数据库运行上一个查询，所得到的结果将与如下内容类似：  
  
 `1`  
  
 `Guaraná Fantástica`  
  
 `2`  
  
 `Aniseed Syrup`  
  
 `3`  
  
 `Teatime Chocolate Biscuits`  
  
 `4`  
  
 `Geitost`  
  
 `5`  
  
 `Filo Mix`  
  
 `6`  
  
 `Tourtière`  
  
 `7`  
  
 `Longlife Tofu`  
  
 `8`  
  
 `Konbu`  
  
## <a name="see-also"></a>请参阅

- [聚合查询](aggregate-queries.md)
- [下载示例数据库](downloading-sample-databases.md)
