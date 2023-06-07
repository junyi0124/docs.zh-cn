---
title: <parameter>
ms.date: 03/30/2017
ms.assetid: 0fb41e2d-64f7-44ab-993e-05892eac6d82
ms.openlocfilehash: 2ef674dc8601bc9afaf6b547265988bb8a99f943
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170163"
---
# \<parameter>

指定当声明类型是泛型类型时的泛型参数。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.serialization>**](system-runtime-serialization.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<dataContractSerializer>**](datacontractserializer.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<declaredTypes>**](declaredtypes.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<add>**](add-of-declaredtypes-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<knownType>**](knowntype.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<parameter>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<parameter index="Integer"
           type="String" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|索引|当声明类型是泛型类型时，指定将返回已知类型的泛型参数。|  
|type|一个字符串，描述用于序列化和反序列化的已知类型。|  
  
## <a name="index-attribute"></a>index 特性  
  
|值|描述|  
|-----------|-----------------|  
|"0"|泛型类型中的第一个参数。 例如，一个 <xref:System.Collections.Generic.List%601> 仅有一个参数。 如果此参数用作声明类型，则将 index 特性设置为“0”。|  
|"1"|泛型类型中的第二个参数。 例如，一个 <xref:System.Collections.Generic.Dictionary%602> 有两个参数。 如果通过第二个参数返回已知类型，则将 index 特性设置为“1”。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<knownType>](knowntype.md)|指定一个可由声明类型的字段或属性返回的已知类型。|  
  
## <a name="remarks"></a>备注  

 有关已知类型的详细信息，请参阅 [数据协定已知类型](../../../wcf/feature-details/data-contract-known-types.md) 和 <xref:System.Runtime.Serialization.DataContractSerializer> 。  
  
 [\<dataContractSerializer>](datacontractserializer-element.md)有关使用此元素的示例，请参见。  
  
 此配置元素不能同时具有两个属性。 如果设置两个属性，则发生 <xref:System.Configuration.ConfigurationErrorsException>。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [数据协定已知类型](../../../wcf/feature-details/data-contract-known-types.md)
- [\<dataContractSerializer>](datacontractserializer-element.md)
- [\<add>](add-of-declaredtypes-element.md)
