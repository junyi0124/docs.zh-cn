---
title: WCF 和 ASP.NET Web API
description: 通过比较每项技术的主要功能，了解 WCF 或 ASP.NET Web API 是否更适合你的需求。
ms.date: 03/30/2017
ms.assetid: 08ceded3-fd9a-4467-9715-c4cbd9c7228e
ms.openlocfilehash: b3a905f890b4dfa9f60c906c3a242be60921e5c8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273589"
---
# <a name="wcf-and-aspnet-web-api"></a>WCF 和 ASP.NET Web API

WCF 是 Microsoft 为生成面向服务的应用程序而提供的统一编程模型。 借助这一模型，开发人员可以构建既能跨平台与现有投资集成又能与现有投资交互的安全、可靠的事务处理解决方案。 [ASP.NET Web API](https://www.asp.net/web-api) 是一种框架，可让你轻松地生成可访问范围广泛的客户端（包括浏览器和移动设备）的 HTTP 服务。 ASP.NET Web API 是用于在 .NET Framework 上生成 RESTful 应用程序的理想平台。 本主题提供了一些指南，可帮助您决定哪种技术能够最佳满足您的需要。  
  
## <a name="choosing-which-technology-to-use"></a>选择要使用的技术  

 下表介绍了每种技术的主要功能。  
  
|WCF|ASP.NET Web API|  
|---------|---------------------|  
|启用支持多种传输协议（HTTP、TCP、UDP 和自定义传输）的生成服务，并允许在这些服务之间切换。|仅限 HTTP。 HTTP 的第一类编程模型。 更适合从各种浏览器、移动设备等访问，从而实现广域覆盖。|  
|启用支持同一消息类型的多种编码（文本、MTOM 和二进制）的生成服务，并允许在这些服务之间切换。|启用支持广泛的媒体类型（包括 XML、JSON 等）的生成 Web API。|  
|支持采用 WS-* 标准的生成服务，如可靠消息传递、事务、消息安全性。|使用基本协议和格式，如 HTTP、Websocket、SSL、JSON 和 XML。 不支持较高级别的协议，如消息传递或事务。|  
|支持请求-答复、单向和双工消息交换模式。|HTTP 是请求/响应，但其他模式可以通过 [SignalR](https://github.com/SignalR/SignalR) 和 websocket 集成来支持。|  
|可以在 WSDL 中描述 WCF SOAP 服务，从而可通过自动工具，针对具有复杂架构的服务来生成客户端代理。|可通过各种方法来描述 Web API：从用于描述代码片段的自动生成的 HTML 帮助页，直至用于 OData 集成 API 的结构化元数据。|  
|随 .NET Framework 附带。|附带了 .NET Framework，但它是开源的，也可以在带外进行下载。|  
  
 使用 WCF 可创建可靠、安全的 web 服务，这些服务可通过各种传输进行访问。 使用 ASP.NET Web API 可创建基于 HTTP 的服务，这些服务可从各种客户端来访问。 如果要创建和设计新的 REST 样式服务，请使用 ASP.NET Web API。 虽然 WCF 针对编写 REST 样式服务提供了一些支持，但 ASP.NET Web API 中的 REST 支持更加完整，并且，所有将来的 REST 功能改进都将在 ASP.NET Web API 中进行。 如果您现在拥有一种 WCF 服务，并且要公开其他 REST 终结点，请使用 WCF 和 <xref:System.ServiceModel.WebHttpBinding>。  
  
## <a name="see-also"></a>另请参阅

- [什么是 Windows Communication Foundation](whats-wcf.md)
- [Windows Communication Foundation 基础概念](fundamental-concepts.md)
