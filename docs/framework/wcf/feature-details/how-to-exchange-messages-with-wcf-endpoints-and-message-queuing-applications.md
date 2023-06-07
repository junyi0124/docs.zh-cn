---
title: 如何：与 WCF 终结点和消息队列应用程序交换消息
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 62210fd8-a372-4d55-ab9b-c99827d1885e
ms.openlocfilehash: 8f8baf345059c01b0fef3b61ef85556151269118
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96246415"
---
# <a name="how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications"></a>如何：与 WCF 终结点和消息队列应用程序交换消息

您可以通过使用 MSMQ 集成绑定将 MSMQ 消息转换为 WCF 消息并将其从 WCF 消息集成，从而将 (MSMQ) 应用程序的现有消息队列与 Windows Communication Foundation (WCF) 应用程序集成。 这样，你就可以从 WCF 客户端调用 MSMQ 接收方应用程序，并从 MSMQ 发送方应用程序调用 WCF 服务。  
  
 在本部分中，我们将介绍如何使用 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 来处理 (1) WCF 客户端与使用 System. 消息编写的 msmq 应用程序服务和 (2) msmq 应用程序客户端和 WCF 服务之间的排队通信。  
  
 有关演示如何从 WCF 客户端调用 MSMQ 接收方应用程序的完整示例，请参阅 [Windows Communication Foundation 到消息队列](../samples/wcf-to-message-queuing.md) 的示例。  
  
 有关演示如何从 MSMQ 客户端调用 WCF 服务的完整示例，请参阅 [消息队列到 Windows Communication Foundation](../samples/message-queuing-to-wcf.md) 示例。  
  
### <a name="to-create-a-wcf-service-that-receives-messages-from-a-msmq-client"></a>创建从 MSMQ 客户端接收消息的 WCF 服务  
  
1. 定义一个接口，该接口定义从 MSMQ 发送方应用程序接收排队消息的 WCF 服务的服务协定，如下面的示例代码所示。  
  
     [!code-csharp[S_MsmqToWcf#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmqtowcf/cs/service.cs#1)]
     [!code-vb[S_MsmqToWcf#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmqtowcf/vb/service.vb#1)]  
  
2. 实现该接口，将 <xref:System.ServiceModel.ServiceBehaviorAttribute> 属性应用于该类，如下面示例代码所示。  
  
     [!code-csharp[S_MsmqToWcf#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmqtowcf/cs/service.cs#2)]
     [!code-vb[S_MsmqToWcf#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmqtowcf/vb/service.vb#2)]  
  
3. 创建一个配置文件，在该配置文件中指定 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>。  

4. 实例化一个 <xref:System.ServiceModel.ServiceHost> 对象，该对象使用所配置的绑定。  

### <a name="to-create-a-wcf-client-that-sends-messages-to-a-msmq-receiver-application"></a>创建向 MSMQ 接收方应用程序发送消息的 WCF 客户端  
  
1. 定义一个接口，该接口定义 WCF 客户端的服务协定，该服务协定向 MSMQ 接收方发送排队消息，如下面的示例代码所示。  
  
     [!code-csharp[S_WcfToMsmq#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/proxy.cs#6)]
     [!code-vb[S_WcfToMsmq#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_wcftomsmq/vb/proxy.vb#6)]  
  
2. 定义 WCF 客户端用于调用 MSMQ 接收方的客户端类。  
  
     [!code-csharp[S_WcfToMsmq#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/snippets.cs#2)]
     [!code-vb[S_WcfToMsmq#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_wcftomsmq/vb/snippets.vb#2)]  
  
3. 创建一个配置文件，在该配置文件中指定 MsmqIntegrationBinding 绑定的用法。  
  
     [!code-csharp[S_WcfToMsmq#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/snippets.cs#3)]
     [!code-vb[S_WcfToMsmq#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_wcftomsmq/vb/snippets.vb#3)]  
  
4. 创建该客户端类的一个实例，并调用消息接收服务所定义的方法。  
  
     [!code-csharp[S_WcfToMsmq#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/client.cs#4)]  
  
## <a name="see-also"></a>另请参阅

- [队列概述](queues-overview.md)
- [如何：使用 WCF 终结点交换排队消息](how-to-exchange-queued-messages-with-wcf-endpoints.md)
- [Windows Communication Foundation 到消息队列](../samples/wcf-to-message-queuing.md)
- [安装“消息队列 (MSMQ)”](../samples/installing-message-queuing-msmq.md)
- [到 Windows Communication Foundation 的消息队列](../samples/message-queuing-to-wcf.md)
- [基于消息队列的消息安全性](../samples/message-security-over-message-queuing.md)
