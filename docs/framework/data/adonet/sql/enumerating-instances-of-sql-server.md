---
title: 枚举 SQL Server 的实例
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ddf1c83c-9d40-45e6-b04d-9828c6cbbfdc
ms.openlocfilehash: 2966157921894356836765edee6160d8e0f3e6e0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156129"
---
# <a name="enumerating-instances-of-sql-server-adonet"></a>枚举 SQL Server 的实例 (ADO.NET)

SQL Server 允许应用程序在当前网络中查找 SQL Server 实例。 <xref:System.Data.Sql.SqlDataSourceEnumerator> 类向应用程序开发人员公开此信息，提供了包含所有可见服务器的相关信息的 <xref:System.Data.DataTable>。 此返回的表包含网络上可用服务器实例的列表（该列表与用户尝试创建新连接时提供的列表匹配），并展开“连接属性”对话框上包含所有可用服务器的下拉列表****。 显示的结果并不总是完整的。  
  
> [!NOTE]
> 与大多数 Windows 服务一样，最好以尽可能小的特权运行 SQL Browser 服务。 有关 SQL Browser 服务以及如何管理其行为的更多信息，请参见 SQL Server 联机丛书。  
  
## <a name="retrieving-an-enumerator-instance"></a>检索枚举器实例  

 若要检索包含有关可用 SQL Server 实例的信息的表，必须先使用共享/静态 <xref:System.Data.Sql.SqlDataSourceEnumerator.Instance%2A> 属性来检索枚举器：  
  
```vb  
Dim instance As System.Data.Sql.SqlDataSourceEnumerator = _  
   System.Data.Sql.SqlDataSourceEnumerator.Instance  
```  
  
```csharp  
System.Data.Sql.SqlDataSourceEnumerator instance =
   System.Data.Sql.SqlDataSourceEnumerator.Instance  
```  
  
 检索静态实例后，可以调用 <xref:System.Data.Sql.SqlDataSourceEnumerator.GetDataSources%2A> 方法，此方法返回包含可用服务器的相关信息的 <xref:System.Data.DataTable>：  
  
```vb  
Dim dataTable As System.Data.DataTable = instance.GetDataSources()  
```  
  
```csharp  
System.Data.DataTable dataTable = instance.GetDataSources();  
```  
  
 通过方法调用返回的表包含以下列，其中所有列都包含 `string` 值：  
  
|列|描述|  
|------------|-----------------|  
|**ServerName**|服务器的名称。|  
|**InstanceName**|服务器实例的名称。 如果服务器作为默认实例运行，则为空。|  
|**IsClustered**|指明服务器是否属于群集。|  
|**版本**|服务器的版本。 例如：<br /><br /> -   9.00.x (SQL Server 2005)<br />-   10.0.xx (SQL Server 2008)<br />-   10.50.x (SQL Server 2008 R2)<br />-   11.0.xx (SQL Server 2012)|  
  
## <a name="enumeration-limitations"></a>枚举限制  

 不一定会列出所有可用服务器。 此列表因超时和网络流量等因素而异。 这可能会导致两次连续调用生成不同的列表。 只有同一网络中的服务器才列出。 广播数据包通常不会遍历路由器，正因为此，可能看不到列出的服务器，但它跨调用是稳定的。  
  
 列出的服务器不一定包含 `IsClustered` 和版本等其他信息。 这取决于列表的获取方式。 通过 SQL Server 浏览器服务列出的服务器比通过 Windows 基础结构发现的服务器具有更详细的信息，后者仅列出名称。  
  
> [!NOTE]
> 仅当在完全信任的环境中运行时，服务器枚举才可用。 在部分信任的环境中运行的程序集无法使用它，即使拥有 <xref:System.Data.SqlClient.SqlClientPermission> 代码访问安全性 (CAS) 权限，也不例外。  
  
 SQL Server 通过使用名为 SQL Browser 的外部 Windows 服务来提供 <xref:System.Data.Sql.SqlDataSourceEnumerator> 的信息。 此服务默认处于启用状态，但管理员可以禁用它，让服务器实例对此类不可见。  
  
## <a name="example"></a>示例  

 以下控制台应用程序检索所有可见 SQL Server 实例的信息，并在控制台窗口中显示该信息。  
  
```vb  
Imports System.Data.Sql  
  
Module Module1  
  Sub Main()  
    ' Retrieve the enumerator instance and then the data.  
    Dim instance As SqlDataSourceEnumerator = _  
     SqlDataSourceEnumerator.Instance  
    Dim table As System.Data.DataTable = instance.GetDataSources()  
  
    ' Display the contents of the table.  
    DisplayData(table)  
  
    Console.WriteLine("Press any key to continue.")  
    Console.ReadKey()  
  End Sub  
  
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
using System.Data.Sql;  
  
class Program  
{  
  static void Main()  
  {  
    // Retrieve the enumerator instance and then the data.  
    SqlDataSourceEnumerator instance =  
      SqlDataSourceEnumerator.Instance;  
    System.Data.DataTable table = instance.GetDataSources();  
  
    // Display the contents of the table.  
    DisplayData(table);  
  
    Console.WriteLine("Press any key to continue.");  
    Console.ReadKey();  
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

- [SQL Server 和 ADO.NET](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
