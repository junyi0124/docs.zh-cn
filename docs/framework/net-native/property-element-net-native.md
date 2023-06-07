---
title: <Property>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: ad4ba56d-3bcb-4c10-ba90-1cc66e2175a1
ms.openlocfilehash: a0bdf95a1d1cadf7423f8c6595add13eda4d0d9a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250848"
---
# <a name="property-element-net-native"></a>\<Property>元素 (.NET Native)

将运行时反射策略应用到一个属性。  
  
## <a name="syntax"></a>语法  
  
```xml  
<Property Name="property_name"  
          Browse="policy_type"  
          Dynamic="policy_type"  
          Serialize="policy_type" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|属性类型|描述|  
|---------------|--------------------|-----------------|  
|`Name`|常规|必需的特性。 指定属性名称。|  
|`Browse`|反射|可选特性。 制对该属性信息的查询或列举该属性，但并不在运行时间启用任何动态访问。|  
|`Dynamic`|反射|可选特性。 控制运行时对该属性的访问，以启用动态编程。 该策略确保一个属性可在运行时间内得到设置或动态检索。|  
|`Serialize`|序列化|可选特性。 控制运行时对一个属性的访问以启用类型实例，使其通过程序库得到序列化，例如通过 Newtonsoft JSON 序列化程序，或被用于绑定数据。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|method_name|属性名称。 属性的类型由父级 [\<Type>](type-element-net-native.md) 或 [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素定义。|  
  
## <a name="all-other-attributes"></a>所有其他特性  
  
|值|描述|  
|-----------|-----------------|  
|*策略_设置*|该设置将应用到这个属性的策略类型。 可能值为 `Auto`、`Excluded`、`Included` 和 `Required`。 有关详细信息，请参阅[运行时指令策略设置](runtime-directive-policy-settings.md)。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Type>](type-element-net-native.md)|将反射策略应用到一种类型及其所有成员。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|将反射策略应用到一种构造泛型类型及其所有成员。|  
  
## <a name="remarks"></a>备注  

 如果一个属性的策略没有得到显式定义，它将继承其父元素的运行时间策略。  
  
## <a name="example"></a>示例  

 以下实例使用反射实例化了一个 `Book` 对象并显示了其属性值。 该项目的原始 default.rd.xml 文件显示如下：  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <Application>  
      <Namespace Name="LibraryApplications"  Browse="Required Public" >  
         <Type Name="Book"   Activate="All" />  
      </Namespace>  
   </Application>  
</Directives>  
```  
  
 该文件将 `All` 值应用到允许通过反射访问类构造函数的 `Activate` 的类的 `Book` 策略。 `Browse` 类的 `Book` 策略是从其父命名空间继承的。 这已设置为 `Required Public`，这使得元数据在运行时可用。  
  
 以下是该实例的源代码。 `outputBlock`变量表示一个 <xref:Windows.UI.Xaml.Controls.TextBlock> 控件。  
  
 [!code-csharp[ProjectN_Reflection#6](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/property1.cs#6)]  
  
 然而，编译和执行此示例将引发 [MissingRuntimeArtifactException](missingruntimeartifactexception-class-net-native.md) 异常。 尽管我们已经使 `Book` 类型的元数据变得可用，我们却未使属性获取者的实施变得动态可用。 我们可通过以下方法之一纠正这个错误：  
  
- 通过 `Dynamic` `Book` 在其元素中定义类型的策略 [\<Type>](type-element-net-native.md) 。  
  
- 通过为要 [\<Property>](property-element-net-native.md) 调用其 getter 的每个属性添加一个嵌套元素，如下 default.rd.xml 文件所示。  
  
    ```xml  
    <Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
       <Application>  
          <Namespace Name="LibraryApplications"  Browse="Required Public" >  
             <Type Name="Book"   Activate="All" >  
                <Property Name="Title" Dynamic="Required" />  
                <Property Name="Author" Dynamic="Required" />  
                  <Property Name="ISBN" Dynamic="Required" />  
             </Type>  
          </Namespace>  
       </Application>  
    </Directives>  
    ```  
  
## <a name="see-also"></a>另请参阅

- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令元素](runtime-directive-elements.md)
- [运行时指令策略设置](runtime-directive-policy-settings.md)
