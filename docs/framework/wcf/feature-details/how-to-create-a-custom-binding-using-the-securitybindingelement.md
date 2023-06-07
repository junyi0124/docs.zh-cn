---
title: 如何：使用 SecurityBindingElement 创建自定义绑定
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [WCF], creating custom bindings
ms.assetid: 203a9f9e-3a73-427c-87aa-721c56265b29
ms.openlocfilehash: 9aaaf6a10e0c51db35720d72512c1a91cfbb9720
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96256737"
---
# <a name="how-to-create-a-custom-binding-using-the-securitybindingelement"></a>如何：使用 SecurityBindingElement 创建自定义绑定

Windows Communication Foundation (WCF) 包含多个系统提供的绑定，这些绑定可进行配置，但在配置 WCF 支持的所有安全选项时不能提供完全的灵活性。 本主题演示如何直接从各个绑定元素创建自定义绑定，并着重说明创建这样的绑定时可以指定的一些安全设置。 有关创建自定义绑定的详细信息，请参阅 [扩展绑定](../extending/extending-bindings.md)。  
  
> [!WARNING]
> <xref:System.ServiceModel.Channels.SecurityBindingElement> 不支持 <xref:System.ServiceModel.Channels.IDuplexSessionChannel> 通道形状，它是 TCP 传输在 <xref:System.ServiceModel.TransferMode> 设置为 <xref:System.ServiceModel.TransferMode.Buffered> 时使用的默认通道形状。 您必须将 <xref:System.ServiceModel.TransferMode> 设置为 <xref:System.ServiceModel.TransferMode.Streamed>，才能在此方案中使用 <xref:System.ServiceModel.Channels.SecurityBindingElement>。  
  
## <a name="creating-a-custom-binding"></a>创建自定义绑定  

 在 WCF 中，所有绑定都由 *绑定元素* 组成。 每个绑定元素都是从 <xref:System.ServiceModel.Channels.BindingElement> 类派生的。 对于系统提供的标准绑定，已为您创建和配置绑定元素，但您仍可以自定义某些属性设置。  
  
 相比之下，若要创建自定义绑定，应创建并配置绑定元素，然后从绑定元素创建 <xref:System.ServiceModel.Channels.CustomBinding>。  
  
 为此，将各个绑定元素添加到由 <xref:System.ServiceModel.Channels.BindingElementCollection> 类的实例表示的集合，然后将 `Elements` 的 `CustomBinding` 属性设置为等于该对象。 必须按以下顺序添加绑定元素：事务流、可靠会话、安全、复合双工、单向、流安全、消息编码和传输。 请注意，并不是每个绑定中都需要所列的所有绑定元素。  
  
## <a name="securitybindingelement"></a>SecurityBindingElement  

 有三个绑定元素与消息级安全性有关，它们都派生自 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类。 这三个绑定元素是 <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 和 <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>。 <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> 用于提供混合模式安全。 当消息层提供安全时，使用其他两个元素。  
  
 在提供传输级安全时，将使用其他类：  
  
- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>  
  
- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>  
  
## <a name="required-binding-elements"></a>必需的绑定元素  

 有大量的可能绑定元素可以组合为绑定。 并非所有这些组合都是有效的。 本节介绍安全绑定中必须存在的元素。  
  
 有效的安全绑定取决于多种因素，其中包括：  
  
- 安全模式。  
  
- 传输协议。  
  
- 协定中指定的消息交换模式 (MEP)。  
  
 下表显示上述因素各种组合的有效绑定元素组合配置。 请注意，这些是最低需求。 你可以向绑定添加更多绑定元素，如消息编码绑定元素、事务绑定元素和其他绑定元素。  
  
|安全模式|Transport|协定消息交换模式|协定消息交换模式|协定消息交换模式|  
|-------------------|---------------|---------------------------------------|---------------------------------------|---------------------------------------|  
|||`Datagram`|`Request Reply`|`Duplex`|  
|Transport|Https||||  
|||OneWayBindingElement|||  
|||HttpsTransportBindingElement|HttpsTransportBindingElement||  
||TCP||||  
|||OneWayBindingElement|||  
|||SSL 或 Windows StreamSecurityBindingElement|SSL 或 Windows StreamSecurityBindingElement|SSL 或 Windows StreamSecurityBindingElement|  
|||TcpTransportBindingElement|TcpTransportBindingElement|TcpTransportBindingElement|  
|消息|Http|SymmetricSecurityBindingElement|SymmetricSecurityBindingElement|SymmetricSecurityBindingElement（身份验证模式 = SecureConversation）|  
|||||CompositeDuplexBindingElement|  
|||OneWayBindingElement||OneWayBindingElement|  
|||HttpTransportBindingElement|HttpTransportBindingElement|HttpTransportBindingElement|  
||Tcp|SecurityBindingElement|SecurityBindingElement|SymmetricSecurityBindingElement（身份验证模式 = SecureConversation）|  
|||TcpTransportBindingElement|TcpTransportBindingElement|TcpTransportBindingElement|  
|混合（带有消息凭据的传输）|Https|TransportSecurityBindingElement|TransportSecurityBindingElement||  
|||OneWayBindingElement|||  
|||HttpsTransportBindingElement|HttpsTransportBindingElement||  
||TCP|TransportSecurityBindingElement|SymmetricSecurityBindingElement（身份验证模式 = SecureConversation）|SymmetricSecurityBindingElement（身份验证模式 = SecureConversation）|  
|||OneWayBindingElement|||  
|||SSL 或 Windows StreamSecurityBindingElement|SSL 或 Windows StreamSecurityBindingElement|SSL 或 Windows StreamSecurityBindingElement|  
|||TcpTransportBindingElement|TcpTransportBindingElement|TcpTransportBindingElement|  
  
 请注意，SecurityBindingElements 有许多可配置的设置。 有关详细信息，请参阅 [SecurityBindingElement Authentication 模式](securitybindingelement-authentication-modes.md)。  
  
 有关详细信息，请参阅 [安全对话和安全会话](secure-conversations-and-secure-sessions.md)。  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-create-a-custom-binding-that-uses-a-symmetricsecuritybindingelement"></a>创建使用 SymmetricSecurityBindingElement 的自定义绑定。  
  
1. 以 <xref:System.ServiceModel.Channels.BindingElementCollection> 名称创建 `outputBec` 类的实例。  
  
2. 调用静态方法 `M:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationBindingElement(true)`，它将返回 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 类的实例。  
  
3. 通过调用  类的  的  方法，将 添加到集合 () 中。  
  
4. 创建 <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> 类的实例并将其添加到集合 (`outputBec`) 中。 这将指定该绑定所使用的编码。  
  
5. 创建一个 <xref:System.ServiceModel.Channels.HttpTransportBindingElement> 并将其添加到集合 (`outputBec`) 中。 这将指定该绑定使用 HTTP 传输。  
  
6. 通过创建 <xref:System.ServiceModel.Channels.CustomBinding> 类的实例并将集合 `outputBec` 传递给构造函数来创建一个新的自定义绑定。  
  
7. 生成的自定义绑定具有与标准 <xref:System.ServiceModel.WSHttpBinding> 相同的许多特性。 它指定消息级别的安全性和 Windows 凭据，但禁用安全会话，要求在带外指定服务凭据且不对签名进行加密。 最后一条只能按步骤 4 所示通过设置 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A> 属性来控制。 其他两条可以使用标准绑定上的设置进行控制。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的示例为创建使用 的自定义绑定提供了一个完整的函数。  
  
### <a name="code"></a>代码  

 [!code-csharp[c_CustomBinding#20](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#20)]
 [!code-vb[c_CustomBinding#20](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombinding/vb/source.vb#20)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>
- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [扩展绑定](../extending/extending-bindings.md)
- [系统提供的绑定](../system-provided-bindings.md)
