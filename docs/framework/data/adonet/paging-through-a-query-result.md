---
title: 通过查询结果分页
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fa360c46-e5f8-411e-a711-46997771133d
ms.openlocfilehash: 065b509d8385ee37b2a86587f520b5fd3207ceff
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186947"
---
# <a name="paging-through-a-query-result"></a>通过查询结果分页

查询结果分页是以较小数据子集（即页）的形式返回查询结果的过程。 它通常用于以易于管理的小块形式向用户显示结果。  
  
 **DataAdapter**通过**Fill**方法的重载提供仅返回一页数据的工具。 但是，这可能不是通过大型查询结果进行分页的最佳选择，尽管 **DataAdapter** 只填充目标 <xref:System.Data.DataTable> 或只使用 <xref:System.Data.DataSet> 请求的记录，但仍会使用返回整个查询的资源。 若要在从数据源中返回一页数据时不使用返回整个查询的资源，请为查询指定附加条件，使返回的行数减少到只返回所需的行。  
  
 若要使用 **Fill** 方法返回数据页，请为数据页中的第一条记录指定 **startRecord** 参数，并为数据页中的记录数指定 **maxRecords** 参数。  
  
 下面的代码示例演示如何使用 **Fill** 方法返回查询结果的第一页，其中页的大小为5条记录。  
  
```vb  
Dim currentIndex As Integer = 0  
Dim pageSize As Integer = 5  
  
Dim orderSQL As String = "SELECT * FROM dbo.Orders ORDER BY OrderID"  
' Assumes that connection is a valid SqlConnection object.  
Dim adapter As SqlDataAdapter = _  
  New SqlDataAdapter(orderSQL, connection)  
  
Dim dataSet As DataSet = New DataSet()  
adapter.Fill(dataSet, currentIndex, pageSize, "Orders")  
```  
  
```csharp  
int currentIndex = 0;  
int pageSize = 5;  
  
string orderSQL = "SELECT * FROM Orders ORDER BY OrderID";  
// Assumes that connection is a valid SqlConnection object.  
SqlDataAdapter adapter = new SqlDataAdapter(orderSQL, connection);  
  
DataSet dataSet = new DataSet();  
adapter.Fill(dataSet, currentIndex, pageSize, "Orders");  
```  
  
 在上面的示例中， **数据集** 只填充了五条记录，但返回了整个 **Orders** 表。 若要用相同的五个记录填充 **数据集** ，但只返回5条记录，请在 SQL 语句中使用 TOP 和 WHERE 子句，如下面的代码示例所示。  
  
```vb  
Dim pageSize As Integer = 5  
  
Dim orderSQL As String = "SELECT TOP " & pageSize & _  
  " * FROM Orders ORDER BY OrderID"  
Dim adapter As SqlDataAdapter = _  
  New SqlDataAdapter(orderSQL, connection)  
  
Dim dataSet As DataSet = New DataSet()  
adapter.Fill(dataSet, "Orders")
```  
  
```csharp  
int pageSize = 5;  
  
string orderSQL = "SELECT TOP " + pageSize +
  " * FROM Orders ORDER BY OrderID";  
SqlDataAdapter adapter = new SqlDataAdapter(orderSQL, connection);  
  
DataSet dataSet = new DataSet();  
adapter.Fill(dataSet, "Orders");  
```  
  
 请注意，当以这种方式进行查询结果分页时，必须保留用于对行进行排序的唯一标识符，以便将唯一 ID 传递给用于返回下一页记录的命令，如以下代码示例所示。  
  
```vb  
Dim lastRecord As String = _  
  dataSet.Tables("Orders").Rows(pageSize - 1)("OrderID").ToString()  
```  
  
```csharp  
string lastRecord =
  dataSet.Tables["Orders"].Rows[pageSize - 1]["OrderID"].ToString();  
```  
  
 若要使用 **Fill** 方法的重载返回 **startRecord** 和 **maxRecords** 参数的下一页记录，请将当前记录索引按页面大小递增，并填充表。 请记住，即使只有一页记录添加到 **数据集**，数据库服务器仍返回整个查询结果。 在以下代码示例中，先清除表行，然后再用下一页数据填充这些表行。 您可能需要在本地缓存中保留一定数量的返回行，以减少与数据库服务器的往返次数。  
  
```vb  
currentIndex = currentIndex + pageSize  
  
dataSet.Tables("Orders").Rows.Clear()  
  
adapter.Fill(dataSet, currentIndex, pageSize, "Orders")  
```  
  
```csharp  
currentIndex += pageSize;  
  
dataSet.Tables["Orders"].Rows.Clear();  
  
adapter.Fill(dataSet, currentIndex, pageSize, "Orders");  
```  
  
 若要返回下一页记录而不让数据库服务器返回整个查询，请指定对 SQL SELECT 语句的限制条件。 由于上例保留了返回的最后一个记录，因此可以在 WHERE 子句中使用它来指定查询的起点，如以下代码示例所示。  
  
```vb  
orderSQL = "SELECT TOP " & pageSize & _  
  " * FROM Orders WHERE OrderID > " & lastRecord & " ORDER BY OrderID"  
adapter.SelectCommand.CommandText = orderSQL  
  
dataSet.Tables("Orders").Rows.Clear()  
  
adapter.Fill(dataSet, "Orders")  
```  
  
```csharp  
orderSQL = "SELECT TOP " + pageSize +
  " * FROM Orders WHERE OrderID > " + lastRecord + " ORDER BY OrderID";  
adapter.SelectCommand.CommandText = orderSQL;  
  
dataSet.Tables["Orders"].Rows.Clear();  
  
adapter.Fill(dataSet, "Orders");  
```  
  
## <a name="see-also"></a>请参阅

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [ADO.NET 概述](ado-net-overview.md)
