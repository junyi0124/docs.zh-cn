---
title: 使用存储过程修改数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7d8e9a46-1af6-4a02-bf61-969d77ae07e0
ms.openlocfilehash: 65116a48533fd6ce86894c6a4522929285f8e1f0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150747"
---
# <a name="modifying-data-with-stored-procedures"></a>使用存储过程修改数据

存储过程可以接受数据作为输入参数并可以返回数据作为输出参数、结果集或返回值。 下面的示例演示 ADO.NET 如何发送和接收输入参数、输出参数及返回值。 该示例将一条新记录插入到一个表中，该表中的主键列为 SQL Server 数据库中的标识列。  
  
> [!NOTE]
> 如果您要通过 SQL Server 存储过程使用 <xref:System.Data.SqlClient.SqlDataAdapter> 来编辑或删除数据，请确保不要在存储过程定义中使用 SET NOCOUNT ON。 这将使返回的受影响的行数为零，`DataAdapter` 会将其解释为并发冲突。 在这种情况下，将引发 <xref:System.Data.DBConcurrencyException>。  
  
## <a name="example"></a>示例  

 该示例使用以下存储过程将新类别插入**Northwind** **category 表。** 该存储过程采用 "列 **名称** " 列中的值作为输入参数，并使用 SCOPE_IDENTITY ( # A1 函数检索标识字段的新值 " **类别 id**"，并在输出参数中将其返回。 RETURN 语句使用 @ @ROWCOUNT 函数返回插入的行数。  
  
```sql
CREATE PROCEDURE dbo.InsertCategory  
  @CategoryName nvarchar(15),  
  @Identity int OUT  
AS  
INSERT INTO Categories (CategoryName) VALUES(@CategoryName)  
SET @Identity = SCOPE_IDENTITY()  
RETURN @@ROWCOUNT  
```  
  
 下面的代码示例使用上面显示的 `InsertCategory` 存储过程作为 <xref:System.Data.SqlClient.SqlDataAdapter.InsertCommand%2A> 的 <xref:System.Data.SqlClient.SqlDataAdapter> 的来源。 如果在将记录插入到数据库后调用 `@Identity` 的 <xref:System.Data.DataSet> 方法，`Update` 中将会反映出 <xref:System.Data.SqlClient.SqlDataAdapter> 输出参数。 此代码还会检索返回值。  
  
> [!NOTE]
> 使用时 <xref:System.Data.OleDb.OleDbDataAdapter> ，必须在 <xref:System.Data.ParameterDirection> 其他参数之前指定带有 **ReturnValue** 的参数。  
  
 [!code-csharp[DataWorks SqlClient.SprocIdentityReturn#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlClient.SprocIdentityReturn/CS/source.cs#1)]
 [!code-vb[DataWorks SqlClient.SprocIdentityReturn#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlClient.SprocIdentityReturn/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [执行命令](executing-a-command.md)
- [ADO.NET 概述](ado-net-overview.md)
