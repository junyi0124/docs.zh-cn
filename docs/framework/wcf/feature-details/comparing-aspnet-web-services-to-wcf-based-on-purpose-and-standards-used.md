---
title: 基于目标和使用的标准比较 ASP.NET Web 服务与 WCF
ms.date: 03/30/2017
ms.assetid: d3890278-fa9b-4902-91ea-8da73b7143cc
ms.openlocfilehash: 62917d7a188d0863f6ebcb7dd3531ef2dd6a5132
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96295062"
---
# <a name="comparing-aspnet-web-services-to-wcf-based-on-purpose-and-standards-used"></a>基于目标和使用的标准比较 ASP.NET Web 服务与 WCF

开发 ASP.NET Web 服务的目标是生成通过使用 Simple Object Access Protocol (SOAP) over HTTP 来发送和接收消息的应用程序。 消息的结构可使用 XML 架构来定义，并且提供了一种工具，以便将消息序列化为 .NET Framework 对象以及将这种对象反序列化为消息。 该技术可以自动生成元数据，以便用 Web 服务描述语言 (WSDL) 描述 Web 服务，并且提供了另一个工具，用来从 WSDL 生成 Web 服务的客户端。  
  
 WCF 用于使 .NET Framework 的应用程序能够与其他软件实体交换消息。 默认情况下使用 SOAP，但消息可采用任何格式，并可以使用任何传输协议传递。 消息的结构可使用 XML 架构来定义，并且提供多种选项以便将消息序列化为 .NET Framework 对象以及将这种对象反序列化为消息。 WCF 可以自动生成元数据来描述使用 WSDL 中的技术生成的应用程序，此外还提供了一个工具，用于为这些应用程序从 WSDL 生成客户端。  
  
 [使用 ASP.NET 创建的 XML Web services 的优点](/previous-versions/dotnet/netframework-4.0/0859ebft(v=vs.100))介绍了 ASP.NET Web 服务支持的标准。 在 [System-Provided 互操作性绑定支持的 Web 服务协议](web-services-protocols-supported-by-system-provided-interoperability-bindings.md)中列出了 WCF 支持的更广泛的标准列表。  
  
## <a name="see-also"></a>另请参阅

- [从开发的角度比较 ASP.NET Web 服务与 WCF](comparing-aspnet-web-services-to-wcf-based-on-development.md)
