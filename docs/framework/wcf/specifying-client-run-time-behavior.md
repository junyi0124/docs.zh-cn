---
title: 指定客户端运行时行为
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- behaviors [WCF], system-provided client
ms.assetid: d16d3405-be70-4edb-8f62-b5f614ddeca5
ms.openlocfilehash: 17031f2100c6760cd14aae57cd4efab7428eb362
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96235891"
---
# <a name="specifying-client-run-time-behavior"></a>指定客户端运行时行为

Windows Communication Foundation 可以将 (WCF) 客户端（如 Windows Communication Foundation (WCF) services）配置为修改运行时行为，以适合客户端应用程序。 有三个属性可用于指定客户端运行时行为。 双工客户端回调对象可以使用 <xref:System.ServiceModel.CallbackBehaviorAttribute> 和 <xref:System.ServiceModel.Description.CallbackDebugBehavior> 属性修改其运行时行为。 另一个属性 <xref:System.ServiceModel.Description.ClientViaBehavior> 可用于将逻辑目标与直接网络目标分开。 此外，双工客户端回调类型可以使用某些服务端行为。 有关详细信息，请参阅 [指定服务 Run-Time 行为](specifying-service-run-time-behavior.md)。  
  
## <a name="using-the-callbackbehaviorattribute"></a>使用 CallbackBehaviorAttribute  

 可以使用 <xref:System.ServiceModel.CallbackBehaviorAttribute> 类来配置或扩展客户端应用程序中回调协定实现的执行行为。 此属性为回调类和 <xref:System.ServiceModel.ServiceBehaviorAttribute> 类执行相似的功能，不同之处在于实例化行为和事务设置。  
  
 必须将 <xref:System.ServiceModel.CallbackBehaviorAttribute> 类应用于实现回调协定的类。 如果将其应用于非双工协定实现，则会在运行时引发 <xref:System.InvalidOperationException> 异常。 下面的代码示例演示回调对象上的一个 <xref:System.ServiceModel.CallbackBehaviorAttribute> 类，该类使用 <xref:System.Threading.SynchronizationContext> 对象确定要封送到的线程，使用 <xref:System.ServiceModel.CallbackBehaviorAttribute.ValidateMustUnderstand%2A> 属性强制执行消息验证，并使用 <xref:System.ServiceModel.CallbackBehaviorAttribute.IncludeExceptionDetailInFaults%2A> 属性将异常作为 <xref:System.ServiceModel.FaultException> 对象返回给服务以便进行调试。  
  
 [!code-csharp[CallbackBehaviorAttribute#3](../../../samples/snippets/csharp/VS_Snippets_CFX/callbackbehaviorattribute/cs/client.cs#3)]
 [!code-vb[CallbackBehaviorAttribute#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/callbackbehaviorattribute/vb/client.vb#3)]  
  
## <a name="using-callbackdebugbehavior-to-enable-the-flow-of-managed-exception-information"></a>使用 CallbackDebugBehavior 启用托管异常信息流  

 可以在客户端回调对象中启用托管异常信息流，使异常信息流回到服务以便进行调试，方法是以编程方式或从应用程序配置文件中将 <xref:System.ServiceModel.Description.CallbackDebugBehavior.IncludeExceptionDetailInFaults%2A> 属性设置为 `true`。  
  
 将托管异常信息返回到服务可能存在安全风险，因为异常详细信息会公开与内部客户端实现有关的信息，而未经授权的服务可能会使用这些信息。 此外，虽然 <xref:System.ServiceModel.Description.CallbackDebugBehavior> 属性也可以通过编程方式进行设置，但在部署时容易忘记禁用 <xref:System.ServiceModel.Description.CallbackDebugBehavior.IncludeExceptionDetailInFaults%2A>。  
  
 由于涉及到一些安全问题，因此强烈建议您：  
  
- 使用应用程序配置文件将 <xref:System.ServiceModel.Description.CallbackDebugBehavior.IncludeExceptionDetailInFaults%2A> 属性的值设置为 `true`。  
  
- 仅在受控调试方案中才能这样做。  
  
 下面的代码示例演示一个客户端配置文件，该文件指示 WCF 从 SOAP 消息中的客户端回调对象返回托管异常信息。  
  
 [!code-xml[SCA.CallbackContract#4](../../../samples/snippets/csharp/VS_Snippets_CFX/sca.callbackcontract/cs/client.exe.config#4)]  

## <a name="using-the-clientviabehavior-behavior"></a>使用 ClientViaBehavior 行为  

 可以使用 <xref:System.ServiceModel.Description.ClientViaBehavior> 行为指定应为其创建传输通道的统一资源标识符。 当直接网络目标不是消息的预期处理者时，可使用此行为。 当调用应用程序不需要知道最终目标时，或者当目标 `Via` 标头不是地址时，使用此行为可启用多跃点对话。  
  
## <a name="see-also"></a>另请参阅

- [指定服务运行时行为](specifying-service-run-time-behavior.md)
