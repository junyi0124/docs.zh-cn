---
title: 加载方法
ms.date: 03/30/2017
dev_langs:
- vb
ms.assetid: e22e5812-89c6-41f0-9302-bb899a46dbff
ms.openlocfilehash: ea437d1f8ed567934acafbd8db1f8dba8eb22bcc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177535"
---
# <a name="the-load-method"></a>加载方法

可以使用 <xref:System.Data.DataTable.Load%2A> 方法为 <xref:System.Data.DataTable> 加载数据源中行。 这是一种重载方法，其最简单的形式是接受一个参数（ **DataReader**）。 在此窗体中，它只是加载包含行的 **DataTable** 。 还可以指定 **LoadOption** 参数，以控制数据添加到 **DataTable**的方式。  
  
 当**DataTable**已经包含数据行时， **LoadOption**参数特别有用，因为它描述了如何将数据源中的传入数据与表中已有的数据组合在一起。 例如， **PreserveCurrentValues** (默认的) 指定在将行标记为 **添加** 到 **DataTable**的情况下，将 **原始** 值或每一列设置为数据源中匹配行的内容。 **当前**值将保留在添加行时分配的值，该行的**RowState**将设置为 "**已更改**"。  
  
 下表给出了对 <xref:System.Data.LoadOption> 枚举值的简要说明。  
  
|LoadOption 值|描述|  
|----------------------|-----------------|  
|**OverwriteRow**|如果传入行的 **PrimaryKey** 值与 **DataTable**中已有行的值相同，则每列的 **原始** 值和 **当前** 值将替换为传入行中的值，并且 **RowState** 属性将设置为 " **未更改**"。<br /><br /> 使用不**变**的**RowState**值添加了数据源中不存在于**DataTable**中的行。<br /><br /> 此选项实际上会刷新 **DataTable** 的内容，使其与数据源的内容相匹配。|  
|**PreserveCurrentValues（默认值）**|如果传入行的 **PrimaryKey** 值与 **DataTable**中已有行的值相同，则将 **原始** 值设置为传入行的内容，并且不会更改 **当前** 值。<br /><br /> 如果**添加**或**修改**了**RowState** ，则将其设置为 "**已修改**"。<br /><br /> 如果**删除**了**RowState** ，则它将保持**删除**。<br /><br /> 添加了数据源中不存在于 **DataTable** 中的行，并将 **RowState** 设置为 " **未更改**"。|  
|**UpdateCurrentValues**|如果传入行的 **PrimaryKey** 值与 **DataTable**中已有行的值相同，则将 **当前** 值复制到 **原始** 值，然后将 **当前** 值设置为传入行的内容。<br /><br /> 如果**添加**了**DataTable**中的**RowState** ，则**RowState**将继续**添加**。 对于标记为 "**已修改**" 或 "**已删除**" 的行，将**修改** **RowState** 。<br /><br /> 添加了数据源中不存在于 **DataTable** 中的行，并将 **RowState** 设置为 "已 **添加**"。|  
  
 下面的示例使用 **Load** 方法为 **Northwind** 数据库中的员工显示生日列表。  
  
```vb  
Private Sub LoadBirthdays(ByVal connectionString As String)  
    ' Assumes that connectionString is a valid connection string  
    ' to the Northwind database on SQL Server.  
    Dim queryString As String = _  
    "SELECT LastName, FirstName, BirthDate " & _  
      " FROM dbo.Employees " & _  
      "ORDER BY BirthDate, LastName, FirstName"  
  
    ' Open and fill a DataSet.
    Dim adapter As SqlDataAdapter = New SqlDataAdapter( _  
        queryString, connectionString)  
    Dim employees As New DataSet  
    adapter.Fill(employees, "Employees")  
  
    ' Create a SqlDataReader for use with the Load Method.  
    Dim reader As DataTableReader = employees.GetDataReader()  
  
    ' Create an instance of DataTable and assign the first  
    ' DataTable in the DataSet.Tables collection to it.  
    Dim dataTableEmp As DataTable = employees.Tables(0)  
  
    ' Fill the DataTable with data by calling Load and  
    ' passing the SqlDataReader.  
    dataTableEmp.Load(reader, LoadOption.OverwriteRow)  
  
    ' Loop through the rows collection and display the values  
    ' in the console window.  
    Dim employeeRow As DataRow  
    For Each employeeRow In dataTableEmp.Rows  
        Console.WriteLine("{0:MM\\dd\\yyyy}" & ControlChars.Tab & _  
          "{1}, {2}", _  
          employeeRow("BirthDate"), _  
          employeeRow("LastName"), _  
          employeeRow("FirstName"))  
    Next employeeRow  
  
    ' Keep the window opened to view the contents.  
    Console.ReadLine()  
End Sub  
```  
  
## <a name="see-also"></a>请参阅

- [操作数据表中的数据](manipulating-data-in-a-datatable.md)
- [ADO.NET 概述](../ado-net-overview.md)
