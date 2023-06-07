---
title: 数据表约束
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 27c9f2fd-f64d-4b4e-bbf6-1d24f47067cb
ms.openlocfilehash: 1224518a9a16f48f770b6839317b9787da97377b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153269"
---
# <a name="datatable-constraints"></a>数据表约束

为了维护数据的完整性，可以使用约束来对 <xref:System.Data.DataTable> 中的数据施加限制。 约束是应用于某列或相关各列的自动规则，它决定了某行的值以某种方式更改时的操作过程。 当 `System.Data.DataSet.EnforceConstraints` 的属性 <xref:System.Data.DataSet> 为 **true**时，将强制实施约束。 有关显示如何设置 `EnforceConstraints` 属性的代码示例，请参见 <xref:System.Data.DataSet.EnforceConstraints%2A> 参考主题。  
  
 ADO.NET 中有两种约束：<xref:System.Data.ForeignKeyConstraint> 和 <xref:System.Data.UniqueConstraint>。 默认情况下，通过将添加 <xref:System.Data.DataRelation> 到 **数据集**，可以在创建两个或多个表之间创建关系时自动创建这两个约束。 不过，您可以**createConstraints**  =  在创建关系时指定 createConstraints**false**来禁用此行为。  
  
## <a name="foreignkeyconstraint"></a>ForeignKeyConstraint  

 **ForeignKeyConstraint**强制执行有关如何传播相关表的更新和删除的规则。 例如，如果更新或删除了一个表的行中的值，并且在一个或多个相关表中也使用相同的值，则 **ForeignKeyConstraint** 将确定相关表中发生的情况。  
  
 <xref:System.Data.ForeignKeyConstraint.DeleteRule%2A>ForeignKeyConstraint 的和 <xref:System.Data.ForeignKeyConstraint.UpdateRule%2A> 属性定义**ForeignKeyConstraint**用户尝试删除或更新相关表中的行时要执行的操作。 下表描述了可用于**ForeignKeyConstraint**的**DeleteRule**和**UpdateRule**属性的不同设置。  
  
|规则设置|描述|  
|------------------|-----------------|  
|**Cascade**|删除或更新相关的行。|  
|**SetNull**|将相关行中的值设置为 **DBNull**。|  
|**SetDefault**|将相关行中的值设置为默认值。|  
|无|对相关行不执行任何操作。 这是默认设置。|  
  
 **ForeignKeyConstraint**可以限制并传播对相关列的更改。 根据为列的**ForeignKeyConstraint**设置的属性，如果**DataSet**的**EnforceConstraints**属性为**true**，则对父行执行某些操作将导致异常。 例如，如果**ForeignKeyConstraint**的**DeleteRule**属性为**None**，则如果父行具有任何子行，则不能将其删除。  
  
 您可以使用 **ForeignKeyConstraint** 构造函数在单个列之间或列的数组之间创建外键约束。 将生成的**ForeignKeyConstraint**对象传递给表的**约束**属性的**Add**方法，该属性是一个**ConstraintCollection**。 还可以将构造函数参数传递给**ConstraintCollection**的**Add**方法的几个重载，以创建**ForeignKeyConstraint**。  
  
 当创建 **ForeignKeyConstraint**时，可以将 **DeleteRule** 和 **UpdateRule** 值作为参数传递给构造函数，也可以将它们设置为属性，如以下示例所示 (其中 **DeleteRule** 值设置为 **None**) 。  
  
```vb  
Dim custOrderFK As ForeignKeyConstraint = New ForeignKeyConstraint("CustOrderFK", _  
  custDS.Tables("CustTable").Columns("CustomerID"), _  
  custDS.Tables("OrdersTable").Columns("CustomerID"))  
custOrderFK.DeleteRule = Rule.None
' Cannot delete a customer value that has associated existing orders.  
custDS.Tables("OrdersTable").Constraints.Add(custOrderFK)  
```  
  
```csharp  
ForeignKeyConstraint custOrderFK = new ForeignKeyConstraint("CustOrderFK",  
  custDS.Tables["CustTable"].Columns["CustomerID"],
  custDS.Tables["OrdersTable"].Columns["CustomerID"]);  
custOrderFK.DeleteRule = Rule.None;
// Cannot delete a customer value that has associated existing orders.  
custDS.Tables["OrdersTable"].Constraints.Add(custOrderFK);  
```  
  
### <a name="acceptrejectrule"></a>AcceptRejectRule  

 可以使用**AcceptChanges**方法接受对行的更改，或使用**DataSet**、 **DataTable**或**DataRow**的**RejectChanges**方法取消对行的更改。 当 **数据集** 包含 **ForeignKeyConstraints**时，调用 **AcceptChanges** 或 **RejectChanges** 方法会强制执行 **AcceptRejectRule**。 **ForeignKeyConstraint**的**AcceptRejectRule**属性决定了对父行调用**AcceptChanges**或**RejectChanges**时要对子行执行的操作。  
  
 下表列出了 **AcceptRejectRule**的可用设置。  
  
|规则设置|描述|  
|------------------|-----------------|  
|**Cascade**|接受或拒绝对子行的更改。|  
|无|对子行不执行任何操作。 这是默认设置。|  
  
### <a name="example"></a>示例  

 下面的示例创建一个 <xref:System.Data.ForeignKeyConstraint>，设置它的一些属性（包括 <xref:System.Data.ForeignKeyConstraint.AcceptRejectRule%2A>），并将它添加到 <xref:System.Data.ConstraintCollection> 对象的 <xref:System.Data.DataTable>。  
  
 [!code-csharp[DataWorks Data.AcceptRejectRule#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks Data.AcceptRejectRule/CS/source.cs#1)]
 [!code-vb[DataWorks Data.AcceptRejectRule#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks Data.AcceptRejectRule/VB/source.vb#1)]  
  
## <a name="uniqueconstraint"></a>UniqueConstraint  

 **UniqueConstraint**对象（可分配给**DataTable**中的一列或一组列）确保指定的一列或多列中的所有数据对于每行都是唯一的。 您可以使用 **UniqueConstraint** 构造函数创建列或列数组的唯一约束。 将生成的**UniqueConstraint**对象传递给表的**约束**属性的**Add**方法，该属性是一个**ConstraintCollection**。 还可以将构造函数参数传递给**ConstraintCollection**的**Add**方法的几个重载，以创建**UniqueConstraint**。 为一列或多列创建 **UniqueConstraint** 时，可以选择指定列或列是否为主键。  
  
 还可以通过将列的 **unique** 属性设置为 **true**，为列创建 unique 约束。 或者，将单个列的 **unique** 属性设置为 **false** 将删除任何可能存在的唯一约束。 如果将一列或多列定义为表的主键，会自动为一个或多个指定的列创建唯一的约束。 如果删除**DataTable**的**PrimaryKey**属性中的列，则会删除**UniqueConstraint** 。  
  
 下面的示例为**DataTable**的两列创建**UniqueConstraint** 。  
  
```vb  
Dim custTable As DataTable = custDS.Tables("Customers")  
Dim custUnique As UniqueConstraint = _  
    New UniqueConstraint(New DataColumn()   {custTable.Columns("CustomerID"), _  
    custTable.Columns("CompanyName")})  
custDS.Tables("Customers").Constraints.Add(custUnique)  
```  
  
```csharp  
DataTable custTable = custDS.Tables["Customers"];  
UniqueConstraint custUnique = new UniqueConstraint(new DataColumn[]
    {custTable.Columns["CustomerID"],
    custTable.Columns["CompanyName"]});  
custDS.Tables["Customers"].Constraints.Add(custUnique);  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataRelation>
- <xref:System.Data.DataTable>
- <xref:System.Data.ForeignKeyConstraint>
- <xref:System.Data.UniqueConstraint>
- [数据表架构定义](datatable-schema-definition.md)
- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
