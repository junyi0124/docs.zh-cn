---
title: <comContract>
ms.date: 03/30/2017
ms.assetid: 3f8e1c0c-cfdf-4c79-ac65-c64e9323a51c
ms.openlocfilehash: 35596f32bf0e0de9081bc0d4c33fb370c7ab708b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173778"
---
# \<comContract>

指定 COM+ 集成服务协定。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<comContracts>**](comcontracts.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<comContract>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<comContracts>
  <comContract contract="String"
               namespace="String"
               name="String"
               requireSession="Boolean">
    <exposedMethods>
      <exposedMethod name="String" />
    </exposedMethods>
    <userDefinedTypes>
      <userDefinedType name="String"
                       typeLibID="String"
                       typeLibVersion="String"
                       typeDefID="String">
      </userDefinedType>
    </userDefinedTypes>
    <persistableTypes>
      <persistableType id="String"
                       name="String">
      </persistableType>
    </persistableTypes>
  </comContract>
</comContracts>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|contract|一个包含协定类型的字符串。|  
|name|一个包含协定名称的字符串。|  
|命名空间|一个包含协定命名空间的字符串。|  
|requiresSession|一个布尔值，指定是否只能在会话绑定上使用该协定。 在初始化服务时，集成运行库可以确保此设置与要使用的绑定的类型一致。 如果协定的一个或多个绑定存在冲突，则会生成异常。 如果此属性为 `false`、使用的是单向通道并且存在任何 [out] 参数，则也会生成异常。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|persistableTypes|所有持久类型。|  
|userDefinedTypes|一个要包括在服务协定中的用户定义的类型 (UDT) 的集合。|  
|exposedMethods|一个在 COM+ 组件上的接口作为 Web 服务公开时所公开的 COM+ 方法的集合。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|comContracts|包含 `comContract` 元素的集合。|  
  
## <a name="remarks"></a>备注  

 COM + 集成服务协定当前仅限于 `http://tempuri.org` 命名空间，协定名称派生自支持的 COM 接口。 但是，可以使用配置文件中的 `comContracts` 节以及 `comContract` 元素来指定替代服务协定。 例如，可以使用下面的配置来指定服务协定的命名空间、协定名称、要包含的用户定义类型以及其他设置。  
  
```xml  
<comContracts>
  <comContract contract="{5163B1E7-F0CF-4B6A-9A02-4AB654F34284}"
               namespace="http://tempuri.org/5163B1E7-F0CF-4B6A-9A02-4AB654F34284"
               name="_Broker"
               requireSession="true">
    <exposedMethods>
      <exposedMethod name="BuyStock" />
      <exposedMethod name="SellStock" />
      <exposedMethod name="ExecuteTransaction" />
    </exposedMethods>
  </comContract>
</comContracts>
```  
  
 在初始化服务时，指定的命名空间和协定名称将应用到生成的服务说明。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ComContractElementCollection>
- <xref:System.ServiceModel.Configuration.ComContractElement>
- [\<comContracts>](comcontracts.md)
- [与 COM + 应用程序集成](../../../wcf/feature-details/integrating-with-com-plus-applications.md)
- [如何：配置 COM+ 服务设置](../../../wcf/feature-details/how-to-configure-com-service-settings.md)
