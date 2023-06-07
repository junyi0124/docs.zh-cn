---
title: 如何：查询信息
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e538d288-2070-40ca-9da6-4fbc68cd6ad0
ms.openlocfilehash: d45fdfa8b8976e3cdc86b905443bf7bcea578714
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166399"
---
# <a name="how-to-query-for-information"></a>如何：查询信息

中 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 的查询使用的语法与 LINQ 中的查询相同。 唯一的区别是查询中引用的对象 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 映射到数据库中的元素。 有关详细信息，请参阅 [LINQ 查询简介 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 将您编写的查询转换成等效的 SQL 查询，然后将它们发送至服务器进行处理。  
  
 LINQ 查询的某些功能在应用程序中可能需要特别注意 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 。 有关详细信息，请参阅 [查询概念](query-concepts.md)。  
  
## <a name="example"></a>示例  

 下面的查询请求来自伦敦的客户的列表。 在此示例中，`Customers` 是 Northwind 示例数据库中的表。  
  
 [!code-csharp[DLinqQuerying#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#1)]
 [!code-vb[DLinqQuerying#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [创建对象模型](creating-the-object-model.md)
- [下载示例数据库](downloading-sample-databases.md)
- [查询数据库](querying-the-database.md)
