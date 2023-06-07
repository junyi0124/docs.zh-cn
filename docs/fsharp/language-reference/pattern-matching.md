---
title: 模式匹配
description: '了解如何在 F # 中使用模式，以便将数据与逻辑结构进行比较，将数据分解为各个构成部分，或从数据中提取信息。'
ms.date: 11/12/2020
ms.openlocfilehash: 932f50b7947f6df728149437dd3ceb19c42e5c6a
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740271"
---
# <a name="pattern-matching"></a>模式匹配

模式是用于转换输入数据的规则。 它们在整个 F # 语言中使用，以将数据与一个或多个逻辑结构进行比较，将数据分解为各个构成部分，或以各种方式从数据中提取信息。

## <a name="remarks"></a>备注

在许多语言构造（如表达式）中使用模式 `match` 。 当您处理 `let` 绑定、lambda 表达式和与表达式关联的异常处理程序中的函数的参数时，将使用这些参数 `try...with` 。 有关详细信息，请参阅 [Match 表达式](match-expressions.md)、 [let 绑定](./functions/let-bindings.md)、 [Lambda 表达式： `fun` 关键字](./functions/lambda-expressions-the-fun-keyword.md)和 [异常： `try...with` 表达式](./exception-handling/the-try-with-expression.md)。

例如，在表达式中 `match` ，管道符号 *pattern* 后跟。

```fsharp
match expression with
| pattern [ when condition ] -> result-expression
...
```

每个模式都充当以某种方式转换输入的规则。 在 `match` 表达式中，将依次检查每个模式，以查看输入数据是否与模式兼容。 如果找到匹配项，则执行结果表达式。 如果找不到匹配项，则测试下一个模式规则。 在 [匹配表达式](match-expressions.md)中说明时，可选的 when *条件* 部分。

下表显示了支持的模式。 在运行时，将按照表中列出的顺序针对以下每个模式对输入进行测试，并以递归方式应用模式：在代码中显示时，从第一个到最后一个，以及从左到右的每行模式。

|“属性”|描述|示例|
|----|-----------|-------|
|常量模式|任何数值、字符或字符串文本、枚举常量或定义的文本标识符|`1.0`, `"test"`, `30`, `Color.Red`|
|标识符模式|可区分联合、异常标签或活动模式用例的 case 值|`Some(x)`<br /><br />`Failure(msg)`|
|变量模式|*identifier*|`a`|
|`as` 化|作为 *标识符* 的 *模式*|`(a, b) as tuple1`|
|或模式|*pattern1* &#124; *pattern2*|<code>([h] &#124; [h; _])</code>|
|AND 模式|*pattern1* &amp;*pattern2*|`(a, b) & (_, "test")`|
|缺点模式|*标识符* ：： *list-标识符*|`h :: t`|
|列表模式|[ *pattern_1*; ...; *pattern_n* ]|`[ a; b; c ]`|
|数组模式|[&#124; *pattern_1*; ...; *pattern_n* &#124;]|<code>[&#124; a; b; c &#124;]</code>|
|带括号模式| ( *模式* ) |`( a )`|
|元组模式| ( *pattern_1*， *pattern_n* ) |`( a, b )`|
|记录模式|{ *identifier1*  = *pattern_1*;... ;*identifier_n*  = *pattern_n* }|`{ Name = name; }`|
|通配符模式|_|`_`|
|模式与类型注释一起|*模式* ： *类型*|`a : int`|
|类型测试模式|:? *类型* [作为 *标识符* ]|`:? System.DateTime as dt`|
|Null 模式|null|`null`|
|Nameof 模式|*nameof expr*|`nameof str`|

## <a name="constant-patterns"></a>常量模式

常量模式包括数值、字符和字符串文本、枚举常量 (包含) 的枚举类型名称。 只能将 `match` 具有恒定模式的表达式与其他语言的 case 语句进行比较。 如果值相等，则将输入与文本值进行比较并且模式匹配。 文本的类型必须与输入的类型兼容。

下面的示例演示如何使用文本模式，并且还使用变量模式和或模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4801.fs)]

文本模式的另一个示例是基于枚举常量的模式。 使用枚举常量时，必须指定枚举类型名称。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4802.fs)]

## <a name="identifier-patterns"></a>标识符模式

如果模式是形成有效标识符的字符串，则标识符的形式决定了模式的匹配方式。 如果标识符的长度超过一个字符并且以大写字符开头，则编译器将尝试与标识符模式进行匹配。 此模式的标识符可以是用文本特性标记的值、可区分联合用例、异常标识符或活动模式用例。 如果未找到匹配的标识符，则匹配将失败，并且会将下一个模式规则（变量模式）与输入进行比较。

可区分联合模式可以是简单的命名事例，也可以具有值或包含多个值的元组。 如果有值，则必须指定值的标识符。 如果是元组，则必须为元组的每个元素提供一个带有标识符的元组模式，或为一个或多个命名联合字段提供字段名称的标识符。 有关示例，请参阅本节中的代码示例。

`option`类型是具有两个用例（和）的可区分联合 `Some` `None` 。 一个 (`Some`) 有一个值，但是另一个 (`None`) 只是一个命名的事例。 因此， `Some` 需要为与用例关联的值使用变量 `Some` ，但 `None` 必须单独出现。 在下面的代码中，将为变量 `var1` 提供通过匹配大小写获得的值 `Some` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4803.fs)]

在下面的示例中， `PersonName` 可区分联合包含字符串的混合和表示可能的名称形式的字符。 可区分联合的情况为 `FirstOnly` 、 `LastOnly` 和 `FirstLast` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4804.fs)]

对于具有命名字段的可区分联合，使用等号 (=) 提取命名字段的值。 例如，请考虑使用类似于下面的声明的可区分联合。

```fsharp
type Shape =
    | Rectangle of height : float * width : float
    | Circle of radius : float
```

您可以使用模式匹配表达式中的命名字段，如下所示。

```fsharp
let matchShape shape =
    match shape with
    | Rectangle(height = h) -> printfn $"Rectangle with length %f{h}"
    | Circle(r) -> printfn $"Circle with radius %f{r}"
```

命名字段的使用是可选的，因此在前面的示例中， `Circle(r)` 和 `Circle(radius = r)` 具有相同的效果。

指定多个字段时，请使用分号 (; ) 作为分隔符。

```fsharp
match shape with
| Rectangle(height = h; width = w) -> printfn $"Rectangle with height %f{h} and width %f{w}"
| _ -> ()
```

使用活动模式可以定义更复杂的自定义模式匹配。 有关活动模式的详细信息，请参阅 [活动模式](active-patterns.md)。

标识符为异常的情况在异常处理程序上下文的模式匹配中使用。 有关异常处理中的模式匹配的信息，请参阅 [异常： `try...with` 表达式](./exception-handling/the-try-with-expression.md)。

## <a name="variable-patterns"></a>变量模式

变量模式将匹配的值分配给一个变量名称，该变量可在符号右侧的执行表达式中使用 `->` 。 变量模式单独与任何输入匹配，但变量模式通常显示在其他模式内，因此可以将更复杂的结构（例如元组和数组）分解为变量。

下面的示例演示元组模式内的变量模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4805.fs)]

## <a name="as-pattern"></a>as 模式

`as`模式是一个附加了子句的模式 `as` 。 `as`子句将匹配的值绑定到可在表达式的执行表达式中使用的名称， `match` 或者，如果在绑定中使用此模式，则会将 `let` 该名称作为绑定添加到本地范围。

下面的示例使用 `as` 模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4806.fs)]

## <a name="or-pattern"></a>或模式

当输入数据可以匹配多个模式，并且您想要执行与结果相同的代码时，可以使用或模式。 或模式两边的类型必须兼容。

下面的示例演示了或模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4807.fs)]

## <a name="and-pattern"></a>AND 模式

和模式要求输入匹配两种模式。 和模式两边的类型必须兼容。

下面的示例类似于 `detectZeroTuple` 本主题后面的元组模式部分中所示的示例，但这两个 `var1` 和 `var2` 都是使用和模式作为值获取的。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4808.fs)]

## <a name="cons-pattern"></a>缺点模式

缺点模式用于将列表分解为第一个元素、 *头* 以及包含剩余元素（ *尾部*）的列表。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4809.fs)]

## <a name="list-pattern"></a>列表模式

列表模式使列表可分解为多个元素。 列表模式本身只能匹配特定数量的元素的列表。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4810.fs)]

## <a name="array-pattern"></a>数组模式

数组模式类似于列表模式，可用于分解特定长度的数组。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4811.fs)]

## <a name="parenthesized-pattern"></a>带括号模式

括号可以围绕模式分组，以实现所需的关联性。 在下面的示例中，括号用于控制和模式与缺点模式之间的相关性。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4812.fs)]

## <a name="tuple-pattern"></a>元组模式

元组模式与元组格式的输入匹配，并通过对元组中的每个位置使用模式匹配变量，将元组分解为其构成元素。

下面的示例演示元组模式，还使用文本模式、变量模式和通配符模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4813.fs)]

## <a name="record-pattern"></a>记录模式

记录模式用于分解记录，以便提取字段的值。 模式不必引用记录的所有字段;所有省略的字段不会参与匹配，也不会被提取。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4814.fs)]

## <a name="wildcard-pattern"></a>通配符模式

通配符模式由下划线 () 字符表示， `_` 并与变量模式相同，只是丢弃输入，而不是分配给变量。 通配符模式通常用在其他模式内作为值的占位符，而在该符号右侧的表达式中不需要这些值 `->` 。 通配符模式还经常用于模式列表的末尾，以匹配任何不匹配的输入。 本主题中的许多代码示例演示了通配符模式。 有关示例，请参阅前面的代码。

## <a name="patterns-that-have-type-annotations"></a>具有类型批注的模式

模式可以具有类型批注。 它们的行为与其他类型注释和其他类型注释类似。 在模式中的类型批注前后需要括号。 下面的代码演示具有类型批注的模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4815.fs)]

## <a name="type-test-pattern"></a>类型测试模式

类型测试模式用于将输入与类型进行匹配。 如果输入类型与在模式中指定的类型) 的 (或派生类型匹配，则匹配成功。

下面的示例演示了类型测试模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4816.fs)]

如果只检查标识符是否为特定的派生类型，则不需要 `as identifier` 模式的一部分，如以下示例中所示：

```fsharp
type A() = class end
type B() = inherit A()
type C() = inherit A()

let m (a: A) =
    match a with
    | :? B -> printfn "It's a B"
    | :? C -> printfn "It's a C"
    | _ -> ()
```

## <a name="null-pattern"></a>Null 模式

Null 模式匹配可以在使用允许 null 值的类型时显示的 null 值。 与 .NET Framework 代码互操作时，经常使用 Null 模式。 例如，.NET API 的返回值可能是表达式的输入 `match` 。 您可以根据返回值是否为 null 以及返回值的其他特征来控制程序流。 可以使用 null 模式来防止空值传播到程序的其余部分。

下面的示例使用 null 模式和变量模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4817.fs)]

## <a name="nameof-pattern"></a>Nameof 模式

`nameof`当某个字符串的值等于关键字后面的表达式时，该模式与字符串匹配 `nameof` 。 例如：

```fsharp
let f (str: string) =
    match str with
    | nameof str -> "It's 'str'!"
    | _ -> "It is not 'str'!"

f "str" // matches
f "asdf" // does not match
```

有关 [`nameof`](nameof.md) 可获取名称的信息，请参阅运算符。

## <a name="see-also"></a>请参阅

- [Match 表达式](match-expressions.md)
- [活动模式](active-patterns.md)
- [F# 语言参考](index.md)
