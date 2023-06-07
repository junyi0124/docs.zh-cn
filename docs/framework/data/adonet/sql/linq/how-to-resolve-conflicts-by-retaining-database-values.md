---
title: 如何：通过保留数据库值解决冲突
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b475cf72-9e64-4f6e-99c1-af7737bc85ef
ms.openlocfilehash: b6f9b0308bcbf53a89ae0690ed44db0a364aef0c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191692"
---
# <a name="how-to-resolve-conflicts-by-retaining-database-values"></a>如何：通过保留数据库值解决冲突

若要先对帐预期数据库值与实际数据库值之间的差异，再尝试重新提交更改，则可以使用 <xref:System.Data.Linq.RefreshMode.OverwriteCurrentValues> 保留在数据库中找到的值。 然后会覆盖对象模型中的当前值。 有关详细信息，请参阅 [乐观 Concurrency：概述](optimistic-concurrency-overview.md)。  
  
> [!NOTE]
> 在所有情况下，都会先通过从数据库中检索更新后的数据来刷新客户端上的记录。 此操作确保了下一次更新尝试将通过相同的并发检查。  
  
## <a name="example"></a>示例  

 在本方案中，当 User1 尝试提交更改时将引发 <xref:System.Data.Linq.ChangeConflictException> 异常，原因是 User2 同时已更改了 Assistant 和 Department 列。 下表说明了这种情况。  
  
||Manager|Assistant|部门|  
|------|-------------|---------------|----------------|  
|原始数据库在被 User1 和 User2 查询时的状态。|Alfreds|Maria|Sales|  
|User1 准备提交这些更改。|Alfred||Marketing|  
|User2 已经提交了这些更改。||Mary|服务|  
  
 User1 决定通过用更新的数据库值覆盖对象模型中的当前值来解决此冲突。  
  
 User1 通过使用 <xref:System.Data.Linq.RefreshMode.OverwriteCurrentValues> 解决了此冲突后，数据库中的结果将如下表中所示：  
  
||Manager|Assistant|部门|  
|------|-------------|---------------|----------------|  
|解决冲突后的新状态。|Alfreds<br /><br /> （原始）|Mary<br /><br /> （来自 User2）|服务<br /><br /> （来自 User2）|  
  
 下面的示例代码演示了如何用数据库值覆盖对象模型中的当前值。 （未对各个成员冲突进行检查或自定义处理。）  
  
 [!code-csharp[System.Data.Linq.RefreshMode#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.refreshmode/cs/program.cs#1)]
 [!code-vb[System.Data.Linq.RefreshMode#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.refreshmode/vb/module1.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [如何：管理更改冲突](how-to-manage-change-conflicts.md)
