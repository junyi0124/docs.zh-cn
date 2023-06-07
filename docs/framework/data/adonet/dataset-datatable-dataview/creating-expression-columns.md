---
title: 创建表达式列
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0af3bd64-92a2-4b47-ae62-f5df35f131a6
ms.openlocfilehash: ad14e4d3d6a1107f994d9536485257f9dc1851f5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166841"
---
# <a name="creating-expression-columns"></a>创建表达式列

您可以为列定义表达式，让它能够包含根据同一行中其他列值或根据表中多行的列值计算而得的值。 要定义要计算的表达式，可使用目标列的 <xref:System.Data.DataColumn.Expression%2A> 属性，并使用 <xref:System.Data.DataColumn.ColumnName%2A> 属性在表达式中引用其他列。 用于表达式列的 <xref:System.Data.DataColumn.DataType%2A> 必须适合于表达式将返回的值。  
  
 下表列出了表中表达式列的几种可能用法。  
  
|表达式类型|示例|  
|---------------------|-------------|  
|比较|"Total >= 500"|  
|计算|"UnitPrice * Quantity"|  
|聚合|Sum(Price)|  
  
 可以在现有**DataColumn**对象上设置**Expression**属性，也可以将属性包含为传递给构造函数的第三个自变量 <xref:System.Data.DataColumn> ，如下面的示例中所示。  
  
```vb  
workTable.Columns.Add("Total",Type.GetType("System.Double"))  
workTable.Columns.Add("SalesTax", Type.GetType("System.Double"), _  
  "Total * 0.086")  
```  
  
```csharp  
workTable.Columns.Add("Total", typeof(Double));  
workTable.Columns.Add("SalesTax", typeof(Double), "Total * 0.086");  
```  
  
 表达式可以引用其他表达式列，但循环引用（其中两个表达式相互引用）将产生异常。 有关编写表达式的规则，请参阅 <xref:System.Data.DataColumn.Expression%2A> **DataColumn** 类的属性。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataColumn>
- <xref:System.Data.DataSet>
- <xref:System.Data.DataTable>
- [数据表架构定义](datatable-schema-definition.md)
- [DataTables](datatables.md)
- [ADO.NET 概述](../ado-net-overview.md)
