---
title: WCF 和 WebSocket
ms.date: 03/30/2017
ms.assetid: 1e53b49e-022c-49c7-8984-4b21b53c05b3
ms.openlocfilehash: 2ee27eef015ee8c2fbee8360b444343a1c757097
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281581"
---
# <a name="wcf-and-websockets"></a>WCF 和 WebSocket

.NET Framework 4.5 引入了对 Windows Communication Foundation 中的 WebSocket 的支持。  WebSocket 是一种高效的、基于标准的技术，可通过标准 HTTP 端口 80 和 443 实现双向通信。 使用标准 HTTP 端口，WebSocket 可通过中介跨 Web 进行通信。  添加了两个新的标准绑定以支持通过 WebSocket 传输进行的通信。 <xref:System.ServiceModel.NetHttpBinding> 和 <xref:System.ServiceModel.NetHttpsBinding>。 可以通过访问属性在上配置 Websocket 特定的设置 <xref:System.ServiceModel.Channels.HttpTransportBindingElement> <xref:System.ServiceModel.Channels.HttpTransportBindingElement.WebSocketSettings%2A> 。
  
## <a name="in-this-section"></a>本节内容  

 [使用 NetHttpBinding](using-the-nethttpbinding.md)  
 讨论 <xref:System.ServiceModel.NetHttpBinding> 以及如何对其进行配置。  
  
 [如何：创建通过 WebSocket 进行通信的 WCF 服务](how-to-create-a-wcf-service-that-communicates-over-websockets.md)  
 说明如何创建通过 Websocket 进行通信的 WCF 服务。  
  
## <a name="reference"></a>参考  
  
## <a name="related-sections"></a>相关章节
