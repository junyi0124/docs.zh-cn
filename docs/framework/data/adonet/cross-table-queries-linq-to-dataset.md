---
title: 跨表查询 (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 6819a16f-8656-41af-a54d-dfec0cb66366
ms.openlocfilehash: a209cfe4142ad8ebdbce1d715a76ac27300f4e19
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202391"
---
# <a name="cross-table-queries-linq-to-dataset"></a>跨表查询 (LINQ to DataSet)

除了查询单个表外，还可以在 LINQ to DataSet 中执行交叉表查询。 这是通过使用 *联接*来完成的。 联接就是将一个数据源中的对象与另一个数据源中具有相同公共属性的对象（例如产品或联系人 ID）相关联。 在面向对象的编程中，由于每个对象都有引用另一个对象的成员，所以对象间的关系相对较容易导航。 但在外部数据库表中，导航关系不像这样简单。 数据库表不包含内置关系。 在这些情况下，可以通过联接操作来匹配每个源中的元素。 例如，假设有两个分别包含产品信息和销售信息的表，您可以使用联接操作来匹配同一销售订单的销售信息和产品。  
  
  (LINQ) 框架的语言集成查询提供两个联接运算符： <xref:System.Linq.Enumerable.Join%2A> 和 <xref:System.Linq.Enumerable.GroupJoin%2A> 。 这些运算符执行 *同等联接*：也就是说，仅当键相等时才匹配两个数据源的联接。  (相反，Transact-sql 支持除之外的联接运算符 `equals` ，例如 `less than` 运算符。 )   
  
 对于关系数据库，<xref:System.Linq.Enumerable.Join%2A> 实现内部联接。 内部联接是仅返回在相对数据集中具有匹配对象的那些对象的一种联接类型。  
  
 <xref:System.Linq.Enumerable.GroupJoin%2A>运算符在关系数据库术语中没有直接等效项; 它们实现了内部联接和左外部联接的超集。 左外部联接是一种联接，它返回第一个 (左) 集合的每个元素，即使它在第二个集合中没有关联元素。  
  
 有关联接的详细信息，请参阅 [联接操作](/previous-versions/visualstudio/visual-studio-2013/bb397908(v=vs.120))。  
  
## <a name="example"></a>示例  

 下面的示例对 AdventureWorks 示例数据库中的 `SalesOrderHeader` 和 `SalesOrderDetail` 表执行传统联接以获取 8 月份以来的在线订单。  
  
 [!code-csharp[DP LINQ to DataSet Examples#Join](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#join)]
 [!code-vb[DP LINQ to DataSet Examples#Join](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#join)]  
  
## <a name="see-also"></a>请参阅

- [查询数据集](querying-datasets-linq-to-dataset.md)
- [单表查询](single-table-queries-linq-to-dataset.md)
- [查询类型化数据集](querying-typed-datasets.md)
- [联接运算](/previous-versions/visualstudio/visual-studio-2013/bb397908(v=vs.120))
- [LINQ to DataSet 示例](linq-to-dataset-examples.md)
