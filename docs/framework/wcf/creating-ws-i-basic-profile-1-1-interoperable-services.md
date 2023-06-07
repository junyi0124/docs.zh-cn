---
title: 创建 WS-I 基本配置文件 1.1 可互操作服务
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- configuration [WCF], interoperable services
ms.assetid: 91b70a21-8f5c-4679-808c-2ed5fa6b2013
ms.openlocfilehash: b5d262c3e0443503ccbf49a1a468c82843799a61
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96255047"
---
# <a name="creating-ws-i-basic-profile-11-interoperable-services"></a>创建 WS-I 基本配置文件 1.1 可互操作服务

将 WCF 服务终结点配置为可与 ASP.NET Web 服务客户端进行互操作：  
  
- 将 <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType> 类型用作服务终结点的绑定类型。  
  
- 不要使用服务终结点上的回调和会话协定功能或事务行为  
  
你可以根据需要在该绑定上启用对 HTTPS 和传输级客户端身份验证的支持。  
  
<xref:System.ServiceModel.BasicHttpBinding> 类的以下特性所要求的功能超出了 WS-I 基本配置文件 1.1 的范围：  
  
- 由 <xref:System.ServiceModel.BasicHttpBinding.MessageEncoding%2A?displayProperty=nameWithType> 属性控制的消息传递优化机制 (MTOM) 消息编码。 将此属性保留为其默认值（即 <xref:System.ServiceModel.WSMessageEncoding.Text?displayProperty=nameWithType>）以便不使用 MTOM。  
  
- 由 <xref:System.ServiceModel.BasicHttpBinding.Security%2A?displayProperty=nameWithType> 值控制的消息安全提供符合 WS-I 基本安全配置文件 1.0 的 WS-Security 支持。 将此属性保留为其默认值（即 <xref:System.ServiceModel.SecurityMode.Transport?displayProperty=nameWithType>）以便不使用 WS-Security。  
  
若要使 WCF 服务的元数据可用于 ASP.NET，请使用 Web 服务客户端生成工具： [web 服务描述语言工具 ( # A0)](/previous-versions/dotnet/netframework-4.0/7h3ystb6(v=vs.100))、 [Web 服务发现工具 ( # A1)](/previous-versions/dotnet/netframework-4.0/cy2a3ybs(v=vs.100))以及 Visual Studio 中的 " **添加 Web 引用** " 功能。 启用元数据发布。 有关详细信息，请参阅 [发布元数据终结点](publishing-metadata-endpoints.md)。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的代码示例演示如何在代码中添加与 ASP.NET Web 服务客户端兼容的 WCF 终结点，也可以在配置文件中添加。  
  
### <a name="code"></a>代码  

 [!code-csharp[C_HowTo-WCFServiceAndASMXClient#0](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/cs/program.cs#0)]
 [!code-vb[C_HowTo-WCFServiceAndASMXClient#0](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/vb/program.vb#0)]  
 [!code-xml[C_HowTo-WCFServiceAndASMXClient#1](../../../samples/snippets/csharp/VS_Snippets_CFX/c_howto-wcfserviceandasmxclient/common/app.config#1)]  
  
## <a name="see-also"></a>请参阅

- [与 ASP.NET Web 服务的互操作性](./feature-details/interop-with-aspnet-web-services.md)
