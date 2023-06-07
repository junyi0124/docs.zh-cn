---
title: <endpoint> 元素
ms.date: 03/30/2017
ms.assetid: 2fc8fedc-78d0-4e87-8142-fbfd26c15a4e
ms.openlocfilehash: befebc090900576b1e0f7ca679e1f5f5cd15af9a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183801"
---
# <a name="endpoint-element"></a>\<endpoint> 元素

指定用于公开服务的服务终结点的绑定、协定和地址属性。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<services>**](services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<service>**](service.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<endpoint>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<endpoint address="String"
          behaviorConfiguration="String"
          binding="String"
          bindingConfiguration="String"
          bindingName="String"
          bindingNamespace="String"
          contract="String"
          endpointConfiguration="String"
          isSystemEndpoint="Boolean"
          kind="String"
          listenUriMode="Explicit/Unique"
          listenUri="Uri">
</endpoint>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|address|一个包含终结点地址的字符串。 可以将地址指定为绝对地址或相对地址。 如果提供的是相对地址，则需要主机提供适合于绑定中所使用的传输方案的基址。 如果未配置地址，则假定基址为该终结点的地址。<br /><br /> 默认值为空字符串。|  
|behaviorConfiguration|一个字符串，其中包含要在终结点中使用的行为的名称。|  
|binding|必需的字符串属性，此属性指定要使用的绑定类型。 该类型必须具有一个已注册的配置节，才能加以引用。 该类型是按节名而不是绑定的类型名注册的。|  
|bindingConfiguration|一个字符串，指定实例化终结点时所使用的绑定的绑定名称。 定义终结点时，绑定名称必须在作用域内。 默认值为一个空字符串。<br /><br /> 此属性与 `binding` 结合使用，以引用配置文件中的特定绑定配置。 如果尝试使用自定义绑定，请设置此属性。 否则，可能引发异常。|  
|bindingName|一个字符串，指定绑定的唯一限定名称，用于通过 WSDL 进行的定义导出。 默认值为空字符串。|  
|bindingNamespace|一个字符串，指定绑定的命名空间的限定名称，用于通过 WSDL 进行的定义导出。 默认值为空字符串。|  
|contract|一个字符串，指示此终结点公开了哪个协定。 程序集必须实现该协定类型。 如果服务实现所实现的是单个协定类型，则可以省略此属性。 默认值为空字符串。|  
|endpointConfiguration|一个字符串，指定由 `kind` 特性设置的标准终结点的名称，此名称引用此标准终结点的其他配置信息。 必须在 `<standardEndpoints>` 节中定义相同的名称。|  
|isSystemEndpoint|一个布尔值，指定终结点是否是基础结构终结点。|  
|kind|一个字符串，指定应用的标准终结点的类型。 该类型必须在 `<extensions>` 节或 machine.config 中注册。如果未指定任何内容，则会创建一个通用的服务终结点。|  
|listenUriMode|指定传输如何处理供服务侦听的 `ListenUri`。 有效值为<br /><br /> -Explicit<br />-唯一<br /><br /> 默认值为 Explicit。|  
|listenUri|一个字符串，指定服务终结点侦听的 URI。 默认值为空字符串。|  
|name|可选特性。 一个指定服务终结点名称的字符串。 默认值为绑定名称和协定说明名称的串联。 服务可能有多个终结点，因此终结点的 `name` 特性是通过服务名称区分的。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<headers>](headers.md)|一个地址标头集合。|  
|[\<identity>](identity.md)|一个标识，与某个终结点交换消息的其他终结点可以使用该标识对该终结点进行身份验证。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<service>](service.md)|一个配置节，定义客户端可以连接的终结点的列表。|  
  
## <a name="example"></a>示例  

 这是服务终结点配置的一个示例。  
  
```xml  
<endpoint address="/HelloWorld/"
          bindingConfiguration="usingDefaults"
          bindingName="MyBinding"
          binding="customBinding"
          contract="HelloWorld">
  <headers>
    <region xmlns="http://tempuri.org/">EastCoast</region>
    <member xmlns="http://tempuri.org/">Gold</member>
  </headers>
</endpoint>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ServiceEndpointElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.Description.ServiceEndpoint>
- [终结点：地址、绑定和协定](../../../wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)
- [如何：在配置中创建服务终结点](../../../wcf/feature-details/how-to-create-a-service-endpoint-in-configuration.md)
