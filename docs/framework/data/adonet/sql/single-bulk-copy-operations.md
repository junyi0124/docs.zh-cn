---
title: 单个批量复制操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 5e7ff0be-3f23-4996-a92c-bd54d65c3836
ms.openlocfilehash: 05e3cf25352e731d320061001f08a835cd520b15
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70780928"
---
# <a name="single-bulk-copy-operations"></a>单个批量复制操作

执行 SQL Server 批量复制操作最简单的方法就是对数据库执行单次操作。 默认情况下，批量复制操作是作为一个独立的操作执行的：该复制操作以非事务处理方式进行，不可进行回滚。

> [!NOTE]
> 如果需要在出错时回滚批量复制的全部或部分，可以使用 <xref:System.Data.SqlClient.SqlBulkCopy> 管理的事务，或在现有事务中执行批量复制操作。 如果连接已（隐<xref:System.Transactions>式或显式）登记到 SqlBulkCopy**事务中**，则也可以使用。
>
> 有关详细信息，请参阅[事务和大容量复制操作](transaction-and-bulk-copy-operations.md)。

执行批量复制操作的一般步骤如下所示：

1. 连接到源服务器上并获取要复制的数据。 如果可以从 <xref:System.Data.IDataReader> 或 <xref:System.Data.DataTable> 对象检索数据，则这些数据还可能来自其他源。

2. 连接到目标服务器（除非你希望**SqlBulkCopy**为你建立连接）。

3. 创建一个 <xref:System.Data.SqlClient.SqlBulkCopy> 对象，设置任何必要的属性。

4. 设置**DestinationTableName**属性以指示大容量插入操作的目标表。

5. 调用**WriteToServer**方法之一。

6. 根据需要更新属性并再次调用**WriteToServer** 。

7. 调用 <xref:System.Data.SqlClient.SqlBulkCopy.Close%2A>，或将批量复制操作包装在 `Using` 语句中。

> [!CAUTION]
> 我们建议源列和目标列的数据类型匹配。 如果数据类型不匹配，则**SqlBulkCopy**会尝试使用所采用<xref:System.Data.SqlClient.SqlParameter.Value%2A>的规则将每个源值转换为目标数据类型。 转换可能会影响性能，还可能会导致意外的错误。 例如，大多数情况下，`Double` 数据类型可以转换为 `Decimal` 数据类型，但是有时就不能。

## <a name="example"></a>示例

以下控制台应用程序演示如何使用 <xref:System.Data.SqlClient.SqlBulkCopy> 类加载数据。 在此示例中， <xref:System.Data.SqlClient.SqlDataReader>用于将数据从 SQL Server **AdventureWorks**数据库中的**Product**表复制到同一个数据库中的类似表。

> [!IMPORTANT]
> 除非已按照[大容量复制示例设置](bulk-copy-example-setup.md)中所述创建了工作表，否则此示例将不会运行。 提供此代码是为了演示仅使用**SqlBulkCopy**的语法。 如果源表和目标表位于同一个 SQL Server 实例中，则使用 Transact-SQL `INSERT … SELECT` 语句复制数据会更加容易、更加迅速。

[!code-csharp[DataWorks BulkCopy.Single#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks BulkCopy.Single/CS/source.cs#1)]
[!code-vb[DataWorks BulkCopy.Single#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks BulkCopy.Single/VB/source.vb#1)]

## <a name="performing-a-bulk-copy-operation-using-transact-sql-and-the-command-class"></a>使用 Transact-SQL 和命令类执行批量复制操作

以下示例说明如何使用 <xref:System.Data.SqlClient.SqlCommand.ExecuteNonQuery%2A> 方法执行 BULK INSERT 语句。

> [!NOTE]
> 数据源的文件路径相对于服务器。 要成功执行批量复制操作，服务器进程必须具有对该路径的访问权限。

```vb
Using connection As SqlConnection = New SqlConnection(connectionString)
Dim queryString As String = _
    "BULK INSERT Northwind.dbo.[Order Details] FROM " & _
    "'f:\mydata\data.tbl' WITH (FORMATFILE='f:\mydata\data.fmt' )"
connection.Open()
SqlCommand command = New SqlCommand(queryString, connection);

command.ExecuteNonQuery()
End Using
```

```csharp
using (SqlConnection connection = New SqlConnection(connectionString))
{
string queryString =  "BULK INSERT Northwind.dbo.[Order Details] " +
    "FROM 'f:\mydata\data.tbl' " +
    "WITH ( FORMATFILE='f:\mydata\data.fmt' )";
connection.Open();
SqlCommand command = new SqlCommand(queryString, connection);

command.ExecuteNonQuery();
}
```

## <a name="see-also"></a>请参阅

- [SQL Server 中的大容量复制操作](bulk-copy-operations-in-sql-server.md)
- [ADO.NET 概述](../ado-net-overview.md)
