---
title: 泛型中的协变和逆变
description: 了解 .NET 泛型中的协变和逆变，使用前者则可使用派生程度更大的类型，而使用后者则可使用派生程度更小的派生类型。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- generics, covariance and contravariance
- generics, variance
- covariance and contravariance in generics
- generic type parameters
ms.assetid: 2678dc63-c7f9-4590-9ddc-0a4df684d42e
ms.openlocfilehash: 9d5d5b27fb77500aa5f6deff3fcb1c739ba8b094
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722656"
---
# <a name="covariance-and-contravariance-in-generics"></a>泛型中的协变和逆变

协变和逆变都是术语，前者指能够使用比原始指定的派生类型的派生程度更大（更具体的）的类型，后者指能够使用比原始指定的派生类型的派生程度更小（不太具体的）的类型 。 泛型类型参数支持协变和逆变，可在分配和使用泛型类型方面提供更大的灵活性。

在引用类型系统时，协变、逆变和不变性具有如下定义。 这些示例假定一个名为 `Base` 的基类和一个名为 `Derived`的派生类。  
  
- `Covariance`  
  
     使你能够使用比原始指定的类型派生程度更大的类型。  
  
     你可以将 `IEnumerable<Derived>` 的实例分配给 `IEnumerable<Base>` 类型的变量。  
  
- `Contravariance`  
  
     使你能够使用比原始指定的类型更泛型（派生程度更小）的类型。  
  
     你可以将 `Action<Base>` 的实例分配给 `Action<Derived>` 类型的变量。  
  
- `Invariance`  
  
     表示只能使用最初指定的类型。 固定泛型类型参数既不是协变，也不是逆变。  
  
     你无法将 `List<Base>` 的实例分配给 `List<Derived>` 类型的变量，反之亦然。  
  
 利用协变类型参数，你可以执行非常类似于普通的[多态性](../../csharp/programming-guide/classes-and-structs/polymorphism.md)的分配，如以下代码中所示。  
  
 [!code-csharp[CoContraSimpleIEnum#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontrasimpleienum/cs/example.cs#1)]
 [!code-vb[CoContraSimpleIEnum#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontrasimpleienum/vb/example.vb#1)]  
  
 <xref:System.Collections.Generic.List%601> 类实现 <xref:System.Collections.Generic.IEnumerable%601> 接口，因此 `List<Derived>` （在 Visual Basic 中为`List(Of Derived)` ）实现 `IEnumerable<Derived>`。 协变类型参数将执行其余的工作。  
  
 相反，逆变看起来却不够直观。 下面的示例创建类型 `Action<Base>` （在 Visual Basic 中为`Action(Of Base)` ）的委托，然后将此委托分配给类型 `Action<Derived>`的变量。  
  
 [!code-csharp[CoContraSimpleAction#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontrasimpleaction/cs/example.cs#1)]
 [!code-vb[CoContraSimpleAction#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontrasimpleaction/vb/example.vb#1)]  
  
 此示例看起来是倒退了，但它是可编译和运行的类型安全代码。 由于 Lambda 表达式与其自身所分配到的委托相匹配，因此它会定义一个方法，此方法采用一个 `Base` 类型的参数且没有返回值。 可以将结果委托分配给类型类型 `Action<Derived>` 的变量，因为 `T` 委托的类型参数 <xref:System.Action%601> 是逆变类型参数。 由于 `T` 指定了一个参数类型，因此该代码是类型安全代码。 在调用类型 `Action<Base>` 的委托（就像它是类型 `Action<Derived>`的委托一样）时，其参数必须属于类型 `Derived`。 始终可以将此实参安全地传递给基础方法，因为该方法的形参属于类型 `Base`。  
  
 通常，协变类型参数可用作委托的返回类型，而逆变类型参数可用作参数类型。 对于接口，协变类型参数可用作接口的方法的返回类型，而逆变类型参数可用作接口的方法的参数类型。  
  
 协变和逆变统称为“变体”。 未标记为协变或逆变的泛型类型参数称为“固定参数” 。 有关公共语言运行时中变体的事项的简短摘要：  
  
- Variant 类型参数仅限于泛型接口和泛型委托类型。  
  
- 泛型接口或泛型委托类型可以同时具有协变和逆变类型参数。  
  
- 变体仅适用于引用类型；如果为 Variant 类型参数指定值类型，则该类型参数对于生成的构造类型是不变的。  
  
- 变体不适用于委托组合。 也就是说，在给定类型 `Action<Derived>` 和 `Action<Base>` （在 Visual Basic 中为`Action(Of Derived)` 和 `Action(Of Base)` ）的两个委托的情况下，无法将第二个委托与第一个委托结合起来，尽管结果将是类型安全的。 变体允许将第二个委托分配给类型 `Action<Derived>`的变量，但只能在这两个委托的类型完全匹配的情况下对它们进行组合。

- 从 C# 9 开始，支持协变返回类型。 重写方法可以声明比它重写的方法派生程度更高的返回类型，而重写的只读属性可以声明派生程度更高的类型。

<a name="InterfaceCovariantTypeParameters"></a>

## <a name="generic-interfaces-with-covariant-type-parameters"></a>具有协变类型参数的泛型接口

某些泛型接口具有协变类型参数；例如：<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.Generic.IEnumerator%601>、<xref:System.Linq.IQueryable%601> 和 <xref:System.Linq.IGrouping%602>。 由于这些接口的所有类型参数都是协变类型参数，因此这些类型参数只用于成员的返回类型。  
  
 下面的示例阐释了协变类型参数。 此示例定义了两个类型： `Base` 具有一个名为 `PrintBases` 的静态方法，该方法采用 `IEnumerable<Base>` （在 Visual Basic 中为`IEnumerable(Of Base)` ）并输出元素。 `Derived` 继承自 `Base`。 此示例创建一个空 `List<Derived>` （在 Visual Basic 中为`List(Of Derived)` ），并且说明可以将该类型传递给 `PrintBases` 且在不进行强制转换的情况下将该类型分配给类型 `IEnumerable<Base>` 的变量。 <xref:System.Collections.Generic.List%601> 实现 <xref:System.Collections.Generic.IEnumerable%601>，它具有一个协变类型参数。 协变类型参数是可使用 `IEnumerable<Derived>` 的实例而非 `IEnumerable<Base>`的原因。  
  
 [!code-csharp[CoContravarianceInClrGenericI#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravarianceinclrgenerici/cs/example.cs#1)]
 [!code-vb[CoContravarianceInClrGenericI#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravarianceinclrgenerici/vb/example.vb#1)]  
  
## <a name="generic-interfaces-with-contravariant-type-parameters"></a>具有逆变类型参数的泛型接口

某些泛型接口具有逆变类型参数；例如：<xref:System.Collections.Generic.IComparer%601>、<xref:System.IComparable%601> 和 <xref:System.Collections.Generic.IEqualityComparer%601>。 由于这些接口只具有逆变类型参数，因此这些类型参数只用作接口成员中的参数类型。  
  
 下面的示例阐释了逆变类型参数。 该示例定义具有 `Area` 属性的抽象（在 Visual Basic 中为 `MustInherit`）`Shape` 类。 该示例还定义一个实现 `IComparer<Shape>`（在 Visual Basic 中为 `IComparer(Of Shape)`）的 `ShapeAreaComparer` 类。 <xref:System.Collections.Generic.IComparer%601.Compare%2A?displayProperty=nameWithType> 方法的实现基于 `Area` 属性的值，所以 `ShapeAreaComparer` 可用于按区域对 `Shape` 对象排序。  
  
 `Circle` 类继承 `Shape` 并替代 `Area`。 该示例使用一个采用 `IComparer<Circle>`（在 Visual Basic 中为 `IComparer(Of Circle)`）的构造函数创建 `Circle` 对象的 <xref:System.Collections.Generic.SortedSet%601>。 但是，该示例不传递 `IComparer<Circle>`，而是传递一个用于实现 `IComparer<Shape>` 的 `ShapeAreaComparer` 对象。 当代码调用派生程度较高的类型 (`Circle`) 的比较器时，该示例可以传递派生程度较低的类型 (`Shape`) 的比较器，因为 <xref:System.Collections.Generic.IComparer%601> 泛型接口的类型参数是逆变参数。  
  
 向 `Circle` 中添加新 `SortedSet<Circle>`对象时，每次将新元素与现有元素进行比较时，都会调用 `IComparer<Shape>.Compare` 对象的`IComparer(Of Shape).Compare` 方法（在 Visual Basic 中为 `ShapeAreaComparer` 方法）。 该方法的参数类型 (`Shape`) 比所传递的类型 (`Circle`) 的派生程度低，所以调用是类型安全的。 逆变使 `ShapeAreaComparer` 可以对派生自 `Shape` 的任意单个类型的集合以及混合类型的集合排序。  
  
 [!code-csharp[CoContravarianceInClrGenericI2#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravarianceinclrgenerici2/cs/example.cs#1)]
 [!code-vb[CoContravarianceInClrGenericI2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravarianceinclrgenerici2/vb/example.vb#1)]  

## <a name="generic-delegates-with-variant-type-parameters"></a>具有 Variant 类型参数的泛型委托

`Func` 泛型委托（如 <xref:System.Func%602>）具有协变返回类型和逆变参数类型。 `Action` 泛型委托（如 <xref:System.Action%602>）具有逆变参数类型。 这意味着，可以将委托指派给具有派生程度较高的参数类型和（对于 `Func` 泛型委托）派生程度较低的返回类型的变量。  
  
> [!NOTE]
> `Func` 泛型委托的最后一个泛型类型参数指定委托签名中返回值的类型。 该参数是协变的（`out` 关键字），而其他泛型类型参数是逆变的（`in` 关键字）。  
  
 下面的代码阐释这一点。 第一段代码定义了一个名为 `Base` 的类、一个名为 `Derived` 的类（此类继承 `Base`）和另一个具有名为 `MyMethod` 的 `static` 方法（在 Visual Basic 中为 `Shared`）的类。 该方法接受 `Base` 的实例，并返回 `Derived` 的实例。 （如果参数是 `Derived` 的实例，则 `MyMethod` 将返回该实例；如果参数是 `Base` 的实例，则 `MyMethod` 将返回 `Derived` 的新实例。）在 `Main()` 中，该示例创建一个表示 `Func<Base, Derived>` 的 `Func(Of Base, Derived)`（在 Visual Basic 中为 `MyMethod`）的实例，并将此实例存储在变量 `f1` 中。  
  
 [!code-csharp[CoContravarianceDelegates#2](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#2)]
 [!code-vb[CoContravarianceDelegates#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#2)]  
  
 第二段代码说明可以将委托分配给类型 `Func<Base, Base>` （在 Visual Basic 中为`Func(Of Base, Base)` ）的变量，因为返回类型是协变的。  
  
 [!code-csharp[CoContravarianceDelegates#3](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#3)]
 [!code-vb[CoContravarianceDelegates#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#3)]  
  
 第三段代码说明可以将委托分配给类型 `Func<Derived, Derived>` （在 Visual Basic 中为`Func(Of Derived, Derived)` ）的变量，因为参数类型是逆变的。  
  
 [!code-csharp[CoContravarianceDelegates#4](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#4)]
 [!code-vb[CoContravarianceDelegates#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#4)]  
  
 最后一段代码说明可以将委托分配给类型 `Func<Derived, Base>` （在 Visual Basic 中为`Func(Of Derived, Base)` ）的变量，从而将逆变参数类型和协变返回类型的作用结合起来。  
  
 [!code-csharp[CoContravarianceDelegates#5](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegates/cs/example.cs#5)]
 [!code-vb[CoContravarianceDelegates#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegates/vb/example.vb#5)]  
  
### <a name="variance-in-non-generic-delegates"></a>非泛型委托中的变体

 在上面的代码中， `MyMethod` 的签名与所构造的泛型委托 `Func<Base, Derived>` （在 Visual Basic 中为`Func(Of Base, Derived)` ）的签名完全匹配。 此示例说明，只要所有委托类型都是从泛型委托类型 <xref:System.Func%602>构造的，就可以将此泛型委托存储在具有派生程度更大的参数类型和派生程度更小的返回类型的变量或方法参数中。  
  
 这一点非常重要。 泛型委托的类型参数中的协方差和逆变的效果类似于普通委托绑定中的协方差和逆变的效果（请参阅[委托中的差异 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md) 和[委托中的差异 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)）。 但是，委托绑定中的变化适用于所有委托类型，而不仅仅适用于具有 Variant 类型参数的泛型委托类型。 此外，通过委托绑定中的变化，可以将方法绑定到具有限制较多的参数类型和限制较少的返回类型的任何委托，而对于泛型委托的指派，只有在委托类型是基于同一个泛型类型定义构造的时才可以进行。  
  
 下面的示例演示委托绑定中的变化和泛型类型参数中的变化的组合效果。 该示例定义了一个类型层次结构，其中包含三个按派生程度从低到高排列的类型，即`Type1`的派生程度最低，`Type3`的派生程度最高。 普通委托绑定中的变化用于将参数类型为 `Type1` 、返回类型为 `Type3` 的方法绑定到参数类型为 `Type2` 、返回类型为 `Type2`的泛型委托。 然后，使用泛型类型参数的协变和逆变，将得到的泛型委托指派给另一个变量，此变量的泛型委托类型的参数类型为 `Type3` ，返回类型为 `Type1`。 第二个指派要求变量类型和委托类型是基于同一个泛型类型定义（在本例中为 <xref:System.Func%602>）构造的。  
  
 [!code-csharp[CoContravarianceDelegatesGenRelaxed#1](../../../samples/snippets/csharp/VS_Snippets_CLR/cocontravariancedelegatesgenrelaxed/cs/example.cs#1)]
 [!code-vb[CoContravarianceDelegatesGenRelaxed#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/cocontravariancedelegatesgenrelaxed/vb/example.vb#1)]  

## <a name="define-variant-generic-interfaces-and-delegates"></a>定义变体泛型接口和委托

Visual Basic 和 C# 提供了一些关键字，利用这些关键字，可以将接口和委托的泛型类型参数标记为协变或逆变。
  
 协变类型参数用 `out` 关键字（在 Visual Basic 中为`Out` 关键字）标记。 可以将协变类型参数用作属于接口的方法的返回值，或用作委托的返回类型。 但不能将协变类型参数用作接口方法的泛型类型约束。  
  
> [!NOTE]
> 如果接口的方法具有泛型委托类型的参数，则接口类型的协变类型参数可用于指定委托类型的逆变类型参数。  
  
 逆变类型参数用 `in` 关键字（在 Visual Basic 中为`In` 关键字）标记。 可以将逆变类型参数用作属于接口的方法的参数类型，或用作委托的参数类型。 也可以将逆变类型参数用作接口方法的泛型类型约束。  
  
 只有接口类型和委托类型才能具有 Variant 类型参数。 接口或委托类型可以同时具有协变和逆变类型参数。  
  
 Visual Basic 和 C# 不允许违反协变和逆变类型参数的使用规则，也不允许将协变和逆变批注添加到接口和委托类型之外的类型参数中。
  
 有关信息和示例代码，请参阅[泛型接口中的差异 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md) 和[泛型接口中的差异 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)。  

## <a name="list-of-types"></a>类型列表

下面的接口和委托类型具有协变和/或逆变类型参数。  
  
|类型|协变类型参数|逆变类型参数|  
|----------|-------------------------------|-----------------------------------|  
|<xref:System.Action%601> 至 <xref:System.Action%6016>||是|  
|<xref:System.Comparison%601>||是|  
|<xref:System.Converter%602>|是|是|  
|<xref:System.Func%601>|是||  
|<xref:System.Func%602> 至 <xref:System.Func%6017>|是|是|  
|<xref:System.IComparable%601>||是|  
|<xref:System.Predicate%601>||是|  
|<xref:System.Collections.Generic.IComparer%601>||是|  
|<xref:System.Collections.Generic.IEnumerable%601>|是||  
|<xref:System.Collections.Generic.IEnumerator%601>|是||  
|<xref:System.Collections.Generic.IEqualityComparer%601>||是|  
|<xref:System.Linq.IGrouping%602>|是||  
|<xref:System.Linq.IOrderedEnumerable%601>|是||  
|<xref:System.Linq.IOrderedQueryable%601>|是||  
|<xref:System.Linq.IQueryable%601>|是||  
  
## <a name="see-also"></a>请参阅

- [协变和逆变 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/index.md)
- [协变和逆变 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/index.md)
- [委托中的变体 (C#)](../../csharp/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)
- [委托中的变体 (Visual Basic)](../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)
