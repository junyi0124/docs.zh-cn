---
title: Null 值
description: '了解如何在 F # 编程语言中使用 null 值。'
ms.date: 08/15/2020
ms.openlocfilehash: 3b2c7ebe7b8eff08f7c3e76b9715ccab1bbd5d36
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558343"
---
# <a name="null-values"></a>Null 值

本主题介绍如何在 F # 中使用 null 值。

## <a name="null-value"></a>空值

Null 值通常不在 F # 中用于值或变量。 但是，在某些情况下，null 显示为异常值。 如果在 F # 中定义了类型，则不允许将 null 作为常规值，除非将 [AllowNullLiteral](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-allownullliteralattribute.html#Value) 属性应用于该类型。 如果某个类型是在其他某个 .NET 语言中定义的，则 null 是可能的值，当您与此类类型进行互操作时，F # 代码可能会遇到 null 值。

对于 F # 中定义并严格从 F # 中使用的类型，使用 F # 库直接创建 null 值的唯一方法是使用 [unchecked.defaultof](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators-unchecked.html#defaultof) 或 [array.zerocreate](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-arraymodule.html#zeroCreate)。 但是，对于其他 .NET 语言中使用的 F # 类型，或者如果使用的是不是用 F # 编写的 API （如 .NET Framework），则可能会出现 null 值。

`option`如果可以在另一种 .net 语言中使用具有可能为 null 值的引用变量，则可以在 F # 中使用类型。 `option`如果没有对象，则可以使用选项值，而不是使用 F # 类型的 null `None` 。 `Some(obj)`当有对象时，可将选项值用于对象 `obj` 。 有关详细信息，请参阅 [选项](../options.md)。 请注意， `null` 如果的为，则仍可将值打包到选项中 `Some x` `x` `null` 。 因此，在值为时使用非常重要 `None` `null` 。

`null`关键字是 F # 语言中的有效关键字，当你使用以其他 .net 语言编写的 .NET Framework api 或其他 api 时，你必须使用它。 在以下两种情况下，您可能需要 null 值：调用 .NET API 并将 null 值作为参数传递，以及从 .NET 方法调用解释返回值或输出参数。

若要将 null 值传递到 .NET 方法，只需 `null` 在调用代码中使用关键字。 下面的代码示例阐释了这一点。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet701.fs)]

若要解释从 .NET 方法获取的 null 值，请在可以时使用模式匹配。 下面的代码示例演示如何使用模式匹配来解释在 `ReadLine` 其尝试读取超出输入流末尾时返回的 null 值。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet702.fs)]

F # 类型的 Null 值还可以通过其他方式生成，例如，在使用时，将 `Array.zeroCreate` 调用 `Unchecked.defaultof` 。 必须谨慎处理此类代码，使封装的值保持为空值。 在仅用于 F # 的库中，无需在每个函数中检查 null 值。 如果要编写与其他 .NET 语言的互操作性的库，则可能必须添加对 null 输入参数的检查并引发 `ArgumentNullException` ，就像在 c # 或 Visual Basic 代码中执行的操作一样。

可以使用以下代码检查任意值是否为 null。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet703.fs)]

## <a name="see-also"></a>请参阅

- [值](index.md)
- [Match 表达式](../match-expressions.md)
