---
title: 服务终结点和队列寻址
ms.date: 03/30/2017
ms.assetid: 7d2d59d7-f08b-44ed-bd31-913908b83d97
ms.openlocfilehash: f7c3221d466d2599139eb29a8358d726c2b4edda
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96253916"
---
# <a name="service-endpoints-and-queue-addressing"></a>服务终结点和队列寻址

本主题讨论客户端如何对从队列中读取的服务进行寻址以及服务终结点如何映射到队列。 作为提醒，下图显示了经典 Windows Communication Foundation (WCF) 排队应用程序部署。  
  
 ![排队应用程序关系图](media/distributed-queue-figure.jpg "分布式队列图")  
  
 客户端为了将消息发送到服务，需要确定该消息在目标队列中的地址。 服务为了从队列中读取消息，需要在目标队列中设置它的侦听地址。 WCF 中的寻址是基于)  (URI 的统一资源标识符，而消息队列 (MSMQ) 队列名称不是基于 URI 的。 因此，必须了解如何使用 WCF 来寻址在 MSMQ 中创建的队列。  
  
## <a name="msmq-addressing"></a>MSMQ 寻址  

 MSMQ 使用路径和格式名来标识队列。 路径指定一个主机名称和一个 `QueueName`。 可以根据需要在主机名称和 `Private$` 之间添加一个 `QueueName`，以指示不是在 Active Directory 目录服务中发布的专用队列。  
  
 路径名称将映射到 "FormatNames"，以确定地址的其他方面，包括路由和队列管理器传输协议。 队列管理器支持两种传输协议：本机 MSMQ 协议和 SOAP 可靠消息协议 (SRMP)。  
  
 有关 MSMQ 路径和格式名的详细信息，请参阅 [关于消息队列](/previous-versions/windows/desktop/legacy/ms706032(v=vs.85))。  
  
## <a name="netmsmqbinding-and-service-addressing"></a>NetMsmqBinding 和服务寻址  

 在将消息发送到服务中的目标地址时，将根据用于通信的传输协议选择 URI 中的方案。 WCF 中的每个传输都具有唯一的方案。 此方案必须反映用于通信的传输协议的性质。 例如，net.tcp、net.pipe、HTTP 等等。  
  
 WCF 中的 MSMQ 排队传输公开了一个 net.pipe 方案。 使用 net.msmq 方案寻址的任何消息都使用 MSMQ 排队传输协议通道上的 `NetMsmqBinding` 发送。  
  
 WCF 中的队列寻址基于以下模式：  
  
 net.tcp：// \<*host-name*> /[private/] \<*queue-name*>  
  
 其中：  
  
- \<*host-name*> 承载目标队列的计算机的名称。  
  
- [private] 可选。 它在对作为专用队列的目标队列寻址时使用。 若要对公共队列寻址，不能指定 private。 请注意，与 MSMQ 路径不同，WCF URI 窗体中没有 "$"。  
  
- \<*queue-name*> 队列的名称。 队列名还可以引用子队列。 因此， \<*queue-name*>  =  \<*name-of-queue*> [;*子队列名称*]。  
  
 示例 1：若要对计算机 abc atadatum.com 上承载的专用队列 PurchaseOrders 寻址，URI 将为 net.msmq://abc.adatum.com/private/PurchaseOrders。  
  
 示例 2：若要对计算机 def atadatum.com 上承载的公共队列 AccountsPayable 寻址，URI 将为 net.msmq://def.adatum.com/AccountsPayable。  
  
 队列地址用作侦听器从中读取消息的侦听 URI。 换言之，队列地址等效于 TCP 套接字的侦听端口。  
  
 从队列中读取消息的终结点必须使用以前在打开 ServiceHost 时指定的同一方案指定队列地址。 有关示例，请参阅 [NET MSMQ 绑定](../samples/net-msmq-binding.md)。  
  
### <a name="multiple-contracts-in-a-queue"></a>队列中存在多个协定  

 队列中的消息可以实现不同的协定。 在这种情况下，下列条件中必须有一个成立才能成功读取并处理所有消息：  
  
- 为服务指定一个实现所有协定的终结点。 这是建议的做法。  
  
- 指定多个具有不同协定的终结点，但要确保所有终结点都使用相同的 `NetMsmqBinding` 对象。 ServiceModel 中的调度逻辑使用一个消息泵，它从传输通道中读取消息以进行调度，并最终根据相应的协定将消息多路分解到不同的终结点。 消息泵是为侦听 URI/绑定对创建的。 队列地址被排队侦听器用作侦听 URI。 通过让所有终结点使用相同的绑定对象，可以确保使用单个消息泵来读取消息，并且根据协定将消息多路分解到相关终结点。  
  
### <a name="srmp-messaging"></a>SRMP 消息传递  

 如前所述，可以使用 SRMP 协议进行队列到队列的传输。 当 HTTP 传输协议在传输队列和目标队列之间传输消息时，经常使用这一协议。  
  
 若要使用 SRMP 传输协议，请使用 net.msmq URI 方案对消息进行寻址（如前所述），并在 `QueueTransferProtocol` 的 `NetMsmqBinding` 属性中指定选择 SRMP 还是 Secured SRMP。  
  
 指定 `QueueTransferProtocol` 属性只具有发送的功能。 这是客户端发出的有关要使用哪种队列传输协议的指示。  
  
### <a name="using-active-directory"></a>使用 Active Directory  

 MSMQ 带有 Active Directory 集成支持。 安装带有 Active Directory 集成的 MSMQ 时，计算机必须属于 Windows 域。 Active Directory 用于为发现发布队列;此类队列称为 *公共队列*。 对队列进行寻址时，可以使用 Active Directory 对队列进行解析。 这与使用域名系统 (DNS) 解析网络名称的 IP 地址的方式相似。 `UseActiveDirectory` 中的 `NetMsmqBinding` 属性是一个布尔值，该值指示排队通道是否必须使用 Active Directory 来解析队列 URI。 默认设置为 `false`。 如果 `UseActiveDirectory` 属性设置为 `true`，则排队通道使用 Active Directory 将 net.msmq:// URI 转换为格式名。  
  
 `UseActiveDirectory` 属性仅对发送消息的客户端有意义，因为它用于在发送消息时解析队列的地址。  
  
### <a name="mapping-netmsmq-uri-to-message-queuing-format-names"></a>将 net.msmq URI 映射到消息队列格式名  

 排队通道负责将提供给通道的 net.msmq URI 名称映射到 MSMQ 格式名。 下表总结了用于在它们之间进行映射的规则。  
  
|基于 WCF URI 的队列地址|使用 Active Directory 属性|队列传输协议属性|得到的 MSMQ 格式名|  
|----------------------------------|-----------------------------------|--------------------------------------|---------------------------------|  
|`Net.msmq://<machine-name>/private/abc`|False（默认值）|Native（默认值）|`DIRECT=OS:machine-name\private$\abc`|  
|`Net.msmq://<machine-name>/private/abc`|False|SRMP|`DIRECT=http://machine/msmq/private$/abc`|  
|`Net.msmq://<machine-name>/private/abc`|True|本机|`PUBLIC=some-guid` (队列的 GUID) |  
  
### <a name="reading-messages-from-the-dead-letter-queue-or-the-poison-message-queue"></a>从死信队列或病毒消息队列读取消息  

 若要从作为目标队列子队列的病毒消息队列中读取消息，请打开具有子队列地址的 `ServiceHost`。  
  
 示例：从本地计算机的 PurchaseOrders 专用队列的病毒消息队列中读取消息的服务将按 net.msmq://localhost/private/PurchaseOrders;poison 寻址。  
  
 若要从系统的事务性死信队列中读取消息，URI 必须采用以下形式：net.msmq://localhost/system$;DeadXact。  
  
 若要从系统的非事务性死信队列中读取消息，URI 必须采用以下形式：net.msmq://localhost/system$;DeadLetter。  
  
 在使用自定义死信队列时，请注意，死信队列必须驻留在本地计算机上。 因此，死信队列的 URI 被限制为以下形式：  
  
 net.tcp：//localhost/[private/]  \<*custom-dead-letter-queue-name*> 。  
  
 WCF 服务验证接收到的所有消息是否已发送到它正在侦听的特定队列。 如果消息的目标队列与它所在的队列不匹配，服务不会处理该消息。 这存在一个问题：侦听死信队列的服务必须寻址，因为死信队列中的任何消息都需要传递到别处。 若要从死信队列或病毒队列中读取消息，必须使用带有 `ServiceBehavior` 参数的 <xref:System.ServiceModel.AddressFilterMode.Any>。 有关示例，请参阅 [死信队列](../samples/dead-letter-queues.md)。  
  
## <a name="msmqintegrationbinding-and-service-addressing"></a>MsmqIntegrationBinding 和服务寻址  

 `MsmqIntegrationBinding` 用于与传统 MSMQ 应用程序进行通信。 为了便于与现有 MSMQ 应用程序进行互操作，WCF 仅支持格式名寻址。 因此，使用此绑定发送的消息必须符合 URI 方案。  
  
 msmq.formatname：\<*MSMQ-format-name*>>  
  
 MSMQ 格式名称采用 MSMQ 在 [关于消息队列](/previous-versions/windows/desktop/legacy/ms706032(v=vs.85))中指定的形式。  
  
 请注意，当使用 `MsmqIntegrationBinding` 从队列接收消息时，只能使用直接格式名以及公共和专用格式名（需要 Active Directory 集成）。 但是，建议您使用直接格式名。 例如，在 Windows Vista 上，使用任何其他格式名会导致错误，因为系统会尝试打开一个子队列，而该子队列只能用直接格式名称打开。  
  
 在使用 `MsmqIntegrationBinding` 对 SRMP 进行寻址时，不需要在直接格式名中添加 /msmq/ 来帮助 Internet 信息服务 (IIS) 进行调度。 例如：使用 SRMP 协议（而不是）对队列 abc 进行寻址时， `DIRECT=http://adatum.com/msmq/private$/abc` 应使用 `DIRECT=http://adatum.com/private$/abc` 。  
  
 请注意，对于 `MsmqIntegrationBinding`，不能使用 net.msmq:// 寻址。 由于 `MsmqIntegrationBinding` 支持自由格式的 MSMQ 格式名寻址，因此你可以使用 WCF 服务，该服务使用此绑定来使用 MSMQ 中的多路广播和通讯组列表功能。 一个例外是在使用 `CustomDeadLetterQueue` 时指定 `MsmqIntegrationBinding`。 它必须采用 net.msmq:// 形式，这与使用 `NetMsmqBinding` 进行指定的方式相似。  
  
## <a name="see-also"></a>另请参阅

- [承载排队应用程序的 Web](web-hosting-a-queued-application.md)
