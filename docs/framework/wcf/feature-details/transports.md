---
title: Windows Communication Foundation 中的传输
ms.date: 03/30/2017
helpviewer_keywords:
- transports [WCF]
- WCF, transports
- Windows Communication Foundation, transports
ms.assetid: 005c894b-af70-48aa-a1c1-c99338083c27
ms.openlocfilehash: 8623b788b848867f25836a657b07349dd50c2780
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251680"
---
# <a name="transports-in-windows-communication-foundation"></a>Windows Communication Foundation 中的传输

传输层是通道堆栈的最低层。 Windows Communication Foundation 中使用的主要传输 (WCF) 是 HTTP、HTTPS、TCP 和命名管道。 本节中的主题讨论如何选择传输、配置传输和设置优化属性。  
  
 WCF 包括其他传输。 有关消息队列 (也称为 MSMQ) 传输的信息，请参阅 [队列和可靠会话](queues-and-reliable-sessions.md)。 有关对等传输的信息，请参阅对等 [网络](peer-to-peer-networking.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [选择传输方式](choosing-a-transport.md)  
 说明三种主要传输和选择其中一种传输时的注意事项。  
  
 [选择消息编码器](choosing-a-message-encoder.md)  
 说明选择消息编码绑定元素时要考虑的因素。  
  
 [流消息传输](streaming-message-transfer.md)  
 说明如何配置传输层进行流式处理。  
  
 [配置 HTTP 和 HTTPS](configuring-http-and-https.md)  
 说明如何配置 HTTP 和 HTTPS 传输绑定元素。  
  
 [如何：用受限预留替换 WCF URL 预留](how-to-replace-the-wcf-url-reservation-with-a-restricted-reservation.md)  
 介绍如何使用 WCFURL 受限预订。  
  
 [传输配额](transport-quotas.md)  
 说明设置传输层中可用配额时的注意事项。  
  
 [使用 NAT 和防火墙](working-with-nats-and-firewalls.md)  
 说明在防火墙后发送或接收消息时或存在网络地址转换 (NAT) 时如何配置传输层。  
  
 [Net.TCP 端口共享](net-tcp-port-sharing.md)  
 介绍如何使用 WCF 的 Net.tcp 端口共享组件。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel.Channels.HttpTransportBindingElement>  
  
 <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>  
  
 <xref:System.ServiceModel.Channels.TcpTransportBindingElement>  
  
 <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement>  
  
## <a name="related-sections"></a>相关章节  

 [绑定](bindings.md)  
  
 [扩展绑定](../extending/extending-bindings.md)
