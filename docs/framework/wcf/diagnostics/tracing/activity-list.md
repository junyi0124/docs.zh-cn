---
title: 活动列表
ms.date: 03/30/2017
ms.assetid: 5540e185-ce8e-4db3-83b0-2b9f5bf71829
ms.openlocfilehash: d28ae2e4750c718c35105d090aff8d085025b9d6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236086"
---
# <a name="activity-list"></a>活动列表

本主题列出了 Windows Communication Foundation (WCF) 定义的所有活动。  
  
> [!NOTE]
> 您还可以用编程方式定义活动，以便对用户跟踪进行分组。 有关详细信息，请参阅 [发出 User-Code 跟踪](emitting-user-code-traces.md)。  
  
## <a name="servicemodel-activities"></a>ServiceModel 活动  

 下表列出了用于主要使用方案的所有活动。  
  
|Label|活动名称|活动类型|描述|  
|-----------|-------------------|-------------------|-----------------|  
|A、M|环境活动|N/A（不受 ServiceModel 控制）|该活动的 ID 是在调用任何 ServiceModel 代码（客户端或服务器端）之前，在 TLS 中设置的。<br /><br /> 示例：在 WCF 客户端或 serviceHost 上调用 open 的活动。调用。|  
|B|构造<br /><br /> ChannelFactory。 ContractType : ‘[Type]’。|构造||  
|C|打开<br /><br /> [ClientBase&#124;ChannelFactory]。 ContractType : ‘[Type]’。|打开||  
|I|Close [ClientBase&#124;ChannelFactory]。 ContractType : ‘[Type]’。|关闭||  
|M|构造 ServiceHost。 ServiceType: ‘[Type]’。|构造||  
|N|打开 ServiceHost。 ServiceType: ‘[Type]’。|打开||  
|Z|关闭 ServiceHost。 ServiceType: ‘[Type]’。|关闭||  
|O|在“[address]”上侦听。|ListenAt|此活动以及下一个活动都是特定于传输的。 ListenAt 活动表示映射到通道侦听器所侦听的地址的内容。 对于 MSMQ，它是队列本身，因为队列映射到一个地址。 对于面向连接的传输，此活动侦听传入的连接；对于 MSMQ，此活动侦听 MSMQ 消息。 此活动在 ServiceHost.Open() 期间创建，并且包含与创建和释放侦听器以及向所有 ReceiveBytes 活动传输数据有关的跟踪。|  
|P|接收连接“[address]”上的字节。 接收 MSMQ 消息。|ReceiveBytes|在此活动中，将对最终获取 WCF 消息的数据进行处理。 在面向连接的传输或 http 中，需要等待传入的字节。 对于 TCP/命名管道，此活动的生存期就是连接的生存期，因为它是在创建连接时创建的。 对于 http，它是消息请求的生存期并且在发送消息时创建。 此活动包含与创建和释放连接（如果适用）以及向所有消息（对象）处理活动传输数据有关的跟踪。<br /><br /> 对于 MSMQ，它就是用来检索 MSMQ 消息的活动。|  
|Q|处理消息 [number]。 （注意，[number] 是一个从 1 开始单调递增的值。）|ProcessMessage|处理传入的消息。 当所有数据 (字节，接收到 MSMQ 消息) 来构成 WCF 消息对象时，将启动此活动。 此活动内的跟踪负责进行标头处理。<br /><br /> 一旦形成可以调度的消息，在查找对应的活动 ID 后，随即切换到 ServiceHost“处理操作”活动。|  
|D、S|处理操作“[action]”。|ProcessAction|通过传输/安全/RM 堆栈处理消息，以便在接收到消息时将消息调度到用户代码，而在发送消息时按相反顺序进行处理。<br /><br /> 在服务器上，如果在消息标头中通过 "活动传播" 发送活动 ID，则此活动将使用传播的活动 ID;否则，将创建新的 GUID。<br /><br /> 请求/答复协定的响应消息也是在该活动中处理的。|  
|T|执行“[IContract.Operation]”。|ExecuteUserCode|在服务端调度后执行用户代码。 此活动提供了用于勾画用户提供代码中的 ServiceHost 代码的边界。|  
  
## <a name="security-activities"></a>安全活动  

 下表列出了所有与安全有关的活动。  
  
|活动名称|活动类型|描述|  
|-------------------|-------------------|-----------------|  
|设置安全会话|SetupSecurity|仅在客户端存在。 包含用于身份验证和设置安全上下文的所有 RST*/SCT 交换。 如果 `propagateActivity` = `true` 为，则此活动与服务的对应处理操作 RST \* /SCT 活动合并。|  
|关闭安全会话|SetupSecurity|存在于客户端。 包含用于关闭安全会话的“取消”消息交换。 如果 `propagateActivity` = `true` 为，则此活动与服务中的处理操作 "取消" 合并在一起。|  
  
 下表列出了所有与 COM+ 有关的活动。  
  
|活动名称|活动类型|描述|  
|-------------------|-------------------|-----------------|  
|创建 COM+ 实例。|TransferToCOMPlus|WCF 代码中的每个 COM + 调用1个活动实例|  
|执行 COM + \<operation>|TransferToCOMPlus|WCF 代码中的每个 COM + 调用1个活动实例|  
  
## <a name="wmi-activities"></a>WMI 活动  

 下表列出了所有与 WMI 有关的活动。  
  
|活动名称|活动类型|描述|  
|-------------------|-------------------|-----------------|  
|WMI get|WMIGetObject|用户正从 WMI 检索数据。|  
|WMI put|WmiPutInstance|用户正向 WMI 更新数据。|
