---
title: <Namespace>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: 57c614e5-18a9-4e87-bfd5-d0fe3396a192
ms.openlocfilehash: 05de04685f8ba746f55bf040c74fd3831c5b63ca
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287886"
---
# <a name="namespace-element-net-native"></a>\<Namespace>元素 (.NET Native)

将运行时反射策略应用到一个指定的命名空间中的所有类型。  
  
## <a name="syntax"></a>语法  
  
```xml  
<Namespace Name="namespace_name"
           Activate="policy_type"
           Browse="policy_type"  
           Dynamic="policy_setting"  
           Serialize="policy_setting"  
           DataContractSerializer="policy_setting"  
           DataContractJsonSerializer="policy_setting"  
           XmlSerializer="policy_setting"  
           MarshalObject="policy_setting"  
           MarshalDelegate="policy_setting"  
           MarshalStructure="policy_setting" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|属性类型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|常规|必需的特性。 指定命名空间的名称。|  
|`Activate`|反射|可选特性。 控制运行时对构造函数的访问，以启用实例激活。|  
|`Browse`|反射|可选特性。 控制对有关程序元素信息的查询，但并不启用任何运行时访问。|  
|`Dynamic`|反射|可选特性。 控制运行时对所有类型成员的访问，包括构造函数、方法、字段、属性和事件，以启用动态编程。|  
|`Serialize`|序列化|可选特性。 控制运行时对构造函数、字段和属性的访问，使类型实例得到序列化和反序列化处理，这是通过库进行的，例如 Newtonsoft JSON 序列化程序。|  
|`DataContractSerializer`|序列化|可选特性。 控制使用 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> 类的序列化策略。|  
|`DataContractJsonSerializer`|序列化|可选特性。 控制使用 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType> 类的 JSON 序列化策略。|  
|`XmlSerializer`|序列化|可选特性。 控制使用 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> 类的 XML 序列化策略。|  
|`MarshalObject`|Interop|可选特性。 控制封送引用类型到 Windows 运行时和 COM 的策略。|  
|`MarshalDelegate`|Interop|可选特性。 控制将委托类型作为函数指针封送到本机代码的策略。|  
|`MarshalStructure`|Interop|可选特性。 控制封送结构到本机代码的策略。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|*namespace_name*|命名空间名称。 如果 \<Namespace> 元素是 [\<Application>](application-element-net-native.md) 、或元素的子元素 [\<Library>](library-element-net-native.md) ，则 [\<Assembly>](assembly-element-net-native.md) *namespace_name* 必须是完全限定的命名空间名称。 如果该 \<Namespace> 元素是另一个元素的子元素 \<Namespace> ，则 *namespace_name* 必须是相对的命名空间名称。|  
  
## <a name="all-other-attributes"></a>所有其他特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该设置将应用这个策略类型到该命名空间的所有类型。 可能值为 `All`、`Auto`、`Excluded`、`Public`、`PublicAndInternal`、`Required Public`、`Required PublicAndInternal` 以及 `Required All`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|`<Namespace>`|将运行时反射策略应用到一个父命名空间中的所有类型。|  
|[\<Type>](type-element-net-native.md)|将反射策略应用到一个类型。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|将反射策略应用到一个构造泛型类型。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Application>](application-element-net-native.md)|作为应用程序范围内的类型和元数据可以反应在运行时间的类型成员的容器而服务。 [\<Application>](application-element-net-native.md)元素可以包含零个、一个或多个 [\<Assembly>](assembly-element-net-native.md) 元素。|  
|[\<Assembly>](assembly-element-net-native.md)|将运行时反射策略应用到指定程序集中的所有类型。|  
|[\<Library>](library-element-net-native.md)|定义包含元数据在运行时间可以用于反射的类型和类型成员的程序集。 [\<Library>](library-element-net-native.md)元素可以有零个或一个 [\<Assembly>](assembly-element-net-native.md) 元素。|  
|`<Namespace>`|将反射策略应用到一个父命名空间中的所有类型。|  
  
## <a name="remarks"></a>备注  

 `Activate`、`Browse`、`Dynamic` 和 `Serialize` 特性都是可选项。 如果这些都不存在，`<Namespace>` 元素仅会充当子元素的容器。 如果它们存在，`<Namespace>` 元素会将运行时反射策略应用到一个指定的命名空间中的所有类型。  
  
 当它是元素的子元素时 [\<Assembly>](assembly-element-net-native.md) ， `<Namespace>` 元素将重写由元素定义的运行时反射策略  [\<Assembly>](assembly-element-net-native.md) 。  
  
## <a name="see-also"></a>另请参阅

- [运行时指令策略设置](runtime-directive-policy-settings.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令元素](runtime-directive-elements.md)
