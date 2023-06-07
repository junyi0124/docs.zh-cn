---
title: 相关概述
ms.date: 03/30/2017
ms.assetid: edcc0315-5d26-44d6-a36d-ea554c418e9f
ms.openlocfilehash: 3cc0bc49ad464401ccff769fd5873d5b7e19dccc
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293710"
---
# <a name="correlation-overview"></a>相关概述

相关是指使工作流服务消息彼此相关或与应用程序实例状态相关的机制，例如，使答复与初始请求相关，或者使特定订单 ID 与订单处理工作流的保留状态相关。 本主题概述了相关。 本节中的其他主题提供了相关的各种类型的附加信息。  
  
## <a name="types-of-correlation"></a>相关的类型  

 相关可以基于协议，也可以基于内容。 基于协议的相关使用消息传递基础结构提供的数据在消息之间提供映射。 使用基于协议的相关而彼此相关的消息通过使用内存中的对象（如 <xref:System.ServiceModel.Channels.RequestContext>）或通过传输协议提供的标记而彼此相关。 基于内容的相关使用应用程序特定数据使消息彼此相关。 使用基于内容的相关而彼此相关的消息通过消息中某些由应用程序定义的数据（如客户编号）彼此相关。  
  
 参与相关的活动使用 <xref:System.ServiceModel.Activities.CorrelationHandle> 将消息传递活动绑定在一起。 例如，<xref:System.ServiceModel.Activities.Send>（用于调用服务）和后续的 <xref:System.ServiceModel.Activities.Receive>（用于接收服务发送的回调）共享同一 <xref:System.ServiceModel.Activities.CorrelationHandle>。 无论相关是基于内容还是基于协议，都会使用此基本模式。 可以在每个活动上显式设置相关句柄，也可以将活动包含在 <xref:System.ServiceModel.Activities.CorrelationScope> 活动中。 <xref:System.ServiceModel.Activities.CorrelationScope> 中包含的活动的相关句柄由 <xref:System.ServiceModel.Activities.CorrelationScope> 管理，这些活动不需要显式设置 <xref:System.ServiceModel.Activities.CorrelationHandle>。 <xref:System.ServiceModel.Activities.CorrelationScope> 范围为请求-答复相关提供 <xref:System.ServiceModel.Activities.CorrelationHandle> 管理，并提供一个附加相关类型。 使用 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 承载的工作流服务与 <xref:System.ServiceModel.Activities.CorrelationScope> 活动具有相同的默认相关管理。 此默认相关管理通常意味着在许多情况下，<xref:System.ServiceModel.Activities.CorrelationScope> 或工作流服务中的消息传递活动不需要设置其 <xref:System.ServiceModel.Activities.CorrelationHandle>，除非有多个消息传递活动并行或重叠，例如，有两个并行 <xref:System.ServiceModel.Activities.Receive> 活动，或两个 <xref:System.ServiceModel.Activities.Send> 活动后跟两个 <xref:System.ServiceModel.Activities.Receive> 活动。 本节中的主题涵盖每种特定的相关类型，并提供了有关默认相关的更多信息。 有关消息传递活动的详细信息，请参阅 [消息传递活动](messaging-activities.md) 和 [如何：使用消息传递活动创建工作流服务](how-to-create-a-workflow-service-with-messaging-activities.md)。  
  
## <a name="protocol-based-correlation"></a>基于协议的相关

基于协议的相关使用传输机制使消息彼此相关或与相应实例相关。 系统提供的一些协议相关包括请求-答复相关和基于上下文的相关。 请求-答复相关用于将一对消息传递活动相关以形成双向操作，如 <xref:System.ServiceModel.Activities.Send> 与 <xref:System.ServiceModel.Activities.ReceiveReply> 成对，或 <xref:System.ServiceModel.Activities.Receive> 与 <xref:System.ServiceModel.Activities.SendReply> 成对。 Visual Studio 工作流设计器还提供了一组活动模板来快速实现此模式。 基于上下文的相关基于 [.Net 上下文交换协议规范](/openspecs/windows_protocols/mc-netcex/a7f26280-491f-465b-9914-c5eb5322dbb4)中描述的上下文交换机制。 若要使用基于上下文的相关，必须在终结点上使用诸如 <xref:System.ServiceModel.BasicHttpContextBinding>、<xref:System.ServiceModel.WSHttpContextBinding> 或 <xref:System.ServiceModel.NetTcpContextBinding> 之类的基于上下文的绑定。  
  
有关协议关联的详细信息，请参阅 [持久双工](durable-duplex-correlation.md) 和 [请求-答复](request-reply-correlation.md)。 有关使用 Visual Studio 工作流设计器活动模板的详细信息，请参阅 [消息传递活动](messaging-activities.md)。 有关示例代码，请参阅 [NetContextExchangeCorrelation](/previous-versions/dotnet/netframework-4.0/ee662963(v=vs.100)) 示例。  
  
## <a name="content-based-correlation"></a>基于内容的相关

基于内容的相关使用消息中的某些信息片段将消息与特定实例关联。 与基于协议的相关不同，基于内容的相关要求应用程序作者显式声明此数据在各相关消息中的位置。 使用基于内容相关的活动通过使用 <xref:System.ServiceModel.MessageQuerySet> 指定此消息数据。 与不使用某个上下文绑定（如 <xref:System.ServiceModel.BasicHttpContextBinding>）的服务进行通信时，基于内容的相关非常有用。
  
## <a name="see-also"></a>另请参阅

- [NetContextExchangeCorrelation](/previous-versions/dotnet/netframework-4.0/ee662963(v=vs.100))
