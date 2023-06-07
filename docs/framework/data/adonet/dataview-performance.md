---
title: DataView 性能
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 90820e49-9d46-41f6-9a3d-6c0741bbd8eb
ms.openlocfilehash: b2483becce31ab75d8b55b7a642c4ada83da59f6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183346"
---
# <a name="dataview-performance"></a>DataView 性能

本主题讨论使用 <xref:System.Data.DataView.Find%2A> 类的 <xref:System.Data.DataView.FindRows%2A> 和 <xref:System.Data.DataView> 方法并在 Web 应用程序中缓存 <xref:System.Data.DataView> 时所具有的性能优势。  
  
## <a name="find-and-findrows"></a>Find 和 FindRows  

 <xref:System.Data.DataView> 构造一个索引。 索引包含由表或视图中的一列或多列生成的键。 这些键存储在结构中，这种结构可使 <xref:System.Data.DataView> 能够快速有效地查找行或与键值关联的行。 使用索引的操作（如筛选和排序）会显著提高性能。 创建 <xref:System.Data.DataView> 及修改任何排序或筛选信息时，均会生成 <xref:System.Data.DataView> 的索引。 创建 <xref:System.Data.DataView> 然后设置排序或筛选信息会使索引生成至少两次：一次是在创建 <xref:System.Data.DataView> 时，另一次是在修改任何排序或筛选属性时。 有关对进行筛选和排序的详细信息 <xref:System.Data.DataView> ，请参阅通过 [Dataview 进行筛选](filtering-with-dataview-linq-to-dataset.md) 和 [通过 dataview 进行排序](sorting-with-dataview-linq-to-dataset.md)。  
  
 如果要返回特定数据查询的结果而不是提供数据子集的动态视图，则可以使用 <xref:System.Data.DataView.Find%2A> 的 <xref:System.Data.DataView.FindRows%2A> 或 <xref:System.Data.DataView> 方法，而不设置 <xref:System.Data.DataView.RowFilter%2A> 属性。 <xref:System.Data.DataView.RowFilter%2A> 属性最适合用于用绑定控件显示筛选结果的数据绑定应用程序。 设置 <xref:System.Data.DataView.RowFilter%2A> 属性会重新生成数据的索引，从而增加应用程序的系统开销并降低性能。 <xref:System.Data.DataView.Find%2A> 和 <xref:System.Data.DataView.FindRows%2A> 方法使用当前索引，而不要求重新生成索引。 如果只想调用 <xref:System.Data.DataView.Find%2A> 或 <xref:System.Data.DataView.FindRows%2A> 一次，则应使用现有的 <xref:System.Data.DataView>。 如果想要调用 <xref:System.Data.DataView.Find%2A> 或 <xref:System.Data.DataView.FindRows%2A> 多次，则应该创建一个新的 <xref:System.Data.DataView> 以便对想要搜索的列重新生成索引，然后调用 <xref:System.Data.DataView.Find%2A> 或 <xref:System.Data.DataView.FindRows%2A> 方法。 有关和方法的详细 <xref:System.Data.DataView.Find%2A> 信息 <xref:System.Data.DataView.FindRows%2A> ，请参阅 [查找行](./dataset-datatable-dataview/finding-rows.md)。  
  
 下面的示例使用 <xref:System.Data.DataView.Find%2A> 方法来查找姓氏为“Zhu”的联系人。  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryOrderByFind](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromqueryorderbyfind)]
 [!code-vb[DP DataView Samples#LDVFromQueryOrderByFind](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromqueryorderbyfind)]  
  
 下面的示例使用 <xref:System.Data.DataView.FindRows%2A> 方法来查找所有红颜色的产品。  
  
 [!code-csharp[DP DataView Samples#LDVFromQueryFindRows](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvfromqueryfindrows)]
 [!code-vb[DP DataView Samples#LDVFromQueryFindRows](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvfromqueryfindrows)]  
  
## <a name="aspnet"></a>ASP.NET  

 ASP.NET 具有一种缓存机制，允许您在内存中存储需要创建大量服务器资源的对象。 缓存这些类型的资源可以显著提高应用程序的性能。 缓存由 <xref:System.Web.Caching.Cache> 类实现，缓存实例专用于每个应用程序。 由于创建新的 <xref:System.Data.DataView> 对象需要大量资源，因此你可能希望在 Web 应用程序中使用此缓存功能，使得每次刷新网页时，不必重新生成 <xref:System.Data.DataView>。  
  
 在下面的示例中，对 <xref:System.Data.DataView> 进行缓存以便在刷新该页时不必对数据重新排序。  
  
```vb  
If (Cache("ordersView") = Nothing) Then  
  
Dim dataSet As New DataSet()  
  
   FillDataSet(dataSet)  
  
   Dim orders As DataTable = dataSet.Tables("SalesOrderHeader")  
  
   Dim query = _  
                    From order In orders.AsEnumerable() _  
                    Where order.Field(Of Boolean)("OnlineOrderFlag") = True _  
                    Order By order.Field(Of Decimal)("TotalDue") _  
                    Select order  
  
   Dim view As DataView = query.AsDataView()  
  
   Cache.Insert("ordersView", view)  
  
End If  
  
Dim ordersView = CType(Cache("ordersView"), DataView)  
  
GridView1.DataSource = ordersView  
GridView1.DataBind()  
```  
  
```csharp  
if (Cache["ordersView"] == null)  
{  
   // Fill the DataSet.
   DataSet dataSet = FillDataSet();  
  
   DataTable orders = dataSet.Tables["SalesOrderHeader"];  
  
   EnumerableRowCollection<DataRow> query =  
                        from order in orders.AsEnumerable()  
                        where order.Field<bool>("OnlineOrderFlag") == true  
                        orderby order.Field<decimal>("TotalDue")  
                        select order;  
  
   DataView view = query.AsDataView();  
   Cache.Insert("ordersView", view);  
}  
  
DataView ordersView = (DataView)Cache["ordersView"];  
  
GridView1.DataSource = ordersView;  
GridView1.DataBind();  
```  
  
## <a name="see-also"></a>请参阅

- [数据绑定和 LINQ to DataSet](data-binding-and-linq-to-dataset.md)
