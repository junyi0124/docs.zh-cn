---
title: 如何：通过使用事务对数据提交进行分类
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 94044a31-de90-479b-935a-8159b4ae5c5a
ms.openlocfilehash: 4d3efedbf15be55fa7a9ab235f881f1c97758953
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161355"
---
# <a name="how-to-bracket-data-submissions-by-using-transactions"></a>如何：通过使用事务对数据提交进行分类

您可以使用 <xref:System.Transactions.TransactionScope> 来封闭您提交到数据库的数据。 有关详细信息，请参阅 [事务支持](transaction-support.md)。  
  
## <a name="example"></a>示例  

 下面的代码将数据库提交数据封闭在 <xref:System.Transactions.TransactionScope> 中。  
  
 [!code-csharp[DLinqSubmittingChanges#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSubmittingChanges/cs/Program.cs#3)]
 [!code-vb[DLinqSubmittingChanges#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSubmittingChanges/vb/Module1.vb#3)]  
  
## <a name="see-also"></a>请参阅

- [下载示例数据库](downloading-sample-databases.md)
- [进行和提交数据更改](making-and-submitting-data-changes.md)
- [事务支持](transaction-support.md)
