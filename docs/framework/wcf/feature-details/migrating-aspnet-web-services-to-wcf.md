---
title: 将 ASP.NET Web 服务迁移到 WCF
ms.date: 03/30/2017
ms.assetid: 1adbb931-f0b1-47f3-9caf-169e4edc9907
ms.openlocfilehash: 1471e9913f787a76b474e9d862a22b24d464be92
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281646"
---
# <a name="migrating-aspnet-web-services-to-wcf"></a>将 ASP.NET Web 服务迁移到 WCF

ASP.NET 提供 .NET Framework 类库和工具以用于生成 Web 服务，并提供用于在 Internet Information Services (IIS) 内承载服务的功能。 Windows Communication Foundation (WCF) 提供 .NET Framework 类库、工具和承载功能，使软件实体能够使用任何协议（包括 Web 服务使用的协议）进行通信。  将 ASP.NET Web 服务迁移到 WCF 后，应用程序便可以利用 WCF 独有的新功能和改进功能。  
  
 与 ASP.NET Web 服务相比，WCF 具有几个重要优势。 尽管 ASP.NET Web 服务工具仅用于构建 Web 服务，但 WCF 提供的工具可在必须进行软件实体相互通信时使用。 这会减少开发人员为适应不同软件通信方案而必需了解的技术的数量，从而降低软件开发资源的成本及完成软件开发项目所需的时间。  
  
 即使对于 Web 服务开发项目，WCF 也支持比 ASP.NET Web 服务支持更多的 Web 服务协议。 这些附加协议提供更完善的解决方案，涉及可靠会话和事务等其他内容。  
  
 WCF 支持更多用于传输消息的协议，而不是 ASP.NET Web 服务。 ASP.NET Web 服务仅支持使用超文本传输协议 (HTTP) 发送消息。 WCF 支持使用 HTTP、传输控制协议 (TCP) 、命名管道和 Microsoft 消息队列 (MSMQ) 来发送消息。 更重要的是，可以扩展 WCF 以支持其他传输协议。 因此，可以改编使用 WCF 开发的软件，使其与更广泛的其他软件一起工作，从而提高投资的潜在回报。  
  
 WCF 提供了更丰富的工具来部署和管理应用程序，而不是 ASP.NET Web 服务提供的功能。 除了 ASP.NET 的配置系统外，WCF 还提供了一个配置编辑器、从发件人到接收方的活动跟踪，还提供了多个中介的活动跟踪、跟踪查看器、消息日志记录、大量性能计数器以及对 Windows Management Instrumentation 的支持。  
  
 考虑到 WCF 相对于 ASP.NET Web 服务的潜在优势（如果你使用的是）或考虑使用 ASP.NET Web 服务，你可以使用以下几个选项：  
  
- 继续使用 ASP.NET Web 服务，并放弃 WCF 提供的权益。  
  
- 继续使用 ASP.NET Web 服务，目的是为了在将来某个时间采用 WCF。 本节中的主题介绍了如何最大程度地利用新的 ASP.NET Web 服务应用程序以及未来的 WCF 应用程序。 本节中的主题还介绍如何生成新的 ASP.NET Web 服务，以便更轻松地将其迁移到 WCF。 但是，如果确保服务安全非常重要，或者需要可靠性或事务保证，或者必须构造自定义管理工具，则最好选择采用 WCF。 WCF 专为此类方案而设计。  
  
- 为新的开发采用 WCF，同时继续维护现有的 ASP.NET Web 服务应用程序。 这种选择很可能是最佳选择。 它会产生 WCF 的优点，同时还会降低修改现有应用程序以使用该应用程序的成本。 在这种情况下，新的 WCF 应用程序可以与现有的 ASP.NET 应用程序共存。 新的 WCF 应用程序将能够使用现有的 ASP.NET Web 服务，并且 WCF 可用于通过 WCF ASP.NET 兼容模式在现有的 ASP.NET 应用程序中计划新的操作功能。  
  
- 采用 WCF，并将现有的 ASP.NET Web 服务应用程序迁移到 WCF。 你可以选择此选项以使用 WCF 提供的功能来增强现有应用程序，或在新的、功能更强大的 WCF 应用程序中再现现有的 ASP.NET Web 服务的功能。  
  
> [!NOTE]
> 如果 WCF 服务由 IIS 1.x 托管并且 ASP.NET 已卸载，则必须小心谨慎。 当 WCF 服务由 IIS 1.x 托管时，如果卸载 ASP.NET，则可以请求服务的代码。 当在运行 IIS 1.x 的操作系统上卸载 ASP.NET 并卸载 WCF 时，具有 .svc 扩展名的文件将被视为文本文件，并将内容（包括任何源代码）返回给请求者。  
  
 本部分将详细介绍这些选项，将 ASP.NET Web 服务与 WCF 进行比较，并提供有关如何将 ASP.NET Web 服务代码迁移到 WCF 的说明。  
  
## <a name="see-also"></a>另请参阅

- [Windows Communication Foundation 使用展望：轻松实现未来的迁移](anticipating-adopting-wcf-migration.md)
- [Windows Communication Foundation 使用展望：轻松实现未来的集成](anticipating-adopting-the-wcf-easing-future-integration.md)
- [采用 Windows Communication Foundation](adopting-wcf.md)
- [基于目标和使用的标准比较 ASP.NET Web 服务与 WCF](comparing-aspnet-web-services-to-wcf-based-on-purpose-and-standards-used.md)
- [从开发的角度比较 ASP.NET Web 服务与 WCF](comparing-aspnet-web-services-to-wcf-based-on-development.md)
