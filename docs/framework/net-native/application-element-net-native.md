---
title: <Application>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: b4e9b37a-059b-4076-8f56-cb3f9cef0cd9
ms.openlocfilehash: a7f2eca5a5bb5cfb7b9827f2463454a17fc128cb
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288133"
---
# <a name="application-element-net-native"></a>\<Application>元素 (.NET Native)

作为应用程序范围内的类型和元数据可以反应在运行时间的类型成员的容器而服务，并将运行时反射策略应用到一个应用的所有程序元素。  
  
 \<Directives> 元素  
\<Application> 元素 ( # A0)   
  
## <a name="syntax"></a>语法  
  
```xml  
<Application Activate="policy_setting"  
             Browse="policy_setting"  
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

 下列各节描述了特性、子元素和父元素。 在子元素表格中，策略指向在运行时间针对特定程序元素可用的一类元数据。  
  
### <a name="attributes"></a>特性  
  
|属性|属性类型|描述|  
|---------------|--------------------|-----------------|  
|`Activate`|反射|可选特性。 控制运行时对构造函数的访问，以启用实例激活。|  
|`Browse`|反射|可选特性。 控制对该类型信息的查询或列举该类型，但并不在运行时间启用任何动态访问。|  
|`Dynamic`|反射|可选特性。 控制运行时对所有类型成员的访问，包括构造函数、方法、字段、属性和事件，以启用动态编程。|  
|`Serialize`|序列化|可选特性。 控制运行时对构造函数、字段和属性的访问，使类型实例得到序列化和反序列化处理，这是通过库进行的，例如 Newtonsoft JSON 序列化程序。|  
|`DataContractSerializer`|序列化|可选特性。 控制使用 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> 类的序列化策略。|  
|`DataContractJsonSerializer`|序列化|可选特性。 控制使用 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType> 类的 JSON 序列化策略。|  
|`XmlSerializer`|序列化|可选特性。 控制使用 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> 类的 XML 序列化策略。|  
|`MarshalObject`|Interop|可选特性。 控制封送引用类型到 Windows 运行时和 COM 的策略。|  
|`MarshalDelegate`|Interop|可选特性。 控制将委托类型作为函数指针封送到本机代码的策略。|  
|`MarshalStructure`|Interop|可选特性。 控制封送结构到本机代码的策略。|  
  
## <a name="all-attributes"></a>所有特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该策略的设置将应用到该应用中的所有类型。 可能值为 `All`、`Auto`、`Excluded`、`Public`、`PublicAndInternal`、`Required Public`、`Required PublicAndInternal` 以及 `Required All`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Assembly>](assembly-element-net-native.md)|将策略应用到特定程序集中的所有类型。|  
|[\<Namespace>](namespace-element-net-native.md)|将策略应用到特定命名空间中的所有类型。|  
|[\<Type>](type-element-net-native.md)|将策略应用到一个特定类型，例如一个类或结构。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|将策略应用到一个构造泛型类型。 例如， [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素可以用于定义类型的策略 `List<String>` 。|  
|[\<Method>](method-element-net-native.md)|将策略应用到一个特定类型上的方法。|  
|[\<MethodInstantiation>](methodinstantiation-element-net-native.md)|将策略应用到一个构造泛型方法。|  
|[\<Property>](property-element-net-native.md)|将策略应用到一个特定类型上的属性。|  
|[\<Field>](field-element-net-native.md)|将策略应用到一个特定类型上的字段。|  
|[\<Event>](event-element-net-native.md)|将策略应用到一个特定类型上的事件。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Directives>](directives-element-net-native.md)|运行时指令文件的根元素。|  
  
## <a name="remarks"></a>备注  

 [\<Directives>](directives-element-net-native.md)元素可以包含零个或一个 `<Application>` 元素。 单独一个反射指令文件中出现的多个 `<Application>` 元素不受支持。  
  
 `<Application>` 元素可通过以下方法之一使用：  
  
- 作为定义在运行时间需要元数据的程序元素的容器。 在这种情况下，`<Application>` 元素不需要具有任何特性。 在编译时间，编译器工具搜索 .NET Framework 核心库等所有库，以查找由 `<Application>` 元素的子元素识别出的程序元素。 与此相反，编译器工具仅搜索由的 [\<Library>](library-element-net-native.md) 子元素标识的程序元素所指定的库 [\<Library>](library-element-net-native.md) 。  
  
- 作为为反射、序列化和互操作设置应用程序范围的策略的元素。 元素的特性 `<Application>` 定义应用程序范围的策略，该策略可能由或元素定义的子元素重写 `<Application>` [\<Library>](library-element-net-native.md) 。  
  
## <a name="see-also"></a>请参阅

- [\<Library> 元素](library-element-net-native.md)
- [\<Directives> 元素](directives-element-net-native.md)
- [运行时指令元素](runtime-directive-elements.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
