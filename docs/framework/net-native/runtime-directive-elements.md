---
title: 运行时指令元素
ms.date: 03/30/2017
ms.assetid: 3fe5848c-ecd7-4136-970b-8e48d250bde6
ms.openlocfilehash: 96bce89c02ad17d1b30eda66237f69a15123dcd3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250796"
---
# <a name="runtime-directive-elements"></a>运行时指令元素

运行时指令 (rd.xml) 文件格式支持以下运行时指令元素。 请参阅[运行时指令 (rd.xml) 配置文件参考](runtime-directives-rd-xml-configuration-file-reference.md)了解分层表示形式。  
  
 [\<Application>](application-element-net-native.md)  
 将运行时反射策略应用到该应用使用的所有类型，作为应用程序范围内的类型和元数据可以反应在运行时间的类型成员的容器而服务。 这是元素的子 [\<Directives>](directives-element-net-native.md) 元素。  
  
 [\<Assembly>](assembly-element-net-native.md)  
 将运行时策略应用到程序集中的所有类型。 这是和元素的子 [\<Application>](application-element-net-native.md) [\<Library>](library-element-net-native.md) 元素。  
  
 [\<AttributeImplies>](attributeimplies-element-net-native.md)  
 如果其包含 [\<Type>](type-element-net-native.md) 指令是一个属性，则将运行时策略应用到该特性应用到的代码元素。  
  
 [\<Directives>](directives-element-net-native.md)  
 .NET Native 的每个运行时指令文件中的根元素。 其子元素为 [\<Application>](application-element-net-native.md) 和 [\<Library>](library-element-net-native.md) 。  
  
 [\<Event>](event-element-net-native.md)  
 将运行时策略应用到一个事件。 这是和元素的子 [\<Type>](type-element-net-native.md) [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素。  
  
 [\<Field>](field-element-net-native.md)  
 将运行时策略应用到一个字段。 这是和元素的子 [\<Type>](type-element-net-native.md) [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素。  
  
 [\<GenericParameter>](genericparameter-element-net-native.md)  
 将运行时策略应用到一个泛型类型或方法的参数类型。  
  
 [\<ImpliesType>](impliestype-element-net-native.md)  
 如果该策略已应用到该包含类型或方法，将该运行时策略应用到一个类型。  
  
 [\<Library>](library-element-net-native.md)  
 将运行时策略应用到程序集中的所有类型。 这是和元素的子 [\<Application>](application-element-net-native.md) [\<Library>](library-element-net-native.md) 元素。  
  
 [\<Method>](method-element-net-native.md)  
 将运行时策略应用到一个方法。 这是和元素的子 [\<Type>](type-element-net-native.md) [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素。  
  
 [\<MethodInstantiation>](methodinstantiation-element-net-native.md)  
 将运行时策略应用到一个构造泛型方法。 这是和元素的子 [\<Type>](type-element-net-native.md) [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素。  
  
 [\<Namespace>](namespace-element-net-native.md)  
 将运行时策略应用到命名空间中的所有类型。  
  
 [\<Parameter>](parameter-element-net-native.md)  
 将运行时策略应用到传递到方法的参数类型。  
  
 [\<Property>](property-element-net-native.md)  
 将运行时策略应用到一个属性。 这是和元素的子 [\<Type>](type-element-net-native.md) [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素。  
  
 [\<Subtypes>](subtypes-element-net-native.md)  
 将运行时策略应用到从包含类型继承的所有类。  
  
 [\<Type>](type-element-net-native.md)  
 将运行时策略应用到一个类型。  
  
 [\<TypeInstantiation>](typeinstantiation-element-net-native.md)  
 将运行时策略应用到一个构造泛型类型。  
  
 [\<TypeParameter>](typeparameter-element-net-native.md)  
 将运行时策略应用到以传递到方法为代表的 <xref:System.Type> 自变量类型。  
  
## <a name="see-also"></a>另请参阅

- [rd.xml 配置文件参考](runtime-directives-rd-xml-configuration-file-reference.md)
