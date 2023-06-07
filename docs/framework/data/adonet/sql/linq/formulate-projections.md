---
title: 构建投影
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 745742df-0eda-479b-83f8-29bd8a80db96
ms.openlocfilehash: f0bc6dfcff7778ebc7156cbb039e13570c90467b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194396"
---
# <a name="formulate-projections"></a>构建投影

下面的示例演示了 `select` c # 中的语句和 Visual Basic 中的语句如何 `Select` 与其他功能组合起来以形成查询投影。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Select` `select` c # ) 中 Visual Basic (子句中的子句来返回的联系人名称序列 `Customers` 。  
  
 [!code-csharp[DLinqQueryExamples#57](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#57)]
 [!code-vb[DLinqQueryExamples#57](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#57)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `Select` c # 中 Visual Basic (子句中的子句 `select` ) 和 *匿名类型* 返回的联系人姓名和电话号码序列 `Customers` 。  
  
 [!code-csharp[DLinqQueryExamples#58](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#58)]
 [!code-vb[DLinqQueryExamples#58](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#58)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `Select` c # 中 Visual Basic (子句中的子句 `select` ) 和 *匿名类型* 返回员工的姓名和电话号码序列。 在产生的序列中，`FirstName` 和 `LastName` 字段组合成单个字段 (`Name`)，`HomePhone` 字段重命名为 `Phone`。  
  
 [!code-csharp[DLinqQueryExamples#59](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#59)]
 [!code-vb[DLinqQueryExamples#59](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#59)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `Select` c # 中 Visual Basic (子句中的子句 `select` ) 和 *匿名类型* 返回一个序列，其中的所有 `ProductID` 和一个名为的计算值 `HalfPrice` 。 此值设置为 `UnitPrice` 的 1/2。  
  
 [!code-csharp[DLinqQueryExamples#60](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#60)]
 [!code-vb[DLinqQueryExamples#60](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#60)]  
  
## <a name="example"></a>示例  

 下面的示例使用了 `Select` c # 中 Visual Basic (子句中的子句 `select` ) 和一个 *条件语句* 来返回产品名称和产品可用性的序列。  
  
 [!code-csharp[DLinqQueryExamples#61](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#61)]
 [!code-vb[DLinqQueryExamples#61](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#61)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `Select` c # 中的 Visual Basic 子句 (`select` 子句 ) 和 (名称) 的 *已知类型* 返回员工姓名的序列。  
  
 [!code-csharp[DLinqQueryExamples#62](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#62)]
 [!code-vb[DLinqQueryExamples#62](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#62)]  
  
## <a name="example"></a>示例  

 下面的示例在 `Select` `Where` Visual Basic (`select` 和 c # ) 中使用和， `where` 以返回伦敦的客户的联系人姓名的 *筛选序列* 。  
  
 [!code-csharp[DLinqQueryExamples#63](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#63)]
 [!code-vb[DLinqQueryExamples#63](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#63)]  
  
## <a name="example"></a>示例  

 下面的示例使用 `Select` c # 中 Visual Basic (子句中的子句 `select` ) 和 *匿名类型* 返回有关客户的数据的 *形状子集* 。  
  
 [!code-csharp[DLinqQueryExamples#64](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#64)]
 [!code-vb[DLinqQueryExamples#64](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#64)]  
  
## <a name="example"></a>示例  

 下面的示例使用嵌套查询返回以下结果：  
  
- 由所有订单及其对应的 `OrderID` 组成的序列。  
  
- 由订单中具有折扣的项组成的子序列。  
  
- 不含运费时节省的资金数额。  
  
 [!code-csharp[DLinqQueryExamples#65](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#65)]
 [!code-vb[DLinqQueryExamples#65](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#65)]  
  
## <a name="see-also"></a>请参阅

- [查询示例](query-examples.md)
