---
title: Windows Communication Foundation 绑定概述
description: 了解绑定，该绑定指定如何连接到 WCF 服务，其中包括绑定的元素，以及如何指定服务终结点的绑定。
ms.date: 03/30/2017
helpviewer_keywords:
- bindings [WCF], overview
ms.assetid: cfb5842f-e0f9-4c56-a015-f2b33f258232
ms.openlocfilehash: e918844342a20e7b6c22ef6a9fd3c01fe27db059
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272470"
---
# <a name="windows-communication-foundation-bindings-overview"></a>Windows Communication Foundation 绑定概述

绑定是用于指定连接到 Windows Communication Foundation (WCF) 服务的终结点所需的通信详细信息的对象。 WCF 服务中的每个端点都需要一个具体指定的绑定。 本主题概述绑定定义的通信详细信息的类型、绑定的元素、WCF 中包含的绑定以及如何为终结点指定绑定。  
  
## <a name="what-a-binding-defines"></a>绑定所定义的内容  

 绑定中的信息可能非常基本，也可能非常复杂。 最基本的绑定仅指定连接终结点所必需的传输协议（如 HTTP）。 一般来说，绑定包含的有关如何连接到终结点的信息属于以下类别之一：  
  
 **协议**  
 确定要使用的安全机制：可靠消息传递功能或事务上下文流设置。  
  
 **编码**  
 确定消息编码（例如，文本或二进制）。  
  
 **Transport**  
 确定要使用的基础传输协议（例如，TCP 或 HTTP）。  
  
## <a name="the-elements-of-a-binding"></a>绑定的元素  

 绑定基本上由绑定元素的有序堆栈组成，其中每个元素都指定了连接到服务终结点所必需的通信信息的一部分。 该堆栈中最底下的两层都是必需的。 该堆栈的最底层为传输绑定元素，而紧挨着它的上面一层为包含消息编码规范的元素。 用于指定其他通信协议的可选绑定元素位于这两个必需元素的上面。 有关这些绑定元素及其正确排序的详细信息，请参阅 [自定义绑定](./extending/custom-bindings.md)。  
  
## <a name="system-provided-bindings"></a>系统提供的绑定  

 绑定中的信息可能十分复杂，而且某些设置可能与其他设置不兼容。 出于此原因，WCF 包含一组系统提供的绑定。 这些绑定旨在满足大多数应用程序需求。 下面的类表示系统提供的绑定的一些示例：  
  
- <xref:System.ServiceModel.BasicHttpBinding>：一个 HTTP 协议绑定，适用于连接到符合 WS-I 基本配置文件规范的 Web 服务（例如，基于 ASP.NET Web 服务的服务）。  
  
- <xref:System.ServiceModel.WSHttpBinding>：一个可互操作的绑定，适用于连接到符合 WS-* 协议的终结点。  
  
- <xref:System.ServiceModel.NetNamedPipeBinding>：使用 .NET Framework 连接到同一计算机上的其他 WCF 终结点。  
  
- <xref:System.ServiceModel.NetMsmqBinding>：使用 .NET Framework 创建与其他 WCF 终结点的排队消息连接。  

- <xref:System.ServiceModel.NetTcpBinding>：此绑定提供比 HTTP 绑定更高的性能，非常适合在本地网络中使用。
  
 有关 WCF 提供的所有绑定的完整列表，请参阅 [系统提供的绑定](system-provided-bindings.md)。  
  
## <a name="using-your-own-bindings"></a>使用自己的绑定  

 如果系统提供的绑定都不具有服务应用程序所需的正确功能组合，则可以创建自己的绑定。 可通过两种方式来执行此操作。 您可以使用 <xref:System.ServiceModel.Channels.CustomBinding> 对象从预先存在的绑定元素创建新的绑定，也可以通过从 <xref:System.ServiceModel.Channels.Binding> 绑定派生来创建完全由用户定义的绑定。 有关使用这两种方法创建自己的绑定的详细信息，请参阅 [自定义绑定](./extending/custom-bindings.md) 和 [创建 User-Defined 绑定](./extending/creating-user-defined-bindings.md)。  
  
## <a name="using-bindings"></a>使用绑定  

 使用绑定需要执行两个基本步骤：  
  
1. 选择或定义绑定。 最简单的方法是选择 WCF 中包含的系统提供的绑定之一，并将其与默认设置一起使用。 您还可以选择一个系统提供的绑定，然后根据您的要求重新设置它的属性值。 或者，你可以创建一个自定义绑定或用户定义的绑定，以实现更高程度的控制和自定义。  
  
2. 创建一个使用所选择或定义的绑定的终结点。  
  
## <a name="code-and-configuration"></a>代码和配置  

 可以通过两种方式来定义绑定：通过代码或通过配置。 这两种方法与你使用的是系统提供的绑定还是自定义绑定无关。 通常，使用代码可以使你在设计时对绑定的定义拥有完全的控制。 另一方面，使用配置允许系统管理员或 WCF 服务或客户端的用户更改绑定的参数，而无需重新编译服务应用程序。 这种灵活性通常是必需的，因为无法预测要在其上部署 WCF 应用程序的特定计算机要求。 通过将绑定（和寻址）信息保持在代码外部，人们可以更改这些信息，而不必重新编译或重新部署应用程序。 请注意，代码中定义的绑定是在配置中指定的绑定之后创建的，这使得代码定义的绑定可以覆盖配置中定义的任何绑定。  
  
## <a name="see-also"></a>另请参阅

- [使用绑定配置服务和客户端](using-bindings-to-configure-services-and-clients.md)
