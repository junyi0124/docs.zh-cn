---
title: 如何：创建双工协定
description: 了解如何创建双工协定，这使得 WCF 客户端和服务器可以独立地相互通信。 可以启动对另一方的调用。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- duplex contracts [WCF]
ms.assetid: 500a75b6-998a-47d5-8e3b-24e3aba2a434
ms.openlocfilehash: cce1784865a1599e69c3f604c288ef62c9c43652
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96243711"
---
# <a name="how-to-create-a-duplex-contract"></a>如何：创建双工协定

本主题演示了创建使用双工（双向）协定的方法所需的基本步骤。 双工协定使得客户端和服务器可以独立地相互通信，这样双方都可以启动对另一方的呼叫。 双工协定是 Windows Communication Foundation (WCF) 服务可使用的三种消息模式之一。 另外两种消息模式是单向模式和请求-答复模式。 双工协定由客户端和服务器之间的两个单向协定组成，并且不需要方法调用是相关的。 当服务必须向客户端查询更多信息或在客户端上显式引发事件时，使用这种协定。 有关创建双工协定的客户端应用程序的详细信息，请参阅 [如何：使用双工协定访问服务](how-to-access-services-with-a-duplex-contract.md)。 有关工作示例，请参阅 [双工](../samples/duplex.md) 示例。  
  
### <a name="to-create-a-duplex-contract"></a>创建双工协定  
  
1. 创建组成双工协定的服务器方的接口。  
  
2. 将 <xref:System.ServiceModel.ServiceContractAttribute> 类应用到该接口。  
  
     [!code-csharp[S_WS_DualHttp#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#3)]
     [!code-vb[S_WS_DualHttp#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#3)]  
  
3. 在接口中声明方法签名。  
  
4. 将 <xref:System.ServiceModel.OperationContractAttribute> 类应用于每个方法签名，该方法签名必须是公共协定的一部分。  
  
5. 创建回调接口以定义服务可以在客户端上调用的操作组。  
  
     [!code-csharp[S_WS_DualHttp#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#4)]
     [!code-vb[S_WS_DualHttp#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#4)]  
  
6. 在回调接口中声明方法签名。  
  
7. 将 <xref:System.ServiceModel.OperationContractAttribute> 类应用于每个方法签名，该方法签名必须是公共协定的一部分。  
  
8. 通过将主接口中的 <xref:System.ServiceModel.ServiceContractAttribute.CallbackContract%2A> 属性设置为回调接口的类型，将两个接口链接到一个双工协定中。  
  
### <a name="to-call-methods-on-the-client"></a>在客户端上调用方法  
  
1. 在主协定的服务实现中，声明回调接口的变量。  
  
2. 将变量设置为通过 <xref:System.ServiceModel.OperationContext.GetCallbackChannel%2A> 类的 <xref:System.ServiceModel.OperationContext> 方法返回的对象引用。  
  
     [!code-csharp[S_WS_DualHttp#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#1)]
     [!code-vb[S_WS_DualHttp#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#1)]  
  
     [!code-csharp[S_WS_DualHttp#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#2)]
     [!code-vb[S_WS_DualHttp#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#2)]  
  
3. 调用由回调接口定义的方法。  
  
## <a name="example"></a>示例  

 下面的代码示例演示双工通信。 服务的协定包含用于向前移动和向后移动的服务操作。 客户端的协定包含用于报告其位置的服务操作。  
  
 [!code-csharp[S_WS_DualHttp#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_ws_dualhttp/cs/service.cs#5)]
 [!code-vb[S_WS_DualHttp#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_ws_dualhttp/vb/service.vb#5)]  
  
- 应用 <xref:System.ServiceModel.ServiceContractAttribute> 和 <xref:System.ServiceModel.OperationContractAttribute> 属性允许自动生成使用 Web 服务描述语言 (WSDL) 的服务协定定义。  
  
- 使用 "配置 [元数据实用工具" 工具 ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 检索 WSDL 文档，并为客户端 (可选的) 代码和配置。  
  
- 公开双工服务的终结点必须是安全的。 当服务接收双工消息时，它会查看传入消息中的 ReplyTo，以确定要发送答复的位置。 如果通道不安全，则不受信任的客户端可能使用目标计算机的 ReplyTo 发送恶意消息，从而导致目标计算机发生拒绝服务。 对于常规的规则请求-答复消息，这不是问题，因为将会忽略 ReplyTo 并在传入原始消息的通道上发送响应。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.ServiceContractAttribute>
- <xref:System.ServiceModel.OperationContractAttribute>
- [如何：使用双工协定访问服务](how-to-access-services-with-a-duplex-contract.md)
- [双工](../samples/duplex.md)
- [设计和实现服务](../designing-and-implementing-services.md)
- [如何：定义服务协定](../how-to-define-a-wcf-service-contract.md)
- [会话](../samples/session.md)
