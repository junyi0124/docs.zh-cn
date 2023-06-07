---
title: 数据服务提供程序（WCF 数据服务）
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, providers
ms.assetid: a0160b1b-3d9c-4cc8-8391-cb0986a60a41
ms.openlocfilehash: dcca6023349148ac7f65f97028d876514ef31163
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91182830"
---
# <a name="data-services-providers-wcf-data-services"></a>数据服务提供程序（WCF 数据服务）

WCF 数据服务支持将数据作为 Open Data Protocol (OData) 源的多个提供程序模型。 本主题提供的信息使您能够为您的数据源选择最佳 WCF 数据服务提供程序。  
  
## <a name="data-source-providers"></a>数据源提供程序  

 WCF 数据服务支持以下提供程序，用于定义数据服务的数据模型。  
  
|提供程序|描述|  
|--------------|-----------------|  
|实体框架提供程序|该提供程序使用 ADO.NET 实体框架，使您能够通过定义映射到关系数据的数据模型，将关系数据与数据服务一起使用。 数据源可以为 SQL Server 或任何其他通过第三方提供程序支持实体框架的数据源。 如果您拥有关系数据源，例如 SQL Server 数据库，则应使用实体框架提供程序。 有关详细信息，请参阅 [实体框架提供程序](entity-framework-provider-wcf-data-services.md)。|  
|反射提供程序|该提供程序使用反射，使您能够基于可以作为 <xref:System.Linq.IQueryable%601> 接口的实例公开的现有数据类定义数据模型。 更新通过实现 <xref:System.Data.Services.IUpdatable> 接口而启用。 如果拥有在运行时定义的静态数据类，应使用该提供程序，例如由 LINQ to SQL 生成的提供程序或由类型化 DataSet 定义的提供程序。 有关详细信息，请参阅 [反射提供程序](reflection-provider-wcf-data-services.md)。|  
|自定义数据服务提供程序|WCF 数据服务包括一组提供程序，使您能够基于后期绑定数据类型动态定义数据模型。 当要公开的数据未知时、当正在设计应用程序时或当实体框架或反射提供程序不足时，应实现这些接口。 有关详细信息，请参阅 [自定义数据服务提供程序](custom-data-service-providers-wcf-data-services.md)。|  
  
## <a name="other-data-service-providers"></a>其他数据服务提供程序  

 WCF 数据服务具有以下额外的数据服务提供程序，该提供程序可增强使用其他某个提供程序定义的数据源的性能。  
  
|提供程序|描述|  
|--------------|-----------------|  
|流提供程序|此提供程序使你能够通过使用 WCF 数据服务公开二进制大型对象数据类型。 流提供程序通过实现 <xref:System.Data.Services.Providers.IDataServiceStreamProvider> 接口而创建。 此提供程序可与任一数据源提供程序一起实现。 有关详细信息，请参阅 [流式处理提供程序](streaming-provider-wcf-data-services.md)。|  
  
## <a name="see-also"></a>请参阅

- [定义 WCF 数据服务](defining-wcf-data-services.md)
- [配置数据服务](configuring-the-data-service-wcf-data-services.md)
- [承载数据服务](hosting-the-data-service-wcf-data-services.md)
