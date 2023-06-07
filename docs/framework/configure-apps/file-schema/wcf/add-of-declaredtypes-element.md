---
title: <add> of <declaredTypes> 元素
ms.date: 03/30/2017
helpviewer_keywords:
- data contracts
- dataContractSerializer element
- DataContractSerializer
- DataContractAttribute
ms.assetid: c3d37ae4-8f1c-463f-b195-658c5a7e90a1
ms.openlocfilehash: 9af47848b03074ec88f38a5884089bc50239ee50
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201663"
---
# <a name="add-of-declaredtypes-element"></a>\<add> of \<declaredTypes> 元素

添加在反序列化过程中由 <xref:System.Runtime.Serialization.DataContractSerializer> 使用的类型。 每个声明类型都包含一些将作为声明类型的字段或属性返回的已知类型。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.runtime.serialization>**](system-runtime-serialization.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<dataContractSerializer>**](datacontractserializer.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<declaredTypes>**](declaredtypes.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<add type="String">
  <knownType type="String">
    <parameter index="Integer"
               type="String" />
  </knownType>
</add>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|type|必需的字符串属性。<br /><br /> 指定类型名称（包括命名空间）、程序集名称、版本号、区域性和公钥标记。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<knownType>](knowntype.md)|指定要添加的声明类型的已知类型。 如果声明类型是泛型类型，则还必须向 `<knownType>` 元素添加一个参数元素，以指定用于返回已知类型的泛型参数。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<declaredTypes>](declaredtypes.md)|包含在 <xref:System.Runtime.Serialization.DataContractSerializer> 进行反序列化过程中需要已知类型的类型。|  
  
## <a name="remarks"></a>备注  

 有关已知类型的详细信息，请参阅 [数据协定已知类型](../../../wcf/feature-details/data-contract-known-types.md) 和 <xref:System.Runtime.Serialization.DataContractSerializer> 。  
  
 [\<dataContractSerializer>](datacontractserializer-element.md)有关使用此元素的示例，请参见。  
  
> [!NOTE]
> 如果将 <xref:System.Object> 类型添加为 `<declaredType>`，则会引发 <xref:System.Configuration.ConfigurationErrorsException>。 这是因为，<xref:System.Object> 类型不能在配置中用作声明的类型。  
  
## <a name="example"></a>示例  
  
```xml  
<add type="MyCompany.Library.Shape,
           MyAssembly, Version=2.0.0.0, Culture=neutral,
           PublicKeyToken=XXXXXX, processorArchitecture=MSIL">
  <knownType type="MyCompany.Library.Circle,
                   MyAssembly, Version=2.0.0.0, Culture=neutral,
                   PublicKeyToken=XXXXXX,
                   processorArchitecture=MSIL" />
</add>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Serialization.DataContractSerializer>
- [数据协定已知类型](../../../wcf/feature-details/data-contract-known-types.md)
- [\<dataContractSerializer>](datacontractserializer-element.md)
- [\<declaredTypes> 的 \<add>](add-of-declaredtypes-element.md)
