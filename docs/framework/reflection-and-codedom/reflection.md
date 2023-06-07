---
title: .NET 中的反射
description: 查看 .NET 中的反射。 获取有关加载的程序集和其中定义的类型的信息，如类、接口、结构和枚举。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET], reflection
- EventInfo class, reflection
- common language runtime, reflection
- FieldInfo class, reflection
- ParameterInfo class, reflection
- ConstructorInfo class, reflection
- Assembly class, reflection
- value types, reflection
- reflection, about reflection
- modules, reflection
- runtime, reflection
- Module class, reflection
- MethodInfo class, reflection
- PropertyInfo class, reflection
- type browsers
- reflection
- discovering type information at run time
- type system, reflection
ms.assetid: d1a58e7f-fb39-4d50-bf84-e3b8f9bf9775
ms.openlocfilehash: 371f56c3a57b8e320c9b329c539075a40cd7a19f
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258408"
---
# <a name="reflection-in-net"></a>.NET 中的反射

<xref:System.Reflection> 命名空间中的类与 <xref:System.Type?displayProperty=nameWithType> 使你能够获取有关加载的[程序集](../../standard/assembly/index.md)和其中定义的类型的信息，如[类](../../standard/base-types/common-type-system.md#classes)、[接口](../../standard/base-types/common-type-system.md#interfaces)和值类型（即[结构](../../standard/base-types/common-type-system.md#structures)和[枚举](../../standard/base-types/common-type-system.md#enumerations)）。 可以使用反射在运行时创建、调用和访问类型实例。 有关反射的特定方面的主题，请参见本概述末的[相关主题](#related_topics)。
  
[公共语言运行时](../../standard/clr.md)加载程序管理[应用程序域](../app-domains/application-domains.md)，应用程序域构成具有相同应用程序范围的对象周围定义的边界。 此管理包括将每个程序集加载到相应的应用程序域中和控制每个程序集内的类型层次结构的内存布局。  
  
[程序集](../app-domains/index.md)包含模块、模块包含类型，而类型包含成员。 反射提供封装程序集、模块和类型的对象。 可以使用反射动态地创建类型的实例，将类型绑定到现有对象，或从现有对象中获取类型。 然后，可以调用类型的方法或访问其字段和属性。 反射的典型用法如下所示：  
  
- 使用 <xref:System.Reflection.Assembly> 来定义和加载程序集，加载程序集清单中列出的模块，以及在此程序集中定位一个类型并创建一个它的实例。  
  
- 使用 <xref:System.Reflection.Module> 发现信息，如包含模块的程序集和模块中的类。 还可以获取所有全局方法或模块上定义的其它特定的非全局方法。  
  
- 使用 <xref:System.Reflection.ConstructorInfo> 发现信息，如名称、参数、访问修饰符（如 `public` 或 `private`）和构造函数的实现详细信息（如 `abstract` 或 `virtual`）。 使用 <xref:System.Type> 的 <xref:System.Type.GetConstructors%2A> 或 <xref:System.Type.GetConstructor%2A> 方法来调用特定构造函数。  
  
- 使用 <xref:System.Reflection.MethodInfo> 发现信息，如名称、返回类型、参数、访问修饰符（如 `public` 或 `private`）和方法的实现详细信息（如 `abstract` 或 `virtual`）。 使用 <xref:System.Type> 的 <xref:System.Type.GetMethods%2A> 或 <xref:System.Type.GetMethod%2A> 方法来调用特定方法。  
  
- 使用 <xref:System.Reflection.FieldInfo> 发现信息，如名称、访问修饰符（如 `public` 或 `private`）和一个字段的实现详细信息 （如 `static`）；并获取或设置字段值。  
  
- 使用 <xref:System.Reflection.EventInfo> 发现信息（如名称、事件处理程序的数据类型、自定义特性、声明类型以及事件的反射的类型），并添加或删除事件处理程序。  
  
- 使用 <xref:System.Reflection.PropertyInfo> 发现信息（如名称、数据类型、声明类型，反射的类型和属性的只读或可写状态），并获取或设置属性值。  
  
- 使用 <xref:System.Reflection.ParameterInfo> 发现信息，如参数的名称、数据类型、参数是输入参数还是输出参数以及参数在方法签名中的位置。  
  
- 使用 <xref:System.Reflection.CustomAttributeData> 在于应用程序域的仅反射上下文中工作时发现有关自定义特性的信息。 <xref:System.Reflection.CustomAttributeData> 使你能够检查特性，而无需创建它们的实例。  
  
<xref:System.Reflection.Emit> 命名空间的类提供一种专用形式的反射，使你能够在运行时生成类型。  
  
还可以使用反射来创建称为类型浏览器的应用程序，它使用户能够选择类型，然后查看有关这些类型的信息。  
  
反射还有其它用途。 JScript 等语言的编译器使用反射来构造符号表。 <xref:System.Runtime.Serialization> 命名空间中的类使用反射来访问数据并确定要保存哪些字段。 <xref:System.Runtime.Remoting> 命名空间中的类通过序列化间接使用反射。  
  
## <a name="runtime-types-in-reflection"></a>反射中的运行时类型  

反射提供类（如 <xref:System.Type> 和 <xref:System.Reflection.MethodInfo>），用于表示类型、成员、参数和其它代码实体。 但使用反射时，你并不直接使用这些类，其中大部分类均是抽象的（Visual Basic 中为 `MustInherit`）。 相反，你使用由公共语言运行时 (CLR) 提供的类型。  
  
例如，使用 C# `typeof` 运算符（Visual Basic 中为 `GetType`）获取 <xref:System.Type> 对象时，该对象实际上是 `RuntimeType`。 `RuntimeType` 派生自 <xref:System.Type>，并提供所有抽象方法的实现。  
  
这些运行时类是 `internal`（Visual Basic 中为 `Friend`）。 它们没有与其基类分开记录，因为它们的行为由基类文档来描述。  
  
<a name="related_topics"></a>

## <a name="related-topics"></a>相关主题  
  
|Title|描述|  
|-----------|-----------------|  
|[查看类型信息](viewing-type-information.md)|介绍 <xref:System.Type> 类，并提供演示如何使用具有几个反射类的 <xref:System.Type> 来获取有关构造函数、方法、字段、属性和事件的信息的代码示例。|  
|[反射类型和泛型类型](reflection-and-generic-types.md)|说明反射如何处理泛型类型和泛型方法的类型参数和类型自变量。|  
|[反射的安全注意事项](security-considerations-for-reflection.md)|描述确定可以在何种程度上使用反射来发现类型信息和访问类型的规则。|  
|[动态加载和使用类型](dynamically-loading-and-using-types.md)|描述支持后期绑定的反射自定义绑定接口。|  
|[如何：将程序集加载到仅反射上下文中](how-to-load-assemblies-into-the-reflection-only-context.md)|描述仅反射的加载上下文。 显示如何加载程序集、如何测试上下文以及如何检查应用到仅反射上下文中的程序集。|  
|[访问自定义属性](accessing-custom-attributes.md)|演示如何使用反射来查询特性的存在和值。|  
|[指定完全限定的类型名称](specifying-fully-qualified-type-names.md)|描述 Backus-Naur 形式 (BNF) 的完全限定类型名称的格式，以及指定特殊字符、程序集名称、指针、引用和数组所需的语法。|  
|[如何：使用反射将委托挂钩](how-to-hook-up-a-delegate-using-reflection.md)|说明如何创建方法的委托并将委托挂钩到事件。 说明如何使用 <xref:System.Reflection.Emit.DynamicMethod> 在运行时创建事件处理方法。|  
|[发出动态方法和程序集](emitting-dynamic-methods-and-assemblies.md)|说明如何生成动态程序集和动态方法。|  
  
## <a name="reference"></a>参考  

<xref:System.Type?displayProperty=nameWithType>  
  
<xref:System.Reflection>  
  
<xref:System.Reflection.Emit>  
