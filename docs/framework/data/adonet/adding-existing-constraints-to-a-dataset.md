---
title: 将现有约束添加到数据集
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 307d2809-208b-4cf8-b6a9-5d16f15fc16c
ms.openlocfilehash: 267d6489d39f86fc06b35de8cf30dad74f501b0b
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2019
ms.locfileid: "72523235"
---
# <a name="adding-existing-constraints-to-a-dataset"></a>将现有约束添加到数据集

**DataAdapter**的**Fill**方法仅使用数据源中的表列和行填充 <xref:System.Data.DataSet>;尽管约束通常由数据源设置，但**Fill**方法默认情况下不会将此架构信息添加到**数据集**。 若要使用数据源中的现有主键约束信息来填充数据**集**，可以调用**dataadapter**的**FillSchema**方法，或设置**dataadapter**的**MissingSchemaAction**属性。为，则在调用**Fill**之前**AddWithKey** 。 这将确保数据**集中**的主键约束反映数据源中的主键约束。 外键约束信息不包括在内，并且必须显式创建，如[DataTable 约束](./dataset-datatable-dataview/datatable-constraints.md)中所示。  
  
在使用数据填充数据**集**之前，将架构信息添加到数据集可确保在**数据集中**的 <xref:System.Data.DataTable> 对象中包含 primary key 约束。 因此，在进行其他调用以填充**数据集**时，将使用主键列信息将数据源中的新行与每个**DataTable**中的当前行相匹配，而表中的当前数据会被数据源。 如果没有架构信息，数据源中的新行将追加到数据**集**，从而导致重复的行。  
  
> [!NOTE]
> 如果数据源中的列被标识为自动递增、 **FillSchema**方法或**MissingSchemaAction**为**AddWithKey**的**Fill**方法，则将使用**自动增量**属性创建**DataColumn**设置为 `true`。 但是，您将需要自己设置**AutoIncrementStep**和**AutoIncrementSeed**值。 有关自动递增列的详细信息，请参阅[创建自动增量列](./dataset-datatable-dataview/creating-autoincrement-columns.md)。  
  
使用**FillSchema**或将**MissingSchemaAction**设置为**AddWithKey**时，需要在数据源上进行额外的处理，以确定主键列信息。 这一额外的处理可能会降低性能。 如果主键信息在设计时已知，为了实现最佳性能，建议显式指定一个或多个主键列。 有关显式设置表的主键信息的信息，请参阅[定义主键](./dataset-datatable-dataview/defining-primary-keys.md)。
  
下面的代码示例演示如何使用**FillSchema**将架构信息添加到**数据集**：
  
```vb  
Dim custDataSet As New DataSet()  
  
custAdapter.FillSchema(custDataSet, SchemaType.Source, "Customers")  
custAdapter.Fill(custDataSet, "Customers")  
```  
  
```csharp  
var custDataSet = new DataSet();  
  
custAdapter.FillSchema(custDataSet, SchemaType.Source, "Customers");  
custAdapter.Fill(custDataSet, "Customers");  
```  
  
下面的代码示例演示如何使用**Fill**方法的**MissingSchemaAction. AddWithKey**属性向**数据集**添加架构信息：
  
```vb  
Dim custDataSet As New DataSet()  
  
custAdapter.MissingSchemaAction = MissingSchemaAction.AddWithKey  
custAdapter.Fill(custDataSet, "Customers")  
```  
  
```csharp  
var custDataSet = new DataSet();  
  
custAdapter.MissingSchemaAction = MissingSchemaAction.AddWithKey;  
custAdapter.Fill(custDataSet, "Customers");  
```  
  
## <a name="handling-multiple-result-sets"></a>处理多个结果集  

如果**DataAdapter**遇到**SelectCommand**返回的多个结果集，它将在**数据集中**创建多个表。 表中会提供**表** *N*的从零开始的增量默认名称，以**表**而不是 "Table0" 开头。 如果表名作为参数传递给**FillSchema**方法，则会为这些表指定从零**开始的名称（从** **tablename**开始，而不是 "TableName0 *"）。*  
  
> [!NOTE]
> 如果对返回多个结果集的命令调用**OleDbDataAdapter**对象的**FillSchema**方法，则仅返回第一个结果集的架构信息。 当使用**OleDbDataAdapter**返回多个结果集的架构信息时，建议指定**AddWithKey**的**MissingSchemaAction** ，并在调用**Fill**时获取架构信息付款方式.  
  
## <a name="see-also"></a>请参阅

- [DataAdapters 和 DataReaders](dataadapters-and-datareaders.md)
- [数据集、数据表和数据视图](./dataset-datatable-dataview/index.md)
- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [ADO.NET 概述](ado-net-overview.md)
