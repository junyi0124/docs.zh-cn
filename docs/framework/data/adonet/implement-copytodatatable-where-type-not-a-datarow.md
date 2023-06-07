---
title: 如何：在泛型 T 不是 DataRow 的位置实现 CopyToDataTable<T>
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b27b52cf-6172-485f-a75c-70ff9c5a2bd4
ms.openlocfilehash: 776ff6062282d12b622ec89cfa9990513da64a07
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194591"
---
# <a name="how-to-implement-copytodatatablet-where-the-generic-type-t-is-not-a-datarow"></a>如何：在泛型 T 不是 DataRow 的位置实现 CopyToDataTable\<T>

<xref:System.Data.DataTable> 对象通常用于数据绑定。 <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> 方法接收查询结果并将数据复制到 <xref:System.Data.DataTable> 中，后者随后会使用该数据进行数据绑定。 但是，只在 <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> 源上执行 <xref:System.Collections.Generic.IEnumerable%601> 方法，其中泛型参数 `T` 的类型为 <xref:System.Data.DataRow>。 虽然这十分有用，但是它并不允许从标量类型的序列、投影匿名类型的查询或执行表联接的查询创建表。  
  
 此主题描述如何实现两个自定义的 `CopyToDataTable<T>` 扩展方法，其接受类型不是 `T` 的泛型参数 <xref:System.Data.DataRow>。 从 <xref:System.Data.DataTable> 源创建 <xref:System.Collections.Generic.IEnumerable%601> 的逻辑包含在 `ObjectShredder<T>` 类中，该类稍后被包装在两个重载的 `CopyToDataTable<T>` 扩展方法中。 `Shred` 类的 `ObjectShredder<T>` 方法返回填充的 <xref:System.Data.DataTable> 并接受三个输入参数：一个 <xref:System.Collections.Generic.IEnumerable%601> 源、一个 <xref:System.Data.DataTable> 和一个 <xref:System.Data.LoadOption> 枚举。 返回的 <xref:System.Data.DataTable> 的初始架构是以类型为 `T` 的架构为基础的。 如果现有表作为输入提供，则该架构必须与类型为 `T` 的架构一致。 每个公共属性和类型为 `T` 的字段将转换为返回的表中的 <xref:System.Data.DataColumn>。 如果源序列包含从 `T` 派生的类型，则为任何其他公共属性或字段扩展返回的表的架构。  
  
 有关使用自定义 `CopyToDataTable<T>` 方法的示例，请参阅[从查询创建数据表](creating-a-datatable-from-a-query-linq-to-dataset.md)。  
  
### <a name="to-implement-the-custom-copytodatatablet-methods-in-your-application"></a>\<T>在应用程序中实现自定义 CopyToDataTable 方法  
  
1. 实现 `ObjectShredder<T>` 类，以从下面的 <xref:System.Data.DataTable> 源创建 <xref:System.Collections.Generic.IEnumerable%601>：  
  
     [!code-csharp[DP Custom CopyToDataTable Examples#ObjectShredderClass](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#objectshredderclass)]
     [!code-vb[DP Custom CopyToDataTable Examples#ObjectShredderClass](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#objectshredderclass)]  

    前面的示例假定的属性和字段 `DataColumn` 不能为 null 值类型。 若要处理具有可为 null 的值类型的属性和字段，请使用以下代码：

    ```csharp
    // Nullable-aware code for properties.
    DataColumn dc = table.Columns.Contains(p.Name) ? table.Columns[p.Name] : table.Columns.Add(p.Name, Nullable.GetUnderlyingType(p.PropertyType) ?? p.PropertyType);

    // Nullable-aware code for fields.
    DataColumn dc = table.Columns.Contains(f.Name) ? table.Columns[f.Name] : table.Columns.Add(f.Name, Nullable.GetUnderlyingType(f.FieldType) ?? f.FieldType);
    ```

2. 在下面类中实现自定义 `CopyToDataTable<T>` 扩展方法：  
  
     [!code-csharp[DP Custom CopyToDataTable Examples#CustomCopyToDataTableMethods](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#customcopytodatatablemethods)]
     [!code-vb[DP Custom CopyToDataTable Examples#CustomCopyToDataTableMethods](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#customcopytodatatablemethods)]  
  
3. 在应用程序中添加 `ObjectShredder<T>` 类和 `CopyToDataTable<T>` 扩展方法。  
  
```vb  
Module Module1  
    Sub Main()  
        ' Your application code using CopyToDataTable<T>.  
    End Sub  
End Module  
  
Public Module CustomLINQtoDataSetMethods  
…  
End Module  
  
Public Class ObjectShredder(Of T)  
…  
End Class
```
  
```csharp
class Program  
{  
    static void Main(string[] args)  
    {  
        // Your application code using CopyToDataTable<T>.  
    }  
}  
public static class CustomLINQtoDataSetMethods  
{  
…  
}  
public class ObjectShredder<T>  
{  
…  
}  
```
  
## <a name="see-also"></a>请参阅

- [从查询创建数据表](creating-a-datatable-from-a-query-linq-to-dataset.md)
- [编程指南](programming-guide-linq-to-dataset.md)
