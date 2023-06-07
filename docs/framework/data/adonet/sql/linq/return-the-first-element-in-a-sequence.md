---
title: 返回序列中的第一个元素
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ccdc3777-b2c2-44e3-a627-abef8d79a555
ms.openlocfilehash: 4506ef1a79c8f7e77160df4d55d0f93ee79f5a41
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200337"
---
# <a name="return-the-first-element-in-a-sequence"></a>返回序列中的第一个元素

使用 <xref:System.Linq.Enumerable.First%2A> 运算符可返回序列中的第一个元素。 使用 <xref:System.Linq.Enumerable.First%2A> 的查询是立即执行的。  
  
> [!NOTE]
> [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 不支持 <xref:System.Linq.Enumerable.Last%2A> 运算符。  
  
## <a name="example"></a>示例  

 下面的代码查找表中的第一个 `Shipper`：  
  
 如果您对 Northwind 示例数据库运行此查询，则结果为  
  
 `ID = 1, Company = Speedy Express`.  
  
 [!code-csharp[DLinqQueryExamples#14](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#14)]
 [!code-vb[DLinqQueryExamples#14](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#14)]  
  
## <a name="example"></a>示例  

 下面的代码查找具有 `Customer` BONAP 的单个 `CustomerID`。  
  
 如果您对 Northwind 示例数据库运行此查询，则结果为 `ID = BONAP, Contact = Laurence Lebihan`。  
  
 [!code-csharp[DLinqQueryExamples#15](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#15)]
 [!code-vb[DLinqQueryExamples#15](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#15)]  
  
## <a name="see-also"></a>请参阅

- [查询示例](query-examples.md)
- [下载示例数据库](downloading-sample-databases.md)
