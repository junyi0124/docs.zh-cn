---
title: Windows Communication Foundation 绑定
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], bindings
- Windows Communication Foundation [WCF], bindings
- bindings [WCF]
ms.assetid: 83639133-89f7-43f0-b4ef-8d9e57c08d25
ms.openlocfilehash: 50c430489144589f6deb0930aeb3006bcb7efe8c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262094"
---
# <a name="windows-communication-foundation-bindings"></a>Windows Communication Foundation 绑定

Windows Communication Foundation (WCF) 分隔应用程序的软件如何从它与其他软件通信的方式进行编写。 使用绑定指定客户端与服务彼此通信所需的传输、编码和协议详细信息。 WCF 使用绑定来生成终结点的基础线路表示形式，因此，通信的各方必须对大多数绑定详细信息达成一致。 完成此任务的最简单方法是让服务的客户端使用该服务的终结点所使用的相同绑定。 有关如何执行此操作的详细信息，请参阅 [使用绑定配置服务和客户端](../using-bindings-to-configure-services-and-clients.md)。  
  
 绑定由绑定元素的集合组成。 每个元素描述终结点与客户端的通信方式的某一方面。 一个绑定必须至少包括一个传输绑定元素，至少包括一个消息编码绑定元素（默认情况下，传输绑定元素可能提供该元素）以及包括任意数目的其他协议绑定元素。 使用本说明中生成运行时的进程，每个绑定元素均可为该运行时提供代码。  
  
 WCF 提供的绑定包含绑定元素的常见选择。 这些绑定可以与其默认设置一起使用，您也可以根据用户需求修改这些默认值。 这些系统提供的绑定具有一些属性，可以通过这些属性直接控制绑定元素及其设置。 通过为绑定的每个版本提供其自己的名称，还可以轻松地并行使用该绑定的多个版本。 有关详细信息，请参阅 [配置 System-Provided 绑定](configuring-system-provided-bindings.md)。  
  
 如果需要的绑定元素集合不是由系统提供的这些绑定之一提供，则可以创建由所需绑定元素的集合组成的自定义绑定。 这些自定义绑定易于创建且不需要新类，但是，它们不提供用于控制绑定元素或其设置的属性。 您可以访问这些绑定元素并通过包含它们的集合修改其设置。 有关详细信息，请参阅 [自定义绑定](../extending/custom-bindings.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [配置系统提供的绑定](configuring-system-provided-bindings.md)  
 描述如何使用和修改 WCF 提供的绑定以支持常见方案。  
  
 [使用绑定配置服务和客户端](../using-bindings-to-configure-services-and-clients.md)  
 介绍如何在代码中以强制方式为服务和客户端定义 Windows Communication Foundation (WCF) 绑定，并使用配置以声明方式定义。  
  
 [自定义绑定](../extending/custom-bindings.md)  
 描述什么是 <xref:System.ServiceModel.Channels.CustomBinding> 以及何时使用它。  
  
## <a name="reference"></a>参考  

 <xref:System.ServiceModel.Channels.Binding>  
  
 <xref:System.ServiceModel.Channels.BindingElement>  
  
 <xref:System.ServiceModel.Channels.CustomBinding>  
  
## <a name="related-sections"></a>相关章节  

 [扩展绑定](../extending/extending-bindings.md)
