---
title: 表值参数
description: 了解如何使用表值参数将多行数据从客户端应用程序封送到 SQL Server。
ms.date: 10/12/2018
dev_langs:
- csharp
- vb
ms.assetid: 370c16d5-db7b-43e3-945b-ccaab35b739b
ms.openlocfilehash: ea063b333ea0680071b880f26753bfd74b71d80f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91188871"
---
# <a name="table-valued-parameters"></a>表值参数

表值参数提供了一将多行数据从客户端应用程序封送到 SQL Server 的种简单方法，而无需进行多次往返或特殊的服务器端逻辑来处理数据。 可使用表值参数来封装客户端应用程序中的数据行，并以单个参数化命令将数据发送到服务器。 传入数据行存储在随后可使用 Transact-SQL 进行操作的表变量中。  
  
 可以使用标准的 Transact-SQL SELECT 语句来访问表值参数中的列值。 表值参数为强类型，其结构会自动进行验证。 表值参数的大小仅受服务器内存的限制。  
  
> [!NOTE]
> 无法返回表值参数中的数据。 表值参数仅限输入；不支持 OUTPUT 关键字。  
  
 若要详细了解表值参数，请参阅以下资源。  
  
|资源|说明|  
|--------------|-----------------|  
|[使用表值参数（数据库引擎）](/sql/relational-databases/tables/use-table-valued-parameters-database-engine)|介绍如何创建和使用表值参数。|  
|[User-Defined Table Types](/previous-versions/sql/sql-server-2008/bb522526(v=sql.100))|说明用于声明表值参数的用户定义的表类型。|  
  
## <a name="passing-multiple-rows-in-previous-versions-of-sql-server"></a>在 SQL Server 的早期版本中传递多行  

 在 SQL Server 2008 中引入表值参数之前，用于将多行数据传递到存储过程或参数化 SQL 命令的选项受到限制。 开发人员可以选择下面的一种方法，将多行传递到服务器：  
  
- 使用一系列单独的参数来表示多列和多行数据中的值。 使用这种方法可以传递的数据量受到允许使用的参数数量限制。 SQL Server 过程最多可以有 2100 个参数。 必须使用服务器端逻辑，将这些单独的值汇编到表变量或临时表中以供处理。  
  
- 将多个数据值绑定到分隔字符串或 XML 文档，然后将这些文本值传递到过程或语句。 这要求过程或语句包含验证数据结构和解除绑定值所需的逻辑。  
  
- 为影响多行的数据修改创建一系列单独的 SQL 语句，例如通过调用 `Update` 的 <xref:System.Data.SqlClient.SqlDataAdapter> 方法创建的语句。 更改可以单独提交给服务器，也可以批量提交给组。 不过，即使是包含多个语句的批量提交，每个语句也是在服务器上单独执行。  
  
- 使用 `bcp` 实用工具或 <xref:System.Data.SqlClient.SqlBulkCopy> 对象将多行数据加载到表中。 尽管这种技术非常高效，但它不支持服务器端处理，除非将数据加载到临时表或表变量中。  
  
## <a name="creating-table-valued-parameter-types"></a>创建表值参数类型  

 表值参数基于使用 Transact-sql CREATE TYPE 语句定义的强类型表结构。 必须先在 SQL Server 中创建一个表类型并定义结构，才能在客户端应用程序中使用表值参数。 有关创建表类型的详细信息，请参阅 [用户定义的表类型](/previous-versions/sql/sql-server-2008/bb522526(v=sql.100))。  
  
 以下语句创建一个名为 CategoryTableType 的表类型，其中包含 CategoryID 列和 CategoryName 列：  
  
```sql
CREATE TYPE dbo.CategoryTableType AS TABLE  
    ( CategoryID int, CategoryName nvarchar(50) )  
```  
  
 创建表类型后，可以基于该类型声明表值参数。 下面的 Transact-SQL 片段演示如何在存储过程定义中声明表值参数。 请注意，声明表值参数需要 READONLY 关键字。  
  
```sql
CREATE PROCEDURE usp_UpdateCategories
    (@tvpNewCategories dbo.CategoryTableType READONLY)  
```  
  
## <a name="modifying-data-with-table-valued-parameters-transact-sql"></a>通过表值参数修改数据 (Transact-SQL)  

 表值参数可用于基于集的数据修改，这些修改通过执行一条语句影响多行。 例如，可以选择表值参数中的所有行并将它们插入数据库表，也可以通过将表值参数联接到要更新的表来创建更新语句。  
  
 下面的 Transact-SQL UPDATE 语句演示如何通过将表值参数联接到 Categories 表来使用它。 在 FROM 子句中结合使用表值参数和 JOIN 时，还必须对表值参数使用别名。如下所示，表值参数的别名为“ec”：  
  
```sql
UPDATE dbo.Categories  
    SET Categories.CategoryName = ec.CategoryName  
    FROM dbo.Categories INNER JOIN @tvpEditedCategories AS ec  
    ON dbo.Categories.CategoryID = ec.CategoryID;  
```  
  
 此 Transact-SQL 示例演示如何从表值参数中选择行以在单个基于集的操作中执行 INSERT。  
  
```sql
INSERT INTO dbo.Categories (CategoryID, CategoryName)  
    SELECT nc.CategoryID, nc.CategoryName FROM @tvpNewCategories AS nc;  
```  
  
## <a name="limitations-of-table-valued-parameters"></a>表值参数的限制  

 表值参数有几个限制：  
  
- 无法将表值参数传递给 [CLR 用户定义的函数](/sql/relational-databases/clr-integration-database-objects-user-defined-functions/clr-user-defined-functions)。  
  
- 表值参数只能通过被编制索引来支持 UNIQUE 或 PRIMARY KEY 约束。 SQL Server 不维护表值参数的统计信息。  
  
- 在 Transact-SQL 代码中表值参数是只读的。 既不能更新表值参数行中的列值，也不能插入或删除行。 若要修改表值参数中传递到存储过程或参数化语句的数据，必须将数据插入临时表或表变量。  
  
- 不能使用 ALTER TABLE 语句来修改表值参数的设计。  
  
## <a name="configuring-a-sqlparameter-example"></a>配置 SqlParameter 示例  

 <xref:System.Data.SqlClient> 支持从 <xref:System.Data.DataTable><xref:System.Data.Common.DbDataReader> 或 <xref:System.Collections.Generic.IEnumerable%601> \ <xref:Microsoft.SqlServer.Server.SqlDataRecord> 对象填充表值参数。 必须使用 <xref:System.Data.SqlClient.SqlParameter.TypeName%2A> 的 <xref:System.Data.SqlClient.SqlParameter> 属性指定表值参数的类型名称。 `TypeName` 必须与先前在服务器上创建的兼容类型的名称相匹配。 下面的代码段演示如何配置 <xref:System.Data.SqlClient.SqlParameter> 以插入数据。  

在以下示例中，`addedCategories` 变量包含一个 <xref:System.Data.DataTable>。 若要查看如何填充变量，请参阅下一节中的示例，[将表值参数传递给存储过程](#passing)。

```csharp  
// Configure the command and parameter.  
SqlCommand insertCommand = new SqlCommand(sqlInsert, connection);  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", addedCategories);  
tvpParam.SqlDbType = SqlDbType.Structured;  
tvpParam.TypeName = "dbo.CategoryTableType";  
```  
  
```vb  
' Configure the command and parameter.  
Dim insertCommand As New SqlCommand(sqlInsert, connection)  
Dim tvpParam As SqlParameter = _  
   insertCommand.Parameters.AddWithValue( _  
  "@tvpNewCategories", addedCategories)  
tvpParam.SqlDbType = SqlDbType.Structured  
tvpParam.TypeName = "dbo.CategoryTableType"  
```  
  
 你也可以使用从 <xref:System.Data.Common.DbDataReader> 中派生的任何对象，将数据行流处理到表值参数，如本代码段所示：  
  
```csharp  
// Configure the SqlCommand and table-valued parameter.  
SqlCommand insertCommand = new SqlCommand("usp_InsertCategories", connection);  
insertCommand.CommandType = CommandType.StoredProcedure;  
SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", dataReader);  
tvpParam.SqlDbType = SqlDbType.Structured;  
```  
  
```vb  
' Configure the SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  dataReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
```  
  
## <a name="passing-a-table-valued-parameter-to-a-stored-procedure"></a><a name="passing"></a> 将表值参数传递给存储过程  

 此示例演示如何将表值参数数据传递到存储过程。 代码使用 <xref:System.Data.DataTable> 方法将添加的行提取到新的 <xref:System.Data.DataTable.GetChanges%2A> 中。 然后，该代码定义一个 <xref:System.Data.SqlClient.SqlCommand>，并将 <xref:System.Data.SqlClient.SqlCommand.CommandType%2A> 属性设置为 <xref:System.Data.CommandType.StoredProcedure>。 使用 <xref:System.Data.SqlClient.SqlParameter> 方法填充 <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A>，并将 <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> 设置为 `Structured`。 然后，使用 <xref:System.Data.SqlClient.SqlCommand> 方法执行 <xref:System.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A>。  
  
```csharp  
// Assumes connection is an open SqlConnection object.  
using (connection)  
{  
  // Create a DataTable with the modified rows.  
  DataTable addedCategories = CategoriesDataTable.GetChanges(DataRowState.Added);  

  // Configure the SqlCommand and SqlParameter.  
  SqlCommand insertCommand = new SqlCommand("usp_InsertCategories", connection);  
  insertCommand.CommandType = CommandType.StoredProcedure;  
  SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", addedCategories);  
  tvpParam.SqlDbType = SqlDbType.Structured;  

  // Execute the command.  
  insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection object.  
Using connection  
   '  Create a DataTable with the modified rows.  
   Dim addedCategories As DataTable = _  
     CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Configure the SqlCommand and SqlParameter.  
   Dim insertCommand As New SqlCommand( _  
     "usp_InsertCategories", connection)  
   insertCommand.CommandType = CommandType.StoredProcedure  
   Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
     "@tvpNewCategories", addedCategories)  
   tvpParam.SqlDbType = SqlDbType.Structured  
  
   '  Execute the command.  
   insertCommand.ExecuteNonQuery()  
End Using  
```  
  
### <a name="passing-a-table-valued-parameter-to-a-parameterized-sql-statement"></a>将表值参数传递给参数化 SQL 语句  

 下面的示例演示如何使用包含将表值参数作为数据源的 SELECT 子查询的 INSERT 语句向 dbo.Categories 表插入数据。 将表值参数传递给参数化 SQL 语句时，必须使用 <xref:System.Data.SqlClient.SqlParameter.TypeName%2A> 的新 <xref:System.Data.SqlClient.SqlParameter> 属性指定表值参数的类型名称。 此 `TypeName` 必须与先前在服务器上创建的兼容类型的名称相匹配。 本示例中的代码使用 `TypeName` 属性来引用在 dbo.CategoryTableType 中定义的类型结构。  
  
> [!NOTE]
> 如果为表值参数中的标识列提供值，则必须为会话发出 SET IDENTITY_INSERT 语句。  
  
```csharp  
// Assumes connection is an open SqlConnection.  
using (connection)  
{  
  // Create a DataTable with the modified rows.  
  DataTable addedCategories = CategoriesDataTable.GetChanges(DataRowState.Added);  

  // Define the INSERT-SELECT statement.  
  string sqlInsert =
      "INSERT INTO dbo.Categories (CategoryID, CategoryName)"  
      + " SELECT nc.CategoryID, nc.CategoryName"  
      + " FROM @tvpNewCategories AS nc;"  

  // Configure the command and parameter.  
  SqlCommand insertCommand = new SqlCommand(sqlInsert, connection);  
  SqlParameter tvpParam = insertCommand.Parameters.AddWithValue("@tvpNewCategories", addedCategories);  
  tvpParam.SqlDbType = SqlDbType.Structured;  
  tvpParam.TypeName = "dbo.CategoryTableType";  

  // Execute the command.  
  insertCommand.ExecuteNonQuery();  
}  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
Using connection  
  ' Create a DataTable with the modified rows.  
  Dim addedCategories As DataTable = _  
    CategoriesDataTable.GetChanges(DataRowState.Added)  
  
  ' Define the INSERT-SELECT statement.  
  Dim sqlInsert As String = _  
  "INSERT INTO dbo.Categories (CategoryID, CategoryName)" _  
  & " SELECT nc.CategoryID, nc.CategoryName" _  
  & " FROM @tvpNewCategories AS nc;"  
  
  ' Configure the command and parameter.  
  Dim insertCommand As New SqlCommand(sqlInsert, connection)  
  Dim tvpParam As SqlParameter = _  
     insertCommand.Parameters.AddWithValue( _  
    "@tvpNewCategories", addedCategories)  
  tvpParam.SqlDbType = SqlDbType.Structured  
  tvpParam.TypeName = "dbo.CategoryTableType"  
  
  ' Execute the query  
  insertCommand.ExecuteNonQuery()  
End Using  
```  
  
## <a name="streaming-rows-with-a-datareader"></a>使用 DataReader 对行进行流处理  

 你也可以使用从 <xref:System.Data.Common.DbDataReader> 中派生的任何对象，将数据行流处理到表值参数。 下面的代码段演示如何使用 <xref:System.Data.OracleClient.OracleCommand> 和 <xref:System.Data.OracleClient.OracleDataReader> 从 Oracle 数据库中检索数据。 然后，该代码将 <xref:System.Data.SqlClient.SqlCommand> 配置为使用单个输入参数调用存储过程。 <xref:System.Data.SqlClient.SqlParameter.SqlDbType%2A> 的 <xref:System.Data.SqlClient.SqlParameter> 属性设置为 `Structured`。 <xref:System.Data.SqlClient.SqlParameterCollection.AddWithValue%2A> 将 `OracleDataReader` 结果集作为表值参数传递给存储过程。  
  
```csharp  
// Assumes connection is an open SqlConnection.  
// Retrieve data from Oracle.  
OracleCommand selectCommand = new OracleCommand(  
   "Select CategoryID, CategoryName FROM Categories;",  
   oracleConnection);  
OracleDataReader oracleReader = selectCommand.ExecuteReader(  
   CommandBehavior.CloseConnection);  
  
 // Configure the SqlCommand and table-valued parameter.  
 SqlCommand insertCommand = new SqlCommand(  
   "usp_InsertCategories", connection);  
 insertCommand.CommandType = CommandType.StoredProcedure;  
 SqlParameter tvpParam =  
    insertCommand.Parameters.AddWithValue(  
    "@tvpNewCategories", oracleReader);  
 tvpParam.SqlDbType = SqlDbType.Structured;  
  
 // Execute the command.  
 insertCommand.ExecuteNonQuery();  
```  
  
```vb  
' Assumes connection is an open SqlConnection.  
' Retrieve data from Oracle.  
Dim selectCommand As New OracleCommand( _  
  "Select CategoryID, CategoryName FROM Categories;", _  
  oracleConnection)  
Dim oracleReader As OracleDataReader = _  
  selectCommand.ExecuteReader(CommandBehavior.CloseConnection)  
  
' Configure SqlCommand and table-valued parameter.  
Dim insertCommand As New SqlCommand("usp_InsertCategories", connection)  
insertCommand.CommandType = CommandType.StoredProcedure  
Dim tvpParam As SqlParameter = _  
  insertCommand.Parameters.AddWithValue("@tvpNewCategories", _  
  oracleReader)  
tvpParam.SqlDbType = SqlDbType.Structured  
  
' Execute the command.  
insertCommand.ExecuteNonQuery()  
```  
  
## <a name="see-also"></a>请参阅

- [配置参数和参数数据类型](../configuring-parameters-and-parameter-data-types.md)
- [命令和参数](../commands-and-parameters.md)
- [DataAdapter 参数](../dataadapter-parameters.md)
- [ADO.NET 中的 SQL Server 数据操作](sql-server-data-operations.md)
- [ADO.NET 概述](../ado-net-overview.md)
