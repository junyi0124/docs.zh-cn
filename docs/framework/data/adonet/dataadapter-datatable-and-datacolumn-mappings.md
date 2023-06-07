---
title: DataAdapter 数据表和 DataColumn 映射
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d023260a-a66a-4c39-b8f4-090cd130e730
ms.openlocfilehash: b979431836b55b23ac9ba6ec4535f33765dce555
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91177730"
---
# <a name="dataadapter-datatable-and-datacolumn-mappings"></a>DataAdapter 数据表和 DataColumn 映射

**DataAdapter** <xref:System.Data.Common.DataTableMapping> 在其 **TableMappings** 属性中包含零个或多个对象的集合。 **DataTableMapping** 提供了针对数据源的查询返回的数据与之间的主映射 <xref:System.Data.DataTable> 。 可以将 **DataTableMapping** 名称替换为将 **DataTable** 名称替换为 **DataAdapter** 的 **Fill** 方法。 下面的示例为 **Authors** 表创建名为 **AuthorsMapping** 的 **DataTableMapping** 。  
  
```vb  
workAdapter.TableMappings.Add("AuthorsMapping", "Authors")  
```  
  
```csharp  
workAdapter.TableMappings.Add("AuthorsMapping", "Authors");  
```  
  
 使用 **DataTableMapping** 可以在 **DataTable** 中使用与数据库中的列名不同的列名。 在更新表时， **DataAdapter** 使用映射来匹配列。  
  
 如果在调用 **dataadapter** 的 **Fill** 或 **Update** 方法时未指定 **TableName** 或 **DataTableMapping** 名称，则 **dataadapter** 将查找名为 "Table" 的 **DataTableMapping** 。 如果该 **DataTableMapping** 不存在，则 **DataTable** 的 **TableName** 为 "Table"。 可以通过创建名称为 "Table" 的 **DataTableMapping** 来指定默认 **DataTableMapping** 。  
  
 下面的代码示例从命名空间) 创建 **DataTableMapping** (<xref:System.Data.Common> ，并通过将其命名为 "Table" 使其成为指定 **DataAdapter** 的默认映射。 然后，该示例将查询结果中第一个表的列映射 (**northwind**) 数据库的 **Customers** 表中，将其映射到中 **northwind Customers** 表中的一组更易于用户使用的名称 <xref:System.Data.DataSet> 。 对于未映射的列，将使用数据源中的列名称。  
  
```vb  
Dim mapping As DataTableMapping = _  
  adapter.TableMappings.Add("Table", "NorthwindCustomers")  
mapping.ColumnMappings.Add("CompanyName", "Company")  
mapping.ColumnMappings.Add("ContactName", "Contact")  
mapping.ColumnMappings.Add("PostalCode", "ZIPCode")  
  
adapter.Fill(custDS)  
```  
  
```csharp  
DataTableMapping mapping =
  adapter.TableMappings.Add("Table", "NorthwindCustomers");  
mapping.ColumnMappings.Add("CompanyName", "Company");  
mapping.ColumnMappings.Add("ContactName", "Contact");  
mapping.ColumnMappings.Add("PostalCode", "ZIPCode");  
  
adapter.Fill(custDS);  
```  
  
 在更高级的情况下，你可能会决定需要相同的 **DataAdapter** 来支持加载具有不同映射的不同表。 为此，只需添加其他 **DataTableMapping** 对象。  
  
 向 **Fill** 方法传递 **数据集** 的实例和 **DataTableMapping** 名称时，如果使用该名称的映射存在，则使用它;否则，将使用具有该名称的 **DataTable** 。  
  
 以下示例创建名为 **Customers** 的 **DataTableMapping** 和 **DataTable** name 为 **BizTalkSchema**。 然后，该示例将 SELECT 语句返回的行映射到 **BizTalkSchema** **DataTable**。  
  
```vb  
Dim mapping As ITableMapping = _  
  adapter.TableMappings.Add("Customers", "BizTalkSchema")  
mapping.ColumnMappings.Add("CustomerID", "ClientID")  
mapping.ColumnMappings.Add("CompanyName", "ClientName")  
mapping.ColumnMappings.Add("ContactName", "Contact")  
mapping.ColumnMappings.Add("PostalCode", "ZIP")  
  
adapter.Fill(custDS, "Customers")  
```  
  
```csharp  
ITableMapping mapping =
  adapter.TableMappings.Add("Customers", "BizTalkSchema");  
mapping.ColumnMappings.Add("CustomerID", "ClientID");  
mapping.ColumnMappings.Add("CompanyName", "ClientName");  
mapping.ColumnMappings.Add("ContactName", "Contact");  
mapping.ColumnMappings.Add("PostalCode", "ZIP");  
  
adapter.Fill(custDS, "Customers");  
```  
  
> [!NOTE]
> 如果没有为列映射提供源列名称或者没有为表映射提供源表名称，则将自动生成默认名称。 如果没有为列映射提供源列，则从 **SourceColumn1** 开始，列映射的增量默认名称为 **SourceColumn** *N* 。 如果没有为表映射提供源表名称，则从 **SourceTable1** 开始为表映射提供增量默认名称 **SourceTable** *N*。  
  
> [!NOTE]
> 建议你避免对列映射使用 **SourceColumn** *n* 的命名约定，或为表映射避免使用 **SourceTable** *n* ，因为所提供的名称可能会与 **DataTableMappingCollection** 中 **采用** 或表映射名称中的现有默认列映射名称冲突。 如果提供的名称已经存在，将引发异常。  
  
## <a name="handling-multiple-result-sets"></a>处理多个结果集  

 如果你的 **SelectCommand** 返回多个表，则 **填充** 会自动为 **数据集中** 的表生成表名称，以指定的表名称开头，并以 **TableName** *N* 开头，从 **TableName1** 开始继续。 您可以使用表映射将自动生成的表名称映射到要在 **数据集中** 为表指定的名称。 例如，对于返回两个表（ **Customers** 和 **Orders**）的 **SelectCommand** ，请发出以下调用以进行 **填充**。  
  
```vb  
adapter.Fill(customersDataSet, "Customers")  
```  

```csharp  
adapter.Fill(customersDataSet, "Customers");  
```  

 在 **数据集中** 创建两个表： **Customers** 和 **Customers1**。 可以使用表映射确保第二个表的名称为 **Orders** ，而不是 **Customers1**。 为此，请将 **Customers1** 的源表映射到 **数据集** 表 **Orders**，如以下示例中所示。  
  
```vb  
adapter.TableMappings.Add("Customers1", "Orders")  
adapter.Fill(customersDataSet, "Customers")  
```  

```csharp  
adapter.TableMappings.Add("Customers1", "Orders");  
adapter.Fill(customersDataSet, "Customers");  
```
  
## <a name="see-also"></a>另请参阅

- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [ADO.NET 概述](ado-net-overview.md)
