---
title: <add> 的 <baseAddressPrefixFilter>
ms.date: 03/30/2017
ms.assetid: b226bede-8459-4de9-b2ac-3d39604ce2bc
ms.openlocfilehash: fefe85381aec113da123e6f2246aee340b0cf97a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91181591"
---
# <a name="add-of-baseaddressprefixfilter"></a>\<add> 的 \<baseAddressPrefixFilter>

表示一个配置元素，该元素指定传递筛选器，该筛选器提供一种机制，用于在 IIS 中承载 Windows Communication Foundation (WCF) 应用程序时，选择适当的 Internet Information Services (IIS) 绑定。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceHostingEnvironment>**](servicehostingenvironment.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<baseAddressPrefixFilters>**](baseaddressprefixfilters.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<serviceHostingEnvironment>
  <baseAddressPrefixFilters>
    <add prefix="String" />
  </baseAddressPrefixFilters>
</serviceHostingEnvironment>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|前缀|用于与基址的一部分进行匹配的 URI。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<baseAddressPrefixFilters>](baseaddressprefixfilters.md)|用于指定传递筛选器的配置元素的集合，这些筛选器提供一种机制，用于在 IIS 中承载 Windows Communication Foundation (WCF) 应用程序时选取适当的 IIS 绑定。|  
  
## <a name="remarks"></a>备注  

 前缀筛选器为共享的宿主提供程序提供一种指定服务要使用的 URI 的方法。 它使得共享主机可以在同一站点上通过同一方案的不同基址承载多个应用程序。  
  
 IIS 网站是包含虚拟目录的虚拟应用程序的容器。 可通过一个或多个 IIS 绑定访问站点上的应用程序。 IIS 绑定提供两条信息：绑定协议和绑定信息。 绑定协议（例如 HTTP）定义发生通信所基于的方案，而绑定信息（例如 IP 地址、端口、主机头）包含用于访问站点的数据。  
  
 IIS 支持为每个站点指定多个 IIS 绑定，这会导致每个方案有多个基址。 因为在站点下承载的 WCF 服务只允许绑定到每个方案的一个基址，所以您可以使用前缀筛选器功能选取所需的承载服务的基址。 根据可选前缀列表筛选器筛选 IIS 提供的传入基址。  
  
 例如，你的站点可包含以下基址：
  
```http
http://testl.fabrikam.com/Service.svc  
http://test2.fabrikam.com/Service.svc  
```  
  
 可以使用下面的配置文件在 appdomain 级指定前缀筛选器。  
  
```xml  
<system.serviceModel>
  <serviceHostingEnvironment>
    <baseAddressPrefixFilters>
      <add prefix="net.tcp://test1.fabrikam.com:8000" />
      <add prefix="http://test2.fabrikam.com:9000" />
    </baseAddressPrefixFilters>
  </serviceHostingEnvironment>
</system.serviceModel>
```  
  
 在此示例中，`net.tcp://test1.fabrikam.com:8000` 和 `http://test2.fabrikam.com:9000` 是允许传递的各自方案的唯一基址。  
  
 默认情况下，未指定前缀时，将传递所有地址。 而指定前缀后，将只允许传递该方案的匹配基址。  
  
> [!NOTE]
> 筛选器不支持任何通配符。 此外，IIS 提供的基址可能有绑定到在 `baseAddressPrefixFilters` 列表中未列出的其他方案的地址。 不会筛选出这些地址。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.BaseAddressPrefixFilterElement>
- <xref:System.ServiceModel.Configuration.ServiceHostingEnvironmentSection>
- <xref:System.ServiceModel.ServiceHostingEnvironment>
- [承载](../../../wcf/feature-details/hosting.md)
