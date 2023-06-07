---
title: 用于 SQL Server 的提供程序统计信息
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 429c9d09-92ac-46ec-829a-fbff0a9575a2
ms.openlocfilehash: 21bf7662094d5bc8948a1ce6378c454713cacc62
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183112"
---
# <a name="provider-statistics-for-sql-server"></a>用于 SQL Server 的提供程序统计信息

从 .NET Framework 2.0 版开始，适用于 SQL Server 的 .NET Framework 数据提供程序支持运行时统计信息。 必须通过将 <xref:System.Data.SqlClient.SqlConnection> 对象的 <xref:System.Data.SqlClient.SqlConnection.StatisticsEnabled%2A> 属性设置为 `True` 来启用统计信息。 启用统计信息后，可以通过 <xref:System.Data.SqlClient.SqlConnection> 对象的 <xref:System.Data.SqlClient.SqlConnection.RetrieveStatistics%2A> 方法检索 <xref:System.Collections.IDictionary> 引用，以“即时快照”的方式查看统计信息。 可以将该列表枚举为一组名称/值对字典条目。 这些名称/值对是无序的。 随时都可以调用 <xref:System.Data.SqlClient.SqlConnection> 对象的 <xref:System.Data.SqlClient.SqlConnection.ResetStatistics%2A> 方法来重置计数器。 如果尚未启用统计信息收集，则不会生成异常。 此外，如果在未首先调用 <xref:System.Data.SqlClient.SqlConnection.StatisticsEnabled%2A> 的情况下调用 <xref:System.Data.SqlClient.SqlConnection.RetrieveStatistics%2A>，则检索的值是每个条目的初始值。 如果启用统计信息，请运行应用程序一段时间，然后禁用统计信息，检索到的值将反映收集的值，直到禁用统计信息为止。 收集的所有统计值都基于每个连接。  
  
## <a name="statistical-values-available"></a>可用的统计信息值  

 Microsoft SQL Server 提供程序中目前提供 18 个不同的项。 可用的项数可以通过 <xref:System.Data.SqlClient.SqlConnection.RetrieveStatistics%2A> 返回的 <xref:System.Collections.IDictionary> 接口引用的 Count 属性进行访问****。 提供程序统计信息的所有计数器均使用公共语言运行时 <xref:System.Int64> 类型（C# 和 Visual Basic 中为 long），宽度为 64 位****。 int64 数据类型的最大值由 int64.MaxValue 字段定义，最大值为 ((2^63)-1))********。 当计数器的值达到此最大值时，它们将不再被认为是准确的。 这意味着 int64.MaxValue-1((2^63)-2) 实际上是任何统计信息的最大有效值****。  
  
> [!NOTE]
> 字典用于返回提供程序统计信息，因为返回的统计信息的数量、名称和顺序可能会在将来发生变化。 应用程序不应依赖于在字典中找到的特定值，而应检查该值是否存在，并相应地设置分支。  
  
 下表显示了当前可用的统计值。 注意，在 Microsoft .NET Framework 的地区版本中，各个值的键名未本地化。  
  
|键名|描述|  
|--------------|-----------------|  
|`BuffersReceived`|返回应用程序开始使用提供程序并启用统计信息后，提供程序从 SQL Server 接收到的表格格式数据流 (TDS) 数据包的数量。|  
|`BuffersSent`|返回启用了统计信息之后，提供程序向 SQL Server 发送的 TDS 数据包的数量。 如果命令较大，可能需要多个缓冲区。 例如，如果将较大的命令发送到服务器，并且它需要六个数据包，则将 `ServerRoundtrips` 增加 1，将 `BuffersSent` 增加 6。|  
|`BytesReceived`|返回在应用程序使用提供程序启动并启用了统计信息之后，提供程序从 SQL Server 接收的 TDS 数据包中的数据字节数。|  
|`BytesSent`|返回在应用程序使用提供程序启动并启用了统计信息之后，发送到 SQL Server 的 TDS 数据包中的数据字节数。|  
|`ConnectionTime`|连接在统计信息启用之后已打开的时间（如果在打开连接之前就启用了统计信息，则为总连接时间），以毫秒为单位。|  
|`CursorOpens`|返回在应用程序使用提供程序启动并启用了统计信息之后，通过连接打开游标的次数。<br /><br /> 请注意，SELECT 语句返回的只读/仅转发结果不被视为游标，因此不会影响此计数器。|  
|`ExecutionTime`|返回在启用统计信息之后，提供程序用于处理方面的累计时间（以毫秒为单位），包括等待服务器回复所用的时间以及执行提供程序本身的代码所用的时间。<br /><br /> 包含计时代码的类如下：<br /><br /> SqlConnection<br /><br /> SqlCommand<br /><br /> SqlDataReader<br /><br /> SqlDataAdapter<br /><br /> SqlTransaction<br /><br /> SqlCommandBuilder<br /><br /> 为了尽可能减少性能关键成员数，不会对以下成员计时：<br /><br /> SqlDataReader<br /><br /> this[] 运算符（所有重载）<br /><br /> GetBoolean<br /><br /> GetChar<br /><br /> GetDateTime<br /><br /> GetDecimal<br /><br /> GetDouble<br /><br /> GetFloat<br /><br /> GetGuid<br /><br /> GetInt16<br /><br /> GetInt32<br /><br /> GetInt64<br /><br /> GetName<br /><br /> GetOrdinal<br /><br /> GetSqlBinary<br /><br /> GetSqlBoolean<br /><br /> GetSqlByte<br /><br /> GetSqlDateTime<br /><br /> GetSqlDecimal<br /><br /> GetSqlDouble<br /><br /> GetSqlGuid<br /><br /> GetSqlInt16<br /><br /> GetSqlInt32<br /><br /> GetSqlInt64<br /><br /> GetSqlMoney<br /><br /> GetSqlSingle<br /><br /> GetSqlString<br /><br /> GetString<br /><br /> IsDBNull|  
|`IduCount`|返回在应用程序使用提供程序启动并启用了统计信息之后，通过连接执行的 INSERT、DELETE 和 UPDATE 语句的总数。|  
|`IduRows`|返回在应用程序使用提供程序启动并启用了统计信息之后，受通过连接执行的 INSERT、DELETE 和 UPDATE 语句影响的行的总数。|  
|`NetworkServerTime`|返回在应用程序使用提供程序启动并启用了统计信息之后，提供程序等待服务器回复所用的累计时间（以毫秒为单位）。|  
|`PreparedExecs`|返回在应用程序使用提供程序启动并启用了统计信息之后，通过连接执行的已准备命令数。|  
|`Prepares`|返回在应用程序使用提供程序启动并启用了统计信息之后，通过连接准备的语句数。|  
|`SelectCount`|返回在应用程序使用提供程序启动并启用了统计信息之后，通过连接执行的 SELECT 语句数。 这包括用于从游标检索行的 FETCH 语句，当到达 <xref:System.Data.SqlClient.SqlDataReader> 的末尾时，将更新 SELECT 语句的计数。|  
|`SelectRows`|返回在应用程序使用提供程序启动并启用了统计信息之后选定的行数。 此技术反映了由 SQL 语句生成的所有行，即使调用方并未真正占用这些行也是如此。 例如，在读取整个结果集之前关闭数据读取器不会影响计数。 这包括通过 FETCH 语句从游标中检索的行。|  
|`ServerRoundtrips`|返回在应用程序使用提供程序启动并启用了统计信息之后，连接将命令发送到服务器并收到回复的次数。|  
|`SumResultSets`|返回在应用程序使用提供程序启动并启用了统计信息之后已使用的结果集的数目。 例如，这将包括返回到客户端的任何结果集。 对于游标，每个提取或块提取操作都被视为独立的结果集。|  
|`Transactions`|返回在应用程序使用提供程序启动并启用了统计信息（包括回滚）后启动的用户事务数。 如果在自动提交启用的情况下运行连接，则每个命令均被视为事务。<br /><br /> 一旦执行 BEGIN TRAN 语句，此计数器就会增加事务计数，而无需考虑稍后是提交还是回滚事务。|  
|`UnpreparedExecs`|返回在应用程序使用提供程序启动并启用了统计信息之后，通过连接执行的未准备语句数。|  
  
### <a name="retrieving-a-value"></a>检索值  

 下面的控制台应用程序显示了如何启用连接的统计信息，检索四个单独的统计值并将它们写出到控制台窗口。  
  
> [!NOTE]
> 下面的示例使用随 SQL Server 提供的 AdventureWorks 示例数据库****。 示例代码中提供的连接字符串假定数据库已安装并且在本地计算机上可用。 根据环境需要修改连接字符串。  
  
```vb  
Option Strict On  
  
Imports System  
Imports System.Collections  
Imports System.Data  
Imports System.Data.SqlClient  
  
Module Module1  
  
  Sub Main()  
    Dim connectionString As String = GetConnectionString()  
  
    Using awConnection As New SqlConnection(connectionString)  
      ' StatisticsEnabled is False by default.  
      ' It must be set to True to start the
      ' statistic collection process.  
      awConnection.StatisticsEnabled = True  
  
      Dim productSQL As String = "SELECT * FROM Production.Product"  
      Dim productAdapter As _  
          New SqlDataAdapter(productSQL, awConnection)  
  
      Dim awDataSet As New DataSet()  
  
      awConnection.Open()  
  
      productAdapter.Fill(awDataSet, "ProductTable")  
  
      ' Retrieve the current statistics as  
      ' a collection of values at this point  
      ' and time.  
      Dim currentStatistics As IDictionary = _  
          awConnection.RetrieveStatistics()  
  
      Console.WriteLine("Total Counters: " & _  
          currentStatistics.Count.ToString())  
      Console.WriteLine()  
  
      ' Retrieve a few individual values  
      ' related to the previous command.  
      Dim bytesReceived As Long = _  
          CLng(currentStatistics.Item("BytesReceived"))  
      Dim bytesSent As Long = _  
          CLng(currentStatistics.Item("BytesSent"))  
      Dim selectCount As Long = _  
          CLng(currentStatistics.Item("SelectCount"))  
      Dim selectRows As Long = _  
          CLng(currentStatistics.Item("SelectRows"))  
  
      Console.WriteLine("BytesReceived: " & bytesReceived.ToString())  
      Console.WriteLine("BytesSent: " & bytesSent.ToString())  
      Console.WriteLine("SelectCount: " & selectCount.ToString())  
      Console.WriteLine("SelectRows: " & selectRows.ToString())  
  
      Console.WriteLine()  
      Console.WriteLine("Press any key to continue")  
      Console.ReadLine()  
    End Using  
  
  End Sub  
  
  Function GetConnectionString() As String  
    ' To avoid storing the connection string in your code,  
    ' you can retrieve it from a configuration file.  
    Return "Data Source=localhost;Integrated Security=SSPI;" & _  
      "Initial Catalog=AdventureWorks"  
  End Function  
End Module  
```  
  
```csharp  
using System;  
using System.Collections;  
using System.Collections.Generic;  
using System.Data;  
using System.Data.SqlClient;  
  
namespace CS_Stats_Console_GetValue  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      string connectionString = GetConnectionString();  
  
      using (SqlConnection awConnection =
        new SqlConnection(connectionString))  
      {  
        // StatisticsEnabled is False by default.  
        // It must be set to True to start the
        // statistic collection process.  
        awConnection.StatisticsEnabled = true;  
  
        string productSQL = "SELECT * FROM Production.Product";  
        SqlDataAdapter productAdapter =
          new SqlDataAdapter(productSQL, awConnection);  
  
        DataSet awDataSet = new DataSet();  
  
        awConnection.Open();  
  
        productAdapter.Fill(awDataSet, "ProductTable");  
        // Retrieve the current statistics as  
        // a collection of values at this point  
        // and time.  
        IDictionary currentStatistics =  
          awConnection.RetrieveStatistics();  
  
        Console.WriteLine("Total Counters: " +  
          currentStatistics.Count.ToString());  
        Console.WriteLine();  
  
        // Retrieve a few individual values  
        // related to the previous command.  
        long bytesReceived =  
            (long) currentStatistics["BytesReceived"];  
        long bytesSent =  
            (long) currentStatistics["BytesSent"];  
        long selectCount =  
            (long) currentStatistics["SelectCount"];  
        long selectRows =  
            (long) currentStatistics["SelectRows"];  
  
        Console.WriteLine("BytesReceived: " +  
            bytesReceived.ToString());  
        Console.WriteLine("BytesSent: " +  
            bytesSent.ToString());  
        Console.WriteLine("SelectCount: " +  
            selectCount.ToString());  
        Console.WriteLine("SelectRows: " +  
            selectRows.ToString());  
  
        Console.WriteLine();  
        Console.WriteLine("Press any key to continue");  
        Console.ReadLine();  
      }  
  
    }  
    private static string GetConnectionString()  
    {  
      // To avoid storing the connection string in your code,  
      // you can retrieve it from a configuration file.  
      return "Data Source=localhost;Integrated Security=SSPI;" +
        "Initial Catalog=AdventureWorks";  
    }  
  }  
}  
```  
  
### <a name="retrieving-all-values"></a>检索所有值  

 下面的控制台应用程序显示了如何启用连接的统计信息，如何使用枚举器检索所有可用的统计信息值并将它们写入控制台窗口。  
  
> [!NOTE]
> 下面的示例使用随 SQL Server 提供的 AdventureWorks 示例数据库****。 示例代码中提供的连接字符串假定数据库已安装并且在本地计算机上可用。 根据环境需要修改连接字符串。  
  
```vb  
Option Strict On  
  
Imports System  
Imports System.Collections  
Imports System.Data  
Imports System.Data.SqlClient  
  
Module Module1  
  Sub Main()  
    Dim connectionString As String = GetConnectionString()  
  
    Using awConnection As New SqlConnection(connectionString)  
      ' StatisticsEnabled is False by default.  
      ' It must be set to True to start the
      ' statistic collection process.  
      awConnection.StatisticsEnabled = True  
  
      Dim productSQL As String = "SELECT * FROM Production.Product"  
      Dim productAdapter As _  
          New SqlDataAdapter(productSQL, awConnection)  
  
      Dim awDataSet As New DataSet()  
  
      awConnection.Open()  
  
      productAdapter.Fill(awDataSet, "ProductTable")  
  
      ' Retrieve the current statistics as  
      ' a collection of values at this point  
      ' and time.  
      Dim currentStatistics As IDictionary = _  
          awConnection.RetrieveStatistics()  
  
      Console.WriteLine("Total Counters: " & _  
          currentStatistics.Count.ToString())  
      Console.WriteLine()  
  
      Console.WriteLine("Key Name and Value")  
  
      ' Note the entries are unsorted.  
      For Each entry As DictionaryEntry In currentStatistics  
        Console.WriteLine(entry.Key.ToString() & _  
            ": " & entry.Value.ToString())  
      Next  
  
      Console.WriteLine()  
      Console.WriteLine("Press any key to continue")  
      Console.ReadLine()  
    End Using  
  
  End Sub  
  
  Function GetConnectionString() As String  
    ' To avoid storing the connection string in your code,  
    ' you can retrieve it from a configuration file.  
    Return "Data Source=localhost;Integrated Security=SSPI;" & _  
      "Initial Catalog=AdventureWorks"  
  End Function  
End Module  
```  
  
```csharp  
using System;  
using System.Collections;  
using System.Collections.Generic;  
using System.Text;  
using System.Data;  
using System.Data.SqlClient;  
  
namespace CS_Stats_Console_GetAll  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      string connectionString = GetConnectionString();  
  
      using (SqlConnection awConnection =
        new SqlConnection(connectionString))  
      {  
        // StatisticsEnabled is False by default.  
        // It must be set to True to start the
        // statistic collection process.  
        awConnection.StatisticsEnabled = true;  
  
        string productSQL = "SELECT * FROM Production.Product";  
        SqlDataAdapter productAdapter =  
            new SqlDataAdapter(productSQL, awConnection);  
  
        DataSet awDataSet = new DataSet();  
  
        awConnection.Open();  
  
        productAdapter.Fill(awDataSet, "ProductTable");  
  
        // Retrieve the current statistics as  
        // a collection of values at this point  
        // and time.  
        IDictionary currentStatistics =  
            awConnection.RetrieveStatistics();  
  
        Console.WriteLine("Total Counters: " +  
            currentStatistics.Count.ToString());  
        Console.WriteLine();  
  
        Console.WriteLine("Key Name and Value");  
  
        // Note the entries are unsorted.  
        foreach (DictionaryEntry entry in currentStatistics)  
        {  
          Console.WriteLine(entry.Key.ToString() +  
              ": " + entry.Value.ToString());  
        }  
  
        Console.WriteLine();  
        Console.WriteLine("Press any key to continue");  
        Console.ReadLine();  
      }  
  
    }  
    private static string GetConnectionString()  
    {  
      // To avoid storing the connection string in your code,  
      // you can retrieve it from a configuration file.  
      return "Data Source=localhost;Integrated Security=SSPI;" +
        "Initial Catalog=AdventureWorks";  
    }  
  }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [SQL Server 和 ADO.NET](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
