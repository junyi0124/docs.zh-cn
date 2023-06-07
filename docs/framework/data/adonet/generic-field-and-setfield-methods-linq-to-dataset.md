---
title: 泛型字段和 SetField 方法 (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1883365f-9d6c-4ccb-9187-df309f47706d
ms.openlocfilehash: 9deda11592389dd799477bdc027d59a0be0036da
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200649"
---
# <a name="generic-field-and-setfield-methods-linq-to-dataset"></a>泛型字段和 SetField 方法 (LINQ to DataSet)

LINQ to DataSet 向类提供用于访问列值的扩展方法 <xref:System.Data.DataRow> ： <xref:System.Data.DataRowExtensions.Field%2A> 方法和 <xref:System.Data.DataRowExtensions.SetField%2A> 方法。 这些方法使开发人员能够更轻松地访问列值，特别是 Null 值。 用于 <xref:System.Data.DataSet> <xref:System.DBNull.Value?displayProperty=nameWithType> 表示 null 值，而 LINQ 使用 <xref:System.Nullable> 和 <xref:System.Nullable%601> 类型。 使用 <xref:System.Data.DataRow> 中预先存在的列访问器需要将返回对象强制转换成相应的类型。 如果 <xref:System.Data.DataRow> 中的特定字段可以为 null，则必须显示检查 Null 值，因为返回 <xref:System.DBNull.Value?displayProperty=nameWithType> 并隐式地将其强制转换为另一种类型会引发 <xref:System.InvalidCastException>。 在下面的示例中，如果不使用 <xref:System.Data.DataRow.IsNull%2A?displayProperty=nameWithType> 方法检查 Null 值，则在索引器返回 <xref:System.DBNull.Value?displayProperty=nameWithType> 并试图将其强制转换为 <xref:System.String> 的情况下会引发异常。  
  
 [!code-csharp[DP LINQ to DataSet Examples#WhereIsNull](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#whereisnull)]
 [!code-vb[DP LINQ to DataSet Examples#WhereIsNull](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#whereisnull)]  
  
 <xref:System.Data.DataRowExtensions.Field%2A> 方法提供对 <xref:System.Data.DataRow> 列值的访问，而 <xref:System.Data.DataRowExtensions.SetField%2A> 设置 <xref:System.Data.DataRow>中的列值。 <xref:System.Data.DataRowExtensions.Field%2A>方法和方法都 <xref:System.Data.DataRowExtensions.SetField%2A> 可以处理可以为 null 的值类型，因此您无需像前面的示例那样显式检查 null 值。 这两种方法也都是泛型方法，因此不必强制转换返回类型。  
  
 下面的示例使用 <xref:System.Data.DataRowExtensions.Field%2A> 方法。  
  
 [!code-csharp[DP LINQ to DataSet Examples#Where3](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#where3)]
 [!code-vb[DP LINQ to DataSet Examples#Where3](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#where3)]  
  
 请注意，`T` 方法和 <xref:System.Data.DataRowExtensions.Field%2A> 方法的泛型参数 <xref:System.Data.DataRowExtensions.SetField%2A> 中指定的数据类型必须与基础值的类型相匹配。 否则，将引发 <xref:System.InvalidCastException> 异常。 指定的列名称也必须与 <xref:System.Data.DataSet> 中的列名称相匹配，否则将引发 <xref:System.ArgumentException>。 在这两种情况下，异常都是在执行查询期间的运行时数据枚举时引发的。  
  
 <xref:System.Data.DataRowExtensions.SetField%2A> 方法本身不执行任何类型转换。 但这并不意味着不会发生类型转换。 <xref:System.Data.DataRowExtensions.SetField%2A>方法公开类的 ADO.NET 行为 <xref:System.Data.DataRow> 。 类型转换可以由 <xref:System.Data.DataRow> 对象执行，转换的值随后将保存到 <xref:System.Data.DataRow> 对象。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataRowExtensions>
