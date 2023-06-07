---
title: 可为 null 的引用类型 - C# 引用
description: 了解 C# 中可为 null 的引用类型及其使用方法
ms.date: 04/06/2020
ms.openlocfilehash: d961af9ba3b4776e6b4ec3eeea5392fb0d0394ce
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822420"
---
# <a name="nullable-reference-types-c-reference"></a>可为 null 的引用类型（C# 引用）

> [!NOTE]
> 本文介绍可为 null 的引用类型。 还可以声明[可为 null 的值类型](nullable-value-types.md)。

由于在可为 null 的感知上下文选择加入了代码，从 C#8.0 开始，可以使用可为 null 的引用类型  。 可为 null 的引用类型、null 静态分析警告和 [null 包容运算符](../operators/null-forgiving.md)是可选的语言功能。 默认情况下，所有功能都将关闭。 可为 null 的上下文在项目级使用生成设置进行控制，或在代码中使用 pragmas 进行控制  。

 在可为 null 的感知上下文中：

- 引用类型 `T` 的变量必须用非 null 值进行初始化，并且不能为其分配可能为 `null` 的值。
- 引用类型 `T?` 的变量可以用 `null` 进行初始化，也可以分配 `null`，但在取消引用之前必须对照 `null` 进行检查。
- 类型为 `T?` 的变量 `m` 在应用 null 包容运算符时被认为是非空的，如 `m!` 中所示。

不可为 null 的引用类型 `T` 和可为 null 的引用类型 `T?` 之间的区别按照编译器对上述规则的解释强制执行的。 类型为 `T` 的变量和类型为 `T?` 的变量由相同的 .NET 类型表示。 下面的示例声明了一个不可为 null 的字符串和一个可为 null 的字符串，然后使用 null 包容运算符将一个值分配给不可为 null 的字符串：

:::code language="csharp" source="snippets/shared/NullableReferenceTypes.cs" id="SnippetCoreSyntax":::

变量 `notNull` 和 `nullable` 都由 <xref:System.String> 类型表示。 因为不可为 null 的类型和可为 null 的类型都存储为相同的类型，所以有几个位置不允许使用可为 null 的引用类型。 通常，可为 null 的引用类型不能用作基类或实现的接口。 可为 null 的引用类型不能用于任何对象创建或类型测试表达式。 可为 null 的引用类型不能是成员访问表达式的类型。 下面的示例说明了这些构造：

```csharp
public MyClass : System.Object? // not allowed
{
}

var nullEmpty = System.String?.Empty; // Not allowed
var maybeObject = new object?(); // Not allowed
try
{
    if (thing is string? nullableString) // not allowed
        Console.WriteLine(nullableString);
} catch (Exception? e) // Not Allowed
{
    Console.WriteLine("error");
}
```

## <a name="nullable-references-and-static-analysis"></a>可为 null 的引用和静态分析

上一部分中的示例说明了可为 null 的引用类型的性质。 可为 null 的引用类型不是新的类类型，而是对现有引用类型的注释。 编译器使用这些注释来帮助你查找代码中潜在的 null 引用错误。 不可为 null 的引用类型和可为 null 的引用类型在运行时没有区别。 编译器不会为不可为 null 的引用类型添加任何运行时检查。 这有利于编译时分析。 编译器将生成警告，帮助你查找和修复代码中潜在的 null 错误。 你需声明意向，如果代码违反该意向，编译器会发出警告。

在可为 null 的上下文中，编译器对任何引用类型的变量（可为 null 的和不可为 null 的）执行静态分析。 编译器跟踪每个引用变量的 null 状态，如“非 null”和“可能为 null”   。 不可为 null 的引用的默认状态为“非 null”  。 可为 null 的引用的默认状态为“可能为 null”  。

不可为 null 的引用类型在取消引用时应该始终是安全的，因为它们的 null 状态是“非 null”  。 若强制执行该规则，如果不可为 null 的引用类型没有初始化为非 null 值，编译器将发出警告。 必须在声明的位置分配局部变量。 每个构造函数都必须分配每个字段，无论是在其主体、被调用的构造函数还是使用字段初始值设定项。 如果将不可为 null 的引用分配给状态为“可能为 null”的引用，编译器将发出警告  。 但是，由于不可为 null 的引用是“非 null”，因此在取消引用这些变量时不会发出警告  。

可为 null 的引用类型可以进行初始化或分配给 `null`。 因此，静态分析必须在取消对变量的引用之前确定该变量为“非 null”  。 如果可为 null 的引用被确定为“可能为 null”，则不能将其分配给不可为 null 的引用变量  。 以下类显示了这些警告的示例：

:::code language="csharp" source="snippets/shared/NullableReferenceTypes.cs" id="SnippetClassWithNullable":::

以下代码段显示了编译器在使用此类时发出警告的位置：

:::code language="csharp" source="snippets/shared/NullableReferenceTypes.cs" id="SnippetLocalWarnings":::

前面的示例演示编译器的静态分析，以确定引用变量的 null 状态。 编译器对 null 检查和分配应用语言规则以通知其分析。  编译器无法对方法或属性的语义进行假设。 如果调用执行 null 检查的方法，则编译器无法得知这些方法会影响变量的 null 状态。 可以将许多属性添加到 API，以通知编译器有关参数和返回值的语义。 这些属性已应用于 .NET Core 库中的许多常见 API。 例如，<xref:System.String.IsNullOrEmpty%2A> 已经更新，编译器正确地将该方法解释为 null 检查。 有关应用于 null 状态静态分析的属性的更多信息，请参阅[可为 null 属性](../attributes/nullable-analysis.md)的文章。

## <a name="setting-the-nullable-context"></a>设置可为 null 的上下文

可以通过两种方式控制可为 null 的上下文。 在项目级别，可以添加 `<Nullable>enable</Nullable>` 项目设置。 在单个 C# 源文件中，可以添加 `#nullable enable` 来启用可为 null 的上下文。 请参阅关于[设置可为 null 策略](../../nullable-migration-strategies.md)的文章。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下建议：

- [可为空引用类型](~/_csharplang/proposals/csharp-8.0/nullable-reference-types.md)
- [可为空引用类型规范草案](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [可以为 null 的值类型](nullable-value-types.md)
