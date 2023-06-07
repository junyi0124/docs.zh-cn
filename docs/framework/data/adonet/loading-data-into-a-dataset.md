---
title: 将数据加载到数据集中
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a53e5dc1-9669-49d4-828d-efa633237066
ms.openlocfilehash: c870cabc875aa0152910ce916819fb1ff1c018f7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166705"
---
# <a name="loading-data-into-a-dataset"></a>将数据加载到数据集中

<xref:System.Data.DataSet>必须先填充对象，然后才能使用 LINQ to DataSet 进行查询。 填充 <xref:System.Data.DataSet> 有多种不同的方式。 例如，您可以使用 [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] 来查询数据库并将结果加载到 <xref:System.Data.DataSet> 中。 有关详细信息，请参阅 [LINQ to SQL](./sql/linq/index.md)。  
  
 另一种将数据加载到 <xref:System.Data.DataSet> 中的常见方式是使用 <xref:System.Data.Common.DataAdapter> 类从数据库中检索数据。 下列示例对此进行了阐释。  
  
## <a name="example"></a>示例  

 此示例使用 <xref:System.Data.Common.DataAdapter> 查询 AdventureWorks 数据库中从 2002 年起的销售信息，并将结果加载到 <xref:System.Data.DataSet> 中。 填充后 <xref:System.Data.DataSet> ，可以使用 LINQ to DataSet 来编写查询。 `FillDataSet`本示例中的方法用于[LINQ to DataSet 示例](linq-to-dataset-examples.md)中的示例查询。 有关详细信息，请参阅 [查询数据集](querying-datasets-linq-to-dataset.md)。  
  
 [!code-csharp[DP LINQ to DataSet Examples#FillDataSet](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#filldataset)]
 [!code-vb[DP LINQ to DataSet Examples#FillDataSet](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#filldataset)]  
  
## <a name="see-also"></a>请参阅

- [LINQ to DataSet 概述](linq-to-dataset-overview.md)
- [查询数据集](querying-datasets-linq-to-dataset.md)
- [LINQ to DataSet 示例](linq-to-dataset-examples.md)
