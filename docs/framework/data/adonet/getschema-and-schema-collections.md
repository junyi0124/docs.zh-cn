---
title: GetSchema 和架构集合
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7ab93b89-1221-427c-84ad-04803b3c64b4
ms.openlocfilehash: cea9deb7fe019fea189a87fc08468d010929db9a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177444"
---
# <a name="getschema-and-schema-collections"></a>GetSchema 和架构集合

每个 .NET Framework 托管提供程序中的 **连接** 类实现了一个 **GetSchema** 方法，该方法用于检索有关当前已连接的数据库的架构信息，并且从 **GetSchema** 方法返回的架构信息采用形式 <xref:System.Data.DataTable> 。 **GetSchema**方法是一种重载方法，该方法提供了用于指定要返回的架构集合以及限制返回的信息量的可选参数。  
  
## <a name="specifying-the-schema-collections"></a>指定架构集合  

 **GetSchema**方法的第一个可选参数是以字符串形式指定的集合名称。 有两种类型的架构集合：所有提供程序通用的通用架构集合以及每个提供程序特定的特定架构集合。  
  
 可以通过调用不带任何参数的 **GetSchema** 方法或使用架构集合名称 "MetaDataCollections"，查询 .NET Framework 托管提供程序以确定支持的架构集合列表。 此时将返回 <xref:System.Data.DataTable>，包含支持的架构集合列表、每个架构集合支持的限制数以及所使用的标识符部分数。  
  
### <a name="retrieving-schema-collections-example"></a>检索架构集合示例  

 下面的示例演示如何使用 <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A> SQL Server 类的 .NET Framework 数据提供程序的方法 <xref:System.Data.SqlClient.SqlConnection> 来检索与 **AdventureWorks** 示例数据库中包含的所有表有关的架构信息：  
  
```vb  
Imports System.Data.SqlClient  
  
Module Module1  
   Sub Main()  
      Dim connectionString As String = GetConnectionString()  
      Using connection As New SqlConnection(connectionString)  
         'Connect to the database then retrieve the schema information.  
         connection.Open()  
         Dim table As DataTable = connection.GetSchema("Tables")  
  
         ' Display the contents of the table.  
         DisplayData(table)  
         Console.WriteLine("Press any key to continue.")  
         Console.ReadKey()  
      End Using  
   End Sub  
  
   Private Function GetConnectionString() As String  
      ' To avoid storing the connection string in your code,
      ' you can retrieve it from a configuration file.  
      Return "Data Source=(local);Database=AdventureWorks;" _  
         & "Integrated Security=true;"  
   End Function  
  
   Private Sub DisplayData(ByVal table As DataTable)  
      For Each row As DataRow In table.Rows  
         For Each col As DataColumn In table.Columns  
            Console.WriteLine("{0} = {1}", col.ColumnName, row(col))  
         Next  
         Console.WriteLine("============================")  
      Next  
   End Sub  
End Module  
```  
  
```csharp  
using System;  
using System.Data;  
using System.Data.SqlClient;  
  
class Program  
{  
  static void Main()  
  {  
  string connectionString = GetConnectionString();  
  using (SqlConnection connection = new SqlConnection(connectionString))  
  {  
   // Connect to the database then retrieve the schema information.  
   connection.Open();  
   DataTable table = connection.GetSchema("Tables");  
  
   // Display the contents of the table.  
   DisplayData(table);  
   Console.WriteLine("Press any key to continue.");  
   Console.ReadKey();  
   }  
 }  
  
  private static string GetConnectionString()  
  {  
   // To avoid storing the connection string in your code,  
   // you can retrieve it from a configuration file.  
   return "Data Source=(local);Database=AdventureWorks;" +  
      "Integrated Security=true;";  
  }  
  
  private static void DisplayData(System.Data.DataTable table)  
  {  
     foreach (System.Data.DataRow row in table.Rows)  
     {  
        foreach (System.Data.DataColumn col in table.Columns)  
        {  
           Console.WriteLine("{0} = {1}", col.ColumnName, row[col]);  
        }  
     Console.WriteLine("============================");  
     }  
  }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [检索数据库架构信息](retrieving-database-schema-information.md)
- [ADO.NET 概述](ado-net-overview.md)
