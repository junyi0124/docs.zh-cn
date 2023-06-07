---
title: 类型参数约束 - C# 编程指南
description: 了解类型参数约束。 约束告知编译器类型参数必须具备的功能。
ms.date: 04/12/2018
helpviewer_keywords:
- generics [C#], type constraints
- type constraints [C#]
- type parameters [C#], constraints
- unbound type parameter [C#]
ms.openlocfilehash: 8230dfed11bb4ba21e922827cc1a525ce45ba3e5
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599110"
---
# <a name="constraints-on-type-parameters-c-programming-guide"></a>类型参数的约束（C# 编程指南）

约束告知编译器类型参数必须具备的功能。 在没有任何约束的情况下，类型参数可以是任何类型。 编译器只能假定 <xref:System.Object?displayProperty=nameWithType> 的成员，它是任何 .NET 类型的最终基类。 有关详细信息，请参阅[使用约束的原因](#why-use-constraints)。 如果客户端代码使用不满足约束的类型，编译器将发出错误。 通过使用 `where` 上下文关键字指定约束。 下表列出了各种类型的约束：

|约束|描述|
|----------------|-----------------|
|`where T : struct`|类型参数必须是不可为 null 的[值类型](../../language-reference/builtin-types/value-types.md)。 有关可为 null 的值类型的信息，请参阅[可为 null 的值类型](../../language-reference/builtin-types/nullable-value-types.md)。 由于所有值类型都具有可访问的无参数构造函数，因此 `struct` 约束表示 `new()` 约束，并且不能与 `new()` 约束结合使用。 `struct` 约束也不能与 `unmanaged` 约束结合使用。|
|`where T : class`|类型参数必须是引用类型。 此约束还应用于任何类、接口、委托或数组类型。 在 C#8.0 或更高版本中的可为 null 上下文中，`T` 必须是不可为 null 的引用类型。 |
|`where T : class?`|类型参数必须是可为 null 或不可为 null 的引用类型。 此约束还应用于任何类、接口、委托或数组类型。|
|`where T : notnull`|类型参数必须是不可为 null 的类型。 参数可以是 C# 8.0 或更高版本中的不可为 null 的引用类型，也可以是不可为 null 的值类型。 |
|`where T : unmanaged`|类型参数必须是不可为 null 的[非托管类型](../../language-reference/builtin-types/unmanaged-types.md)。 `unmanaged` 约束表示 `struct` 约束，且不能与 `struct` 约束或 `new()` 约束结合使用。|
|`where T : new()`|类型参数必须具有公共无参数构造函数。 与其他约束一起使用时，`new()` 约束必须最后指定。 `new()` 约束不能与 `struct` 和 `unmanaged` 约束结合使用。|
|`where T :` *\<base class name>*|类型参数必须是指定的基类或派生自指定的基类。 在 C# 8.0 及更高版本中的可为 null 上下文中，`T` 必须是从指定基类派生的不可为 null 的引用类型。 |
|`where T :` *\<base class name>?*|类型参数必须是指定的基类或派生自指定的基类。 在 C# 8.0 及更高版本中的可为 null 上下文中，`T` 可以是从指定基类派生的可为 null 或不可为 null 的类型。 |
|`where T :` *\<interface name>*|类型参数必须是指定的接口或实现指定的接口。 可指定多个接口约束。 约束接口也可以是泛型。 在 C# 8.0 及更高版本中的可为 null 上下文中，`T` 必须是实现指定接口的不可为 null 的类型。|
|`where T :` *\<interface name>?*|类型参数必须是指定的接口或实现指定的接口。 可指定多个接口约束。 约束接口也可以是泛型。 在 C# 8.0 中的可为 null 上下文中，`T` 可以是可为 null 的引用类型、不可为 null 的引用类型或值类型。 `T` 不能是可为 null 的值类型。|
|`where T : U`|为 `T` 提供的类型参数必须是为 `U` 提供的参数或派生自为 `U` 提供的参数。 在可为 null 的上下文中，如果 `U` 是不可为 null 的引用类型，`T` 必须是不可为 null 的引用类型。 如果 `U` 是可为 null 的引用类型，则 `T` 可以是可为 null 的引用类型，也可以是不可为 null 的引用类型。 |

## <a name="why-use-constraints"></a>使用约束的原因

约束指定类型参数的功能和预期。 声明这些约束意味着你可以使用约束类型的操作和方法调用。 如果泛型类或方法对泛型成员使用除简单赋值之外的任何操作或调用 <xref:System.Object?displayProperty=nameWithType> 不支持的任何方法，则必须对类型参数应用约束。 例如，基类约束告诉编译器，仅此类型的对象或派生自此类型的对象可用作类型参数。 编译器有了此保证后，就能够允许在泛型类中调用该类型的方法。 以下代码示例演示可通过应用基类约束添加到（[泛型介绍](../../../standard/generics/index.md)中的）`GenericList<T>` 类的功能。

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#9)]

约束使泛型类能够使用 `Employee.Name` 属性。 约束指定类型 `T` 的所有项都保证是 `Employee` 对象或从 `Employee` 继承的对象。

可以对同一类型参数应用多个约束，并且约束自身可以是泛型类型，如下所示：

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#10)]

在应用 `where T : class` 约束时，请避免对类型参数使用 `==` 和 `!=` 运算符，因为这些运算符仅测试引用标识而不测试值相等性。 即使在用作参数的类型中重载这些运算符也会发生此行为。 下面的代码说明了这一点；即使 <xref:System.String> 类重载 `==` 运算符，输出也为 false。

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#11)]

编译器只知道 `T` 在编译时是引用类型，并且必须使用对所有引用类型都有效的默认运算符。 如果必须测试值相等性，建议同时应用 `where T : IEquatable<T>` 或 `where T : IComparable<T>` 约束，并在用于构造泛型类的任何类中实现该接口。

## <a name="constraining-multiple-parameters"></a>约束多个参数

可以对多个参数应用多个约束，对一个参数应用多个约束，如下例所示：

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#12)]

## <a name="unbounded-type-parameters"></a>未绑定的类型参数

 没有约束的类型参数（如公共类 `SampleClass<T>{}` 中的 T）称为未绑定的类型参数。 未绑定的类型参数具有以下规则：

- 不能使用 `!=` 和 `==` 运算符，因为无法保证具体的类型参数能支持这些运算符。
- 可以在它们与 `System.Object` 之间来回转换，或将它们显式转换为任何接口类型。
- 可以将它们与 [null](../../language-reference/keywords/null.md) 进行比较。 将未绑定的参数与 `null` 进行比较时，如果类型参数为值类型，则该比较将始终返回 false。

## <a name="type-parameters-as-constraints"></a>类型参数作为约束

在具有自己类型参数的成员函数必须将该参数约束为包含类型的类型参数时，将泛型类型参数用作约束非常有用，如下例所示：

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#13)]

在上述示例中，`T` 在 `Add` 方法的上下文中是一个类型约束，而在 `List` 类的上下文中是一个未绑定的类型参数。

类型参数还可在泛型类定义中用作约束。 必须在尖括号中声明该类型参数以及任何其他类型参数：

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#14)]

类型参数作为泛型类的约束的作用非常有限，因为编译器除了假设类型参数派生自 `System.Object` 以外，不会做其他任何假设。 如果要在两个类型参数之间强制继承关系，可以将类型参数用作泛型类的约束。

## <a name="notnull-constraint"></a>NotNull 约束

从 C# 8.0 开始，在可为 null 上下文中，可以使用 `notnull` 约束指定类型参数必须是不可为 null 的值类型或不可为 null 的引用类型。 `notnull` 约束只能在 `nullable enable` 上下文中使用。 如果在可以为 null 的不明显上下文中添加 `notnull` 约束，则编译器将生成警告。

与其他约束不同，如果类型参数违反 `notnull` 约束，那么在 `nullable enable` 上下文中编译该代码时，编译器会生成警告。 如果在可以为 null 的不明显上下文中编译代码，则编译器不会生成任何警告或错误。

从 C# 8.0 开始，在可为 null 上下文中，`class` 约束指定类型参数必须是不可为 null 的引用类型。 在可为 null 上下文中，当类型参数是可为 null 的引用类型时，编译器会生成警告。

## <a name="unmanaged-constraint"></a>非托管约束

从 C# 7.3 开始，可使用 `unmanaged` 约束来指定类型参数必须是不可为 null 的[非托管类型](../../language-reference/builtin-types/unmanaged-types.md)。 通过 `unmanaged` 约束，用户能编写可重用例程，从而使用可作为内存块操作的类型，如以下示例所示：

[!code-csharp[using the unmanaged constraint](snippets/GenericWhereConstraints.cs#15)]

以上方法必须在 `unsafe` 上下文中编译，因为它并不是在已知的内置类型上使用 `sizeof` 运算符。 如果没有 `unmanaged` 约束，则 `sizeof` 运算符不可用。

`unmanaged` 约束表示 `struct` 约束，且不能与其结合使用。 因为 `struct` 约束表示 `new()` 约束，且 `unmanaged` 约束也不能与 `new()` 约束结合使用。

## <a name="delegate-constraints"></a>委托约束

同样从 C# 7.3 开始，可将 <xref:System.Delegate?displayProperty=nameWithType> 或 <xref:System.MulticastDelegate?displayProperty=nameWithType> 用作基类约束。 CLR 始终允许此约束，但 C# 语言不允许。 使用 `System.Delegate` 约束，用户能够以类型安全的方式编写使用委托的代码。 以下代码定义了合并两个同类型委托的扩展方法：

[!code-csharp[using the delegate constraint](snippets/GenericWhereConstraints.cs#16)]

可使用上述方法来合并相同类型的委托：

[!code-csharp[using the unmanaged constraint](snippets/GenericWhereConstraints.cs#17)]

如果取消评论最后一行，它将不会编译。 `first` 和 `test` 均为委托类型，但它们是不同的委托类型。

## <a name="enum-constraints"></a>枚举约束

从 C# 7.3 开始，还可指定 <xref:System.Enum?displayProperty=nameWithType> 类型作为基类约束。 CLR 始终允许此约束，但 C# 语言不允许。 使用 `System.Enum` 的泛型提供类型安全的编程，缓存使用 `System.Enum` 中静态方法的结果。 以下示例查找枚举类型的所有有效的值，然后生成将这些值映射到其字符串表示形式的字典。

[!code-csharp[using the enum constraint](snippets/GenericWhereConstraints.cs#18)]

`Enum.GetValues` 和 `Enum.GetName` 使用反射，这会对性能产生影响。 可调用 `EnumNamedValues` 来生成可缓存和重用的集合，而不是重复执行需要反射才能实施的调用。

如以下示例所示，可使用它来创建枚举并生成其值和名称的字典：

[!code-csharp[enum definition](snippets/GenericWhereConstraints.cs#19)]

[!code-csharp[using the enum constrained method](snippets/GenericWhereConstraints.cs#20)]

## <a name="see-also"></a>请参阅

- <xref:System.Collections.Generic>
- [C# 编程指南](../index.md)
- [泛型介绍](./index.md)
- [泛型类](./generic-classes.md)
- [new 约束](../../language-reference/keywords/new-constraint.md)
