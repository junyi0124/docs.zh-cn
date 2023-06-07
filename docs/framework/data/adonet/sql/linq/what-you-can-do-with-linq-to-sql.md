---
title: 使用 LINQ to SQL 可执行的操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 061d98b2-baa7-4336-8ad2-c14de8134d91
ms.openlocfilehash: a2e65cb558eec3cec21ea0efbcc66bb8c56ec7b5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163916"
---
# <a name="what-you-can-do-with-linq-to-sql"></a>使用 LINQ to SQL 可执行的操作

[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 支持您作为 SQL 开发人员所期望的所有关键功能。 您可以查询表中的信息、在表中插入信息以及更新和删除表中的信息。  
  
## <a name="selecting"></a>选择  

 选择 (*投影*) 只需编写您自己的编程语言的 LINQ 查询，然后执行该查询以检索结果。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 自行将所有必要操作转换为您所熟悉的必要 SQL 操作。 有关详细信息，请参阅 [LINQ to SQL](index.md)。  
  
 在下面的示例中，检索来自伦敦的客户的公司名称并将其显示在控制台窗口中。  
  
 [!code-csharp[DLinqGettingStarted#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqGettingStarted/cs/Program.cs#1)]
 [!code-vb[DLinqGettingStarted#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqGettingStarted/vb/Module1.vb#1)]  
  
## <a name="inserting"></a>插入  

 若要执行 SQL `Insert`，只需向您已创建的对象模型添加对象，然后对 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 调用 <xref:System.Data.Linq.DataContext>即可。  
  
 在下面的示例中，通过使用 `Customers` 向 <xref:System.Data.Linq.Table%601.InsertOnSubmit%2A>表添加了一位新客户以及有关该客户的信息。  
  
 [!code-csharp[DLinqGettingStarted#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqGettingStarted/cs/Program.cs#2)]
 [!code-vb[DLinqGettingStarted#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqGettingStarted/vb/Module1.vb#2)]  
  
## <a name="updating"></a>更新  

 若要 `Update` 某一数据库项，首先要检索该项，然后直接在对象模型中编辑它。 在修改了该对象之后，请对 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 调用 <xref:System.Data.Linq.DataContext> 以更新数据库。  
  
 在下面的示例中，检索来自伦敦的所有客户。 然后将其所在城市的名称从“London”更改为“London - Metro”。 最后，调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 以将所做的更改发送至数据库。  
  
 [!code-csharp[DLinqGettingStarted#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqGettingStarted/cs/Program.cs#3)]
 [!code-vb[DLinqGettingStarted#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqGettingStarted/vb/Module1.vb#3)]  
  
## <a name="deleting"></a>正在删除  

 若要 `Delete` 某一项，请从其所属集合中移除该项，然后对 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 调用 <xref:System.Data.Linq.DataContext> 以提交所做的更改。  
  
> [!NOTE]
> [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 无法识别级联删除操作。 如果要删除表中具有约束的行，请参阅 [如何：从数据库中删除行](how-to-delete-rows-from-the-database.md)。  
  
 在下面的示例中，从数据库中检索 `CustomerID` 为 `98128` 的客户。 然后，在确认检索到客户行之后，调用 <xref:System.Data.Linq.Table%601.DeleteOnSubmit%2A> 以将该对象从集合中移除。 最后，调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 以将删除内容转发至数据库。  
  
 [!code-csharp[DLinqGettingStarted#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqGettingStarted/cs/Program.cs#4)]
 [!code-vb[DLinqGettingStarted#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqGettingStarted/vb/Module1.vb#4)]  
  
## <a name="see-also"></a>请参阅

- [编程指南](programming-guide.md)
- [LINQ to SQL 对象模型](the-linq-to-sql-object-model.md)
- [入门](getting-started.md)
