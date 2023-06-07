---
title: 泛型
description: 了解如何使用F#泛型函数和类型, 这使你可以编写适用于各种类型的代码, 而无需重复代码。
ms.date: 05/16/2016
ms.openlocfilehash: 5d6e57762095e44836425f90d21a6c1dc71edaaa
ms.sourcegitcommit: cdf67135a98a5a51913dacddb58e004a3c867802
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/21/2019
ms.locfileid: "69666694"
---
# <a name="generics"></a>泛型

F# 函式值、方法、属性和聚合类型（如类、记录和可区分联合）都可以是“泛型”。 泛型构造至少包含一个类型参数，该类型参数通常由泛型构造的用户提供。 通过泛型函数和类型，可编写可用于各种类型的代码，而无需针对每个类型重复编写代码。 利用 F# 编写泛型代码很简单，这是因为编译器的类型推理和自动泛化机制通常会将代码隐式地推断为泛型代码。

## <a name="syntax"></a>语法

```fsharp
// Explicitly generic function.
let function-name<type-parameters> parameter-list =
function-body

// Explicitly generic method.
[ static ] member object-identifer.method-name<type-parameters> parameter-list [ return-type ] =
method-body

// Explicitly generic class, record, interface, structure,
// or discriminated union.
type type-name<type-parameters> type-definition
```

## <a name="remarks"></a>备注

显式泛型函数或类型的声明与非泛型函数或类型的声明非常相似，但类型参数的规范（和用法）不同，它们位于函数或类型名称后面的尖括号中。

声明通常为隐式泛型。 如果未完全指定用于组合函数或类型的每个参数的类型，编译器将尝试从所编写的代码中推断每个参数、值和变量的类型。 有关更多信息，请参见[类型推理](../type-inference.md)。 如果类型或函数的代码没有约束参数的类型，则该函数或类型为隐式泛型。 此过程被称为自动泛化。 自动泛化有一些限制。 例如，如果 F# 编译器无法推断泛型构造的类型，编译器会报告错误，指出存在一个称作“值限制”的限制。 在此情况下，可能必须添加一些类型批注。 有关自动泛化和值限制以及如何更改代码来解决问题的更多信息，请参见[自动泛化](automatic-generalization.md)。

在之前的语法中，*type-parameters* 是一个表示未知类型的参数的逗号分隔列表，其中每个参数都以单引号开头，并且可选择包含一个约束子句，用来进一步限制可用于该类型参数的类型。 有关各种约束子句的语法以及有关约束的其他信息，请参见[约束](constraints.md)。

语法中的 *type-definition* 与非泛型类型的类型定义相同。 它包括类类型的构造函数参数、可选的 `as` 子句、等号、记录字段、`inherit` 子句、可区分联合的选项、`let` 绑定和 `do` 绑定、成员定义，以及非泛型类型定义中允许的任何其他内容。

其他语法元素与非泛型函数和类型的语法元素相同。 例如，*object-identifier* 是一个表示包含对象本身的标识符。

属性、字段和构造函数不能比封闭类型更加泛型化。 此外，模块中的值不能为泛型。

## <a name="implicitly-generic-constructs"></a>隐式泛型构造

当 F# 编译器推断代码中的类型时，会自动将任何可以为泛型的函数视为泛型。 如果显式指定某个类型（例如参数类型），则将阻止自动泛化。

在以下代码示例中，`makeList` 为泛型，尽管未将它或它的参数显式声明为泛型。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1700.fs)]

该函数的签名被推断为 `'a -> 'a -> 'a list`。 请注意，本示例中的 `a` 和 `b` 被推断为具有相同的类型。 这是因为它们一起包括在一个列表中，并且列表中的所有元素必须是相同类型的元素。

还可以通过在类型批注中使用单引号语法来指示参数类型是泛型类型参数，从而使一个函数成为泛型函数。 在以下代码示例中，`function1` 为泛型，因为其参数以这种方式被声明为类型参数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1701.fs)]

## <a name="explicitly-generic-constructs"></a>显式泛型构造

还可以通过在尖括号 (`<type-parameter>`) 中显式声明函数的类型参数来使一个函数成为泛型函数。 下面的代码阐释这一点。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1703.fs)]

## <a name="using-generic-constructs"></a>使用泛型构造

使用泛型函数或方法时，不必指定类型参数。 编译器使用类型推理来推断适当的类型参数。 如果仍然存在多义性，则可以在尖括号中提供类型参数，并用逗号分隔多个类型参数。

以下代码显示之前部分中定义的函数的用法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1702.fs)]

> [!NOTE]
> 有两种方法可以通过名称指代泛型类型。 例如，`list<int>` 和 `int list` 这两种方法可用来指代具有单个类型参数 `list` 的泛型类型 `int`。 后一种形式通常只用于内置 F# 类型，例如 `list` 和 `option`。 如果存在多个类型参数，通常会使用语法 `Dictionary<int, string>`，但还可使用语法 `(int, string) Dictionary`。

## <a name="wildcards-as-type-arguments"></a>通配符作为类型参数

若要指定应由编译器推断类型参数，可以使用下划线或通配符 (`_`)，而不要使用已命名的类型参数。 以下代码对此进行了演示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1704.fs)]

## <a name="constraints-in-generic-types-and-functions"></a>泛型类型和函数中的约束

在泛型类型或函数定义中，只可使用那些已知可用于泛型类型参数的构造。 这对于在编译时启用对函数调用和方法调用的验证是必需的。 如果显式声明类型参数，则可对泛型类型参数应用显式约束，以通知编译器某些方法和函数是可用的。 但是，如果允许 F# 编译器推断泛型参数类型，它将为你确定适当的约束。 有关详细信息，请参见[约束](constraints.md)。

## <a name="statically-resolved-type-parameters"></a>静态解析的类型参数

有两种可在 F# 程序中使用的类型参数。 第一种类型参数是上一部分中描述的那种泛型类型参数。 这种类型参数等效于在 Visual Basic 和 C# 等语言中使用的泛型类型参数。 另一种类型参数为 F# 专用，被称为“静态解析的类型参数”。 有关这些构造的信息，请参阅[静态解析的类型参数](statically-resolved-type-parameters.md)。

## <a name="examples"></a>示例

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1705.fs)]

## <a name="see-also"></a>请参阅

- [语言参考](../index.md)
- [类型](../fsharp-types.md)
- [静态解析的类型参数](statically-resolved-type-parameters.md)
- [泛型](../../../standard/generics/index.md)
- [自动泛化](automatic-generalization.md)
- [约束](constraints.md)
