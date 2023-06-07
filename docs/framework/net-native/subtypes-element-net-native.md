---
title: <Subtypes>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: fb854070-248b-46cf-9dab-c322e2b4d624
ms.openlocfilehash: 7484152c351f59ee84b601584bd84347186628a3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287808"
---
# <a name="subtypes-element-net-native"></a>\<Subtypes>元素 (.NET Native)

将运行时策略应用到从包含类型继承的所有类。  
  
## <a name="syntax"></a>语法  
  
```xml  
<Subtypes Activate="policy_type"  
          Browse="policy_type"  
          Dynamic="policy_type"  
          Serialize="policy_type"
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
  
## <a name="all-attributes"></a>所有特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该设置将应用到这种策略类型。 可能值为 `All`、`Auto`、`Excluded`、`Public`、`PublicAndInternal`、`Required Public`、`Required PublicAndInternal` 以及 `Required All`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|将反射策略应用到一种类型及其所有成员。|  
  
## <a name="remarks"></a>备注  

 `<Subtypes>` 元素将策略应用到其包含类型的所有子类型。 当你想将不同的策略应用到派生类型及其基类时可使用它。  
  
 反射、序列化和互操作特性都是可选项，但必须存在至少一项。  
  
## <a name="example"></a>示例  

 以下实例定义了一个名为 `BaseClass` 的类和一个名为 `Derived1` 的子类。  
  
 [!code-csharp[ProjectN_Reflection#4](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/subtypes.cs#4)]  
  
 如以下代码所示，运行时指令文件为 `Dynamic` 到 `Activate` 显式设置了 `BaseClass` 和 `Excluded` 策略。 因此，类型 `BaseClass` 的对象无法得到动态实例化或通过调用 `BaseClass` 类构造函数得到实例化。 然而，`<Subtypes>` 元素允许从 `BaseClass` 派生的类得到动态实例化并通过对其类构造函数的调用得到实例化。  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
  <Application>  
   <Assembly Name="*Application*" Dynamic="Required All" />  
     <Type Name="Examples.Libraries.BaseClass" Activate ="Excluded" Dynamic="Excluded" >  
        <Subtypes Activate="Public" Dynamic ="Public"/>  
     </Type>  
  </Application>  
</Directives>  
```  
  
 由于 `<Subtypes>` 指令的存在，以下代码通过成功调用 `Derived1` 方法执行动态实例化了一个 <xref:System.Activator.CreateInstance%28System.Type%29?displayProperty=nameWithType> 实例。  这里的块变量是一个空白 Windows 应用商店应用的一个 <xref:System.Windows.Controls.TextBlock> 对象。  
  
 [!code-csharp[ProjectN_Reflection#5](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/subtypes.cs#5)]  
  
## <a name="see-also"></a>请参阅

- [\<Type> 元素](type-element-net-native.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令元素](runtime-directive-elements.md)
- [运行时指令策略设置](runtime-directive-policy-settings.md)
