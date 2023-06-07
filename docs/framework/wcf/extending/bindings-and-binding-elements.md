---
title: 绑定和绑定元素
ms.date: 03/30/2017
helpviewer_keywords:
- binding elements [WCF]
ms.assetid: 765ff77b-7682-4ea3-90eb-e4d751e37379
ms.openlocfilehash: 16e1b7840be6a3ec247033fa3a2eada9e5799bdb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96262133"
---
# <a name="bindings-and-binding-elements"></a>绑定和绑定元素

绑定是特殊配置元素（称为 *绑定元素*）的集合，每当构造客户端或服务终结点时，服务运行时都将对这些元素进行评估。 绑定内绑定元素的类型和顺序确定了终结点的通道堆栈中协议和传输通道的选择和堆叠顺序。  
  
 绑定（尤其是系统提供的绑定）通常还具有很多配置属性，这些属性反映了已封装绑定元素的最经常修改的属性。  
  
 绑定必须恰好包含一个传输协议绑定元素。 每个传输协议绑定元素都隐含一个默认的消息编码绑定元素，该元素可以通过向绑定中最多添加一个消息编码绑定元素进行重写。 除了传输协议和编码器绑定元素以外，绑定还可能包含任意数量的协议绑定元素，这些元素共同实现从一个终结点向其他终结点提供和发送 SOAP 消息所需的功能。 有关详细信息，请参阅 [使用绑定配置服务和客户端](../using-bindings-to-configure-services-and-clients.md)。  
  
## <a name="extending-bindings-and-binding-elements"></a>扩展绑定和绑定元素  

 Windows Communication Foundation (WCF) 包括系统提供的绑定，涵盖各种方案。 有关详细信息，请参阅 [系统提供的绑定](../system-provided-bindings.md)。在某些情况下，可能需要创建和使用未包含在 WCF 中的绑定，但可能存在一些情况下 ) 的 (。 下面的方案需要创建新绑定。  
  
- 若要使用新的绑定元素（例如，新的传输协议、编码或协议绑定元素），您必须创建一个包含该绑定元素的新绑定。 例如，如果你为 UDP 传输协议添加了一个自定义 `UdpTransportBindingElement`，则需要创建一个新绑定来利用它。 有关使用类型执行此行为的信息 <xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> ，请参阅 [自定义绑定](custom-bindings.md)。  
  
- 以系统提供的绑定没有在公共属性上公开的方式配置现有绑定元素。 例如，必须创建一个新绑定以更改签名和加密操作的执行顺序。 有关执行此行为的信息，请参阅 [如何：自定义 System-Provided 绑定](how-to-customize-a-system-provided-binding.md)。  
  
- 建立仅公开特定配置选项的公司标准绑定。 例如，若要为你的公司创建不能禁用安全的 <xref:System.ServiceModel.WSHttpBinding> 变体，可以创建一个行为方式类似于 <xref:System.ServiceModel.WSHttpBinding> 但总是启用安全的新绑定。 有关详细信息，请参阅 [创建 User-Defined 绑定](creating-user-defined-bindings.md)。  
  
- 若要执行某种元数据自定义，通常可以配置或使用某个自定义绑定元素，但并非必须这样做。 有关为绑定和绑定元素提供元数据支持的详细信息，请参阅 [配置和元数据支持](configuration-and-metadata-support.md)。  

## <a name="channels-bindings-and-binding-elements"></a>通道、绑定和绑定元素  

 绑定和绑定元素是应用程序编程模型（包括属性和行为）与通道模型（包括工厂和侦听器、消息编码器以及传输协议和协议实现）之间的联系。 通常，实现绑定元素和绑定的目的是使通道能够被应用程序层使用。  
  
 通道层向服务层发送消息或从服务层接收消息，并且在终结点之间传输这些消息。 在客户端上，通道层是一个通道工厂堆栈，这些通道工厂创建通向网络终结点的通道。 在服务上，通道层是一个通道侦听器堆栈，这些侦听器接受在网络终结点收到的通道。  
  
 有两种常规类型的通道：协议通道和传输通道。 传输通道负责在网络终结点之间实际传输消息。 传输通道必须具有默认消息编码器，并且应该能够使用通过消息编码器绑定元素提供的备用消息编码器。 消息编码器负责将 <xref:System.ServiceModel.Channels.Message?displayProperty=nameWithType> 转换为网络表示以及执行反向转换。 协议通道负责实现 SOAP 级协议（例如，WS-Security 或 WS-ReliableMessaging）。  
  
 传输通道和协议通道的主要要求是它们需要实现必需的通道接口。 若要创建有效的通道层，它们必须具有关联的工厂和侦听器，等等。 若要从 WCF 使用通道实现，需要为每个通道从派生的关联绑定元素 <xref:System.ServiceModel.Channels.BindingElement> ，并且应存在一个要包含在派生自的配置文件中的相关绑定扩展元素 <xref:System.ServiceModel.Configuration.BindingElementExtensionElement> 。  
  
 如前所述，可以将消息编码器、协议和传输通道实现的绑定元素堆叠起来，以形成一个通道堆栈，而用来将它们组织成一个有序集的机制就是绑定。 绑定和绑定元素将应用程序编程模型与通道模型联系起来。 您可以在代码中直接使用自己的通道实现，但是除非将编码器、传输和协议实现为绑定元素，否则无法在服务层编程模型中使用它们。  
  
 有关开发通道及其绑定元素的详细信息，请参阅 [扩展通道层](extending-the-channel-layer.md)。
