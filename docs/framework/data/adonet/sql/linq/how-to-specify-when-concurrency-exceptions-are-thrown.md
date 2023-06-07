---
title: 如何：指定何时引发并发异常
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 344ae068-ff63-4a2e-8b00-af22e143675f
ms.openlocfilehash: 9d71ca82c3fe1abd23a82d952d38c3ea23a7f1c0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197100"
---
# <a name="how-to-specify-when-concurrency-exceptions-are-thrown"></a>如何：指定何时引发并发异常

在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中，当因出现开放式并发冲突而导致对象不能更新时，会引发 <xref:System.Data.Linq.ChangeConflictException> 异常。 有关详细信息，请参阅 [乐观 Concurrency：概述](optimistic-concurrency-overview.md)。  
  
 在向数据库提交您所做的更改前，您可以指定应何时引发并发异常：  
  
- 第一次失败时引发异常 (<xref:System.Data.Linq.ConflictMode.FailOnFirstConflict>)。  
  
- 完成所有更新尝试，积累所有失败，然后在异常中报告积累的失败 (<xref:System.Data.Linq.ConflictMode.ContinueOnConflict>)。  
  
 引发 <xref:System.Data.Linq.ChangeConflictException> 异常时，该异常会提供对 <xref:System.Data.Linq.ChangeConflictCollection> 集合的访问。 此集合提供了有关每个冲突（映射到单个失败的更新尝试）的详细信息，包括对 <xref:System.Data.Linq.ObjectChangeConflict.MemberConflicts%2A> 集合的访问。 每个成员冲突映射到未通过并发检查的更新中的单个成员。  
  
## <a name="example"></a>示例  

 下面的代码显示了这两个值的示例。  
  
 [!code-csharp[System.Data.Linq.ConflictModeEnumeration#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.conflictmodeenumeration/cs/program.cs#1)]
 [!code-vb[System.Data.Linq.ConflictModeEnumeration#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.conflictmodeenumeration/vb/module1.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [如何：管理更改冲突](how-to-manage-change-conflicts.md)
- [进行和提交数据更改](making-and-submitting-data-changes.md)
