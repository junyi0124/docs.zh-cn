---
title: <TypeParameter>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: d37bb1b7-1ddc-4c6d-8ecf-583f804a2479
ms.openlocfilehash: dc04115914b7571b677c6d069d2d4b820b895d59
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287665"
---
# <a name="typeparameter-element-net-native"></a>\<TypeParameter>元素 (.NET Native)

将策略应用到以传递到方法为代表的类型参数类型。  
  
## <a name="syntax"></a>语法  
  
```xml  
<Parameter Name="parameter_name"  
           Activate="policy_type"  
           Browse="policy_type"  
           Dynamic="policy_type"  
           Serialize="policy_type"  
           DataContractSerializer="policy_type"  
           DataContractJsonSerializer="policy_type"  
           XmlSerializer="policy_type"  
           MarshalObject="policy_type"  
           MarshalDelegate="policy_type"  
           MarshalStructure="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|属性类型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|常规|必需的特性。 类型 <xref:System.Type> 的参数名称。 例如，对于方法签名 `Type.GetInterfaceMap(Type interfaceType)`，`Name` 特性的值为“接口类型”。|  
|`Activate`|反射|可选特性。 控制运行时对构造函数的访问，以启用实例激活。|  
|`Browse`|反射|可选特性。 控制对有关程序元素信息的查询，但并不启用任何运行时访问。|  
|`Dynamic`|反射|可选特性。 控制运行时对所有类型成员的访问，包括构造函数、方法、字段、属性和事件，以启用动态编程。|  
|`Serialize`|序列化|可选特性。 控制运行时对构造函数、字段和属性的访问，使类型实例得到序列化和反序列化处理，这是通过库进行的，例如 Newtonsoft JSON 序列化程序。|  
|`DataContractSerializer`|序列化|可选特性。 控制使用 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> 类的序列化策略。|  
|`DataContractJsonSerializer`|序列化|可选特性。 控制使用 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType> 类的 JSON 序列化策略。|  
|`XmlSerializer`|序列化|可选特性。 控制使用 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> 类的 XML 序列化策略。|  
|`MarshalObject`|Interop|可选特性。 控制封送引用类型到 Windows 运行时和 COM 的策略。|  
|`MarshalDelegate`|Interop|可选特性。 控制将委托类型作为函数指针封送到本机代码的策略。|  
|`MarshalStructure`|Interop|可选特性。 控制封送处理值类型到本机代码的策略。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|*parameter_name*|类型 <xref:System.Type> 的参数名称。 例如，对于方法签名 `Type.GetInterfaceMap(Type interfaceType)`，`Name` 特性的值为“接口类型”。|  
  
## <a name="all-other-attributes"></a>所有其他特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该设置将应用到这种策略类型。 可能值为 `All`、`Public`、`PublicAndInternal`、`Required Public`、`Required PublicAndInternal` 以及 `Required All`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Method>](method-element-net-native.md)|将运行时反射策略应用到一个构造函数或方法。|  
  
## <a name="remarks"></a>备注  

 `<TypeParameter>`元素类似于 [\<Parameter>](parameter-element-net-native.md) 元素，只不过它只能应用于类型的参数 <xref:System.Type> 。 它将策略应用到任何在运行时间由 `Name` 特性指定的以类型参数为代表的类型。  
  
 例如，NewtonSoft JSON 序列化程序包含一个静态 `JsonConvert.DeserializeObject(String value, Type type)` 方法。 以下是反射指令：  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <Type Name="Newtonsoft.Json.JsonConvert" >  
      <Method Name="DeserializeObject">  
         <GenericParameter Name="type" Serialize="Required All" />  
      </Method>  
   </Type>  
</Directives>  
```  
  
 指定 `type` 参数所代表的运行时类型的元数据应可用于序列化。 如果这些运行时指令适用于一个包含以下源代码的项目：  
  
```csharp  
Type t = typeof(StockQuote);  
Object obj = JsonConvert.DeserializeObject(data, t);  
```  
  
 反射指令使 `StockQuote` 类型的元数据在运行时间可由 NewtonSoft JSON 序列化程序使用。  
  
## <a name="see-also"></a>请参阅

- [\<Method> 元素](method-element-net-native.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令策略设置](runtime-directive-policy-settings.md)
- [运行时指令元素](runtime-directive-elements.md)
