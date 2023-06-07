---
title: 反射类型和泛型类型
description: .NET 中的反射和泛型类型入门。 与普通类型不同，泛型类型与一组类型形参或类型实参相关联。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- reflection emit, generic types
- reflection, generic types
- type arguments
- generics [.NET Framework], reflection
- viewing type information
- type information, viewing
- types, generic
- type parameters
ms.assetid: f7180fc5-dd41-42d4-8a8e-1b34288e06de
ms.openlocfilehash: 2add589d171163d5497e80ed7b7990c3596add20
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96259910"
---
# <a name="reflection-and-generic-types"></a>反射类型和泛型类型

从反射的角度来说，泛型类型和普通类型之间的区别在于泛型类型具有与之关联的一组类型形参（若是泛型类型定义）或类型实参（若是构造类型）。 泛型方法和普通方法以相同方式互不相同。  
  
 有两个关键点可了解反射如何处理泛型类型和方法：  
  
- 泛型类型定义和泛型方法定义的类型参数由 <xref:System.Type> 类的实例表示。  
  
    > [!NOTE]
    > <xref:System.Type> 对象表示泛型类型参数时， <xref:System.Type> 的很多属性和方法具有不同的行为。 这些差异记录在属性和方法主题中。 有关示例，请参阅 <xref:System.Type.IsAutoClass%2A> 和 <xref:System.Type.DeclaringType%2A>。 此外，某些成员仅在 <xref:System.Type> 对象表示泛型类型参数时有效。 有关示例，请参阅 <xref:System.Type.GetGenericTypeDefinition%2A>。  
  
- 如果 <xref:System.Type> 的实例表示泛型类型，则它包含表示类型形参（对于泛型类型定义）或类型实参（对于构造类型）的类型数组。 这同样适用于表示泛型方法的 <xref:System.Reflection.MethodInfo> 的类的实例。  
  
 反射提供 <xref:System.Type> 和 <xref:System.Reflection.MethodInfo> 的方法，它允许你访问类型形参的数组并确定 <xref:System.Type> 的实例是表示类型形参还是表示实际类型。  
  
 有关演示此处讨论的方法的示例代码，请参阅[如何：使用反射检查和实例化泛型类型](how-to-examine-and-instantiate-generic-types-with-reflection.md)中提供的其他信息。  
  
 以下讨论假定熟悉泛型的术语，例如类型形参和实参之间的差异以及开放式或封闭式构造类型之间的差异。 有关详细信息，请参阅[泛型](../../standard/generics/index.md)。  

## <a name="is-this-a-generic-type-or-method"></a>这是泛型类型还是泛型方法？  

 当使用反射来检查未知类型（由 <xref:System.Type>的实例表示）时，请使用 <xref:System.Type.IsGenericType%2A> 属性来确定未知类型是否为泛型。 如果类型是泛型，它会返回 `true` 。 同样，当检查未知方法（由 <xref:System.Reflection.MethodInfo> 类的实例表示）时，请使用 <xref:System.Reflection.MethodBase.IsGenericMethod%2A> 属性来确定此方法是否为泛型。  
  
### <a name="is-this-a-generic-type-or-method-definition"></a>这是泛型类型定义还是泛型方法定义？  

 使用 <xref:System.Type.IsGenericTypeDefinition%2A> 属性来确定 <xref:System.Type> 对象是否表示泛型类型定义，并使用 <xref:System.Reflection.MethodBase.IsGenericMethodDefinition%2A> 方法来确定 <xref:System.Reflection.MethodInfo> 是否表示泛型方法定义。  
  
 泛型类型定义和泛型方法定义是一些从中创建可实例化类型的模板。 .NET Framework 类库中的泛型类型（如 <xref:System.Collections.Generic.Dictionary%602>）是泛型类型定义。  
  
### <a name="is-the-type-or-method-open-or-closed"></a>类型或方法是开放式还是封闭式的？  

 如果可实例化类型都已替换为所有类型形参（包括所有封闭类型的所有类型形参），则泛型类型或方法是封闭式的。 若为封闭式，则只能创建泛型类型的实例。 如果类型为开放式， <xref:System.Type.ContainsGenericParameters%2A?displayProperty=nameWithType> 属性将返回 `true` 。 对于方法， <xref:System.Reflection.MethodBase.ContainsGenericParameters%2A?displayProperty=nameWithType> 方法执行相同的功能。

## <a name="generating-closed-generic-types"></a>生成封闭式泛型类型  

 在具有泛型类型或方法定义之后，请使用 <xref:System.Type.MakeGenericType%2A> 方法来创建封闭式泛型类型或者使用 <xref:System.Reflection.MethodInfo.MakeGenericMethod%2A> 方法来为封闭式泛型方法创建 <xref:System.Reflection.MethodInfo> 。  
  
### <a name="getting-the-generic-type-or-method-definition"></a>获取泛型类型或方法定义  

 如果你具有开放式泛型类型或方法（非泛型类型或方法定义），则不能创建它的实例，也不能提供缺少的类型形参。 必须具有泛型类型或方法定义。 使用 <xref:System.Type.GetGenericTypeDefinition%2A> 方法来获取泛型类型定义或使用 <xref:System.Reflection.MethodInfo.GetGenericMethodDefinition%2A> 方法来获取泛型方法定义。  
  
 例如，如果你有一个表示 <xref:System.Type> 的 `Dictionary<int, string>` 对象（在 Visual Basic 中为`Dictionary(Of Integer, String)` ）且想创建类型 `Dictionary<string, MyClass>`，则可以使用 <xref:System.Type.GetGenericTypeDefinition%2A> 方法来获取表示 <xref:System.Type> 的 `Dictionary<TKey, TValue>` ，然后使用 <xref:System.Type.MakeGenericType%2A> 方法来生成表示 <xref:System.Type> 的 `Dictionary<int, MyClass>`。  
  
 有关非泛型类型的开放式泛型类型的示例，请参阅本主题稍后部分的“类型形参或类型实参”。

## <a name="examining-type-arguments-and-type-parameters"></a>检查类型实参和类型形参  

 使用 <xref:System.Type.GetGenericArguments%2A?displayProperty=nameWithType> 方法来获取 <xref:System.Type> 对象的数组（表示泛型类型的类型形参或类型实参），并使用 <xref:System.Reflection.MethodInfo.GetGenericArguments%2A?displayProperty=nameWithType> 方法为泛型方法执行相同的操作。  
  
 在了解 <xref:System.Type> 对象表示类型形参之后，即可借助反射来回答很多其他问题。 可以确定类型形参的源、位置以及约束。  
  
### <a name="type-parameter-or-type-argument"></a>类型形参或类型实参  

 若要确定数组的某个特定元素是类型形参还是类型实参，请使用 <xref:System.Type.IsGenericParameter%2A> 属性。 如果元素是类型形参，则 <xref:System.Type.IsGenericParameter%2A> 属性为 `true` 。  
  
 在不是泛型类型定义的情况下，泛型类型也可以是开放式，此时它同时具有类型实参和类型形参。 例如，在下面的代码中，类 `D` 派生自通过将 `D` 的第一个类型参数替换为 `B`的第二个类型参数来创建的类型。  
  
```csharp  
class B<T, U> {}  
class D<V, W> : B<int, V> {}  
```  
  
```vb  
Class B(Of T, U)  
End Class  
Class D(Of V, W)  
    Inherits B(Of Integer, V)  
End Class  
```  
  
```cpp  
generic<typename T, typename U> ref class B {};  
generic<typename V, typename W> ref class D : B<int, V> {};  
```  
  
 如果你获取表示 <xref:System.Type> 的 `D<V, W>` 对象，并使用 <xref:System.Type.BaseType%2A> 属性来获取其基类型，则产生的 `type B<int, V>` 是开放式的，但它不是泛型类型定义。  
  
### <a name="source-of-a-generic-parameter"></a>泛型参数的源  

 泛型类型形参可能来自要检查的类型、封闭类型或者泛型方法。 可以按以下方式确定泛型类型形参的源：  
  
- 首先，使用 <xref:System.Type.DeclaringMethod%2A> 属性来确定类型形参是否来自泛型方法。 如果属性值不是 null 引用（在 Visual Basic 中为`Nothing` ），则源是泛型方法。  
  
- 如果源不是泛型方法，则使用 <xref:System.Type.DeclaringType%2A> 属性来确定泛型类型形参所属的泛型类型。  
  
 如果类型形参属于泛型方法，则 <xref:System.Type.DeclaringType%2A> 属性返回声明泛型方法的类型，这是不相关的类型。  
  
### <a name="position-of-a-generic-parameter"></a>泛型参数的位置  

 在极少数情况下，必须确定类型形参在其声明类的类型形参列表中的位置。 例如，假设你有表示上述示例中的 <xref:System.Type> 类型的 `B<int, V>` 对象。 <xref:System.Type.GetGenericArguments%2A> 方法提供类型实参的列表，在检查 `V` 时即可使用 <xref:System.Type.DeclaringMethod%2A> 和 <xref:System.Type.DeclaringType%2A> 属性来发现它的来源位置。 然后，可以使用 <xref:System.Type.GenericParameterPosition%2A> 属性来确定它在定义它的类型形参列表中的位置。 在此示例中， `V` 在定义它的类型形参列表中位于位置 0（零）。  
  
### <a name="base-type-and-interface-constraints"></a>基类型和接口约束  

 使用 <xref:System.Type.GetGenericParameterConstraints%2A> 方法来获取类型形参的基类型约束和接口约束。 数组的元素顺序并不重要。 如果元素是接口类型，则它表示接口约束。  
  
### <a name="generic-parameter-attributes"></a>泛型参数特性  

 <xref:System.Type.GenericParameterAttributes%2A> 属性获取 <xref:System.Reflection.GenericParameterAttributes> 值，该值指示类型形参的方差（协变或逆变）和特殊约束。  
  
#### <a name="covariance-and-contravariance"></a>协变和逆变  

 若要确定类型形参是协变还是逆变，请将 <xref:System.Reflection.GenericParameterAttributes.VarianceMask?displayProperty=nameWithType> 掩码应用到 <xref:System.Reflection.GenericParameterAttributes> 属性返回的 <xref:System.Type.GenericParameterAttributes%2A> 值。 如果结果为 <xref:System.Reflection.GenericParameterAttributes.None?displayProperty=nameWithType>，则类型形参不变。 请参阅 [协变和逆变](../../standard/generics/covariance-and-contravariance.md)。  
  
#### <a name="special-constraints"></a>特殊约束  

 若要确定类型形参的特殊约束，请将 <xref:System.Reflection.GenericParameterAttributes.SpecialConstraintMask?displayProperty=nameWithType> 掩码应用到 <xref:System.Reflection.GenericParameterAttributes> 属性返回的 <xref:System.Type.GenericParameterAttributes%2A> 值。 如果结果为 <xref:System.Reflection.GenericParameterAttributes.None?displayProperty=nameWithType>，则没有任何特殊约束。 可将类型形参约束为引用类型、不可以为 null 的类型以及具有无参数构造函数。

## <a name="invariants"></a>固定协定  

 有关泛型类型反射中常用术语的固定条件表格，请参阅 <xref:System.Type.IsGenericType%2A?displayProperty=nameWithType>。 有关与泛型方法相关的其他术语，请参阅 <xref:System.Reflection.MethodBase.IsGenericMethod%2A?displayProperty=nameWithType>。  

## <a name="related-topics"></a>相关主题  
  
|Title|描述|  
|-----------|-----------------|  
|[如何：使用反射检查和实例化泛型类型](how-to-examine-and-instantiate-generic-types-with-reflection.md)|演示如何使用 <xref:System.Type> 和 <xref:System.Reflection.MethodInfo> 的属性和方法来检查泛型类型。|  
|[泛型](../../standard/generics/index.md)|描述泛型功能以及 .NET Framework 如何支持它。|  
|[如何：使用反射发出定义泛型类型](how-to-define-a-generic-type-with-reflection-emit.md)|演示如何在动态程序集中使用反射发出生成泛型类型。|  
|[查看类型信息](viewing-type-information.md)|介绍 <xref:System.Type> 类并提供演示如何使用具有各种反射类的 <xref:System.Type> 来获取有关构造函数、方法、字段、属性和事件的信息的代码示例。|
