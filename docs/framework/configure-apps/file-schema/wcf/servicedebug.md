---
title: <serviceDebug>
ms.date: 03/30/2017
ms.assetid: 6d7ea986-f232-49fe-842c-f934d9966889
ms.openlocfilehash: 8236e581ee754a39ae32fa97084b8e2bfbaee652
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153867"
---
# \<serviceDebug>

指定 Windows Communication Foundation (WCF) 服务的调试和帮助信息功能。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceDebug>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<serviceDebug httpHelpPageBinding="String"
              httpHelpPageBindingConfiguration="String"
              httpHelpPageEnabled="Boolean"
              httpHelpPageUrl="Uri"
              httpsHelpPageBinding="String"
              httpsHelpPageBindingConfiguration="String"
              httpsHelpPageEnabled="Boolean"
              httpsHelpPageUrl="Uri"
              includeExceptionDetailInFaults="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|httpHelpPageBinding|一个字符串值，指定在利用 HTTP 访问服务帮助页时使用的绑定类型。<br /><br /> 仅支持具有支持 <xref:System.ServiceModel.Channels.IReplyChannel?displayProperty=nameWithType> 的内部绑定元素的绑定。 此外，绑定的 <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> 属性必须为 <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType>。|  
|httpHelpPageBindingConfiguration|一个字符串，指定在 `httpHelpPageBinding` 特性中指定的绑定的名称，此名称引用此绑定的其他配置信息。 必须在 `<bindings>` 节中定义相同的名称。|  
|httpHelpPageEnabled|一个布尔值，该值控制 WCF 是否在由属性指定的地址发布 HTML 帮助页 `httpHelpPageUrl` 。 默认值为 `true`。<br /><br /> 可以将此属性设置为 `false` 以禁止发布对于 HTML 浏览器可见的 HTML 帮助页。<br /><br /> 若要确保在 `httpHelpPageUrl` 属性控制的位置发布 HTML 帮助页，必须将此属性设置为 `true`。 另外，还必须满足以下条件之一：<br /><br /> - `httpHelpPageUrl` 属性是支持 HTTP 协议方案的绝对地址。<br />-存在支持 HTTP 协议方案的服务的基址。<br /><br /> 尽管为 `httpHelpPageUrl` 属性指定不支持 HTTP 协议方案的绝对地址会引发异常，但不满足前面两个条件的任何其他方案都不会引发异常，也不会发布 HTML 帮助页。|  
|httpHelpPageUrl|一个 URI，指定在使用 HTML 浏览器查看终结点时，用户所见自定义 HTML 帮助文件的基于 HTTP 的相对或绝对 URL。<br /><br /> 可以使用此属性启用自定义 HTML 帮助文件，例如，从 HTML 浏览器通过 HTTP/Get 请求返回的帮助文件。 HTML 帮助文件位置的解析方式如下。<br /><br /> 1. 如果此属性的值是相对地址，则 HTML 帮助文件的位置是支持 HTTP 请求的服务基址加上此属性值的值。<br />2. 如果此属性的值是绝对地址并支持 HTTP 请求，则 HTML 帮助文件的位置是此属性的值。<br />3. 如果此属性的值是绝对的但不支持 HTTP 请求，则会引发异常。<br /><br /> 仅当属性为时，此属性才有效 `httpHelpPageEnabled` `true` 。|  
|httpsHelpPageBinding|一个字符串值，指定在利用 HTTPS 访问服务帮助页时使用的绑定类型。<br /><br /> 仅支持具有支持 <xref:System.ServiceModel.Channels.IReplyChannel> 的内部绑定元素的绑定。 此外，绑定的 <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> 属性必须为 <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType>。|  
|httpsHelpPageBindingConfiguration|一个字符串，指定在 `httpsHelpPageBinding` 特性中指定的绑定的名称，此名称引用此绑定的其他配置信息。 必须在 `<bindings>` 节中定义相同的名称。|  
|httpsHelpPageEnabled|一个布尔值，该值控制 WCF 是否在由属性指定的地址发布 HTML 帮助页 `httpsHelpPageUrl` 。 默认值为 `true`。<br /><br /> 可以将此属性设置为 `false` 以禁止发布对于 HTML 浏览器可见的 HTML 帮助页。<br /><br /> 若要确保在 `httpsHelpPageUrl` 属性控制的位置发布 HTML 帮助页，必须将此属性设置为 `true`。 另外，还必须满足以下条件之一：<br /><br /> - `httpsHelpPageUrl` 属性是支持 HTTPS 协议方案的绝对地址。<br />-存在支持 HTTPS 协议方案的服务的基址。<br /><br /> 尽管为 `httpsHelpPageUrl` 属性指定不支持 HTTPS 协议方案的绝对地址会引发异常，但不满足前面两个条件的任何其他方案都不会引发异常，也不会发布 HTML 帮助页。|  
|httpsHelpPageUrl|一个 URI，指定在使用 HTML 浏览器查看终结点时，用户所见自定义 HTML 帮助文件的基于 HTTPS 的相对或绝对 URL。<br /><br /> 可以使用此属性启用自定义 HTML 帮助文件，例如，从 HTML 浏览器通过 HTTPS/Get 请求返回的帮助文件。 HTML 帮助文件位置的解析方式如下：<br /><br /> -如果此属性的值是相对地址，则 HTML 帮助文件的位置是支持 HTTPS 请求的服务基址加上此属性值的值。<br />-如果此属性的值是绝对地址并支持 HTTPS 请求，则 HTML 帮助文件的位置是此属性的值。<br />-如果此属性的值是绝对的但不支持 HTTPS 请求，则会引发异常。<br /><br /> 仅当属性为时，此属性才有效 `httpHelpPageEnabled` `true` 。|  
|includeExceptionDetailInFaults|一个值，指定是否在返回给客户端的 SOAP 错误详细信息中包含托管异常信息以供调试。 默认为 `false`。<br /><br /> 如果将此属性设置为 `true`，则可以将托管异常信息流到客户端以便进行调试，还可以为在 Web 浏览器中浏览该服务的用户发布 HTML 信息文件。 **警告：**  向客户端返回托管异常信息可能会带来安全风险。 这是因为，异常详细信息公开了有关内部服务实现的信息，这些信息可能被未经授权的客户端使用。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|指定行为元素。|  
  
## <a name="remarks"></a>备注  

 `includeExceptionDetailInFaults`如果设置为， `true` 则允许服务返回应用程序代码引发的任何异常，即使未使用声明异常也是如此 <xref:System.ServiceModel.FaultContractAttribute> 。 此设置在调试服务器引发意外异常的事例时很有用。 通过使用此属性，可以返回未知异常的序列化形式，从而可以检查该异常的更多详细信息。  
  
> [!CAUTION]
> 将托管异常信息返回给客户端可能存在安全风险，因为异常详细信息会公开有关内部服务实现的信息，而未经授权的客户端可能会利用这些信息。 由于涉及到安全问题，强烈建议您仅在受控调试情况下执行此操作。 部署应用程序时应将 `includeExceptionDetailInFaults` 设置为 `false`。  
  
 有关与托管异常相关的安全问题的详细信息，请参阅 [在协定和服务中指定和处理错误](../../../wcf/specifying-and-handling-faults-in-contracts-and-services.md)。 有关代码示例，请参阅 [服务调试行为](../../../wcf/samples/service-debug-behavior.md)。  
  
 也可以设置 `httpsHelpPageEnabled` 和 `httpsHelpPageUrl` 来启用或禁用帮助页。 每个服务可以选择公开一个帮助页，该帮助页包含有关服务的信息，其中包括要获取服务的 WSDL 的终结点。 通过将 `httpHelpPageEnabled` 设置为 `true` 可以启用该帮助页。 这将使帮助页返回到对服务基址的 GET 请求。 可以通过设置 `httpHelpPageUrl` 属性来更改此地址。 此外，通过使用 HTTPS（而不是 HTTP）可以使其安全。  
  
 可以利用可选的 `httpHelpPageBinding` 和 `httpHelpPageBinding`属性来配置用于访问服务网页的绑定。 如果未指定这两个属性，则根据情况使用相应的默认绑定（采用 HTTP 时为 `HttpTransportBindingElement`，采用 HTTPS 时为 `HttpsTransportBindingElement`）来访问服务帮助页。 请注意：不能将这些属性用于内置 WCF 绑定。 仅支持具有支持 x： IReplyChannel> 的内部绑定元素的绑定。 此外，绑定的 <xref:System.ServiceModel.Channels.MessageVersion?displayProperty=nameWithType> 属性必须为 <xref:System.ServiceModel.Channels.MessageVersion.None?displayProperty=nameWithType>。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ServiceDebugElement>
- <xref:System.ServiceModel.Description.ServiceDebugBehavior>
- [在协定和服务中指定和处理错误](../../../wcf/specifying-and-handling-faults-in-contracts-and-services.md)
- [处理异常和错误](../../../wcf/extending/handling-exceptions-and-faults.md)
- [服务调试行为](../../../wcf/samples/service-debug-behavior.md)
