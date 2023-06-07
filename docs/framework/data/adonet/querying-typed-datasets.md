---
title: 查询类型化数据集
ms.date: 08/15/2018
dev_langs:
- csharp
- vb
ms.assetid: ad712fa1-2baf-462a-b163-574cce6d376a
ms.openlocfilehash: 55714c4dae73cd17a849cc35681797dfa4266e3b
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70782949"
---
# <a name="query-typed-datasets"></a>查询类型化数据集

如果在应用程序设计<xref:System.Data.DataSet>时已知的架构，则建议在使用 LINQ to DataSet 时使用类型化<xref:System.Data.DataSet> 。 类型化 <xref:System.Data.DataSet> 是从 <xref:System.Data.DataSet> 中派生的类。 因此，它继承 <xref:System.Data.DataSet> 的所有方法、事件和属性。 此外，类型化 <xref:System.Data.DataSet> 还提供强类型方法、事件和属性。 这意味着可以按名称而不使用基于集合的方法来访问表和列。 这可使查询更简单、更具可读性。 有关详细信息，请参阅[类型化数据集](./dataset-datatable-dataview/typed-datasets.md)。

LINQ to DataSet 还支持对类型化<xref:System.Data.DataSet>进行查询。 对于类型化 <xref:System.Data.DataSet>，您不必使用泛型 <xref:System.Data.DataRowExtensions.Field%2A> 方法或 <xref:System.Data.DataRowExtensions.SetField%2A> 方法即可访问列数据。 由于 <xref:System.Data.DataSet> 中包括类型信息，因此属性名称在编译时可用。 LINQ to DataSet 提供作为正确类型的列值的访问，以便在编译代码而不是在运行时捕获类型不匹配错误。

必须先使用 Visual Studio 中的<xref:System.Data.DataSet>**数据集设计器**生成类，然后才能开始查询类型化的。 有关详细信息，请参阅[创建和配置数据集](/visualstudio/data-tools/create-and-configure-datasets-in-visual-studio)。

## <a name="example"></a>示例

下面的示例演示对类型化 <xref:System.Data.DataSet> 进行查询：

```csharp
var query = from o in orders
            where o.OnlineOrderFlag == true
            select new { o.SalesOrderID,
                         o.OrderDate,
                         o.SalesOrderNumber };

foreach(var order in query)
{
    Console.WriteLine("{0}\t{1:d}\t{2}",
      order.SalesOrderID,
      order.OrderDate,
      order.SalesOrderNumber);
}
```

```vb
Dim orders = ds.Tables("SalesOrderHeader")

Dim query = _
       From o In orders _
       Where o.OnlineOrderFlag = True _
       Select New {SalesOrderID := o.SalesOrderID, _
                   OrderDate := o.OrderDate, _
                   SalesOrderNumber := o.SalesOrderNumber}

For Each Dim onlineOrder In query
 Console.WriteLine("{0}\t{1:d}\t{2}", _
 onlineOrder.SalesOrderID, _
 onlineOrder.OrderDate, _
 onlineOrder.SalesOrderNumber)
Next
```

## <a name="see-also"></a>请参阅

- [查询数据集](querying-datasets-linq-to-dataset.md)
- [跨表查询](cross-table-queries-linq-to-dataset.md)
- [单表查询](single-table-queries-linq-to-dataset.md)
