---
title: <Library>元素 (.NET Native)
ms.date: 03/30/2017
ms.assetid: f642276b-33fb-4a81-b882-8808c31ba69e
ms.openlocfilehash: aeaa6b1a9c3c4ceebdd0eab3f331a044971398bf
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287912"
---
# <a name="library-element-net-native"></a>\<Library>元素 (.NET Native)

定义包含元数据在运行时间可以用于反射的类型和类型成员的程序集。  
  
 \<Directives> 元素  
\<Library> 元素  
  
## <a name="syntax"></a>语法  
  
```xml  
<Library Name="assembly_name" />  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`Name`|必需的特性。 指定一个程序集的名称。 这个 `<Library>` 元素的子元素为在这个程序集中找到的类型和类型成员定义运行时反射策略。|  
  
## <a name="name-attribute"></a>Name 特性  
  
|值|描述|  
|-----------|-----------------|  
|assembly_name|程序集的简单名称，不要包含文件扩展名。 此特性对应 <xref:System.Reflection.AssemblyName.Name%2A?displayProperty=nameWithType> 属性。 例如，一个名为 Extensions.dll 的程序集的名称为“Extensions”。 参阅“备注”部分，了解支持对来自程序集的元数据有条件包含的 assembly_name 的一种特殊形式。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Assembly>](assembly-element-net-native.md)|将策略应用到特定程序集中的所有类型。|  
|[\<Namespace>](namespace-element-net-native.md)|将策略应用到特定命名空间中的所有类型。|  
|[\<Type>](type-element-net-native.md)|将策略应用到一个特定类型，例如一个类或结构。|  
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|将策略应用到一个构造泛型类型。 例如， [\<TypeInstantiation>](typeinstantiation-element-net-native.md) 元素可以用于定义类型的策略 `List<String>` 。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<Directives>](directives-element-net-native.md)|运行时指令文件的根元素。|  
  
## <a name="remarks"></a>备注  

 [\<Directives>](directives-element-net-native.md)元素可以包含零个、一个或多个 `<Library>` 元素。  
  
 `<Library>` 元素充当容器，用来定义其元数据在运行时间需要存在的程序元素；此元素不表示策略。 在编译时间，编译器工具仅搜索由 `<Library>` 元素指定的库，以查找其子元素识别出的程序元素。 与此相反，编译器工具会搜索所有库，including.NET Framework core 库，适用于元素的子元素标识的程序元素 [\<Application>](application-element-net-native.md) 。  
  
 `<Library>` 指令可以有条件地使用。 如果元素的名称以 `<Library>` 星号 () 的开头和结尾 \* ，则 `<Library>` 只有当应用程序引用星号之间指定的程序集时，指令才会生效。 例如，仅当应用引用 Utilities.dll 程序集时，以下运行时指令才适用。  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
  <Library Name="*Utilities*">  
   ...  
  </Library>  
</Directives>  
```  
  
## <a name="see-also"></a>请参阅

- [\<Application> 元素](application-element-net-native.md)
- [\<Directives> 元素](directives-element-net-native.md)
- [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)
- [运行时指令元素](runtime-directive-elements.md)
