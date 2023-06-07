---
title: 第一类函数
description: '了解第一类函数以及它们对于 F # 中的函数编程是非常重要的。'
ms.date: 10/29/2018
ms.openlocfilehash: 1dc8eae1655187282f05bf4e9501ecc8a17deba8
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88810906"
---
# <a name="first-class-functions"></a>第一类函数

函数编程语言的一种定义特性是将函数提升为第一类状态。 您应该能够对使用其他内置类型的值的函数执行任何操作，并且能够以相当的工作量来实现此目的。

第一类状态的典型度量值包括：

- 是否可以将函数绑定到标识符？ 也就是说，可以为其指定名称吗？

- 能否在数据结构（如列表中）中存储函数？

- 是否可以在函数调用中将函数作为参数传递？

- 是否可以从函数调用返回函数？

最后两个度量值定义了所谓的 *高阶运算* 或 *高阶函数*。 高阶函数接受函数作为参数，并返回函数作为函数调用的值。 这些操作支持将函数编程作为映射函数和函数的组合。

## <a name="give-the-value-a-name"></a>为值指定名称

如果函数是第一类值，则您必须能够命名它，就像您命名整数、字符串和其他内置类型一样。 这在函数编程文献中称为将标识符绑定到某个值。 F # 使用[ `let` 绑定](../language-reference/functions/let-bindings.md)将名称绑定到值： `let <identifier> = <value>` 。 下面的代码演示了两个示例。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet20.fs)]

您可以轻松地命名函数。 下面的示例定义了一个名为的函数，该函数将 `squareIt` 标识符绑定 `squareIt` 到 [lambda 表达式](../language-reference/functions/lambda-expressions-the-fun-keyword.md) `fun n -> n * n` 。 函数 `squareIt` 具有一个参数， `n` 并返回该参数的平方。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet21.fs)]

F # 提供了以下更简洁的语法，可实现相同的结果，并且键入内容更少。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet22.fs)]

下面的示例使用第一种样式 `let <function-name> = <lambda-expression>` 来强调函数声明和其他类型值的声明之间的相似之处。 但是，还可以用简洁的语法来编写所有命名函数。 其中的一些示例是以这两种方式编写的。

## <a name="store-the-value-in-a-data-structure"></a>将值存储在数据结构中

第一类值可以存储在数据结构中。 下面的代码演示将值存储在列表和元组中的示例。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet23.fs)]

若要验证存储在元组中的函数名称实际上是否计算为函数，下面的示例使用 `fst` 和 `snd` 运算符从元组中提取第一个和第二个元素 `funAndArgTuple` 。 元组中的第一个元素是 `squareIt` ，第二个元素是 `num` 。 标识符 `num` 在前面的示例中绑定到整数10，这是函数的有效参数 `squareIt` 。 第二个表达式将元组中的第一个元素应用到元组中的第二个元素： `squareIt num` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet24.fs)]

同样， `num` 可以互换使用标识符和整数10，因此可以使用标识符 `squareIt` 和 lambda 表达式 `fun n -> n * n` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet25.fs)]

## <a name="pass-the-value-as-an-argument"></a>将值作为参数传递

如果某个值具有语言形式的第一类状态，则可以将其作为参数传递给函数。 例如，通常将整数和字符串作为参数传递。 下面的代码显示在 F # 中作为参数传递的整数和字符串。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet26.fs)]

如果函数具有一流状态，则您必须能够以相同的方式将它们作为参数传递。 请记住，这是高阶函数的第一个特征。

在下面的示例中，函数 `applyIt` 具有两个参数： `op` 和 `arg` 。 如果在具有一个参数的函数中进行发送， `op` 并向提供函数的适当参数 `arg` ，则函数将返回应用到的结果 `op` `arg` 。 在下面的示例中，函数参数和整数参数都是通过使用其名称以相同方式发送的。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet27.fs)]

将函数作为自变量发送到另一个函数的能力是在函数编程语言（例如映射或筛选器操作）中为常见的抽象。 例如，映射操作是一个高阶函数，该函数捕获逐句通过列表的函数所共享的计算，对每个元素执行一些操作，然后返回结果列表。 您可能想要在一个整数列表中递增每个元素，或在每个元素的后面递增，或将字符串列表中的每个元素更改为大写。 计算中易出错的部分是单步执行列表并生成要返回的结果列表的递归过程。 该部件在映射函数中捕获。 对于特定应用程序，只需对每个列表元素应用一项功能， (添加、求平方、变化大小写) 。 该函数作为参数发送到 mapping 函数，就像 `squareIt` `applyIt` 在前面的示例中一样。

F # 为大多数集合类型（包括 [列表](../language-reference/lists.md)、 [数组](../language-reference/arrays.md)和 [序列](../language-reference/sequences.md)）提供映射方法。 下面的示例使用列表。 语法为 `List.map <the function> <the list>`。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet28.fs)]

有关详细信息，请参阅 [列表](../language-reference/lists.md)。

## <a name="return-the-value-from-a-function-call"></a>从函数调用返回值

最后，如果某个函数在某种语言中具有一流状态，则您必须能够将其作为函数调用的值返回，就像您返回其他类型（如整数和字符串）一样。

以下函数调用返回整数并显示它们。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet29.fs)]

以下函数调用返回一个字符串。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet30.fs)]

以下函数调用（以内联方式声明）将返回一个布尔值。 显示的值为 `True` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet31.fs)]

将函数作为函数调用值返回的能力是高阶函数的第二个特征。 在下面的示例中， `checkFor` 将定义为一个函数，该函数采用一个参数， `item` 并返回一个新函数作为其值。 返回的函数采用列表作为其参数， `lst` 并在中进行搜索 `item` `lst` 。 如果 `item` 存在，则该函数返回 `true` 。 如果 `item` 不存在，则该函数将返回 `false` 。 如上一节所示，以下代码使用提供的列表函数 [list](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-collections-listmodule.html#exists)，以搜索列表。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet32.fs)]

下面的代码使用 `checkFor` 创建一个新函数，该函数采用一个自变量和一个列表，并在列表中搜索7。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet33.fs)]

下面的示例使用 F # 中函数的第一类状态声明一个函数，该函数 `compose` 返回两个函数参数的组合。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet34.fs)]

> [!NOTE]
> 对于更短的版本，请参阅下一节 "扩充函数"。

下面的代码将两个函数作为自变量发送到 `compose` ，这两个函数都采用同一类型的单个自变量。 返回值是一个新函数，它是两个函数参数的组合。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet35.fs)]

> [!NOTE]
> F # 提供了两个 `<<` `>>` 编写函数的运算符和。 例如， `let squareAndDouble2 = doubleIt << squareIt` `let squareAndDouble = compose doubleIt squareIt` 在前面的示例中，等效于。

以下用于将函数作为函数调用值返回的示例将创建一个简单的推测游戏。 若要创建游戏，请调用，并在中调用要将其 `makeGame` 发送到的某个值 `target` 。 函数的返回值 `makeGame` 是一个函数，它采用一个自变量 (推测) 并报告推测是否正确。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet36.fs)]

下面的代码调用 `makeGame` ，并发送的 `7` 值 `target` 。 标识符 `playGame` 绑定到返回的 lambda 表达式。 因此，它 `playGame` 是一个函数，它将作为一个参数的值 `guess` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet37.fs)]

## <a name="curried-functions"></a>扩充函数

可以通过使用 F # 函数声明中的隐式 *currying* ，将上一节中的许多示例编写得更简洁。 Currying 是将具有多个参数的函数转换为一系列嵌入函数的过程，其中每个函数都有一个参数。 在 F # 中，具有多个参数的函数在本质上是扩充的。 例如， `compose` 可以按照下面的简洁样式中所示，用三个参数来编写上述部分。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet38.fs)]

但是，结果是一个参数的函数，该函数返回一个参数的函数，该函数反过来返回一个参数的另一个函数，如中所示 `compose4curried` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet39.fs)]

可以通过多种方式访问此函数。 下面的每个示例都返回并显示18。 您可以 `compose4` `compose4curried` 在任何示例中将替换为。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet40.fs)]

若要验证函数是否仍像以前一样工作，请再次尝试原始测试用例。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet41.fs)]

> [!NOTE]
> 可以通过在元组中包含参数来限制 currying。 有关详细信息，请参阅 [参数和参数](../language-reference/parameters-and-arguments.md)中的 "参数模式"。

下面的示例使用隐式 currying 来编写较短的版本 `makeGame` 。 `makeGame`在此格式中，构造和返回函数的方式的详细信息 `game` 不太明确，但你可以使用结果相同的原始测试用例进行验证。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet42.fs)]

有关 currying 的详细信息，请参阅 [函数](../language-reference/functions/index.md)中的 "参数的部分应用"。

## <a name="identifier-and-function-definition-are-interchangeable"></a>标识符和函数定义可互换

`num`前面的示例中的变量名的计算结果为整数10，并且在其中有效的情况 `num` 下，也是很奇怪的，10也是有效的。 函数标识符及其值也是如此：任何位置都可以使用函数的名称，可以使用它绑定到的 lambda 表达式。

下面的示例定义了一个名为的 `Boolean` 函数 `isNegative` ，然后使用函数的名称和函数的定义。 接下来的三个示例都返回并显示 `False` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet43.fs)]

若要进一步执行此操作，请将 `applyIt` 绑定到的值替换为 `applyIt` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet44.fs)]

## <a name="functions-are-first-class-values-in-f"></a>函数是 F 中的第一类值\#

前面几节中的示例演示 F # 中的函数满足 F # 中作为第一类值的条件：

- 可以将标识符绑定到函数定义。
[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet21.fs)]

- 您可以将函数存储在数据结构中。
[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet45.fs)]

- 可以将函数作为自变量传递。
[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet46.fs)]

- 您可以返回函数作为函数调用的值。
[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet32.fs)]

有关 F # 的详细信息，请参阅 [f # 语言参考](../language-reference/index.md)。

## <a name="example"></a>示例

### <a name="description"></a>说明

下面的代码包含本主题中的所有示例。

### <a name="code"></a>代码

[!code-fsharp[Main](~/samples/snippets/fsharp/contour/snippet47.fs)]

## <a name="see-also"></a>请参阅

- [列表](../language-reference/lists.md)
- [元组](../language-reference/tuples.md)
- [函数](../language-reference/functions/index.md)
- [`let` 绑定](../language-reference/functions/let-bindings.md)
- [Lambda 表达式： `fun` 关键字](../language-reference/functions/lambda-expressions-the-fun-keyword.md)
