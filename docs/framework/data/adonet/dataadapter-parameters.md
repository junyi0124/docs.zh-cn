---
title: DataAdapter 参数
description: 了解 DbDataAdapter 的属性，这些属性从数据源返回数据并管理对数据源所做的更改。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f21e6aba-b76d-46ad-a83e-2ad8e0af1e12
ms.openlocfilehash: 1264d678b4823149498150f13d8783a82890f6a0
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91177706"
---
# <a name="dataadapter-parameters"></a>DataAdapter 参数

<xref:System.Data.Common.DbDataAdapter> 具有四个用于从数据源检索数据和更新数据源中数据的属性：<xref:System.Data.Common.DbDataAdapter.SelectCommand%2A> 属性返回数据源中的数据；<xref:System.Data.Common.DbDataAdapter.InsertCommand%2A>、<xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> 和 <xref:System.Data.Common.DbDataAdapter.DeleteCommand%2A> 属性用于管理数据源中的更改。 调用 `SelectCommand` 的 `Fill` 方法之前必须设置 `DataAdapter` 属性。 在调用 `InsertCommand` 的 `UpdateCommand` 方法之前必须设置 `DeleteCommand`、`Update` 或 `DataAdapter` 属性，具体取决于对 <xref:System.Data.DataTable> 中的数据做了哪些更改。 例如，如果已添加行，在调用 `InsertCommand` 之前必须设置 `Update`。 当 `Update` 正在处理已插入、已更新或已删除的行时，`DataAdapter` 将使用相应的 `Command` 属性来处理该操作。 有关已修改行的当前信息将通过 `Command` 集合传递到 `Parameters` 对象。  
  
 当您在数据源中更新行时，将调用 UPDATE 语句，该语句使用唯一标识符来标识要更新的表中的行。 该唯一标识符通常是主键字段的值。 UPDATE 语句使用的参数既包含唯一标识符又包含要更新的列和值，如下面的 Transact-SQL 语句所示。  
  
```sql
UPDATE Customers SET CompanyName = @CompanyName
  WHERE CustomerID = @CustomerID  
```  
  
> [!NOTE]
> 参数占位符的语法取决于数据源。 此示例显示 SQL Server 数据源的占位符。 使用问号 (?) 占位符代表 <xref:System.Data.OleDb> 和 <xref:System.Data.Odbc> 参数。  
  
 在此 Visual Basic 示例中，将 `CompanyName` 用 `@CompanyName` `CustomerID` 等于参数值的行的参数值更新字段 `@CustomerID` 。 参数使用对象的属性从修改的行中检索信息 <xref:System.Data.SqlClient.SqlParameter.SourceColumn%2A> <xref:System.Data.SqlClient.SqlParameter> 。 下面是上一示例 UPDATE 语句的参数。 代码假定变量 `adapter` 表示有效的 <xref:System.Data.SqlClient.SqlDataAdapter> 对象。  
  
```vb
adapter.Parameters.Add( _  
  "@CompanyName", SqlDbType.NChar, 15, "CompanyName")  
Dim parameter As SqlParameter = _  
  adapter.UpdateCommand.Parameters.Add("@CustomerID", _  
  SqlDbType.NChar, 5, "CustomerID")  
parameter.SourceVersion = DataRowVersion.Original  
```  
  
 `Add` 集合的 `Parameters` 方法接受参数的名称、数据类型、大小（如果适用于该类型）以及 <xref:System.Data.Common.DbParameter.SourceColumn%2A> 中的 `DataTable` 的名称。 请注意，<xref:System.Data.Common.DbParameter.SourceVersion%2A> 参数的 `@CustomerID` 设置为 `Original`。 这样可以保证，如果标识列的值已经在修改后的 <xref:System.Data.DataRow> 中被更改，就一定会更新数据源中的现有行。 在这种情况下，`Original` 行值将匹配数据源中的当前值，而 `Current` 行值将包含更新的值。 没有设置 `SourceVersion` 参数的 `@CompanyName`，而将使用默认的 `Current` 行值。  
  
> [!NOTE]
> 对于的 `Fill` 操作 `DataAdapter` 和的 `Get` 方法，将 `DataReader` 从 .NET Framework 数据提供程序返回的类型推断 .NET Framework 类型。 ADO.NET 中的 [数据类型映射](data-type-mappings-in-ado-net.md)中介绍了 Microsoft SQL Server、OLE DB 和 ODBC 数据类型的推断 .NET Framework 类型和访问器方法。  
  
## <a name="parametersourcecolumn-parametersourceversion"></a>Parameter.SourceColumn，Parameter.SourceVersion  

 `SourceColumn` 和 `SourceVersion` 可以作为自变量传递给 `Parameter` 构造函数，也可以设置为现有 `Parameter` 的属性。 `SourceColumn` 是将要从中检索 <xref:System.Data.DataColumn> 值的 <xref:System.Data.DataRow> 中的 `Parameter` 的名称。 `SourceVersion` 指定 `DataRow` 用于检索该值的 `DataAdapter` 版本。  
  
 下表显示可以与 <xref:System.Data.DataRowVersion> 一起使用的 `SourceVersion` 枚举值。  
  
|DataRowVersion 枚举|说明|  
|--------------------------------|-----------------|  
|`Current`|该参数使用列的当前值。 这是默认设置。|  
|`Default`|该参数使用列的 `DefaultValue`。|  
|`Original`|该参数使用列的原始值。|  
|`Proposed`|该参数使用建议值。|  
  
 下一节中的 `SqlClient` 代码示例为 <xref:System.Data.Common.DbDataAdapter.UpdateCommand%2A> 定义了一个参数，在该示例中 `CustomerID` 列用作以下两个参数的 `SourceColumn`：`@CustomerID` (`SET CustomerID = @CustomerID`) 和 `@OldCustomerID` (`WHERE CustomerID = @OldCustomerID`)。 `@CustomerID`参数用于将 **CustomerID** 列更新为中的当前值 `DataRow` 。 因此， `CustomerID` `SourceColumn` `SourceVersion` 使用带有的 `Current` 。 `@OldCustomerID`参数用于标识数据源中的当前行。 由于在该行的 `Original` 版本中找到了匹配列值，所以将使用 `SourceColumn` 为 `CustomerID` 的相同 `SourceVersion` (`Original`)。  
  
## <a name="working-with-sqlclient-parameters"></a>使用 SqlClient 参数  

 下面的示例演示如何创建 <xref:System.Data.SqlClient.SqlDataAdapter> 并将 <xref:System.Data.Common.DataAdapter.MissingSchemaAction%2A> 设置为 <xref:System.Data.MissingSchemaAction.AddWithKey>，以便从数据库中检索其他架构信息。 <xref:System.Data.SqlClient.SqlDataAdapter.SelectCommand%2A>、<xref:System.Data.SqlClient.SqlDataAdapter.InsertCommand%2A>、<xref:System.Data.SqlClient.SqlDataAdapter.UpdateCommand%2A> 和 <xref:System.Data.SqlClient.SqlDataAdapter.DeleteCommand%2A> 属性集及其相应的 <xref:System.Data.SqlClient.SqlParameter> 对象已添加到 <xref:System.Data.SqlClient.SqlCommand.Parameters%2A> 集合。 该方法返回一个 `SqlDataAdapter` 对象。  
  
 [!code-csharp[Classic WebData SqlDataAdapter.SqlDataAdapter Example#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/Classic WebData SqlDataAdapter.SqlDataAdapter Example/CS/source.cs#1)]
 [!code-vb[Classic WebData SqlDataAdapter.SqlDataAdapter Example#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/Classic WebData SqlDataAdapter.SqlDataAdapter Example/VB/source.vb#1)]  
  
## <a name="oledb-parameter-placeholders"></a>OleDb 参数占位符  

 对于 <xref:System.Data.OleDb.OleDbDataAdapter> 对象和 <xref:System.Data.Odbc.OdbcDataAdapter> 对象，必须使用问号 (?) 占位符来标识参数。  
  
```vb  
Dim selectSQL As String = _  
  "SELECT CustomerID, CompanyName FROM Customers " & _  
  "WHERE CountryRegion = ? AND City = ?"  
Dim insertSQL AS String = _  
  "INSERT INTO Customers (CustomerID, CompanyName) VALUES (?, ?)"  
Dim updateSQL AS String = _  
  "UPDATE Customers SET CustomerID = ?, CompanyName = ? " & _  
  WHERE CustomerID = ?"  
Dim deleteSQL As String = "DELETE FROM Customers WHERE CustomerID = ?"  
```  
  
```csharp  
string selectSQL =
  "SELECT CustomerID, CompanyName FROM Customers " +  
  "WHERE CountryRegion = ? AND City = ?";  
string insertSQL =
  "INSERT INTO Customers (CustomerID, CompanyName) " +  
  "VALUES (?, ?)";  
string updateSQL =
  "UPDATE Customers SET CustomerID = ?, CompanyName = ? " +  
  "WHERE CustomerID = ? ";  
string deleteSQL = "DELETE FROM Customers WHERE CustomerID = ?";  
```  
  
 参数化查询语句定义必须创建的输入和输出参数。 若要创建参数，请使用 `Parameters.Add` 方法或 `Parameter` 构造函数来指定列名称、数据类型和大小。 对于内部数据类型（如 `Integer`）无需包含大小，也可以指定默认大小。  
  
 下面的代码示例创建 SQL 语句的参数，然后填充 `DataSet`。  
  
## <a name="oledb-example"></a>OleDb 示例  
  
```vb  
' Assumes that connection is a valid OleDbConnection object.  
Dim adapter As OleDbDataAdapter = New OleDbDataAdapter
  
Dim selectCMD AS OleDbCommand = New OleDbCommand(selectSQL, connection)  
adapter.SelectCommand = selectCMD  
  
' Add parameters and set values.  
selectCMD.Parameters.Add( _  
  "@CountryRegion", OleDbType.VarChar, 15).Value = "UK"  
selectCMD.Parameters.Add( _  
  "@City", OleDbType.VarChar, 15).Value = "London"  
  
Dim customers As DataSet = New DataSet  
adapter.Fill(customers, "Customers")  
```  
  
```csharp  
// Assumes that connection is a valid OleDbConnection object.  
OleDbDataAdapter adapter = new OleDbDataAdapter();  
  
OleDbCommand selectCMD = new OleDbCommand(selectSQL, connection);  
adapter.SelectCommand = selectCMD;  
  
// Add parameters and set values.  
selectCMD.Parameters.Add(  
  "@CountryRegion", OleDbType.VarChar, 15).Value = "UK";  
selectCMD.Parameters.Add(  
  "@City", OleDbType.VarChar, 15).Value = "London";  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
```  
  
## <a name="odbc-parameters"></a>Odbc 参数  
  
```vb  
' Assumes that connection is a valid OdbcConnection object.  
Dim adapter As OdbcDataAdapter = New OdbcDataAdapter  
  
Dim selectCMD AS OdbcCommand = New OdbcCommand(selectSQL, connection)  
adapter.SelectCommand = selectCMD  
  
' Add Parameters and set values.  
selectCMD.Parameters.Add("@CountryRegion", OdbcType.VarChar, 15).Value = "UK"  
selectCMD.Parameters.Add("@City", OdbcType.VarChar, 15).Value = "London"  
  
Dim customers As DataSet = New DataSet  
adapter.Fill(customers, "Customers")  
```  
  
```csharp  
// Assumes that connection is a valid OdbcConnection object.  
OdbcDataAdapter adapter = new OdbcDataAdapter();  
  
OdbcCommand selectCMD = new OdbcCommand(selectSQL, connection);  
adapter.SelectCommand = selectCMD;  
  
//Add Parameters and set values.  
selectCMD.Parameters.Add("@CountryRegion", OdbcType.VarChar, 15).Value = "UK";  
selectCMD.Parameters.Add("@City", OdbcType.VarChar, 15).Value = "London";  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
```  
  
> [!NOTE]
> 如果没有为参数提供参数名称，则会为该参数指定增量默认名称 parameter *N* *，* 以 "Parameter1" 开头。 建议在提供参数名称时避免使用参数 *N* 命名约定，因为所提供的名称可能会与中现有的默认参数名称冲突 `ParameterCollection` 。 如果提供的名称已经存在，将引发异常。  
  
## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [命令和参数](commands-and-parameters.md)
- [使用 DataAdapter 更新数据源](updating-data-sources-with-dataadapters.md)
- [使用存储过程修改数据](modifying-data-with-stored-procedures.md)
- [ADO.NET 中的数据类型映射](data-type-mappings-in-ado-net.md)
- [ADO.NET 概述](ado-net-overview.md)
