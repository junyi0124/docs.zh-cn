---
title: <endpoint> 的 <client>
ms.date: 03/30/2017
ms.assetid: de6238ae-bbf8-48e9-a1b5-e24c0bea8afa
ms.openlocfilehash: 79d827691ec3898ad94af9835077c61ea35990ab
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185842"
---
# <a name="endpoint-of-client"></a>\<endpoint> 的 \<client>

指定通道终结点的协定、绑定和地址属性，客户端使用通道终结点与服务器上的服务终结点连接。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<endpoint>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<endpoint address="String"
          behaviorConfiguration="String"
          binding="String"
          bindingConfiguration="String"
          contract="String"
          endpointConfiguration="String"
          kind="String"
          name="String">
</endpoint>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|address|必需的字符串属性。<br /><br /> 指定终结点的地址。 默认值为一个空字符串。 该地址必须为绝对 URI。|  
|behaviorConfiguration|一个字符串，其中包含要用于实例化终结点的行为的行为名。 定义服务时，该行为名必须在作用域内。 默认值为一个空字符串。|  
|binding|必需的字符串属性。<br /><br /> 一个字符串，指示要使用的绑定的类型。 该类型必须具有一个已注册的配置节，才能加以引用。 该类型是按节名而不是绑定的类型名注册的。|  
|bindingConfiguration|可选。 一个字符串，其中包含实例化终结点时要使用的绑定配置的名称。 定义终结点时，绑定配置必须在作用域内。 默认值为空字符串。<br /><br /> 此属性与 `binding` 结合使用，以引用配置文件中的特定绑定配置。 如果尝试使用自定义绑定，请设置此属性。 否则，可能引发异常。|  
|contract|必需的字符串属性。<br /><br /> 一个字符串，指示此终结点公开了哪个协定。 程序集必须实现该协定类型。|  
|endpointConfiguration|一个字符串，指定由 `kind` 特性设置的标准终结点的名称，此名称引用此标准终结点的其他配置信息。 必须在 `<standardEndpoints>` 节中定义相同的名称。|  
|kind|一个字符串，指定应用的标准终结点的类型。 该类型必须在 `<extensions>` 节或 machine.config 中注册。如果未指定任何内容，则会创建一个通用的通道终结点。|  
|name|可选的字符串属性。 此属性唯一标识给定协定的终结点。 可以为一个给定协定类型定义多个客户端。 每个定义都必须用唯一的配置名称加以区分。 如果省略此属性，则将对应的终结点用作与指定协定类型相关联的默认终结点。 默认值为空字符串。<br /><br /> 绑定的 `name` 属性用于通过 WSDL 进行的定义导出。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<headers>](headers.md)|一个地址标头集合。|  
|[\<identity>](identity.md)|一个标识，与某个终结点交换消息的其他终结点可以使用该标识对该终结点进行身份验证。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<client>](client.md)|一个配置节，定义客户端可以连接的终结点的列表。|  
  
## <a name="example"></a>示例  

 这是通道终结点配置的一个示例。  
  
```xml  
<endpoint address="/HelloWorld/"
          bindingConfiguration="usingDefaults"
          name="MyBinding"
          binding="customBinding"
          contract="HelloWorld">
</endpoint>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ChannelEndpointElement>
- <xref:System.ServiceModel.Configuration.ClientSection>
- <xref:System.ServiceModel.Configuration.ChannelEndpointElementCollection>
- <xref:System.ServiceModel.Configuration.ClientSection.Endpoints%2A>
- <xref:System.ServiceModel.Configuration.ChannelEndpointElement>
- [WCF 客户端配置](../../../wcf/feature-details/client-configuration.md)
- [客户端](../../../wcf/feature-details/clients.md)
