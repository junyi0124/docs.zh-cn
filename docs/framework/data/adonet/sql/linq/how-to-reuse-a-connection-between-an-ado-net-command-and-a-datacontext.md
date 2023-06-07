---
title: 如何：重复使用 ADO.NET 命令和 DataContext 之间的连接
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7e26c7eb-c18a-43b5-a8f0-28fd8b04b0f0
ms.openlocfilehash: 89c9a12399d3d76487d1fdc2bd82aa037c167710
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197347"
---
# <a name="how-to-reuse-a-connection-between-an-adonet-command-and-a-datacontext"></a>如何：重复使用 ADO.NET 命令和 DataContext 之间的连接

由于 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 是 ADO.NET 系列技术的一部分，并且基于 ADO.NET 提供的服务，因此你可以重复使用 ADO.NET 命令和之间的连接 <xref:System.Data.Linq.DataContext> 。  
  
## <a name="example"></a>示例  

 下面的示例演示如何在 ADO.NET 命令和之间重复使用相同的连接 <xref:System.Data.Linq.DataContext> 。  
  
 [!code-csharp[DLinqCommunicatingWithDatabase#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqCommunicatingWithDatabase/cs/Program.cs#4)]
 [!code-vb[DLinqCommunicatingWithDatabase#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqCommunicatingWithDatabase/vb/Module1.vb#4)]  
  
## <a name="see-also"></a>请参阅

- [背景信息](background-information.md)
- [ADO.NET 和 LINQ to SQL](ado-net-and-linq-to-sql.md)
- [与数据库通信](communicating-with-the-database.md)
