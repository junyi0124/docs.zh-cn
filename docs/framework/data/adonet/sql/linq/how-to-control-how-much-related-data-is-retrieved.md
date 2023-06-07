---
title: 如何：控制检索的相关数据量
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: efdc203e-3da9-4477-815e-54f10c3d7c6c
ms.openlocfilehash: 77ac29eeeaa30ef438b635364dc8e883a0e4c158
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161329"
---
# <a name="how-to-control-how-much-related-data-is-retrieved"></a>如何：控制检索的相关数据量

使用 <xref:System.Data.Linq.DataLoadOptions.LoadWith%2A> 方法可指定应同时检索与主目标相关的哪些数据。 例如，如果您了解将需要有关客户订单的信息，则可以使用 <xref:System.Data.Linq.DataLoadOptions.LoadWith%2A> 来确保在检索客户信息时会检索订单信息。 这种方法的结果是只检索一次数据库即可获取这两组信息。  
  
> [!NOTE]
> 通过将叉积作为一个大型投影检索，可以检索与您的查询主目标相关的数据，例如在以客户为目标时检索订单。 但这种方法往往存在弊端。 例如，所得结果仅仅为投影，而不是可以由 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 更改和持久化的实体。 此外，您可能会检索到您并不需要的大量数据。  
  
## <a name="example"></a>示例  

 在下面的示例中，在执行查询时会检索到位于伦敦的所有 `Orders` 所下的所有 `Customers`。 这样一来，连续访问 `Orders` 对象的 `Customer` 属性不会触发新的数据库查询。  
  
 [!code-csharp[System.Data.Linq.DataLoadOptions#2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/system.data.linq.dataloadoptions/cs/program.cs#2)]
 [!code-vb[System.Data.Linq.DataLoadOptions#2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/system.data.linq.dataloadoptions/vb/module1.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [查询数据库](querying-the-database.md)
