---
title: 架构限制
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 73d2980e-e73c-4987-913a-8ddc93d09144
ms.openlocfilehash: c0a3cafef45341cd95fa0a4f65c818129e120e44
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91147819"
---
# <a name="schema-restrictions"></a>架构限制

**GetSchema**方法的第二个可选参数是用于限制返回的架构信息量的限制，并将其作为字符串数组传递到**GetSchema**方法。 在数组中的位置确定可以传递的值，这等效于限制数。  
  
 例如，下表说明使用适用于 SQL Server 的 .NET Framework 数据提供程序时“Tables”架构集合支持的限制。 SQL Server 架构集合的其他限制在本主题的结尾处列出。  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
## <a name="specifying-restriction-values"></a>指定限制值  

 要使用“Tables”架构集合的一个限制，只需创建一个包含四个元素的字符串数组，然后在与限制数匹配的元素中填充值。 例如，若要将 **GetSchema** 方法返回的表仅限制为 "sales" 架构中的表，请先将数组的第二个元素设置为 "sales"，然后再将其传递给 **GetSchema** 方法。  
  
> [!NOTE]
> `SqlClient` 和 `OracleClient` 的限制集合还有附加的 `ParameterName` 列。 为了向后兼容，仍提供限制默认列，但是目前忽略该列。 在指定限制值时，应使用参数化查询（而不是字符串替换）来最大程度地降低受到 SQL 注入式攻击的风险。  
  
> [!NOTE]
> 数组中的元素数必须小于或等于指定架构集合支持的限制数，否则，将引发 <xref:System.ArgumentException>。 可以小于最大限制数。 缺少的限制假定为空（无限制）。  
  
 可以通过将 **GetSchema** 方法与限制架构集合的名称（即 "限制"）一起调用，来 .NET Framework 查询受支持的限制列表。 此时将返回 <xref:System.Data.DataTable>，包含集合名称、限制名称、默认限制值和限制数的列表。  
  
### <a name="example"></a>示例  

 下面的示例演示如何使用 <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A> SQL Server 类的 .NET Framework 数据提供程序的方法 <xref:System.Data.SqlClient.SqlConnection> 来检索与 **AdventureWorks** 示例数据库中包含的所有表有关的架构信息，并将返回的信息仅限于 "Sales" 架构中的表：  
  
```vb  
Imports System.Data.SqlClient  
  
Module Module1  
Sub Main()  
  Dim connectionString As String = _  
    "Data Source=(local);Database=AdventureWorks;" & _  
       "Integrated Security=true;";  
  
  Dim restrictions(3) As String  
  Using connection As New SqlConnection(connectionString)  
    connection.Open()  
  
    'Specify the restrictions.  
    restrictions(1) = "Sales"  
    Dim table As DataTable = connection.GetSchema("Tables", _  
       restrictions)  
  
    ' Display the contents of the table.  
      For Each row As DataRow In table.Rows  
         For Each col As DataColumn In table.Columns  
            Console.WriteLine("{0} = {1}", col.ColumnName, row(col))  
         Next  
         Console.WriteLine("============================")  
      Next  
    Console.WriteLine("Press any key to continue.")  
    Console.ReadKey()  
  End Using  
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
    string connectionString =
       "Data Source=(local);Database=AdventureWorks;" +  
       "Integrated Security=true;";  
    using (SqlConnection connection =  
       new SqlConnection(connectionString))  
    {  
        connection.Open();  
  
        // Specify the restrictions.  
        string[] restrictions = new string[4];  
        restrictions[1] = "Sales";  
        System.Data.DataTable table = connection.GetSchema(  
          "Tables", restrictions);  
  
        // Display the contents of the table.  
        foreach (System.Data.DataRow row in table.Rows)  
        {  
            foreach (System.Data.DataColumn col in table.Columns)  
            {  
                Console.WriteLine("{0} = {1}",
                  col.ColumnName, row[col]);  
            }  
            Console.WriteLine("============================");  
        }  
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
  
## <a name="sql-server-schema-restrictions"></a>SQL Server 架构限制  

 下表列出了 SQL Server 架构集合的限制。  
  
### <a name="users"></a>用户  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|User_Name|@Name|name|1|  
  
### <a name="databases"></a>数据库  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|名称|@Name|名称|1|  
  
### <a name="tables"></a>表  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Name|TABLE_NAME|3|  
|TableType|@TableType|TABLE_TYPE|4|  
  
### <a name="columns"></a>列  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
|列|@Column|COLUMN_NAME|4|  
  
### <a name="structuredtypemembers"></a>StructuredTypeMembers  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
|列|@Column|COLUMN_NAME|4|  
  
### <a name="views"></a>视图  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
  
### <a name="viewcolumns"></a>ViewColumns  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|VIEW_CATALOG|1|  
|所有者|@Owner|VIEW_SCHEMA|2|  
|表|@Table|VIEW_NAME|3|  
|列|@Column|COLUMN_NAME|4|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|SPECIFIC_CATALOG|1|  
|所有者|@Owner|SPECIFIC_SCHEMA|2|  
|名称|@Name|SPECIFIC_NAME|3|  
|参数|@Parameter|PARAMETER_NAME|4|  
  
### <a name="procedures"></a>过程  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|SPECIFIC_CATALOG|1|  
|所有者|@Owner|SPECIFIC_SCHEMA|2|  
|名称|@Name|SPECIFIC_NAME|3|  
|类型|@Type|ROUTINE_TYPE|4|  
  
### <a name="indexcolumns"></a>IndexColumns  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|db_name()|1|  
|所有者|@Owner|user_name()|2|  
|表|@Table|o.name|3|  
|ConstraintName|@ConstraintName|x.name|4|  
|列|@Column|c.name|5|  
  
### <a name="indexes"></a>索引  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|db_name()|1|  
|所有者|@Owner|user_name()|2|  
|表|@Table|o.name|3|  
  
### <a name="userdefinedtypes"></a>UserDefinedTypes  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|assembly_name|@AssemblyName|assemblies.name|1|  
|udt_name|@UDTName|types.assembly_class|2|  
  
### <a name="foreignkeys"></a>ForeignKeys  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|CONSTRAINT_CATALOG|1|  
|所有者|@Owner|CONSTRAINT_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
|名称|@Name|CONSTRAINT_NAME|4|  
  
## <a name="sql-server-2008-schema-restrictions"></a>SQL Server 2008       

 下表列出了 SQL Server 2008 架构集合的限制。 这些限制从 .NET Framework 版本 3.5 SP1 和 SQL Server 2008 开始生效。 .NET Framework 和 SQL Server 的早期版本不支持这些限制。  
  
### <a name="columnsetcolumns"></a>ColumnSetColumns  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
  
### <a name="allcolumns"></a>AllColumns  
  
|限制名称|参数名称|限制默认值|限制数|  
|----------------------|--------------------|-------------------------|------------------------|  
|目录|@Catalog|TABLE_CATALOG|1|  
|所有者|@Owner|TABLE_SCHEMA|2|  
|表|@Table|TABLE_NAME|3|  
|列|@Column|COLUMN_NAME|4|  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
