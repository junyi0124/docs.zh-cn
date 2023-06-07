---
title: 如何：检查和修改服务上的消息
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9c5b1cc7-84f3-45f8-9226-d59c278e8c42
ms.openlocfilehash: 8d1ce6ef00462adb38e3d59c3d9bd35694c4dbe9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249054"
---
# <a name="how-to-inspect-and-modify-messages-on-the-service"></a>如何：检查和修改服务上的消息

通过实现 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector?displayProperty=nameWithType> 并将其插入到服务运行时中，可以通过 Windows Communication Foundation (WCF) 客户端检查或修改传入或传出消息。 有关详细信息，请参阅 [扩展调度](extending-dispatchers.md)程序。 服务上的等效功能为 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType>。  
  
### <a name="to-inspect-or-modify-messages"></a>检查或修改消息  
  
1. 实现 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector?displayProperty=nameWithType> 接口。  
  
2. 根据您希望轻松插入服务消息检查器的范围，实现 <xref:System.ServiceModel.Description.IServiceBehavior?displayProperty=nameWithType>、<xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType> 接口。  
  
3. 在 <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> 上调用 <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> 方法之前，先插入行为。 有关详细信息，请参阅 [配置和扩展运行时的行为](configuring-and-extending-the-runtime-with-behaviors.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例按顺序演示以下各项：  
  
- 服务检查器实现。  
  
- 插入检查器的服务行为。  
  
- 在服务应用程序中加载和运行该行为的配置文件。  
  
 [!code-csharp[Interceptors#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/interceptors/cs/interceptors.cs#7)]
 [!code-vb[Interceptors#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/interceptors/vb/interceptors.vb#7)]  
  
 [!code-csharp[Interceptors#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/interceptors/cs/insertingbehaviors.cs#8)]
 [!code-vb[Interceptors#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/interceptors/vb/insertingbehaviors.vb#8)]  
  
 [!code-xml[Interceptors#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/interceptors/cs/hostapplication.exe.config#9)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Dispatcher.IClientMessageInspector?displayProperty=nameWithType>
- <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector?displayProperty=nameWithType>
- [使用行为配置和扩展运行时](configuring-and-extending-the-runtime-with-behaviors.md)
