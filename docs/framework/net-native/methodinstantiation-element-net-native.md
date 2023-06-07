---
title: <MethodInstantiation>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: a3355d78-2a88-4109-8521-830d7cae260a
ms.openlocfilehash: e247db05f8442d4fcfddbf03b5eb8955b8ff425a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250952"
---
# <a name="methodinstantiation-element-net-native"></a>\<MethodInstantiation>元素 (.NET Native)

将运行时反射策略应用到一个构造泛型方法。  
  
## <a name="syntax"></a>语法  
  
```xml  
<MethodInstantiation Name="method_name"  
                     Signature="method_signature"  
                     Arguments="method_arguments"  
                     Browse="policy_type"  
                     Dynamic="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|属性类型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|常规|必需的特性。 指定方法名称。|  
|`Signature`|常规|可选特性。 指定该类型的命名参数。 多个命名参数由逗号分隔。 `Signature` 特性用于区分重载方法。|  
|`Arguments`|常规|必需的特性。 指定泛型类型参数。 如果存在多个自变量，它们之间用逗号分割。|  
|`Browse`|反射|可选特性。 控制对该方法信息的查询或列举该方法，但并不在运行时间启用任何动态调用。|  
|`Dynamic`|反射|可选特性。 控制运行时对构造函数或方法的访问，以启用动态编程。 该策略确保一个成员可在运行时间内得到调用。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|method_name|方法名称。 方法的类型由父级 [\<Type>](type-element-net-native.md) 或 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素定义。|  
  
## <a name="signature-attribute"></a>签名特性  
  
|值|描述|  
|-----------|-----------------|  
|*method_signature*|指定该类型的命名参数。 如果存在多个参数，它们之间用逗号分割。|  
  
## <a name="arguments-attribute"></a>自变量特性  
  
|值|描述|  
|-----------|-----------------|  
|method_arguments|指定泛型类型参数。 如果存在多个自变量，它们之间用逗号分割。 每个自变量必须包含一个完全限定的类型名称。|  
  
## <a name="all-other-attributes"></a>所有其他特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该设置将应用到这个方法的策略类型。 可能值为 `Auto`、`Excluded`、`Included` 和 `Required`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|将反射策略应用到一种类型及其所有成员。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|将反射策略应用到一种构造泛型类型及其所有成员。|  
  
## <a name="remarks"></a>备注  

 `<MethodInstantiation>` 元素替代其相应的开发泛型方法的运行时反射策略。  
  
## <a name="see-also"></a>另请参阅

- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令元素](runtime-directive-elements.md)
- [运行时指令策略设置](runtime-directive-policy-settings.md)
- [\<Method> 元素](method-element-net-native.md)
