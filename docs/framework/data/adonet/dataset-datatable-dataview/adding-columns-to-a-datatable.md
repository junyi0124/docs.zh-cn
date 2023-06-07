---
title: 向数据表添加列
description: DataTable 包含由表的 Columns 属性引用的 DataColumn 对象。 使用此示例代码将列添加到 ADO.NET 中的表。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e85c4a0e-4f3f-458c-b58b-0ddbc06bf974
ms.openlocfilehash: 7f220f5d8cbc4b1c12dec018a4497c6bc492f3c1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164878"
---
# <a name="adding-columns-to-a-datatable"></a>向数据表添加列

<xref:System.Data.DataTable>包含 <xref:System.Data.DataColumn> 由表的**Columns**属性引用的对象的集合。 这个列的集合与任何约束一起定义表的架构（即结构）。  
  
 您可以使用**DataColumn**构造函数在表中创建**DataColumn**对象，或调用表的**Columns**属性的**Add**方法（即） <xref:System.Data.DataColumnCollection> 。 **Add**方法接受可选的**ColumnName**、 **DataType**和**Expression**参数，并创建新的**DataColumn**作为集合的成员。 它还接受现有 **DataColumn** 对象并将其添加到集合中，并返回对添加的 **datacolumn** 的引用（如果请求）。 因为 **DataTable** 对象不特定于任何数据源，所以在指定 **DataColumn**的数据类型时，将使用 .NET Framework 类型。  
  
 下面的示例将四列添加到 **DataTable**。  
  
```vb  
Dim workTable As DataTable = New DataTable("Customers")  
  
Dim workCol As DataColumn = workTable.Columns.Add( _  
    "CustID", Type.GetType("System.Int32"))  
workCol.AllowDBNull = false  
workCol.Unique = true  
  
workTable.Columns.Add("CustLName", Type.GetType("System.String"))  
workTable.Columns.Add("CustFName", Type.GetType("System.String"))  
workTable.Columns.Add("Purchases", Type.GetType("System.Double"))  
```  
  
```csharp  
DataTable workTable = new DataTable("Customers");  
  
DataColumn workCol = workTable.Columns.Add("CustID", typeof(Int32));  
workCol.AllowDBNull = false;  
workCol.Unique = true;  
  
workTable.Columns.Add("CustLName", typeof(String));  
workTable.Columns.Add("CustFName", typeof(String));  
workTable.Columns.Add("Purchases", typeof(Double));  
```  
  
 在此示例中，请注意， **CustID** 列的属性设置为不允许 **DBNull** 值并将值限制为唯一。 但是，如果将 **CustID** 列定义为表的主键列，则 **AllowDBNull** 属性将自动设置为 **false** ，并且 **Unique** 属性将自动设置为 **true**。 有关详细信息，请参阅 [定义主键](defining-primary-keys.md)。  
  
> [!CAUTION]
> 如果没有为列提供列名称，则在将列添加到**DataColumnCollection**时，将为该列指定递增的默认名称 *（* 以 "Column1" 开头）。 建议在提供列名时避免使用 "Column*N*" 命名约定，因为所提供的名称可能与 **DataColumnCollection**中现有的默认列名冲突。 如果提供的名称已经存在，将引发异常。  
  
 如果将 <xref:System.Xml.Linq.XElement> 用作 <xref:System.Data.DataColumn.DataType%2A> 中的 <xref:System.Data.DataColumn> 的 <xref:System.Data.DataTable>，则在读入数据时，XML 序列化将不起作用。 例如，如果通过使用 <xref:System.Xml.XmlDocument> 方法来写出 `DataTable.WriteXml`，则在序列化为 XML 时，<xref:System.Xml.Linq.XElement> 中会出现一个额外的父节点。 若要解决此问题，请使用 <xref:System.Data.SqlTypes.SqlXml> 类型来代替 <xref:System.Xml.Linq.XElement>。 `ReadXml` 和 `WriteXml` 对 <xref:System.Data.SqlTypes.SqlXml> 均能正确使用。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataColumn>
- <xref:System.Data.DataColumnCollection>
- <xref:System.Data.DataTable>
- [数据表架构定义](datatable-schema-definition.md)
- [DataTables](datatables.md)
- [ADO.NET 概述](../ado-net-overview.md)
