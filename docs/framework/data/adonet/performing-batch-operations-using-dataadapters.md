---
title: 使用 DataAdapter 执行批处理操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e72ed5af-b24f-486c-8429-c8fd2208f844
ms.openlocfilehash: 9dd6abb91b3549e3bc8b4ae84cbb227171512ecb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177418"
---
# <a name="performing-batch-operations-using-dataadapters"></a>使用 DataAdapter 执行批处理操作

通过 ADO.NET 中的批处理支持，<xref:System.Data.Common.DataAdapter> 可以将 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 中的 INSERT、UPDATE 和 DELETE 操作分组发向服务器，而不是每次发送一项操作。 因为减少了与服务器的往返次数，通常可以大大提高性能。 SQL Server .NET 数据提供程序 (<xref:System.Data.SqlClient>) 和 Oracle .NET 数据提供程序 (<xref:System.Data.OracleClient>) 支持批量更新。  
  
 在 ADO.NET 的以前版本中用 <xref:System.Data.DataSet> 中的更改更新数据库时，`Update` 的 `DataAdapter` 方法执行一次会向数据库中更新一行。 当该方法循环访问指定 <xref:System.Data.DataTable> 中的各行时，它会检查每个 <xref:System.Data.DataRow> 以查看其是否已被修改。 如果行已被修改，它会调用相应的 `UpdateCommand`、`InsertCommand` 或 `DeleteCommand`，具体取决于该行的 <xref:System.Data.DataRow.RowState%2A> 属性值。 每行更新都需要通过网络往返访问一次数据库。  
  
 从 ADO.NET 2.0 开始，<xref:System.Data.Common.DbDataAdapter> 公开一个 <xref:System.Data.Common.DbDataAdapter.UpdateBatchSize%2A> 属性。 将 `UpdateBatchSize` 设置为正整数值可使对数据库的更新以指定大小的批处理形式发送。 例如，将 `UpdateBatchSize` 设置为 10 可将 10 个单独的语句编成一组并作为单个批处理进行提交。 将 `UpdateBatchSize` 设置为 0 可使 <xref:System.Data.Common.DataAdapter> 使用服务器能够处理的最大批大小。 将其设置为 1 可禁用批处理更新，因为这时一次只发送一行。  
  
 执行极大的批处理会降低性能。 因此，在实现应用程序前应进行测试以得到最佳的批大小。  
  
## <a name="using-the-updatebatchsize-property"></a>使用 UpdateBatchSize 属性  

 启用批处理更新时，的 <xref:System.Data.IDbCommand.UpdatedRowSource%2A>、`UpdateCommand` 和 `InsertCommand` 的 `DeleteCommand` 属性值应设置为 <xref:System.Data.UpdateRowSource.None> 或 <xref:System.Data.UpdateRowSource.OutputParameters>。 执行批处理更新时，命令的 <xref:System.Data.IDbCommand.UpdatedRowSource%2A> 或 <xref:System.Data.UpdateRowSource.FirstReturnedRecord> 的 <xref:System.Data.UpdateRowSource.Both> 属性值无效。  
  
 下面的过程演示 `UpdateBatchSize` 属性的用法。 此过程采用两个自变量：一个 <xref:System.Data.DataSet> 对象，该对象的列表示**ProductCategory**表中的**ProductCategoryID**和**Name**字段，一个整数表示批大小 (批处理) 中的行数。 代码创建一个新的 <xref:System.Data.SqlClient.SqlDataAdapter> 对象，并设置其 <xref:System.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A>、<xref:System.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 和 <xref:System.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> 属性。 代码假定 <xref:System.Data.DataSet> 对象具有经过修改的行。 它设置 `UpdateBatchSize` 属性并执行更新。  
  
```vb  
Public Sub BatchUpdate( _  
  ByVal dataTable As DataTable, ByVal batchSize As Int32)  
    ' Assumes GetConnectionString() returns a valid connection string.  
    Dim connectionString As String = GetConnectionString()  
  
    ' Connect to the AdventureWorks database.  
    Using connection As New SqlConnection(connectionString)  
        ' Create a SqlDataAdapter.  
        Dim adapter As New SqlDataAdapter()  
  
        'Set the UPDATE command and parameters.  
        adapter.UpdateCommand = New SqlCommand( _  
          "UPDATE Production.ProductCategory SET " _  
          & "Name=@Name WHERE ProductCategoryID=@ProdCatID;", _  
          connection)  
        adapter.UpdateCommand.Parameters.Add("@Name", _  
          SqlDbType.NVarChar, 50, "Name")  
        adapter.UpdateCommand.Parameters.Add("@ProdCatID",  _  
          SqlDbType.Int, 4, " ProductCategoryID ")  
        adapter.UpdateCommand.UpdatedRowSource = _  
          UpdateRowSource.None  
  
        'Set the INSERT command and parameter.  
        adapter.InsertCommand = New SqlCommand( _  
          "INSERT INTO Production.ProductCategory (Name) VALUES (@Name);", _  
  connection)  
        adapter.InsertCommand.Parameters.Add("@Name", _  
          SqlDbType.NVarChar, 50, "Name")  
        adapter.InsertCommand.UpdatedRowSource = _  
          UpdateRowSource.None  
  
        'Set the DELETE command and parameter.  
        adapter.DeleteCommand = New SqlCommand( _  
          "DELETE FROM Production.ProductCategory " _  
          & "WHERE ProductCategoryID=@ProdCatID;", connection)  
        adapter.DeleteCommand.Parameters.Add("@ProdCatID", _  
           SqlDbType.Int, 4, " ProductCategoryID ")  
        adapter.DeleteCommand.UpdatedRowSource = UpdateRowSource.None  
  
        ' Set the batch size.  
        adapter.UpdateBatchSize = batchSize  
  
        ' Execute the update.  
        adapter.Update(dataTable)  
    End Using  
End Sub  
```  
  
```csharp  
public static void BatchUpdate(DataTable dataTable,Int32 batchSize)  
{  
    // Assumes GetConnectionString() returns a valid connection string.  
    string connectionString = GetConnectionString();  
  
    // Connect to the AdventureWorks database.  
    using (SqlConnection connection = new
      SqlConnection(connectionString))  
    {  
  
        // Create a SqlDataAdapter.  
        SqlDataAdapter adapter = new SqlDataAdapter();  
  
        // Set the UPDATE command and parameters.  
        adapter.UpdateCommand = new SqlCommand(  
            "UPDATE Production.ProductCategory SET "  
            + "Name=@Name WHERE ProductCategoryID=@ProdCatID;",
            connection);  
        adapter.UpdateCommand.Parameters.Add("@Name",
           SqlDbType.NVarChar, 50, "Name");  
        adapter.UpdateCommand.Parameters.Add("@ProdCatID",
           SqlDbType.Int, 4, "ProductCategoryID");  
         adapter.UpdateCommand.UpdatedRowSource = UpdateRowSource.None;  
  
        // Set the INSERT command and parameter.  
        adapter.InsertCommand = new SqlCommand(  
            "INSERT INTO Production.ProductCategory (Name) VALUES (@Name);",
            connection);  
        adapter.InsertCommand.Parameters.Add("@Name",
          SqlDbType.NVarChar, 50, "Name");  
        adapter.InsertCommand.UpdatedRowSource = UpdateRowSource.None;  
  
        // Set the DELETE command and parameter.  
        adapter.DeleteCommand = new SqlCommand(  
            "DELETE FROM Production.ProductCategory "  
            + "WHERE ProductCategoryID=@ProdCatID;", connection);  
        adapter.DeleteCommand.Parameters.Add("@ProdCatID",
          SqlDbType.Int, 4, "ProductCategoryID");  
        adapter.DeleteCommand.UpdatedRowSource = UpdateRowSource.None;  
  
        // Set the batch size.  
        adapter.UpdateBatchSize = batchSize;  
  
        // Execute the update.  
        adapter.Update(dataTable);  
    }  
}  
```  
  
## <a name="handling-batch-update-related-events-and-errors"></a>处理与批处理更新相关的事件和错误  

 **DataAdapter**有两个与更新相关的事件： **RowUpdating**和**RowUpdated**。 在 ADO.NET 的以前版本中，如果禁用批处理，则每处理一行就会生成一次这些事件。 **RowUpdating** 是在更新执行之前生成的，在数据库更新完成后生成 **RowUpdated** 。  
  
### <a name="event-behavior-changes-with-batch-updates"></a>批处理更新的事件行为更改  

 启用批处理时，在单个数据库操作中可更新多行。 因此，每个批处理只发生一次 `RowUpdated` 事件，而对于处理每一行，`RowUpdating` 事件都会发生。 禁用批处理时，这两个事件一对一交错触发，即一行触发一个 `RowUpdating` 事件和一个 `RowUpdated` 事件，下一行触发一个 `RowUpdating` 事件和一个 `RowUpdated` 事件，直到处理完所有行。  
  
### <a name="accessing-updated-rows"></a>访问更新的行  

 禁用批处理时，可以使用 <xref:System.Data.Common.RowUpdatedEventArgs.Row%2A> 类的 <xref:System.Data.Common.RowUpdatedEventArgs> 属性访问要进行更新的行。  
  
 启用批处理时，会为多行生成单个 `RowUpdated` 事件。 因此，每一行的 `Row` 属性值为空。 但仍会为每一行生成 `RowUpdating` 事件。 使用 <xref:System.Data.Common.RowUpdatedEventArgs.CopyToRows%2A> 类的 <xref:System.Data.Common.RowUpdatedEventArgs> 方法可以通过将对行的引用复制到一个数组来访问已处理的行。 如果没有要进行处理的行，`CopyToRows` 将引发一个 <xref:System.ArgumentNullException>。 在调用 <xref:System.Data.Common.RowUpdatedEventArgs.RowCount%2A> 方法之前，使用 <xref:System.Data.Common.RowUpdatedEventArgs.CopyToRows%2A> 属性可返回已处理行的数目。  
  
### <a name="handling-data-errors"></a>处理数据错误  

 执行批处理与执行每个单独的语句具有相同的效果。 各语句按照其添加到批处理中的顺序执行。 在批处理模式下处理错误的方式与禁用批处理模式时相同。 每一行均单独处理。 只有在数据库中经过成功处理的行才能在 <xref:System.Data.DataRow> 内的相应 <xref:System.Data.DataTable> 中更新。  
  
 数据提供程序和后端数据库服务器确定支持哪些 SQL 构造以执行批处理。 如果为执行提交了不支持的语句，则可能引发异常。  
  
## <a name="see-also"></a>请参阅

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [使用 DataAdapter 更新数据源](updating-data-sources-with-dataadapters.md)
- [处理 DataAdapter 事件](handling-dataadapter-events.md)
- [ADO.NET 概述](ado-net-overview.md)
