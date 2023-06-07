---
title: 代码示例
description: 这些示例演示 .NET Framework 程序员如何使用 ADO.NET 数据提供程序和 ADO.NET 实体框架从数据库中检索数据。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c119657a-9ce6-4940-91e4-ac1d5f0d9584
ms.openlocfilehash: 920912591b20102f81e1fed2f4ffbe51df8015fb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153529"
---
# <a name="adonet-code-examples"></a>ADO.NET 代码示例

本页上的代码列表演示如何使用以下 ADO.NET 技术从数据库中检索数据：

- ADO.NET 数据提供程序：

  - [SqlClient](#sqlclient) (`System.Data.SqlClient`) 

  - [OleDb](#oledb) (`System.Data.OleDb`) 

  - [Odbc](#odbc) (`System.Data.Odbc`) 

  - [System.data.oracleclient](#oracleclient) (`System.Data.OracleClient`) 

- ADO.NET 实体框架：

  - [LINQ to Entities](#linq-to-entities)

  - [类型化 ObjectQuery](#typed-objectquery)

  - [EntityClient](#entityclient) (`System.Data.EntityClient`) 

- [LINQ to SQL](#linq-to-sql)

## <a name="adonet-data-provider-examples"></a>ADO.NET 数据提供程序示例

以下代码列表演示如何使用 ADO.NET 数据提供程序从数据库中检索数据。 数据在一个 `DataReader` 中返回。 有关详细信息，请参阅 [使用 DataReader 检索数据](retrieving-data-using-a-datareader.md)。

### <a name="sqlclient"></a>SqlClient

本示例中的代码假定您可以连接到 `Northwind` Microsoft SQL Server 上的示例数据库。 在此情形 5 中，示例代码创建一个 <xref:System.Data.SqlClient.SqlCommand> 以从 Products 表中选择行，并添加 <xref:System.Data.SqlClient.SqlParameter> 来将结果限制为其 UnitPrice 大于指定参数值的行。 在 <xref:System.Data.SqlClient.SqlConnection> 块中打开 `using` ，这可确保在代码退出时关闭并释放资源。 示例代码使用 <xref:System.Data.SqlClient.SqlDataReader> 执行命令，并在控制台窗口中显示结果。

 [!code-csharp[DataWorks SampleApp.SqlClient#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.SqlClient/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.SqlClient#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.SqlClient/VB/source.vb#1)]

### <a name="oledb"></a>OleDb

此示例中的代码假定你可以连接到 Microsoft Access Northwind 示例数据库。 在此情形 5 中，示例代码创建一个 <xref:System.Data.OleDb.OleDbCommand> 以从 Products 表中选择行，并添加 <xref:System.Data.OleDb.OleDbParameter> 来将结果限制为其 UnitPrice 大于指定参数值的行。 <xref:System.Data.OleDb.OleDbConnection> 在 `using` 块内打开，这将确保在代码退出时会关闭和释放资源。 示例代码使用 <xref:System.Data.OleDb.OleDbDataReader> 执行命令，并在控制台窗口中显示结果。

 [!code-csharp[DataWorks SampleApp.OleDb#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.OleDb/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.OleDb#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.OleDb/VB/source.vb#1)]

### <a name="odbc"></a>Odbc

此示例中的代码假定你可以连接到 Microsoft Access Northwind 示例数据库。 在此情形 5 中，示例代码创建一个 <xref:System.Data.Odbc.OdbcCommand> 以从 Products 表中选择行，并添加 <xref:System.Data.Odbc.OdbcParameter> 来将结果限制为其 UnitPrice 大于指定参数值的行。 在 <xref:System.Data.Odbc.OdbcConnection> 块中打开 `using` ，这可确保在代码退出时关闭并释放资源。 示例代码使用 <xref:System.Data.Odbc.OdbcDataReader> 执行命令，并在控制台窗口中显示结果。

[!code-csharp[DataWorks SampleApp.Odbc#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.Odbc/CS/source.cs#1)]
[!code-vb[DataWorks SampleApp.Odbc#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.Odbc/VB/source.vb#1)]

### <a name="oracleclient"></a>OracleClient

此示例中的代码假定已建立与 Oracle 服务器上的 DEMO.CUSTOMER 的连接。 您还必须添加对 System.Data.OracleClient.dll 的引用。 示例代码在 <xref:System.Data.OracleClient.OracleDataReader> 中返回数据。

 [!code-csharp[DataWorks SampleApp.Oracle#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SampleApp.Oracle/CS/source.cs#1)]
 [!code-vb[DataWorks SampleApp.Oracle#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SampleApp.Oracle/VB/source.vb#1)]

## <a name="entity-framework-examples"></a>实体框架示例

以下代码列表演示如何通过查询实体数据模型 (EDM) 中的实体来从数据源检索数据。 这些示例使用基于 Northwind 示例数据库的模型。 有关实体框架的详细信息，请参阅 [实体框架概述](./ef/overview.md)。

### <a name="linq-to-entities"></a>LINQ to Entities

此示例中的代码使用 LINQ 查询以 Categories 对象的形式返回数据，这些对象将作为仅包含 CategoryID 和 CategoryName 属性的匿名类型提取。 有关详细信息，请参阅 [LINQ to Entities 概述](./ef/language-reference/linq-to-entities.md)。

```csharp
using System;
using System.Linq;
using System.Data.Objects;
using NorthwindModel;

class LinqSample
{
    public static void ExecuteQuery()
    {
        using (NorthwindEntities context = new NorthwindEntities())
        {
            try
            {
                var query = from category in context.Categories
                            select new
                            {
                                categoryID = category.CategoryID,
                                categoryName = category.CategoryName
                            };

                foreach (var categoryInfo in query)
                {
                    Console.WriteLine("\t{0}\t{1}",
                        categoryInfo.categoryID, categoryInfo.categoryName);
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Linq
Imports System.Data.Objects
Imports NorthwindModel

Class LinqSample
    Public Shared Sub ExecuteQuery()
        Using context As NorthwindEntities = New NorthwindEntities()
            Try
                Dim query = From category In context.Categories _
                            Select New With _
                            { _
                                .categoryID = category.CategoryID, _
                                .categoryName = category.CategoryName _
                            }

                For Each categoryInfo In query
                    Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                        categoryInfo.categoryID, categoryInfo.categoryName)
                Next
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
        End Using
    End Sub
End Class
```

### <a name="typed-objectquery"></a>类型化 ObjectQuery

此示例中的代码使用 <xref:System.Data.Objects.ObjectQuery%601> 以 Categories 对象的形式返回数据。 有关详细信息，请参阅 [对象查询](/previous-versions/dotnet/netframework-4.0/bb896241(v=vs.100))。

```csharp
using System;
using System.Data.Objects;
using NorthwindModel;

class ObjectQuerySample
{
    public static void ExecuteQuery()
    {
        using (NorthwindEntities context = new NorthwindEntities())
        {
            ObjectQuery<Categories> categoryQuery = context.Categories;

            foreach (Categories category in
                categoryQuery.Execute(MergeOption.AppendOnly))
            {
                Console.WriteLine("\t{0}\t{1}",
                    category.CategoryID, category.CategoryName);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Data.Objects
Imports NorthwindModel

Class ObjectQuerySample
    Public Shared Sub ExecuteQuery()
        Using context As NorthwindEntities = New NorthwindEntities()
            Dim categoryQuery As ObjectQuery(Of Categories) = context.Categories

            For Each category As Categories In _
                categoryQuery.Execute(MergeOption.AppendOnly)
                Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                    category.CategoryID, category.CategoryName)
            Next
        End Using
    End Sub
End Class
```

### <a name="entityclient"></a>EntityClient

此示例中的代码使用 <xref:System.Data.EntityClient.EntityCommand> 来执行实体 SQL 查询。 此查询会返回表示 Categories 实体类型的实例的记录的列表。 <xref:System.Data.EntityClient.EntityDataReader> 用于访问结果集中的数据记录。 有关详细信息，请参阅 [用于 Entity Framework 的 EntityClient 提供程序](./ef/entityclient-provider-for-the-entity-framework.md)。

```csharp
using System;
using System.Data;
using System.Data.Common;
using System.Data.EntityClient;
using NorthwindModel;

class EntityClientSample
{
    public static void ExecuteQuery()
    {
        string queryString =
            @"SELECT c.CategoryID, c.CategoryName
                FROM NorthwindEntities.Categories AS c";

        using (EntityConnection conn =
            new EntityConnection("name=NorthwindEntities"))
        {
            try
            {
                conn.Open();
                using (EntityCommand query = new EntityCommand(queryString, conn))
                {
                    using (DbDataReader rdr =
                        query.ExecuteReader(CommandBehavior.SequentialAccess))
                    {
                        while (rdr.Read())
                        {
                            Console.WriteLine("\t{0}\t{1}", rdr[0], rdr[1]);
                        }
                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    }
}
```

```vb
Option Explicit On
Option Strict On

Imports System.Data
Imports System.Data.Common
Imports System.Data.EntityClient
Imports NorthwindModel

Class EntityClientSample
    Public Shared Sub ExecuteQuery()
        Dim queryString As String = _
            "SELECT c.CategoryID, c.CategoryName " & _
                "FROM NorthwindEntities.Categories AS c"

        Using conn As EntityConnection = _
            New EntityConnection("name=NorthwindEntities")

            Try
                conn.Open()
                Using query As EntityCommand = _
                New EntityCommand(queryString, conn)
                    Using rdr As DbDataReader = _
                        query.ExecuteReader(CommandBehavior.SequentialAccess)
                        While rdr.Read()
                            Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                                              rdr(0), rdr(1))
                        End While
                    End Using
                End Using
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
        End Using
    End Sub
End Class
```

## <a name="linq-to-sql"></a>LINQ to SQL

此示例中的代码使用 LINQ 查询以 Categories 对象的形式返回数据，这些对象将作为仅包含 CategoryID 和 CategoryName 属性的匿名类型提取。 此示例基于 Northwind 数据上下文。 有关详细信息，请参阅[入门](./sql/linq/getting-started.md)。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Northwind;

    class LinqSqlSample
    {
        public static void ExecuteQuery()
        {
            using (NorthwindDataContext db = new NorthwindDataContext())
            {
                try
                {
                    var query = from category in db.Categories
                                select new
                                {
                                    categoryID = category.CategoryID,
                                    categoryName = category.CategoryName
                                };

                    foreach (var categoryInfo in query)
                    {
                        Console.WriteLine("vbTab {0} vbTab {1}",
                            categoryInfo.categoryID, categoryInfo.categoryName);
                    }
                }
                catch (Exception ex)
                {
                    Console.WriteLine(ex.Message);
                }
            }
        }
    }
```

```vb
Option Explicit On
Option Strict On

Imports System.Collections.Generic
Imports System.Linq
Imports System.Text
Imports Northwind

Class LinqSqlSample
    Public Shared Sub ExecuteQuery()
        Using db As NorthwindDataContext = New NorthwindDataContext()
            Try
                Dim query = From category In db.Categories _
                                Select New With _
                                { _
                                    .categoryID = category.CategoryID, _
                                    .categoryName = category.CategoryName _
                                }

                For Each categoryInfo In query
                    Console.WriteLine(vbTab & "{0}" & vbTab & "{1}", _
                            categoryInfo.categoryID, categoryInfo.categoryName)
                Next
            Catch ex As Exception
                Console.WriteLine(ex.Message)
            End Try
            End Using
    End Sub
End Class
```

## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [创建数据应用程序](/previous-versions/visualstudio/visual-studio-2013/h0y4a0f6(v=vs.120))
- [查询实体数据模型（实体框架任务）](/previous-versions/bb738455(v=vs.90))
- [如何：执行返回匿名类型对象的查询](/previous-versions/dotnet/netframework-4.0/bb738512(v=vs.100))
