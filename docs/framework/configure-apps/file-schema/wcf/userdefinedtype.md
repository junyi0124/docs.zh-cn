---
title: <userDefinedType>
ms.date: 03/30/2017
ms.assetid: 0f70ec06-8249-4f0c-9f49-b4df59985fb8
ms.openlocfilehash: a4bbd677aba27d93389f8d2f99aadd801c86b65f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172835"
---
# \<userDefinedType>

表示一个要包括到服务协定中的用户定义类型 (UDT)。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<comContracts>**](comcontracts.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<comContract>**](comcontract.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<userDefinedTypes>**](userdefinedtypes.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<userDefinedType>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<comContracts>
  <comContract>
    <userDefinedTypes>
      <userDefinedType name="String"
                       typeLibID="String"
                       typeLibVersion="String"
                       typeDefID="String">
      </userDefinedType>
    </userDefinedTypes>
  </comContract>
</comContracts>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`name`|一个可选属性，包含提供可读类型名称的字符串。 运行库不使用该属性，但该属性可以帮助读取器区分类型。|  
|`TypeDefID`|一个 GUID 字符串，标识已注册类型库中的特定 UDT 类型。|  
|`TypeLibID`|一个 GUID 字符串，标识定义该类型的已注册类型库。|  
|`TypeLibVersion`|一个字符串，标识定义该类型的类型库版本。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|`userDefinedTypes`|一个 `userDefinedType` 元素集合。|  
  
## <a name="remarks"></a>备注  

 COM+ 集成运行时通过检查类型库来创建服务。 当 COM+ 组件包含传递 VARIANT 的方法时，系统将无法确定要在运行时之前传递的实际类型。 因此，当您试图在 VARIANT 内传递用户定义类型 (UDT) 时，将由于该类型不是可供序列化的已知类型而失败。  
  
 若要避免此问题，可以将 UDT 添加到配置文件，以使 UDT 作为已知类型包括到相应的服务协定中。 为此，必须唯一地标识 UDT 和协定（即使用它的原始 COM 接口）。  
  
 下面的示例演示将两个特定的 UDT 添加到配置文件的 <`userDefinedTypes`> 节，以便达到此目的。  
  
```xml  
<comContracts>
  <comContract contract="{5163B1E7-F0CF-4B6A-9A02-4AB654F34284}"
               namespace="http://tempuri.org/5163B1E7-F0CF-4B6A-9A02-4AB654F34284"
               name="_Broker"
               requireSession="true">
    <userDefinedTypes>
      <userDefinedType name="CustomerType"
                       typeLibID="{91DC728C-4F1A-45de-A9B6-B538E209CEA6}"
                       typeLibVersion="1.0"
                       typeDefID="{D129765C-F211-434e-825A-9A63198C41F2}">
      </userDefinedType>
      <userDefinedType name="AddressType"
                       typeLibID="{91DC728C-4F1A-45de-A9B6-B538E209CEA6}"
                       typeLibVersion="1.0"
                       typeDefID="{4616AE0D-687A-43B7-BC63-141AE3DFD099}">
      </userDefinedType>
    </userDefinedTypes>
    <exposedMethods>
      <exposedMethod name="BuyStock" />
      <exposedMethod name="SellStock" />
      <exposedMethod name="ExecuteTransaction" />
    </exposedMethods>
  </comContract>
</comContracts>
```  
  
 在初始化服务时，集成运行库会查找指定的类型，然后将这些类型添加到指定协定的已知类型集合中。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ComContractElement.UserDefinedTypes%2A>
- <xref:System.ServiceModel.Configuration.ComUdtElementCollection>
- <xref:System.ServiceModel.Configuration.ComUdtElement>
- [\<comContracts>](comcontracts.md)
- [与 COM + 应用程序集成](../../../wcf/feature-details/integrating-with-com-plus-applications.md)
- [如何：配置 COM+ 服务设置](../../../wcf/feature-details/how-to-configure-com-service-settings.md)
