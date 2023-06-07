---
title: WCF 数据服务概述
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services
- WCF Data Services, about
ms.assetid: 7924cf94-c9a6-4015-afc9-f5d22b1743bb
ms.openlocfilehash: a1844ab3449b6bce526276eba693a1f2da0ebf6b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202170"
---
# <a name="wcf-data-services-overview"></a>WCF 数据服务概述

WCF 数据服务可以通过使用 Open Data Protocol (OData) 来创建和使用 Web 或 intranet 的数据服务。 OData 使你能够将数据公开为可通过 Uri 进行寻址的资源。 这样，您就可以通过使用具象状态传输 (REST) 的语义（尤其是标准 HTTP 谓词 GET、PUT、POST 和 DELETE）来访问和更改数据。 本主题概述 OData 定义的模式和实践，并概述 WCF 数据服务提供的工具，以利用基于 .NET Framework 的应用程序中的 OData。  
  
## <a name="address-data-as-resources"></a>以资源形式对数据进行寻址  

 OData 将数据公开为可通过 URI 进行寻址的资源。 基于实体数据模型的实体关系约定构造资源路径。 在此模型中，实体表示应用程序域中数据的操作单元，例如客户、订单、项目和产品。 有关详细信息，请参阅 [实体数据模型](../adonet/entity-data-model.md)。  
  
 在 OData 中，将实体资源作为包含实体类型实例的实体集进行寻址。 例如，URI `https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders` 返回 `Northwind` 数据服务中与值为的客户相关的所有订单 `CustomerID``ALFKI.`  
  
 利用查询表达式，可以对资源执行传统的查询操作，如筛选、排序和分页。 例如，URI `https://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders?$filter=Freight gt 50` 对资源进行筛选，以仅返回运费高于 50 美元的订单。 有关详细信息，请参阅 [访问数据服务资源](accessing-data-service-resources-wcf-data-services.md)。  
  
## <a name="interoperable-data-access"></a>可互操作的数据访问  

 OData 基于标准 Internet 协议建立，使数据服务可与不使用 .NET Framework 的应用程序进行互操作。 由于您可以使用标准 URI 对数据进行寻址，因此应用程序可以通过使用具象状态传输 (REST) 的语义（尤其是标准 HTTP 谓词 GET、PUT、POST 和 DELETE）来访问和更改数据。 这样您就可以从任何可分析和访问通过标准 HTTP 协议传输的数据的客户端访问这些服务。  
  
OData)  (AtomPub 定义一组对 Atom 发布协议的扩展。 它支持采用多种数据格式的 HTTP 请求和响应，以适应各种客户端应用程序和平台。 OData 数据源可以表示 Atom 中的数据，JavaScript 对象表示法 (JSON) ，以及纯 XML 格式。 尽管 Atom 是默认格式，但源的格式会在 HTTP 请求的标头中指定。 有关详细信息，请参阅 [odata： Atom 格式](https://www.odata.org/documentation/odata-version-2-0/atom-format/) 和 [Odata： JSON 格式](https://www.odata.org/documentation/odata-version-2-0/json-format/)。  
  
 将数据作为 OData 源发布时，WCF 数据服务依赖于其他现有的 Internet 设备进行缓存和身份验证等操作。 为实现此目的，WCF 数据服务与现有的宿主应用程序和服务（如 ASP.NET、Windows Communication Foundation (WCF) ）和 Internet Information Services (IIS) 集成。  
  
## <a name="storage-independence"></a>存储独立性  

 尽管资源基于实体关系模型进行寻址，但 WCF 数据服务公开 OData 源，而不考虑基础数据源。 WCF 数据服务接受 URI 标识的资源的 HTTP 请求后，将对该请求进行反序列化，并将该请求的表示形式传递给 WCF 数据服务提供程序。 此提供程序将该请求转换为特定于数据源的格式，并在基础数据源上执行该请求。 WCF 数据服务通过将用于处理由 OData 指定的资源的概念模型与基础数据源的特定架构进行分离，实现存储独立性。  
  
 WCF 数据服务与 ADO.NET 实体框架集成，使您能够创建公开关系数据的数据服务。 可以使用实体数据模型工具创建包含以实体形式存在的可寻址资源的数据模型，同时定义此模型与基础数据库中表之间的映射。 有关详细信息，请参阅 [实体框架提供程序](entity-framework-provider-wcf-data-services.md)。  
  
 WCF 数据服务还可以创建数据服务，这些服务公开返回接口实现的任何数据结构 <xref:System.Linq.IQueryable%601> 。 这样您就可以创建公开 .NET Framework 类型中数据的数据服务。 当您也实现 <xref:System.Data.Services.IUpdatable> 接口时，支持创建、更新和删除操作。 有关详细信息，请参阅 [反射提供程序](reflection-provider-wcf-data-services.md)。  
  
 有关 WCF 数据服务如何与这些数据访问接口集成的说明，请参阅本主题后面的体系结构关系图。  
  
## <a name="custom-business-logic"></a>自定义业务逻辑  

 通过使用 WCF 数据服务，可以轻松地通过服务操作和侦听器将自定义业务逻辑添加到数据服务。 服务操作是在可通过 URI（采用与数据资源相同的形式）进行寻址的服务器上定义的方法。 服务操作还可以使用查询表达式语法对操作返回的数据进行筛选、排序和分页。 例如，URI `http://localhost:12345/Northwind.svc/GetOrdersByCity?city='London'&$orderby=OrderDate&$top=10&$skip=10` 表示对 Northwind 数据服务上的名为 `GetOrdersByCity` 的服务操作的调用，此调用将返回来自伦敦的客户的订单，并且会对结果进行分页并按 `OrderDate` 排序。 有关详细信息，请参阅 [服务操作](service-operations-wcf-data-services.md)。  
  
 利用侦听器，可以将自定义应用程序逻辑集成到数据服务对请求或响应消息的处理中。 在指定的实体集上执行查询、插入、更新或删除操作时，将调用相应的侦听器。 然后，侦听器可能会更改数据、执行授权策略或者甚至终止操作。 必须为由数据服务公开的给定实体集显式注册侦听器方法。 有关详细信息，请参阅 [拦截](interceptors-wcf-data-services.md)程序。  
  
## <a name="client-libraries"></a>客户端库  

 OData 定义一组统一模式，用于与数据服务进行交互。 这为创建基于这些服务的可重用组件（如方便使用数据服务的客户端库）提供了机会。  
  
 WCF 数据服务包含用于基于 .NET Framework 的客户端和基于 Silverlight 的客户端应用程序的客户端库。 通过这些客户端库，可以使用 .NET Framework 对象与数据服务进行交互。 它们还支持基于对象的查询和 LINQ 查询、加载相关对象、更改跟踪和标识解析。 有关详细信息，请参阅 [WCF 数据服务客户端库](wcf-data-services-client-library.md)。  
  
 除了 .NET Framework 和 Silverlight 附带的 OData 客户端库外，还有其他客户端库，使你能够在客户端应用程序中使用 OData 源，例如 PHP、AJAX 和 Java 应用程序。 有关 OData SDK 的详细信息，请参阅 [ODATA sdk-示例代码](https://www.odata.org/ecosystem/#sdk)。
  
## <a name="architecture-overview"></a>体系结构概述  

 下图演示了公开 OData 源并在支持 OData 的客户端库中使用这些源的 WCF 数据服务体系结构：  
  
 ![显示 WCF 数据服务的体系结构关系图的屏幕截图。](./media/wcf-data-services-overview/windows-communication-foundation-data-services-architecture.gif)  
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务 4.5](index.md)
- [入门](getting-started-with-wcf-data-services.md)
- [定义 WCF 数据服务](defining-wcf-data-services.md)
- [访问数据服务资源（WCF 数据服务）](/previous-versions/dotnet/netframework-4.0/dd728283(v=vs.100))
- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
- [Representational State Transfer (REST)](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)（表述性状态转移 (REST)）
