---
title: 如何：检索成员冲突信息
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7dd6829e-79a5-4480-9023-9e588cb0bf2e
ms.openlocfilehash: 4848ef69914f0520d2365538faea7fa064c1a15c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155804"
---
# <a name="how-to-retrieve-member-conflict-information"></a>如何：检索成员冲突信息

您可以使用 <xref:System.Data.Linq.MemberChangeConflict> 类检索有关发生冲突的各成员的信息。 在此上下文中，您可以提供任何成员的冲突的自定义处理方法。 有关详细信息，请参阅 [乐观 Concurrency：概述](optimistic-concurrency-overview.md)。  
  
## <a name="example"></a>示例  

 下面的代码循环访问 <xref:System.Data.Linq.ObjectChangeConflict> 对象。 对于每个对象，它会接着循环访问 <xref:System.Data.Linq.MemberChangeConflict> 对象。  
  
> [!NOTE]
> 请将 <xref:System.Reflection> 包含在内以提供 <xref:System.Data.Linq.MemberChangeConflict.Member%2A> 信息。  
  
 [!code-csharp[System.Data.Linq.MemberChangeConflict#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.memberchangeconflict/cs/program.cs#1)]
 [!code-vb[System.Data.Linq.MemberChangeConflict#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.memberchangeconflict/vb/module1.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [如何：管理更改冲突](how-to-manage-change-conflicts.md)
