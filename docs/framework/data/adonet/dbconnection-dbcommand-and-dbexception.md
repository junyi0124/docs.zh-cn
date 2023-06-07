---
title: DbConnection、DbCommand 和 DbException
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 58aab611-7e6f-4749-b983-28ab7ae87dbe
ms.openlocfilehash: 3075999c15fccc3a41c191297a146c362b3638e8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183320"
---
# <a name="dbconnection-dbcommand-and-dbexception"></a>DbConnection、DbCommand 和 DbException

创建了 <xref:System.Data.Common.DbProviderFactory> 和 <xref:System.Data.Common.DbConnection> 后，您便可以使用命令和数据读取器来从数据源检索数据。  
  
## <a name="retrieving-data-example"></a>检索数据示例  

 此示例将 `DbConnection` 对象用作参数。 通过将 <xref:System.Data.Common.DbCommand> 设置为 SQL SELECT 语句，可创建 <xref:System.Data.Common.DbCommand.CommandText%2A> 以从类别表中选择数据。 该代码假定数据源中存在类别表。 打开连接并使用 <xref:System.Data.Common.DbDataReader> 检索数据。  
  
 [!code-csharp[DataWorks DbProviderFactories.DbCommandData#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbCommandData/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories.DbCommandData#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbCommandData/VB/source.vb#1)]  
  
## <a name="executing-a-command-example"></a>执行命令示例  

 此示例将 `DbConnection` 对象用作参数。 如果 `DbConnection` 有效，则会打开该连接，并创建和执行 <xref:System.Data.Common.DbCommand>。 将 <xref:System.Data.Common.DbCommand.CommandText%2A> 设置为 SQL INSERT 语句，以执行插入到 Northwind 数据库类别表中的操作。 该代码假定数据源中存在 Northwind 数据库，并且 INSERT 语句中使用的 SQL 语法对于指定提供程序有效。 数据源中发生的错误由 <xref:System.Data.Common.DbException> 代码块处理，其他所有异常在 <xref:System.Exception> 块中处理。  
  
 [!code-csharp[DataWorks DbProviderFactories.DbCommand#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbCommand/CS/source.cs#1)]
 [!code-vb[DataWorks DbProviderFactories.DbCommand#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks DbProviderFactories.DbCommand/VB/source.vb#1)]  
  
## <a name="handling-data-errors-with-dbexception"></a>使用 DbException 处理数据错误  

 <xref:System.Data.Common.DbException> 类是代表数据源引发的所有异常的基类。 你可以在异常处理代码中使用该类来处理由各种提供程序引发的异常，而不必引用特定异常类。 下面的代码段演示如何使用 <xref:System.Data.Common.DbException> 来显示使用 <xref:System.Exception.GetType%2A>、<xref:System.Exception.Source%2A>、<xref:System.Runtime.InteropServices.ExternalException.ErrorCode%2A> 和 <xref:System.Exception.Message%2A> 属性的数据源返回的错误信息。 输出将显示错误类型、指示提供程序名称的源、错误代码以及与错误相关的消息。  
  
```vb  
Try  
    ' Do work here.  
Catch ex As DbException  
    ' Display information about the exception.  
    Console.WriteLine("GetType: {0}", ex.GetType())  
    Console.WriteLine("Source: {0}", ex.Source)  
    Console.WriteLine("ErrorCode: {0}", ex.ErrorCode)  
    Console.WriteLine("Message: {0}", ex.Message)  
Finally  
    ' Perform cleanup here.  
End Try  
```  
  
```csharp  
try  
{  
    // Do work here.  
}  
catch (DbException ex)  
{  
    // Display information about the exception.  
    Console.WriteLine("GetType: {0}", ex.GetType());  
    Console.WriteLine("Source: {0}", ex.Source);  
    Console.WriteLine("ErrorCode: {0}", ex.ErrorCode);  
    Console.WriteLine("Message: {0}", ex.Message);  
}  
finally  
{  
    // Perform cleanup here.  
}  
```  
  
## <a name="see-also"></a>请参阅

- [DbProviderFactories](dbproviderfactories.md)
- [获取 DbProviderFactory](obtaining-a-dbproviderfactory.md)
- [使用 DbDataAdapter 修改数据](modifying-data-with-a-dbdataadapter.md)
- [ADO.NET 概述](ado-net-overview.md)
