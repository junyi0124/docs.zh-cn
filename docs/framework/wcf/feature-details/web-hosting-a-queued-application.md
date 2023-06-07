---
title: 承载排队应用程序的 Web
ms.date: 03/30/2017
ms.assetid: c7a539fa-e442-4c08-a7f1-17b7f5a03e88
ms.openlocfilehash: c2b41ee1d0a82693760bc3e1b6144d2190153f24
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249769"
---
# <a name="web-hosting-a-queued-application"></a>承载排队应用程序的 Web

Windows 进程激活服务 (WAS) 管理辅助进程的激活和生命周期，这些进程包含托管 Windows Communication Foundation (WCF) 服务的应用程序。 WAS 进程模型通过删除对 HTTP 的依赖，将 HTTP 服务器的 IIS 6.0 进程模型通用化。 这使 WCF 服务可以在支持基于消息的激活的宿主环境中同时使用 HTTP 和非 HTTP 协议（如 msmq.formatname 和 msmq），并提供在给定计算机上托管大量应用程序的功能。  
  
 WAS 中包含一项消息队列 (MSMQ) 激活服务，当有一条或多条消息放入某排队的应用程序所使用的某个队列时，该服务将激活该应用程序。 MSMQ 激活服务是默认情况下自动启动的 NT 服务。  
  
 有关 WAS 及其优点的详细信息，请参阅 [在 Windows 进程激活服务中托管](hosting-in-windows-process-activation-service.md)。 有关 MSMQ 的详细信息，请参阅 [队列概述](queues-overview.md)。
  
## <a name="queue-addressing-in-was"></a>WAS 中的队列寻址  

 WAS 应用程序具有统一资源标识符 (URI) 地址。 应用程序地址有两部分：基本 URI 前缀和应用程序特定的相关地址（路径）。 这两部分连接在一起时可提供应用程序的外部地址。 基本 URI 前缀是从站点绑定构造的，并用于站点下的所有应用程序，例如 "net.tcp：//localhost"、"msmq.formatname：//localhost" 或 "net.tcp：//localhost"。 然后，将使用应用程序特定的路径片段 (如 "/applicationOne ) "）构造应用程序地址，并将其追加到基 URI 前缀，以到达完整的应用程序 URI，例如 "//localhost/applicationOne"。  
  
 MSMQ 激活服务使用应用程序 URI 来匹配 MSMQ 激活服务必须监视其消息的队列。 MSMQ 激活服务启动时，它将枚举配置为从其中接收消息的计算机上所有的公共队列和专用队列，以及监视消息的公共队列和专用队列。 每隔 10 分钟，MSMQ 激活服务就刷新一次要监视的队列列表。 在队列中找到消息时，激活服务将队列名称与 net.msmq 绑定的最长匹配应用程序 URI 进行匹配，并激活该应用程序。  
  
> [!NOTE]
> 被激活的应用程序必须与队列名称的前缀匹配（最长的匹配）。  
  
 例如，队列名称为：msmqWebHost/orderProcessing/service.svc。 如果应用程序 1 的下面有一个带有 service.svc 的虚拟目录 /msmqWebHost/orderProcessing，应用程序 2 的下面有一个带有 orderProcessing.svc 的虚拟目录 /msmqWebHost，则将激活应用程序 1。 如果应用程序 1 已删除，则将激活应用程序 2。  
  
> [!NOTE]
> 创建队列后，在 MSMQ 激活服务刷新队列列表（自创建该队列时起最多 10 分钟）之前，发送到该队列的任何消息都不会激活应用程序。 重新启动激活服务也会刷新队列列表。  
  
### <a name="the-effect-of-private-and-public-queues-on-addressing"></a>专用队列和公共队列对寻址的影响  

 MSMQ 激活服务并不区分专用队列和公共队列监视。 因此，您不能拥有名称相同的公共队列和专用队列。 如果有，则 Web 承载的应用程序可能被激活，从这两个队列之一中进行读取。  
  
### <a name="queue-configuration-for-activation"></a>用于激活的队列配置  

 MSMQ 激活服务将作为 NETWORK SERVICE 运行。 这是监视要激活应用程序的队列的服务。 由于它将从队列激活应用程序，因此该队列必须提供 NETWORK SERVICE 访问权限以查看其访问控制列表 (ACL) 中的消息。  
  
### <a name="poison-messaging"></a>病毒消息  

 WCF 中的有害消息处理由通道进行处理，该通道不仅检测到消息是中毒的，还会根据用户配置选择一个处置。 因此，队列中仅有一条消息。 Web 承载的应用程序可连续多次中止并将该消息移到重试队列中。 在重试周期延迟确定的点，消息从重试队列中移到主队列以便进行重试。 但是这要求排队通道应是活动的。 如果 WAS 回收了该应用程序，则在另一条消息到达主队列以激活排队应用程序之前，此消息仍保留在重试队列中。 本例中的解决方法就是手动将消息从重试队列移回主队列，以便重新激活应用程序。  
  
### <a name="subqueue-and-system-queue-caveat"></a>子队列和系统队列注意事项  

 不能基于系统队列（如系统级死信队列）或子队列（如病毒子队列）中的消息激活 WAS 承载的应用程序。 这是此版本产品的一个限制。  
  
## <a name="see-also"></a>另请参阅

- [病毒消息处理](poison-message-handling.md)
- [服务终结点和队列寻址](service-endpoints-and-queue-addressing.md)
