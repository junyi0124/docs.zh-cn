---
title: 如何：创建要求会话的服务
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8a7613ef-0df9-47c3-b8dc-47f42cb1fd8b
ms.openlocfilehash: 13287d0d5c989fc3a5dc95c6df5d548bca9df4d8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286352"
---
# <a name="how-to-create-a-service-that-requires-sessions"></a>如何：创建要求会话的服务

会话在两个或更多终结点之间创建一个共享状态，从而启用一些有用的功能，例如回调、多跳安全性以及客户端和服务实例之间的关联。 有关 Windows Communication Foundation 中的会话 (WCF) 应用程序的详细信息，请参阅 [使用会话](../using-sessions.md)。  
  
### <a name="to-specify-that-a-contract-require-its-binding-to-support-sessions"></a>指定协定需要其绑定来支持会话  
  
1. 创建至少包含一个操作的服务协定。 有关如何创建服务协定的示例，请参阅 [如何：定义服务协定](../how-to-define-a-wcf-service-contract.md)。  
  
2. 修改声明协定的 <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType>，将 <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType> 属性设置为：  
  
    - <xref:System.ServiceModel.SessionMode.Required?displayProperty=nameWithType>（如果必须在会话中运行此协定）。  
  
    - <xref:System.ServiceModel.SessionMode.Allowed?displayProperty=nameWithType>（如果可以在会话中运行此协定）。  
  
    - <xref:System.ServiceModel.SessionMode.NotAllowed?displayProperty=nameWithType>（如果不得在会话中运行此协定）。  
  
3. 配置服务终结点以使用支持会话的绑定。 下面的配置示例演示了支持 WS<xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType>ReliableMessaging 会话的 `-` 的用法。  
  
     [!code-xml[SCA.Session#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/sca.session/cs/hostapplication.exe.config#2)]
  
## <a name="example"></a>示例  

 下面的示例代码演示如何指定协定级别会话需求并使用配置文件来支持 <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType> 绑定的需求。  
  
 [!code-csharp[SCA.Session#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/sca.session/cs/services.cs#1)]
 [!code-vb[SCA.Session#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/sca.session/vb/services.vb#1)]
 [!code-xml[SCA.Session#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/sca.session/cs/hostapplication.exe.config#2)]
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.ServiceContractAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A?displayProperty=nameWithType>
- <xref:System.ServiceModel.SessionMode?displayProperty=nameWithType>
