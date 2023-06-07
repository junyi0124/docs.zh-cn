---
title: 在客户端应用程序中使用数据服务（WCF 数据服务）
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, client library
- WCF Data Services, getting started
ms.assetid: 90872d0c-e989-4490-b3e9-54afb10d33d4
ms.openlocfilehash: b921e78c1b3d3d90d9ac8b4a459df496cf6d14dd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180577"
---
# <a name="using-a-data-service-in-a-client-application-wcf-data-services"></a>在客户端应用程序中使用数据服务（WCF 数据服务）

通过向 Web 浏览器提供 URI，可以访问公开 Open Data Protocol (OData) 源的服务。 URI 提供某个资源的地址，系统将向这些地址发送请求消息以访问或更改该资源表示的基础数据。 浏览器发出 HTTP GET 命令，并以 OData 源的形式返回请求的资源。 有关详细信息，请参阅 [从 Web 浏览器访问服务](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)。  
  
 尽管可以使用 Web 浏览器来测试 OData 服务是否返回了预期的数据，但通常可以通过应用程序代码或网页中的脚本编写语言来创建、更新和删除数据。 本主题概述了如何从客户端应用程序访问 OData 源。  
  
## <a name="accessing-and-changing-data-using-rest-semantics"></a>使用 REST 语义访问和更改数据  

 OData 有助于保证公开 OData 源的服务与使用 OData 源的应用程序之间的互操作性。 应用程序通过发送特定 HTTP 操作的请求消息和用于处理应为其执行操作的实体资源的 URI 来访问和更改基于 OData 的服务中的数据。 如果必须提供实体数据，则会在消息正文中以明确编码的负载的形式提供该数据。  
  
### <a name="http-actions"></a>HTTP 操作  

 OData 支持下列 HTTP 操作，以对寻址的资源所代表的实体数据执行创建、读取、更新和删除操作：  
  
- **HTTP GET** -这是从浏览器访问资源时的默认操作。 请求消息中不提供负载，将返回具有包含所请求数据的负载的响应方法。  
  
- **HTTP** 向提供的资源中插入新的实体数据。 将在请求消息的负载中提供要插入的数据。 响应消息的负载包含新建实体的数据。 这包括所有自动生成的键值。 标头还包含用于对新实体资源进行寻址的 URI。  
  
- **HTTP DELETE** -删除指定资源表示的实体数据。 请求消息或响应消息中没有负载。  
  
- **HTTP PUT** -将请求资源中的现有实体数据替换为请求消息负载中提供的新数据。  
  
- **HTTP MERGE** -由于执行删除操作时，如果在数据源中执行插入操作效率低下，只是为了更改实体数据，OData 会引入新的 HTTP MERGE 操作。 请求消息的负载包含必须在寻址的实体资源中更改的属性。 由于 http 规范中未定义 HTTP MERGE，因此可能需要进行其他处理才能通过非 OData 感知服务器路由 HTTP 合并请求。  
  
 有关详细信息，请参阅 [OData：操作](https://www.odata.org/documentation/odata-version-2-0/operations/)。
  
### <a name="payload-formats"></a>负载格式  

 对于 HTTP PUT、HTTP POST 或 HTTP MERGE 请求，请求消息的负载包含向数据服务发送的实体数据。 负载的内容取决于消息的数据格式。 除 DELETE 外，所有操作的 HTTP 响应也都包含这样一个负载。 OData 支持以下有效负载格式，以便通过服务访问和更改数据：  
  
- **Atom** -一种基于 XML 的消息编码，由 OData 定义为 Atom 发布协议的扩展 (AtomPub) 为 Web 源、播客、wiki 和基于 XML 的 Internet 功能启用通过 HTTP 进行的数据交换。 有关详细信息，请参阅 [OData： Atom 格式](https://www.odata.org/documentation/odata-version-2-0/atom-format/)。
  
- **Json** JAVASCRIPT 对象表示法 (json) 是一种基于 JavaScript 编程语言子集的轻型数据交换格式。 有关详细信息，请参阅 [OData： JSON 格式](https://www.odata.org/documentation/odata-version-2-0/json-format/)。
  
 将在 HTTP 请求消息的标头中请求负载的消息格式。 有关详细信息，请参阅 [OData：操作](https://www.odata.org/documentation/odata-version-2-0/operations/)。
  
## <a name="accessing-and-changing-data-using-client-libraries"></a>使用客户端库访问和更改数据  

 WCF 数据服务包括客户端库，使你能够更轻松地从基于 .NET Framework 和 Silverlight 的客户端应用程序中使用 OData 源。 这些库简化了 HTTP 消息的发送和接收。 它们还可将消息负载转换为代表实体数据的 CLR 对象。 客户端库具有两个核心类 <xref:System.Data.Services.Client.DataServiceContext> 和 <xref:System.Data.Services.Client.DataServiceQuery%601>。 通过使用这些类，可以查询数据服务，然后作为 CLR 对象使用返回的实体数据。 有关详细信息，请参阅 [WCF 数据服务客户端库](wcf-data-services-client-library.md) 和 [WCF 数据服务 (Silverlight) ](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v=vs.95))。  
  
 您可以使用 Visual Studio 中的 **添加服务引用** 对话框添加对数据服务的引用。 此工具将向所引用的数据服务请求服务元数据，然后生成代表数据服务的 <xref:System.Data.Services.Client.DataServiceContext>，并生成代表实体的客户端数据服务类。 有关详细信息，请参阅 [生成数据服务客户端库](generating-the-data-service-client-library-wcf-data-services.md)。  
  
 有可用的编程库，可用于在其他类型的客户端应用程序中使用 OData 源。 有关 OData SDK 的详细信息，请参阅 [ODATA sdk-示例代码](https://www.odata.org/ecosystem/#sdk)。
  
## <a name="see-also"></a>请参阅

- [访问数据服务资源](accessing-data-service-resources-wcf-data-services.md)
- [快速入门](quickstart-wcf-data-services.md)
