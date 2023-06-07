---
title: <Method>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: 348b49e5-589d-4eb2-a597-d6ff60ab52d1
ms.openlocfilehash: 1d57457c90e44c70caa301eccc02c5831d283cea
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287899"
---
# <a name="method-element-net-native"></a>\<Method>元素 (.NET Native)

将运行时反射策略应用到一个构造函数或方法。  
  
## <a name="syntax"></a>语法  
  
```xml  
<Method Name="method_name"  
        Signature="method_signature"  
        Browse="policy_type"  
        Dynamic="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|属性类型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|常规|必需的特性。 指定方法名称。|  
|`Signature`|常规|可选特性。 指定方法签名。 如果存在多个参数，它们之间用逗号分割。 例如，以下 `<Method>` 元素定义了 <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29> 方法的策略。<br /><br /> `<Type Name="System.DateTime">    <Method Name="ToString" Signature="System.String,System.IFormatProvider"            Dynamic="Required" /> </Type>`<br /><br /> 如果该特性不存在，运行时指令将应用到该方法的所有重载。|  
|`Browse`|反射|可选特性。 控制对该方法信息的查询或列举该方法，但并不在运行时间启用任何动态调用。|  
|`Dynamic`|反射|可选特性。 控制运行时对构造函数或方法的访问，以启用动态编程。 该策略确保一个成员可在运行时间内得到调用。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|method_name|方法名称。 方法的类型由父级 [\<Type>](type-element-net-native.md) 或 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素定义。|  
  
## <a name="signature-attribute"></a>签名特性  
  
|值|描述|  
|-----------|-----------------|  
|*method_signature*|形成方法签名的参数类型。 多个参数由逗号分隔，例如，`"System.String,System.Int32,System.Int32)"`。 参数类型名称应是完全限定的。|  
  
## <a name="all-other-attributes"></a>所有其他特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该设置将应用到这种策略类型。 可能值为 `Auto`、`Excluded`、`Included` 和 `Required`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Parameter>](parameter-element-net-native.md)|将策略应用到传递到方法的参数类型。|  
|[\<GenericParameter>](genericparameter-element-net-native.md)|将策略应用到一个泛型类型或方法的参数类型。|  
|[\<ImpliesType>](impliestype-element-net-native.md)|如果该策略已应用到以包含 `<Method>` 元素为代表的方法，将该策略应用到一个类型。|  
|[\<TypeParameter>](typeparameter-element-net-native.md)|将策略应用到以传递到方法为代表的 <xref:System.Type> 自变量类型。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|将反射策略应用到一种类型及其所有成员。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|将反射策略应用到一种构造泛型类型及其所有成员。|  
  
## <a name="remarks"></a>备注  

 一个泛型方法的 `<Method>` 元素会将其策略应用到所有不具有自身策略的实例化。  
  
 你可以使用 `Signature` 特性为特定的方法重载指定策略。 如果 `Signature` 特性不存在，运行时指令将应用到该方法的所有重载。  
  
 你无法通过使用 `<Method>` 元素为一个构造函数定义运行时反射策略。 相反，请使用 `Activate`  [\<Assembly>](assembly-element-net-native.md) 、 [\<Namespace>](namespace-element-net-native.md) 、 [\<Type>](type-element-net-native.md) 或元素的特性 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 。  
  
## <a name="example"></a>示例  

 以下实例中的 `Stringify` 方法是使用反射将一个对象转化为其字符串表示形式的一个通用格式化方法。 除调用该对象的默认 `ToString` 方法外，该方法可以通过穿过一个对象的 `ToString` 方法格式字符车产生一个格式化的字符串或/和一个 <xref:System.IFormatProvider> 实现。 它也可以调用将一个成员转化为其二进制、十六进制或八进制表示的 <xref:System.Convert.ToString%2A?displayProperty=nameWithType> 重载之一。  
  
 [!code-csharp[ProjectN_Reflection#7](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/method1.cs#7)]  
  
 `Stringify` 方法可以通过类似以下的代码调用：  
  
 [!code-csharp[ProjectN_Reflection#7](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/method1.cs#7)]  
  
 然而，使用 .NET Native 编译时，该示例可能会在运行时引发一系列异常，包括 <xref:System.NullReferenceException> 和 [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常。原因在于 `Stringify` 方法的本来目的是支持动态地设置 .NET Framework 类库中基元类型的格式。 然而，它们的元数据不会通过默认指令文件变得可以使用。 即使在它们的元数据变得可用时，实例还会引发 [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常，因为适当的 `ToString` 实现并未包含在本机代码之中。  
  
 通过使用 [\<Type>](type-element-net-native.md) 元素定义其元数据必须存在的类型，以及通过添加 `<Method>` 元素来确保也存在可以动态调用的方法重载的实现，可以消除这些异常。 以下的 default.rd.xml 文件可以清除这些异常并允许实例没有错误地执行。  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
  <Application>  
     <Assembly Name="*Application*" Dynamic="Required All" />  
  
     <Type Name = "System.Convert" Browse="Required Public" Dynamic="Required Public" >  
        <Method Name="ToString"    Dynamic ="Required" />  
     </Type>  
     <Type Name="System.Double" Browse="Required Public">  
        <Method Name="ToString" Dynamic="Required" />  
     </Type>  
     <Type Name ="System.Int32" Browse="Required Public" >  
        <Method Name="ToString" Dynamic="Required" />  
     </Type>  
     <Type Name ="System.Int64" Browse="Required Public" >  
        <Method Name="ToString" Dynamic="Required" />  
     </Type>  
     <Namespace Name="System" >  
        <Type Name="Byte" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="DateTime" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Decimal" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Guid" Browse ="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Int16" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="SByte" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="Single" Browse="Required Public" >  
          <Method Name="ToString" Dynamic="Required" />
        </Type>  
        <Type Name="TimeSpan" Browse="Required Public" >  
          <Method Name="ToString" Dynamic="Required" />
        </Type>  
        <Type Name="UInt16" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="UInt32" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
        <Type Name="UInt64" Browse="Required Public" >  
           <Method Name="ToString" Dynamic="Required" />  
        </Type>  
     </Namespace>  
  </Application>  
</Directives>  
```  
  
## <a name="see-also"></a>另请参阅

- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令元素](runtime-directive-elements.md)
- [运行时指令策略设置](runtime-directive-policy-settings.md)
- [\<MethodInstantiation> 元素](methodinstantiation-element-net-native.md)
