---
title: 扩展 ServiceHost 和服务模块层
ms.date: 03/30/2017
helpviewer_keywords:
- extending service models [WCF]
ms.assetid: 954c138a-1cd0-45a0-8abe-e4d2b8ff5400
ms.openlocfilehash: 184719f5c3e2e3830d7e1c9c69b73649b66fff34
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273030"
---
# <a name="extending-servicehost-and-the-service-model-layer"></a>扩展 ServiceHost 和服务模块层

服务模型层负责从基础通道提取出传入的消息，将它们翻译成应用程序代码形式的方法调用，并将结果发送回调用方。 服务模型扩展将修改或实现执行或通信行为与功能，包括客户端或调度程序功能、自定义行为、消息和参数截获以及其他扩展功能。  
  
## <a name="in-this-section"></a>本节内容  

 [扩展客户端](extending-clients.md)  
 描述可以截获和修改客户端运行时的接口以及可以向其中插入客户端应用程序中的自定义扩展的类。 例如，您可以执行自定义客户端消息记录，执行自定义消息序列化，等等。  
  
 [扩展调度程序](extending-dispatchers.md)  
 描述可以截获和修改服务运行时的接口以及可以向其中插入服务应用程序中的自定义扩展的类。 例如，您可以执行自定义服务记录、服务侧消息验证、自定义调度，等等。  
  
 [可扩展对象](extensible-objects.md)  
 描述五个可扩展对象和 <xref:System.ServiceModel.IExtensibleObject%601> 模式。 可扩展对象模式用于使用新功能扩展现有运行时类，或者向对象中添加新状态。 附加到可扩展对象之一的扩展名，在访问附加到公共可扩展对象的共享状态和功能过程的各个不同阶段启用行为，各可扩展对象可以访问该公共扩展对象。  
  
 [使用行为配置和扩展运行时](configuring-and-extending-the-runtime-with-behaviors.md)  
 若要在 WCF 运行时中更改设置或插入扩展，请使用行为。 WCF 包括系统实现的用于控制遏制、实例化和大量服务及操作的其他方面的行为。 本节介绍如何通过编程方式和使用配置文件来创建您自己的自定义行为，以及如何使这些自定义行为变为可用。  
  
 [使用 ServiceHostFactory 扩展宿主](extending-hosting-using-servicehostfactory.md)  
 描述如何扩展 <xref:System.ServiceModel.ServiceHostBase?displayProperty=nameWithType> 和 <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> 以及使用 <xref:System.ServiceModel.Activation.ServiceHostFactory?displayProperty=nameWithType> 类来自定义主机环境。  
  
## <a name="reference"></a>参考  
  
## <a name="related-sections"></a>相关章节
