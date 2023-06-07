---
title: 上下文交换协议
ms.date: 03/30/2017
ms.assetid: 3dfd38e0-ae52-491c-94f4-7a862b9843d4
ms.openlocfilehash: ba613a2d12843ad00034057f8bbf08d5357d7f04
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96237815"
---
# <a name="context-exchange-protocol"></a>上下文交换协议

本部分介绍 Windows Communication Foundation (WCF .NET Framework) 版本3.5 中引入的上下文交换协议。 此协议允许客户端通道接受某个服务提供的上下文，并将其应用于通过相同客户端通道实例发送的针对该服务的所有后续请求。 上下文交换协议的实现可以使用以下两个机制之一在服务器和客户端之间传播上下文：HTTP Cookie 或 SOAP 标头。  
  
 上下文交换协议是在自定义通道层中实现的。 通道使用 <xref:System.ServiceModel.Channels.ContextMessageProperty> 属性在应用程序层之间来回传递上下文。 对于终结点之间的传输，上下文的值或者在通道层序列化为 SOAP 标头，或者在表示 HTTP 请求和响应的消息属性之间来回转换。 在后一种情况下，应有一个基础通道层将 HTTP 请求和响应消息属性分别与 HTTP Cookie 来回进行转换。 可通过使用 <xref:System.ServiceModel.Channels.ContextExchangeMechanism> 上的 <xref:System.ServiceModel.Channels.ContextBindingElement> 属性来选择用于交换上下文的机制。 有效值为 `HttpCookie` or `SoapHeader`进行求值的基于 SQL 语言的筛选器表达式。  
  
 在客户端上，通道的实例可以基于通道属性 <xref:System.ServiceModel.Channels.IContextManager.Enabled%2A> 上的设置在两种模式中操作。  
  
## <a name="mode-1-channel-context-management"></a>模式 1：通道上下文管理  

 这是默认模式，其中将 <xref:System.ServiceModel.Channels.IContextManager.Enabled%2A> 设置为 `true`。 在此模式中，上下文通道将管理上下文，并在上下文的生存期内将其缓存。 通过调用 `IContextManager` 方法，可以通过通道属性 `GetContext` 从通道检索上下文。 在通过对通道属性调用 `SetContext` 方法打开通道之前，还可以使用特定的上下文来预先初始化通道。 在使用上下文初始化通道之后，将不能重置通道。  
  
 以下是此模式中的固定行为的列表：  
  
- 任何试图在已经打开通道之后使用 `SetContext` 来重置上下文的操作都会引发 <xref:System.InvalidOperationException>。  
  
- 任何试图在传出消息中使用 <xref:System.ServiceModel.Channels.ContextMessageProperty> 来发送上下文的操作都会引发 <xref:System.InvalidOperationException>。  
  
- 在已使用特定的上下文对通道进行初始化之后，如果使用特定的上下文从服务器接收消息，则会导致 <xref:System.ServiceModel.ProtocolException>。  
  
    > [!NOTE]
    > 仅在打开通道时没有显式设置任何上下文的情况下，从服务器上接收初始上下文才是合适的。  
  
- 传入消息上的 <xref:System.ServiceModel.Channels.ContextMessageProperty> 始终为空。  
  
## <a name="mode-2-application-context-management"></a>模式 2：应用程序上下文管理  

 在此模式中，<xref:System.ServiceModel.Channels.IContextManager.Enabled%2A> 设置为 `false`。 在此模式中，上下文通道并不对上下文进行管理， 而是由应用程序负责通过使用 <xref:System.ServiceModel.Channels.ContextMessageProperty> 来检索、管理和应用上下文。 任何尝试调用 `GetContext` 或 `SetContext` 的操作都会导致 <xref:System.InvalidOperationException>。  
  
 无论选择哪种模式，客户端通道工厂都支持 <xref:System.ServiceModel.Channels.IRequestChannel>、<xref:System.ServiceModel.Channels.IRequestSessionChannel> 和 <xref:System.ServiceModel.Channels.IDuplexSessionChannel> 消息交换模式。  
  
 在服务上，通道的实例负责将客户端提供的传入消息的上下文转换为 <xref:System.ServiceModel.Channels.ContextMessageProperty>。 然后应用程序层或调用堆栈中更上层的其他通道就可以访问消息属性。 服务通道还允许应用程序层通过将 <xref:System.ServiceModel.Channels.ContextMessageProperty> 附加到响应消息，指定要传播回客户端的新上下文值。 此属性将会转换为包含上下文的 SOAP 标头或 HTTP Cookie，具体取决于绑定的配置。 服务通道侦听器支持 <xref:System.ServiceModel.Channels.IReplyChannel>、<xref:System.ServiceModel.Channels.IReplySessionChannel> 和 <xref:System.ServiceModel.Channels.IReplySessionChannel> 消息交换模式。  
  
 上下文交换协议引入了一个新的 `wsc:Context` SOAP 标头，以便在未使用 HTTP Cookie 来传播上下文的情况下表示上下文信息。 上下文标头架构允许任意数量的子元素，每个元素都有一个字符串键和字符串内容。 下面是一个上下文标头示例。  
  
 `<Context xmlns="http://schemas.microsoft.com/ws/2006/05/context">`  
  
 `<property name="myContext">context-2</property>`  
  
 `</Context>`  
  
 在 `HttpCookie` 模式中，将使用 `SetCookie` 标头设置 Cookie。 相应的 Cookie 名称为 `WscContext`。 Cookie 的值是 `wsc:Context` 标头的 Base64 编码。 此值包含在引号中。  
  
 在传输过程中必须保护上下文的值，使之不会被修改，其原因与保护 WS-Addressing 标头的原因相同（标头可用于确定在将请求调度到服务上的何处）。 因此，当绑定提供消息保护功能时，需要在 SOAP 或传输级对 `wsc:Context` 标头进行数字签名或进行签名并加密。 当使用 HTTP Cookie 来传播上下文时，应使用传输安全来保护 HTTP Cookie。  
  
 要求支持上下文交换协议的服务终结点可以在其发布的策略中明确此要求。 已经引入了两个新的策略断言来表示要求客户端在 SOAP 级支持上下文交换协议或启用 HTTP Cookie 支持。 由 <xref:System.ServiceModel.Channels.ContextBindingElement.ContextExchangeMechanism%2A> 属性控制如何将这些断言生成到服务策略中，如下所示：  
  
- 对于 <xref:System.ServiceModel.Channels.ContextExchangeMechanism.ContextSoapHeader>，将生成以下断言：  
  
    ```xml  
    <IncludeContext
    xmlns="http://schemas.microsoft.com/ws/2006/05/context"  
    protectionLevel="Sign" />  
    ```  
  
- 对于 <xref:System.ServiceModel.Channels.ContextExchangeMechanism.HttpCookie>，将生成以下断言：  
  
    ```xml  
    <HttpUseCookie xmlns="http://schemas.xmlsoap.org/soap/http"/>  
    ```  
  
## <a name="see-also"></a>另请参阅

- [Web 服务协议互操作性指南](web-services-protocols-interoperability-guide.md)
