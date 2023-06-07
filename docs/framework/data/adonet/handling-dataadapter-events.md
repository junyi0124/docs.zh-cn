---
title: 处理 DataAdapter 事件
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 11515b25-ee49-4b1d-9294-a142147c1ec5
ms.openlocfilehash: a2c2dc71cc9e5c445fd05534dad5ad47fd66f436
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194721"
---
# <a name="handling-dataadapter-events"></a>处理 DataAdapter 事件

ADO.NET <xref:System.Data.Common.DataAdapter> 公开三个可用于响应数据源中数据更改的事件。 下表演示了 `DataAdapter` 事件。  
  
|事件|描述|  
|-----------|-----------------|  
|`RowUpdating`|将要开始对某行执行 UPDATE、INSERT 或 DELETE 操作（通过调用 `Update` 方法之一）。|  
|`RowUpdated`|对某行的 UPDATE、INSERT 或 DELETE 操作（通过调用 `Update` 方法之一）已完成。|  
|`FillError`|执行 `Fill` 操作期间出错。|  
  
## <a name="rowupdating-and-rowupdated"></a>RowUpdating 和 RowUpdated  

 在数据源中处理对  `RowUpdating` 中某行的任何更新之前，将引发 <xref:System.Data.DataSet>。 在数据源中处理对 `RowUpdated` 中某行的任何更新之后，将引发 `DataSet`。 因此，可以使用 `RowUpdating` 执行下列操作：在更新行为发生之前对其进行修改，在更新将发生时提供附加处理，保留对已更新行的引用，取消当前更新并将其安排在以后进行批处理，等等。 `RowUpdated` 对于响应更新期间发生的错误和异常是非常有用的。 您可以向 `DataSet` 以及重试逻辑等添加错误信息。  
  
 传递给 <xref:System.Data.Common.RowUpdatingEventArgs> 和 <xref:System.Data.Common.RowUpdatedEventArgs> 事件的 `RowUpdating` 和 `RowUpdated` 自变量包括：`Command` 属性，它引用用来执行更新的 `Command` 对象；`Row` 属性，它引用包含更新信息的 `DataRow` 对象；`StatementType` 属性，它指示所执行的更新类型；`TableMapping`（如果适用）；以及操作的 `Status`。  
  
 可以使用 `Status` 属性来确定在执行该操作期间是否发生了错误；如果需要，还可以使用该属性来控制对当前行和结果行所执行的操作。 当该事件发生时，`Status` 属性将为 `Continue` 或 `ErrorsOccurred`。 下表演示为了控制更新过程中的后继操作，可以将 `Status` 属性设置为的值。  
  
|状态|描述|  
|------------|-----------------|  
|`Continue`|继续执行更新操作。|  
|`ErrorsOccurred`|中止更新操作并引发异常。|  
|`SkipCurrentRow`|忽略当前行并继续执行更新操作。|  
|`SkipAllRemainingRows`|中止更新操作但不引发异常。|  
  
 如果将 `Status` 属性设置为 `ErrorsOccurred`，则会引发异常。 您可以通过将 `Errors` 属性设置为所需异常来控制所引发的异常。 如果使用 `Status` 的其他值之一，则可防止引发异常。  
  
 也可以使用 `ContinueUpdateOnError` 属性为更新的行处理错误。 如果 `DataAdapter.ContinueUpdateOnError` 为 `true`，那么当行的更新导致引发异常时，该异常的文本被放入特定行的 `RowError` 信息中，并且处理将会继续而不会引发异常。 这使您能够在 `Update` 完成时对错误作出响应；与此相反的是 `RowUpdated` 事件，它使您能够在遇到错误时响应错误。  
  
 以下代码示例显示如何添加和移除事件处理程序。 `RowUpdating` 事件处理程序编写带有时间戳的所有已删除记录的日志。 `RowUpdated`事件处理程序将错误信息添加到 `RowError` 中的行的属性中 `DataSet` ，取消隐藏异常，并继续处理 (镜像) 的行为 `ContinueUpdateOnError`  =  `true` 。  
  
```vb  
' Assumes that connection is a valid SqlConnection object.  
Dim custAdapter As SqlDataAdapter = New SqlDataAdapter( _  
  "SELECT CustomerID, CompanyName FROM Customers", connection)  
  
' Add handlers.  
AddHandler custAdapter.RowUpdating, New SqlRowUpdatingEventHandler( _  
  AddressOf OnRowUpdating)  
AddHandler custAdapter.RowUpdated, New SqlRowUpdatedEventHandler(  
  AddressOf OnRowUpdated)  
  
' Set DataAdapter command properties, fill DataSet, and modify DataSet.  
  
custAdapter.Update(custDS, "Customers")  
  
' Remove handlers.  
RemoveHandler custAdapter.RowUpdating, _  
  New SqlRowUpdatingEventHandler(AddressOf OnRowUpdating)  
RemoveHandler custAdapter.RowUpdated, _  
  New SqlRowUpdatedEventHandler(AddressOf OnRowUpdated)  
  
Private Shared Sub OnRowUpdating(sender As Object, _  
  args As SqlRowUpdatingEventArgs)  
  If args.StatementType = StatementType.Delete Then  
    Dim tw As System.IO.TextWriter = _  
  System.IO.File.AppendText("Deletes.log")  
    tw.WriteLine( _  
      "{0}: Customer {1} Deleted.", DateTime.Now, args.Row(_  
      "CustomerID", DataRowVersion.Original))  
    tw.Close()  
  End If  
End Sub  
  
Private Shared Sub OnRowUpdated( _  
  sender As Object, args As SqlRowUpdatedEventArgs)  
  If args.Status = UpdateStatus.ErrorsOccurred  
    args.Status = UpdateStatus.SkipCurrentRow  
    args.Row.RowError = args.Errors.Message  
  End If  
End Sub  
```  
  
```csharp  
// Assumes that connection is a valid SqlConnection object.  
SqlDataAdapter custAdapter = new SqlDataAdapter(  
  "SELECT CustomerID, CompanyName FROM Customers", connection);  
  
// Add handlers.  
custAdapter.RowUpdating += new SqlRowUpdatingEventHandler(OnRowUpdating);  
custAdapter.RowUpdated += new SqlRowUpdatedEventHandler(OnRowUpdated);  
  
// Set DataAdapter command properties, fill DataSet, modify DataSet.  
  
custAdapter.Update(custDS, "Customers");  
  
// Remove handlers.  
custAdapter.RowUpdating -= new SqlRowUpdatingEventHandler(OnRowUpdating);  
custAdapter.RowUpdated -= new SqlRowUpdatedEventHandler(OnRowUpdated);  
  
protected static void OnRowUpdating(  
  object sender, SqlRowUpdatingEventArgs args)  
{  
  if (args.StatementType == StatementType.Delete)  
  {  
    System.IO.TextWriter tw = System.IO.File.AppendText("Deletes.log");  
    tw.WriteLine(  
      "{0}: Customer {1} Deleted.", DateTime.Now,
       args.Row["CustomerID", DataRowVersion.Original]);  
    tw.Close();  
  }  
}  
  
protected static void OnRowUpdated(  
  object sender, SqlRowUpdatedEventArgs args)  
{  
  if (args.Status == UpdateStatus.ErrorsOccurred)  
  {  
    args.Row.RowError = args.Errors.Message;  
    args.Status = UpdateStatus.SkipCurrentRow;  
  }  
}  
```  
  
## <a name="fillerror"></a>FillError  

 如果在执行 `DataAdapter` 操作期间出错，`FillError` 将发出 `Fill` 事件。 当所添加行中的数据必须损失一些精度才能转换成 .NET Framework 类型时，通常会发生这种类型的错误。  
  
 如果在执行 `Fill` 操作期间出错，则当前行将不会被添加到 `DataTable`。 通过 `FillError` 事件可更正该错误并添加当前行，或者忽略已排除的行并继续执行 `Fill` 操作。  
  
 传递给 `FillErrorEventArgs` 事件的 `FillError` 包含几项可用于响应和更正错误的属性。 下表演示 `FillErrorEventArgs` 对象的属性。  
  
|属性|描述|  
|--------------|-----------------|  
|`Errors`|已发生的 `Exception`。|  
|`DataTable`|出错时所填充的 `DataTable` 对象。|  
|`Values`|一个对象数组，它包含出错时所添加的行的值。 `Values` 数组的序号引用与所添加的行的列的序号引用相对应。 例如，`Values[0]` 是作为当前行的第一列添加的值。|  
|`Continue`|用于选择是否引发异常。 如果将 `Continue` 属性设置为 `false`，则会暂停当前 `Fill` 操作并将会引发异常。 如果将 `Continue` 设置为 `true`，那么即使出错，仍将继续执行 `Fill` 操作。|  
  
 下面的代码示例为 `FillError` 的 `DataAdapter` 事件添加一个事件处理程序。 在 `FillError` 事件代码中，该示例可确定是否可能出现精度缺失，并可用于响应该异常。  
  
```vb  
AddHandler adapter.FillError, New FillErrorEventHandler( _  
  AddressOf FillError)  
  
Dim dataSet As DataSet = New DataSet  
adapter.Fill(dataSet, "ThisTable")  
  
Private Shared Sub FillError(sender As Object, _  
  args As FillErrorEventArgs)  
  If args.Errors.GetType() Is Type.GetType("System.OverflowException") Then  
    ' Code to handle precision loss.  
    ' Add a row to table using the values from the first two columns.  
    DataRow myRow = args.DataTable.Rows.Add(New Object() _  
      {args.Values(0), args.Values(1), DBNull.Value})  
    ' Set the RowError containing the value for the third column.  
    myRow.RowError = _  
      "OverflowException encountered. Value from data source: " & _  
      args.Values(2)  
    args.Continue = True  
  End If  
End Sub  
```  
  
```csharp  
adapter.FillError += new FillErrorEventHandler(FillError);  
  
DataSet dataSet = new DataSet();  
adapter.Fill(dataSet, "ThisTable");  
  
protected static void FillError(object sender, FillErrorEventArgs args)  
{  
  if (args.Errors.GetType() == typeof(System.OverflowException))  
  {  
    // Code to handle precision loss.  
    //Add a row to table using the values from the first two columns.  
    DataRow myRow = args.DataTable.Rows.Add(new object[]  
       {args.Values[0], args.Values[1], DBNull.Value});  
    //Set the RowError containing the value for the third column.  
    myRow.RowError =
       "OverflowException Encountered. Value from data source: " +  
       args.Values[2];  
    args.Continue = true;  
  }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [处理数据集事件](./dataset-datatable-dataview/handling-dataset-events.md)
- [处理数据表事件](./dataset-datatable-dataview/handling-datatable-events.md)
- [事件](../../../standard/events/index.md)
- [ADO.NET 概述](ado-net-overview.md)
