---
title: <Field>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: 6a14125f-1a8d-41a1-8a32-659ca0ad12de
ms.openlocfilehash: e63dc293c42aa620b7f7ac15fc0454bc603b9dde
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96251043"
---
# <a name="field-element-net-native"></a>\<Field>元素 (.NET Native)

将运行时反射策略应用到一个字段。  
  
## <a name="syntax"></a>语法  
  
```xml  
<Field Name="field_name"  
       Browse="policy_type"  
       Dynamic="policy_type"  
       Serialize="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|属性类型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|常规|必需的特性。 指定字段名称。|  
|`Browse`|反射|可选特性。 控制对该字段信息的查询或列举该字段，但并不在运行时间启用任何动态访问。|  
|`Dynamic`|反射|可选特性。 控制运行时对该字段的访问，以启用动态编程。 该策略确保一个字段可在运行时间内得到设置或动态检索。|  
|`Serialize`|序列化|可选特性。 控制运行时对一个字段的访问以启用类型实例，使其通过程序库得到序列化，例如通过 Newtonsoft JSON 序列化程序，或被用于绑定数据。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|method_name|字段名。 字段的类型由父级 [\<Type>](type-element-net-native.md) 或 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素定义。|  
  
## <a name="all-other-attributes"></a>所有其他特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该设置将应用到这个字段的策略类型。 可能值为 `Auto`、`Excluded`、`Included` 和 `Required`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|将反射策略应用到一种类型及其所有成员。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|将反射策略应用到一种构造泛型类型及其所有成员。|  
  
## <a name="remarks"></a>备注  

 如果一个字段的策略没有得到显式定义，它将继承其父元素的运行时策略。  
  
## <a name="see-also"></a>另请参阅

- [运行时指令元素](runtime-directive-elements.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令策略设置](runtime-directive-policy-settings.md)
