---
title: 如何：加载分页结果（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, deferred content
- WCF Data Services, loading data
ms.assetid: bb786ea4-f3ef-4ad3-9a41-3a0b7feb6a1f
ms.openlocfilehash: c49e0b170743332d6cc3aaef7e5503eabab52d97
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91182787"
---
# <a name="how-to-load-paged-results-wcf-data-services"></a>如何：加载分页结果（WCF 数据服务）

WCF 数据服务允许数据服务限制单个响应源中返回的实体数。 在此情况下，源中的最后一项包含指向下一页数据的链接。 通过调用执行 <xref:System.Data.Services.Client.QueryOperationResponse%601.GetContinuation%2A> 时返回的 <xref:System.Data.Services.Client.QueryOperationResponse%601> 的 <xref:System.Data.Services.Client.DataServiceQuery%601> 方法可以获取下一页数据的 URI。 然后，可以使用此对象所表示的 URI 加载下一页结果。 有关详细信息，请参阅 [加载延迟的内容](loading-deferred-content-wcf-data-services.md)。  
  
 本主题中的示例使用罗斯文示例数据服务和自动生成的客户端数据服务类。 此服务和客户端数据类是在完成 [WCF 数据服务快速入门](quickstart-wcf-data-services.md)时创建的。  
  
## <a name="example"></a>示例  

 下面的示例使用 `do…while` 循环从数据服务的分页结果中加载 `Customers` 实体。  
  
 [!code-csharp[Astoria Northwind Client#GetCustomersPaged](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getcustomerspaged)]
 [!code-vb[Astoria Northwind Client#GetCustomersPaged](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getcustomerspaged)]  
  
## <a name="example"></a>示例  

 下面的示例返回每个 `Orders` 实体的相关 `Customers` 实体，并使用 `do…while` 循环从数据服务中加载 `Customers` 实体页以及使用嵌套的 `while` 循环从数据服务中加载相关的 `Orders` 实体的页。  
  
 [!code-csharp[Astoria Northwind Client#GetCustomersPagedNested](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getcustomerspagednested)]
 [!code-vb[Astoria Northwind Client#GetCustomersPagedNested](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getcustomerspagednested)]  
  
## <a name="see-also"></a>请参阅

- [加载延迟的内容](loading-deferred-content-wcf-data-services.md)
- [如何：加载相关实体](how-to-load-related-entities-wcf-data-services.md)
