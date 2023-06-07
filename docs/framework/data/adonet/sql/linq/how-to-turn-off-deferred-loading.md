---
title: 如何：禁用推迟加载
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1b84b852-3cad-41a7-8077-149a70d50c8b
ms.openlocfilehash: b2193e7e8bda396451274d2da96e7cb86774fd03
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91196957"
---
# <a name="how-to-turn-off-deferred-loading"></a>如何：禁用推迟加载

可以通过将 <xref:System.Data.Linq.DataContext.DeferredLoadingEnabled%2A> 设置为 `false` 来关闭延迟加载。 有关详细信息，请参阅 [延迟与立即加载](deferred-versus-immediate-loading.md)。  
  
> [!NOTE]
> 关闭对象跟踪时，也隐式地关闭了延迟加载。 有关详细信息，请参阅 [如何：将信息检索为只读](how-to-retrieve-information-as-read-only.md)。  
  
## <a name="example"></a>示例  

 下面的示例演示如何通过将 <xref:System.Data.Linq.DataContext.DeferredLoadingEnabled%2A> 设置为 `false` 来关闭延迟加载。  
  
 [!code-csharp[DLinqQuerying#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#3)]
 [!code-vb[DLinqQuerying#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#3)]  
  
## <a name="see-also"></a>请参阅

- [查询概念](query-concepts.md)
- [查询数据库](querying-the-database.md)
