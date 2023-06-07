---
title: F# 教程
description: '在本教程中，通过代码示例检查 F # 编程语言的一些重要功能。'
ms.date: 08/14/2020
ms.openlocfilehash: b115317e1f47ef7e18333cae4145b99e11645579
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88558590"
---
# <a name="tour-of-f"></a>F 教程\#

了解 F # 的最佳方式是读取和写入 F # 代码。 本文将通过 F # 语言的某些重要功能来提供教程，并提供可在计算机上执行的一些代码片段。 若要了解如何设置开发环境，请查看 [入门](get-started/index.md)。

F #：函数和类型中有两个主要概念。  本教程将重点介绍这两个概念的语言功能。

## <a name="executing-the-code-online"></a>联机执行代码

如果你的计算机上未安装 F #，则可以在 [WebAssembly 上使用 Try F #](https://tryfsharp.fsbolero.io/)执行浏览器中的所有示例。 Fable 是在浏览器中直接执行的 F # 的方言。 若要查看复制中跟随的示例，请查看示例 > 了解 Fable 复制的左侧菜单栏中 **的 F # > 教程** 。

## <a name="functions-and-modules"></a>函数和模块

任何 F # 程序的最基本部分都是组织到***模块***中的***函数***。  [函数](./language-reference/functions/index.md) 对输入进行操作以生成输出，并按模块进行组织，这些 [模块](./language-reference/modules.md)是在 F # 中对项进行分组的主要方式。  它们是使用绑定定义的，该[ `let` 绑定](./language-reference/functions/let-bindings.md)为函数指定名称并定义其参数。

[!code-fsharp[BasicFunctions](~/samples/snippets/fsharp/tour.fs#L101-L133)]

`let` 绑定还介绍如何将值绑定到名称，类似于其他语言中的变量。  `let` 默认情况下，绑定是 ***不可变*** 的，这意味着一旦值或函数绑定到名称，就不能就地更改。  这与其他语言中的变量不同，后者是 ***可变***的，这意味着在任何时间点都可以更改其值。  如果需要可变绑定，可使用 `let mutable ...` 语法。

[!code-fsharp[Immutability](~/samples/snippets/fsharp/tour.fs#L75-L94)]

## <a name="numbers-booleans-and-strings"></a>数字、布尔值和字符串

作为一种 .NET 语言，F # 支持 .NET 中存在的相同基础 [基元类型](language-reference/basic-types.md) 。

下面是 F # 中的各种数值类型的表示方式：

[!code-fsharp[Numbers](~/samples/snippets/fsharp/tour.fs#L49-L68)]

下面是布尔值和执行基本条件逻辑的形式：

[!code-fsharp[Bools](~/samples/snippets/fsharp/tour.fs#L142-L152)]

基本 [字符串](./language-reference/strings.md) 操作如下所示：

[!code-fsharp[Strings](~/samples/snippets/fsharp/tour.fs#L158-L180)]

## <a name="tuples"></a>元组

[元组](./language-reference/tuples.md) 是 F # 中的一个大型交易。  它们是未命名但有序值的分组，可被视为值本身。  将它们视为聚合自其他值的值。  它们有很多用途，如从函数中方便地返回多个值，或者为某些即席的便利分组值。

[!code-fsharp[Tuples](~/samples/snippets/fsharp/tour.fs#L186-L203)]

你还可以创建 `struct` 元组。  它们还与 c # 7/Visual Basic 15 元组完全互操作，后者也是 `struct` 元组：

[!code-fsharp[Tuples](~/samples/snippets/fsharp/tour.fs#L205-L218)]

请务必注意，因为 `struct` 元组是值类型，所以它们不能隐式转换为引用元组，反之亦然。  必须在 reference 和 struct 元组之间显式转换。

## <a name="pipelines-and-composition"></a>管道和组合

`|>`处理 F # 中的数据时，会广泛使用管道运算符（例如）。 这些运算符是允许您以灵活的方式建立函数的 "管道" 的函数。 下面的示例演示如何利用这些运算符来生成简单的功能管道：

[!code-fsharp[Pipelines](~/samples/snippets/fsharp/tour.fs#L227-L282)]

之前的示例使用了 F # 的许多功能，包括列表处理函数、第一类函数和 [部分应用程序](./language-reference/functions/index.md#partial-application-of-arguments)。 尽管深入了解其中每个概念可能会变得很简单，但应清楚地了解如何在生成管道时使用函数来处理数据。

## <a name="lists-arrays-and-sequences"></a>列表、数组和序列

列表、数组和序列是 F # 核心库中的三种主要集合类型。

[列表](./language-reference/lists.md) 是具有相同类型的元素的有序集合，不可变集合。  它们是一种单向链接列表，这意味着它们适用于枚举，但如果它们很大，则随机访问和串联的选择会很糟糕。  这与其他常用语言的列表相反，后者通常不使用单向链接列表来表示列表。

[!code-fsharp[Lists](~/samples/snippets/fsharp/tour.fs#L309-L359)]

[数组](./language-reference/arrays.md) 是具有相同类型的元素的固定大小、 *可变* 集合。  它们支持快速随机访问元素，并且速度快于 F # 列表，因为它们只是连续的内存块。

[!code-fsharp[Arrays](~/samples/snippets/fsharp/tour.fs#L368-L407)]

[序列](./language-reference/sequences.md) 是一系列逻辑元素，它们都属于同一类型。  这些是比列表和数组更通用的类型，可以是 "视图" 到任何逻辑序列的元素。  它们还会显得很明显，因为它们可能是 ***延迟***的，也就是说，仅当需要时才可以计算元素。

[!code-fsharp[Sequences](~/samples/snippets/fsharp/tour.fs#L418-L452)]

## <a name="recursive-functions"></a>递归函数

处理集合或元素序列通常是通过 F # 中的 [递归](./language-reference/functions/index.md#recursive-functions) 来完成的。  尽管 F # 支持循环和命令式编程，但递归是首选的，因为这样可以更轻松地保证正确性。

> [!NOTE]
> 下面的示例通过表达式利用模式匹配 `match` 。  本文的后面部分介绍了这一基本构造。

[!code-fsharp[RecursiveFunctions](~/samples/snippets/fsharp/tour.fs#L461-L500)]

F # 还完全支持尾调用优化，这是一种优化递归调用的方法，使其与循环构造一样快。

## <a name="record-and-discriminated-union-types"></a>记录和可区分联合类型

记录类型和联合类型是 F # 代码中使用的两种基本数据类型，通常是在 F # 程序中表示数据的最佳方式。  虽然这使得它们类似于其他语言中的类，但其主要区别之一是它们具有结构相等语义。  这意味着，它们是 "本机" 可比较的，相等非常简单-只需检查是否有一个与另一个相等。

[记录](./language-reference/records.md) 是命名值的聚合，可选成员 (如) 的方法。  如果熟悉 c # 或 Java，则这些内容应类似于 "Poco" 或 "Pojo"。

[!code-fsharp[Records](~/samples/snippets/fsharp/tour.fs#L507-L559)]

您还可以将记录表示为结构。 这是通过属性完成的 `[<Struct>]` ：

[!code-fsharp[Records](~/samples/snippets/fsharp/tour.fs#L561-L568)]

[Du) 的可区分联合 (](./language-reference/discriminated-unions.md) 是可能是一些命名窗体或事例的值。  类型中存储的数据可以是多个不同值之一。

[!code-fsharp[Unions](~/samples/snippets/fsharp/tour.fs#L575-L631)]

你还可以使用 Du 作为 *单用例可区分联合*，以帮助进行基于基元类型的域建模。  通常情况下，字符串和其他基元类型用于表示某些内容，因此具有特定的含义。  但是，仅使用数据的基元表示形式可能会导致错误地分配不正确的值！  将每种类型的信息表示为不同的单用例联合可在这种情况下强制执行正确性。

[!code-fsharp[Unions](~/samples/snippets/fsharp/tour.fs#L633-L654)]

如上面的示例所示，若要获取单个用例可区分联合的基础值，必须将其显式解包。

此外，Du 还支持递归定义，使你能够轻松地表示树和本质上递归的数据。  例如，下面介绍了如何使用和函数表示二元搜索树 `exists` `insert` 。

[!code-fsharp[Unions](~/samples/snippets/fsharp/tour.fs#L656-L683)]

由于 Du 允许您在数据类型中表示树的递归结构，因此，对此递归结构进行操作非常简单，并且保证正确性。  它还支持模式匹配，如下所示。

此外，还可以将 Du 表示为 `struct` 具有 `[<Struct>]` 属性的：

[!code-fsharp[Unions](~/samples/snippets/fsharp/tour.fs#L685-L696)]

但是，在执行此操作时，请记住以下两个重要事项：

1. 不能以递归方式定义结构 DU。
2. 结构 DU 的每个事例都必须具有唯一的名称。

如果未遵循上述操作，则会导致编译错误。

## <a name="pattern-matching"></a>模式匹配

[模式匹配](./language-reference/pattern-matching.md) 是 f # 语言功能，可实现对 f # 类型的操作的正确性。  在上述示例中，您可能注意到了相当多的 `match x with ...` 语法。  此构造允许编译器（可以理解数据类型的 "形状"）强制你考虑使用数据类型（通过所谓的 "完全模式匹配"）时的所有可能情况。  这是非常强大的功能，可以巧妙用于 "提升" 通常是运行时问题的编译时问题。

[!code-fsharp[PatternMatching](~/samples/snippets/fsharp/tour.fs#L705-L742)]

你可能注意到的是使用 `_` 模式。  这称为 [通配符模式](./language-reference/pattern-matching.md#wildcard-pattern)，这是一种 "我不在意什么内容" 的方法。  尽管很方便，但如果不小心使用，则可能会意外绕过穷举模式匹配，并且不再受益于编译时操作 `_` 。  当您在模式匹配时不关心分解类型的某些部分时，最好使用此方法; 当您枚举了模式匹配表达式中的所有有意义的事例时，最好使用该方法。

在下面的示例中， `_` 分析操作失败时使用事例。

[!code-fsharp[PatternMatching](~/samples/snippets/fsharp/tour.fs#L744-L762)]

[活动模式](./language-reference/active-patterns.md) 是另一个功能强大的构造，可与模式匹配一起使用。  它们允许您将输入数据分区为自定义窗体，并在模式匹配调用站点分解它们。  它们也可以参数化，从而允许将分区定义为函数。  扩展前面的示例以支持活动模式，如下所示：

[!code-fsharp[ActivePatterns](~/samples/snippets/fsharp/tour.fs#L764-L786)]

## <a name="optional-types"></a>可选类型

可区分联合类型的一种特殊情况是 "选项类型"，它非常有用，因为它是 F # 核心库的一部分。

[选项类型](./language-reference/options.md) 是一种类型，该类型表示以下两种情况之一：值或根本不显示任何内容。  它用于某个值可能因特定操作而不会产生的任何情况。  这会强制你考虑这两种情况，使其成为编译时问题，而不是运行时问题。  这通常用于 Api 中，其中 `null` 用于表示 "无"，从而避免了 `NullReferenceException` 在许多情况下需要担心的情况。

[!code-fsharp[Options](~/samples/snippets/fsharp/tour.fs#L789-L814)]

## <a name="units-of-measure"></a>度量单位

F # 类型系统的一项独特功能是能够通过度量单位为数字文本提供上下文。

[度量单位](./language-reference/units-of-measure.md) 允许您将数值类型与单位（例如计量）相关联，并使函数可对单元（而不是数字文本）执行工作。  这使编译器能够验证传入的数值类型在特定上下文中是否有效，从而消除了与此类工作相关的运行时错误。

[!code-fsharp[UnitsOfMeasure](~/samples/snippets/fsharp/tour.fs#L817-L842)]

F # 核心库定义了许多 SI 单元类型和单位转换。  若要了解详细信息，请查看 [UnitSystems 命名空间 fsharp.core](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-data-unitsystems-si-unitsymbols.html)。

## <a name="classes-and-interfaces"></a>类和接口

F # 还对 .NET 类、 [接口](./language-reference/interfaces.md)、 [抽象类](./language-reference/abstract-classes.md)、 [继承](./language-reference/inheritance.md)等提供完全支持。

[类](./language-reference/classes.md) 是表示 .net 对象的类型，这些对象可以具有属性、方法和事件作为其 [成员](./language-reference/members/index.md)。

[!code-fsharp[Classes](~/samples/snippets/fsharp/tour.fs#L845-L880)]

定义泛型类也非常简单。

[!code-fsharp[Classes](~/samples/snippets/fsharp/tour.fs#L883-L908)]

若要实现接口，可以使用 `interface ... with` 语法或 [对象表达式](./language-reference/object-expressions.md)。

[!code-fsharp[Classes](~/samples/snippets/fsharp/tour.fs#L911-L934)]

## <a name="which-types-to-use"></a>要使用的类型

类、记录、可区分联合和元组的存在导致了重要问题：应使用哪一个？  与生活中的大多数操作一样，答案取决于你的情况。

元组非常适合从函数返回多个值，并使用临时值聚合作为值本身。

记录是元组中的 "单步执行"，其命名标签和对可选成员的支持。  它们非常适用于通过您的程序传输的数据的低计划表示形式。  由于它们具有结构相等性，因此它们可用于比较。

可区分联合具有许多用途，但核心好处是能够将它们与模式匹配一起使用，以考虑数据可以具有的所有可能的 "形状"。  

类对于大量原因非常有用，例如，当你需要表示信息并将该信息绑定到功能时。  作为经验法则，当你具有在概念上与某些数据关联的功能时，使用类和面向对象编程的原则是一项巨大的好处。  与 c # 和 Visual Basic 进行互操作时，类也是首选的数据类型，因为这些语言几乎都使用类。

## <a name="next-steps"></a>后续步骤

现在，您已了解了该语言的一些主要功能，您应该准备好编写您的第一个 F # 程序！  请查看 [入门](get-started/index.md) ，了解如何设置开发环境并编写一些代码。

了解详细信息的后续步骤可以是你喜欢的任何内容，但建议 [在 F # 中介绍函数编程](./introduction-to-functional-programming/index.md) ，以熟悉核心功能编程概念。  在 F # 中构建可靠的程序时，这一点非常重要。

另外，请查看 [f # 语言参考](./language-reference/index.md) ，查看 f # 上的概念内容的完整集合。
