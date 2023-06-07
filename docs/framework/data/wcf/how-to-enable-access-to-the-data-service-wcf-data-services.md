---
title: 如何：启用对数据服务的访问（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, configuring
ms.assetid: 3d830bcd-32b4-4f26-9287-d58a071452c6
ms.openlocfilehash: 62622a5788a735497a6869c114c572e947067449
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155414"
---
# <a name="how-to-enable-access-to-the-data-service-wcf-data-services"></a>如何：启用对数据服务的访问（WCF 数据服务）

在 WCF 数据服务中，必须显式授予对数据服务公开的资源的访问权限。 这意味着您在创建新的数据服务之后，仍必须显式提供对实体集形式的各个资源的访问。 本主题演示如何启用对 Northwind 数据服务中的五个实体集的读写访问，这些实体集是在完成 [快速入门](quickstart-wcf-data-services.md)时创建的。 由于 <xref:System.Data.Services.EntitySetRights> 枚举是通过使用 <xref:System.FlagsAttribute> 定义的，因此您可以使用逻辑 OR 运算符来为单个实体集指定多个权限。  
  
> [!NOTE]
> 任何可以访问该 ASP.NET 应用程序的客户端也能够访问由数据服务公开的资源。 在生产数据服务中，为防止对资源进行未经授权的访问，还应保护应用程序本身的安全。 有关详细信息，请参阅 [保护 ASP.NET 网站](/previous-versions/aspnet/91f66yxt(v=vs.100))。  
  
### <a name="to-enable-access-to-the-data-service"></a>启用对数据服务的访问  
  
- 在数据服务的代码中，用下列代码替换 `InitializeService` 函数中的占位符代码：  
  
     [!code-csharp[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#allreadconfig)]
     [!code-vb[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#allreadconfig)]  
  
     这使得客户端能够对 `Orders` 和 `Order_Details` 实体集进行读写访问，以及对 `Customers` 实体集进行只读访问。  
  
## <a name="see-also"></a>请参阅

- [如何：开发在 IIS 上运行的 WCF 数据服务](how-to-develop-a-wcf-data-service-running-on-iis.md)
- [配置数据服务](configuring-the-data-service-wcf-data-services.md)
