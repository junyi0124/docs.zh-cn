---
title: 从 DataAdapter 填充数据集
description: 了解如何在 ADO.NET 中从 DataAdapter 填充数据集，这提供了独立于数据源的一致关系编程模型。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 3fa0ac7d-e266-4954-bfac-3fbe2f913153
ms.openlocfilehash: ac84af884238b166266d4206802878c1e21169fd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177405"
---
# <a name="populating-a-dataset-from-a-dataadapter"></a>从 DataAdapter 填充数据集

ADO.NET <xref:System.Data.DataSet> 是数据的内存驻留表示形式，提供与数据源无关的一致关系编程模型。 `DataSet` 表示整个数据集，其中包含表、约束和表之间的关系。 由于 `DataSet` 独立于数据源，因此 `DataSet` 可以包含应用程序本地的数据，也可以包含来自多个数据源的数据。 与现有数据源的交互通过 `DataAdapter`来控制。  
  
 `SelectCommand` 的 `DataAdapter` 属性是一个 `Command` 对象，用于从数据源中检索数据。 `InsertCommand`的 `UpdateCommand`、 `DeleteCommand` 和 `DataAdapter` 属性是 `Command` 对象，用于按照对 `DataSet`中数据的修改来管理对数据源中数据的更新。 [通过 Dataadapter 更新数据源](updating-data-sources-with-dataadapters.md)中更详细地介绍了这些属性。  
  
 `Fill` 的 `DataAdapter` 方法用于使用 `DataSet` 的 `SelectCommand` 结果填充 `DataAdapter`。 `Fill` 将要填充的 `DataSet` 、和 `DataTable` 对象（或要使用从 `DataTable` 中返回的行来填充的 `SelectCommand`的名称）作为它的参数。  
  
> [!NOTE]
> 使用 `DataAdapter` 检索表的全部内容会花费些时间，尤其是在表中有很多行时。 这是因为访问数据库，定位和处理数据，然后将数据传输到客户端是需要很长时间的。 将表中全部内容提取到客户端还会在服务器上锁定所有行。 若要提高性能，您可以使用 `WHERE` 子句使返回客户端的行数大为减少。 还可以通过只显式列出 `SELECT` 语句要求的列减少返回到客户端的数据量。 另一种好的变通方法是以批次检索行（例如一次检索几百行），并且在客户端完成当前批次后只检索下一批次。  
  
 `Fill` 方法使用 `DataReader` 对象来隐式地返回用于在 `DataSet`中创建表的列名称和类型，以及用于填充 `DataSet`中的表行的数据。 表和列仅在不存在时才创建；否则， `Fill` 将使用现有的 `DataSet` 架构。 根据 [ADO.NET 中的数据类型映射](data-type-mappings-in-ado-net.md)中的表，列类型作为 .NET Framework 类型创建。 除非它们存在于数据源和中，否则不会创建主键 `DataAdapter` **。**`MissingSchemaAction` 设置为 `MissingSchemaAction` **.** `AddWithKey` 。 如果 `Fill` 发现某个表存在主键，对于主键列的值与从数据源返回的行的主键列的值匹配的行，将使用数据源中的数据重写 `DataSet` 中的数据。 如果未找到任何主键，则将数据追加到 `DataSet`中的表。 `Fill` 使用填充时可能存在的任何映射 `DataSet` (参阅 [DataAdapter DataTable And DataColumn 映射](dataadapter-datatable-and-datacolumn-mappings.md)) 。  
  
> [!NOTE]
> 如果 `SelectCommand` 返回 OUTER JOIN 的结果，则 `DataAdapter` 不会为生成的 `PrimaryKey` 设置 `DataTable`值。 您必须自己定义 `PrimaryKey` 以确保正确解析重复行。 有关详细信息，请参阅 [定义主键](./dataset-datatable-dataview/defining-primary-keys.md)。  
  
 以下代码示例创建了一个 <xref:System.Data.SqlClient.SqlDataAdapter> 实例，使用 Microsoft SQL Server <xref:System.Data.SqlClient.SqlConnection> 数据库的 `Northwind` 并使用客户列表填充 <xref:System.Data.DataTable> 中的 `DataSet` 。 向 <xref:System.Data.SqlClient.SqlConnection> 构造函数传递的 SQL 语句和 <xref:System.Data.SqlClient.SqlDataAdapter> 参数用于创建 <xref:System.Data.SqlClient.SqlDataAdapter.SelectCommand%2A> 的 <xref:System.Data.SqlClient.SqlDataAdapter>属性。  
  
## <a name="example"></a>示例  
  
```vb  
' Assumes that connection is a valid SqlConnection object.  
Dim queryString As String = _  
  "SELECT CustomerID, CompanyName FROM dbo.Customers"  
Dim adapter As SqlDataAdapter = New SqlDataAdapter( _  
  queryString, connection)  
  
Dim customers As DataSet = New DataSet  
adapter.Fill(customers, "Customers")  
```  
  
```csharp  
// Assumes that connection is a valid SqlConnection object.  
string queryString =
  "SELECT CustomerID, CompanyName FROM dbo.Customers";  
SqlDataAdapter adapter = new SqlDataAdapter(queryString, connection);  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
```  
  
> [!NOTE]
> 此示例中所示的代码不显式打开和关闭 `Connection`。 如果 `Fill` 方法发现连接尚未打开，它将隐式地打开 `Connection` 正在使用的 `DataAdapter` 。 如果 `Fill` 已打开连接，则它还将在 `Fill` 完成时关闭连接。 当处理单一操作（如 `Fill` 或 `Update`）时，这可以简化您的代码。 但是，如果您在执行多项需要打开连接的操作，则可以通过以下方式提高应用程序的性能：显式调用 `Open` 的 `Connection`方法，对数据源执行操作，然后调用 `Close` 的 `Connection`方法。 应尝试使数据源的连接打开的时间尽可能短，以便释放资源供其他客户端应用程序使用。  
  
## <a name="multiple-result-sets"></a>多个结果集  

 如果 `DataAdapter` 遇到多个结果集，则将在 `DataSet`中创建多个表。 这些表的命名方式为默认名称 Table 加上*N*，N 从 0 开始递增，如以 Table0 为第一个表名，依次类推。 如果以参数形式向 `Fill` 方法传递表名，则这些表的命名方式为默认名称 TableName 加上*N*，N 从 0 开始递增，如以 TableName0 为第一个表名，依次类推。  
  
## <a name="populating-a-dataset-from-multiple-dataadapters"></a>从多个 DataAdapter 填充 DataSet  

 可以将任意数量的 `DataAdapter` 对象与一起使用 `DataSet` 。 每个 `DataAdapter` 都可用于填充一个或多个 `DataTable` 对象并将更新解析回相关数据源。 `DataRelation` 和 `Constraint` 对象可以在本地添加到 `DataSet` ，这样您就可以关联来自不同数据源的数据。 例如， `DataSet` 可以包含来自 Microsoft SQL Server 数据库、通过 OLE DB 公开的 IBM DB2 数据库以及对 XML 进行流处理的数据源的数据。 一个或多个 `DataAdapter` 对象可以处理与每个数据源的通信。  
  
### <a name="example"></a>示例  

 以下代码示例从 Microsoft SQL Server 上的 `Northwind` 数据库填充客户列表，从存储在 Microsoft Access 2000 上的 `Northwind` 数据库填充订单列表。 已填充的表通过 `DataRelation`相关联，这样，客户列表将与相应客户的订单一起显示出来。 有关对象的详细信息 `DataRelation` ，请参阅 [添加 Datarelation](./dataset-datatable-dataview/adding-datarelations.md) 和 [导航 datarelation](./dataset-datatable-dataview/navigating-datarelations.md)。  
  
```vb  
' Assumes that customerConnection is a valid SqlConnection object.  
' Assumes that orderConnection is a valid OleDbConnection object.  
Dim custAdapter As SqlDataAdapter = New SqlDataAdapter( _  
  "SELECT * FROM dbo.Customers", customerConnection)  
  
Dim ordAdapter As OleDbDataAdapter = New OleDbDataAdapter( _  
  "SELECT * FROM Orders", orderConnection)  
  
Dim customerOrders As DataSet = New DataSet()  
custAdapter.Fill(customerOrders, "Customers")  
ordAdapter.Fill(customerOrders, "Orders")  
  
Dim relation As DataRelation = _  
  customerOrders.Relations.Add("CustOrders", _  
  customerOrders.Tables("Customers").Columns("CustomerID"), _
  customerOrders.Tables("Orders").Columns("CustomerID"))  
  
Dim pRow, cRow As DataRow  
For Each pRow In customerOrders.Tables("Customers").Rows  
  Console.WriteLine(pRow("CustomerID").ToString())  
  
  For Each cRow In pRow.GetChildRows(relation)  
    Console.WriteLine(vbTab & cRow("OrderID").ToString())  
  Next  
Next  
```  
  
```csharp  
// Assumes that customerConnection is a valid SqlConnection object.  
// Assumes that orderConnection is a valid OleDbConnection object.  
SqlDataAdapter custAdapter = new SqlDataAdapter(  
  "SELECT * FROM dbo.Customers", customerConnection);  
OleDbDataAdapter ordAdapter = new OleDbDataAdapter(  
  "SELECT * FROM Orders", orderConnection);  
  
DataSet customerOrders = new DataSet();  
  
custAdapter.Fill(customerOrders, "Customers");  
ordAdapter.Fill(customerOrders, "Orders");  
  
DataRelation relation = customerOrders.Relations.Add("CustOrders",  
  customerOrders.Tables["Customers"].Columns["CustomerID"],  
  customerOrders.Tables["Orders"].Columns["CustomerID"]);  
  
foreach (DataRow pRow in customerOrders.Tables["Customers"].Rows)  
{  
  Console.WriteLine(pRow["CustomerID"]);  
   foreach (DataRow cRow in pRow.GetChildRows(relation))  
    Console.WriteLine("\t" + cRow["OrderID"]);  
}  
```  
  
## <a name="sql-server-decimal-type"></a>SQL Server Decimal 类型  

 默认情况下， `DataSet` 使用 .NET Framework 数据类型存储数据。 对于大多数应用程序，这些类型都提供了一种方便的数据源信息表示形式。 但是，当数据源中的数据类型是 SQL Server decimal 或 numeric 数据类型时，这种表示形式可能会导致问题。 .NET Framework `decimal` 数据类型最多允许28个有效位，而 SQL Server `decimal` 数据类型允许38个有效位。 如果 `SqlDataAdapter` 在 `Fill` 操作过程中确定 SQL Server `decimal` 字段的精度大于 28 个字符，则不会将当前行添加到 `DataTable`。 此时将发生 `FillError` 事件，使您能够确定是否将发生精度损失并作出适当的响应。 有关事件的详细信息 `FillError` ，请参阅 [处理 DataAdapter 事件](handling-dataadapter-events.md)。 若要获取 SQL Server `decimal` 值，还可以使用 <xref:System.Data.SqlClient.SqlDataReader> 对象并调用 <xref:System.Data.SqlClient.SqlDataReader.GetSqlDecimal%2A> 方法。  
  
 ADO.NET 2.0 在中引入了的增强支持 <xref:System.Data.SqlTypes> `DataSet` 。 有关详细信息，请参阅 [SqlTypes and the DataSet](./sql/sqltypes-and-the-dataset.md)。  
  
## <a name="ole-db-chapters"></a>OLE DB 章节  

 分层行集或章节（OLE DB 类型 `DBTYPE_HCHAPTER`、ADO 类型 `adChapter`）可用于填充 `DataSet`的内容。 当 <xref:System.Data.OleDb.OleDbDataAdapter> 在 `Fill` 操作过程中遇到章节列时，将为章节列创建一个 `DataTable` ，并使用章节中的列和行填充该表。 为章节列创建的表使用父表名称和章节列名称来命名，其形式为“*ParentTableNameChapteredColumnName*”。 如果 `DataSet` 中已存在与章节列的名称相匹配的表，则将使用章节数据填充当前表。 如果现有表中不存在与章节中的列相匹配的列，则将添加新列。  
  
 在使用章节列中的数据填充 `DataSet` 中的表之前，将创建分层行集的父表和子表之间的关系，方法是向父表和子表添加一个整数列，将父列设置为自动递增，然后使用两个表中所添加的列来创建 `DataRelation` 。 所添加的关系使用父表和章节列名称来命名，其形式为“*ParentTableNameChapterColumnName*”。  
  
 请注意，相关列仅存在于 `DataSet`中。 如果在随后从数据源进行填充，则将使新行被添加到表中，而不是使更改被并入现有行。  
  
 另请注意，如果使用采用 `DataAdapter.Fill` 的 `DataTable`重载，则将只填充该表。 仍会将自动递增整数列添加到该表中，但不会创建或填充任何子表并且不会创建任何关系。  
  
 以下示例使用 MSDataShape 提供程序为客户列表中的每个客户生成订单的章节列。 然后使用相应数据来填充 `DataSet` 。  
  
```vb  
Using connection As OleDbConnection = New OleDbConnection( _  
  "Provider=MSDataShape;Data Provider=SQLOLEDB;" & _  
  "Data Source=(local);Integrated " & _  
  "Security=SSPI;Initial Catalog=northwind")  
  
Dim adapter As OleDbDataAdapter = New OleDbDataAdapter( _  
  "SHAPE {SELECT CustomerID, CompanyName FROM Customers} " & _  
  "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS Orders " & _  
  "RELATE CustomerID TO CustomerID)", connection)  
  
Dim customers As DataSet = New DataSet()  
  
adapter.Fill(customers, "Customers")  
End Using  
```  
  
```csharp  
using (OleDbConnection connection = new OleDbConnection("Provider=MSDataShape;Data Provider=SQLOLEDB;" +  
  "Data Source=(local);Integrated Security=SSPI;Initial Catalog=northwind"))  
{  
OleDbDataAdapter adapter = new OleDbDataAdapter("SHAPE {SELECT CustomerID, CompanyName FROM Customers} " +  
  "APPEND ({SELECT CustomerID, OrderID FROM Orders} AS Orders " +  
  "RELATE CustomerID TO CustomerID)", connection);  
  
DataSet customers = new DataSet();  
adapter.Fill(customers, "Customers");  
}  
```  
  
 完成 `Fill` 操作后， `DataSet` 包含两个表： `Customers` 和 `CustomersOrders`，其中 `CustomersOrders` 表示章节列。 将名为 `Orders` 的附加列添加到 `Customers` 表中，将名为 `CustomersOrders` 的附加列添加到 `CustomersOrders` 表中。 将 `Orders` 表中的 `Customers` 列设置为自动递增。 `DataRelation`这个 `CustomersOrders`是通过向上述两个表添加列创建的，其中以 `Customers` 表作为父表。 下表显示了一些示例结果。  
  
### <a name="tablename-customers"></a>TableName：Customers  
  
|CustomerID|CompanyName|Orders|  
|----------------|-----------------|------------|  
|ALFKI|Alfreds Futterkiste|0|  
|ANATR|Ana Trujillo Emparedados y helados|1|  
  
### <a name="tablename-customersorders"></a>TableName：CustomersOrders  
  
|CustomerID|OrderID|CustomersOrders|  
|----------------|-------------|---------------------|  
|ALFKI|10643|0|  
|ALFKI|10692|0|  
|ANATR|10308|1|  
|ANATR|10625|1|  
  
## <a name="see-also"></a>请参阅

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [ADO.NET 中的数据类型映射](data-type-mappings-in-ado-net.md)
- [使用 DbDataAdapter 修改数据](modifying-data-with-a-dbdataadapter.md)
- [多重活动结果集 (MARS)](./sql/multiple-active-result-sets-mars.md)
- [ADO.NET 概述](ado-net-overview.md)
