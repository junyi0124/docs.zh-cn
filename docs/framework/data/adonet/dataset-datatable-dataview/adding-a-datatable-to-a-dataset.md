---
title: 将数据表添加到数据集中
description: 请参阅此示例代码，了解如何创建 DataTable 对象并将其添加到 ADO.NET 中的现有数据集。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 556d29a3-8fc9-4e38-b3ee-c188f7e7b155
ms.openlocfilehash: 6a5e3a89870b3959a6ac042b93414694e8a6cc1c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164891"
---
# <a name="adding-a-datatable-to-a-dataset"></a>将数据表添加到数据集中

ADO.NET 使您能够创建 <xref:System.Data.DataTable> 对象并将其添加到现有 <xref:System.Data.DataSet> 中。 可以使用 <xref:System.Data.DataTable> 和 <xref:System.Data.DataTable.PrimaryKey%2A> 属性为 <xref:System.Data.DataColumn.Unique%2A> 设置约束信息。  
  
## <a name="example"></a>示例  

 以下示例构造一个 <xref:System.Data.DataSet>，将一个新的 <xref:System.Data.DataTable> 对象添加到该 <xref:System.Data.DataSet> 中，然后将三个 <xref:System.Data.DataColumn> 对象添加到该表中。 最后，该代码将一个列设置为主键列。  
  
 [!code-csharp[DataWorks Data.DataTableAdd#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks Data.DataTableAdd/CS/source.cs#1)]
 [!code-vb[DataWorks Data.DataTableAdd#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks Data.DataTableAdd/VB/source.vb#1)]  
  
## <a name="case-sensitivity"></a>区分大小写  

 <xref:System.Data.DataSet> 中可以存在两个或两个以上的同名但是大小写不同的表或关系。 在这种情况下，通过名称对表和关系的引用将区分大小写。 例如，如果 <xref:System.Data.DataSet> **数据集**包含表表**Table1** 1 和**table1**表1，则将按 name 作为 Dataset 引用**table1** 。**表 ["Table1"]**， **table1**表1作为**数据集。表 ["table1"]**。 尝试将其中一个表作为 DataSet 引用 **。表 ["TABLE1"]** 将生成异常。  
  
 如果只有一个具有特定名称的表或关系，则区分大小写行为不适用。 例如，如果 <xref:System.Data.DataSet> 只有 **Table1**，则可以使用 dataSet 引用它 **。表 ["Table1"]**。  
  
> [!NOTE]
> <xref:System.Data.DataSet.CaseSensitive%2A> 的 <xref:System.Data.DataSet> 属性不影响此行为。 <xref:System.Data.DataSet.CaseSensitive%2A> 属性应用于 <xref:System.Data.DataSet> 中的数据，并会影响排序、搜索、筛选、执行约束，等等。  
  
## <a name="namespace-support"></a>命名空间支持  

 在 2.0 之前的 ADO.NET 版本中，两个表即使处于不同的命名空间中也不能同名。 ADO.NET 2.0 中取消了此限制。 <xref:System.Data.DataSet> 可以包含具有相同 <xref:System.Data.DataTable.TableName%2A> 属性值但是具有不同 <xref:System.Data.DataTable.Namespace%2A> 属性值的两个表。  
  
## <a name="see-also"></a>请参阅

- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
