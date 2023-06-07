---
title: 如何：检查或修改参数
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ab6c0ac7-aac4-45ba-93d6-a0e9afd1756f
ms.openlocfilehash: e8b2674d8efc0ef3ac2f1dd6ab0df559195c274c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96249028"
---
# <a name="how-to-inspect-or-modify-parameters"></a>如何：检查或修改参数

您可以通过实现 <xref:System.ServiceModel.Dispatcher.IParameterInspector?displayProperty=nameWithType> 接口并将其插入到客户端或服务运行时，来检查或修改 Windows Communication Foundation (WCF) 客户端对象或 wcf 服务上的单个操作的传入或传出消息。 通常，操作行为用于为单个操作添加参数检查器；其他行为可以用于在更大范围内提供对运行库的方便访问。 有关详细信息，请参阅 [扩展客户端](extending-clients.md) 和 [扩展调度](extending-dispatchers.md)程序。  
  
### <a name="inspecting-or-modifying-parameters"></a>检查或修改参数  
  
1. 实现 <xref:System.ServiceModel.Dispatcher.IParameterInspector?displayProperty=nameWithType> 接口。  
  
2. 实现 <xref:System.ServiceModel.Description.IOperationBehavior?displayProperty=nameWithType>、<xref:System.ServiceModel.Description.IEndpointBehavior?displayProperty=nameWithType>、<xref:System.ServiceModel.Description.IServiceBehavior?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Description.IContractBehavior?displayProperty=nameWithType>（取决于要求的范围），以便将参数检查器添加到 <xref:System.ServiceModel.Dispatcher.ClientOperation.ParameterInspectors%2A?displayProperty=nameWithType> 或 <xref:System.ServiceModel.Dispatcher.DispatchOperation.ParameterInspectors%2A?displayProperty=nameWithType> 属性。  
  
3. 在对 <xref:System.ServiceModel.ClientBase%601.Open%2A?displayProperty=nameWithType> 调用 <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> 或 <xref:System.ServiceModel.ChannelFactory%601?displayProperty=nameWithType> 方法前，插入行为。 有关详细信息，请参阅 [配置和扩展运行时的行为](configuring-and-extending-the-runtime-with-behaviors.md)。  
  
## <a name="example"></a>示例  

 下面的代码示例按顺序演示以下各项：  
  
- 参数检查器实现。  
  
- 行为实现，它使用 、 和 插入参数检查器。  
  
- 配置文件，它在客户端应用程序中加载和运行终结点行为，以便在客户端上插入参数检查器。  
  
 [!code-csharp[Interceptors#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/interceptors/cs/interceptors.cs#4)]
 [!code-vb[Interceptors#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/interceptors/vb/interceptors.vb#4)]  
  
 [!code-csharp[Interceptors#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/interceptors/cs/insertingbehaviors.cs#5)]
 [!code-vb[Interceptors#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/interceptors/vb/insertingbehaviors.vb#5)]  
  
 [!code-xml[Interceptors#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/interceptors/cs/client.exe.config#3)]  
  
## <a name="see-also"></a>另请参阅

- [使用行为配置和扩展运行时](configuring-and-extending-the-runtime-with-behaviors.md)
