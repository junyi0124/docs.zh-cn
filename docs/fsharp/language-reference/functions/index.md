---
title: 函数
description: 了解 F# 中的函数，以及 F# 如何支持常见函数编程构造。
ms.date: 05/16/2016
ms.openlocfilehash: e49183e0634dee1750757abadbfe9e9c824f51a8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596469"
---
# <a name="functions"></a>函数

函数是任何编程语言中程序执行的基本单元。 和其他语言一样，F# 函数有一个名称，可以有形参并采用实参，且具有一个主体。 F# 还支持函数编程构造，例如将函数视为值、在表达式中使用未命名的函数、组合函数以形成新的函数、扩充函数以及通过部分应用函数参数来隐式定义函数。

可通过使用 `let` 关键字或 `let rec` 关键字组合（如果函数为递归函数）定义函数。

## <a name="syntax"></a>语法

```fsharp
// Non-recursive function definition.
let [inline] function-name parameter-list [ : return-type ] = function-body
// Recursive function definition.
let rec function-name parameter-list = recursive-function-body
```

## <a name="remarks"></a>备注

*function-name* 是表示函数的标识符。 *parameter-list* 包含由空格分隔的连续参数。 可按“参数”部分所述，为每个参数指定一个显式类型。 如果未指定特定参数类型，编译器将尝试通过函数体推断类型。 *function-body* 包含一个表达式。 组成函数体的表达式通常是一个由许多表达式组成的复合表达式，组成它的那些表达式以作为返回值的最终表达式结束。 *return-type* 是一个冒号后跟一个类型，且为可选项。 如果不显式指定返回值的类型，编译器将从最终表达式确定返回类型。

简单的函数定义类似：

```fsharp
let f x = x + 1
```

在上一示例中，函数名称为 `f`，参数为 `x`（具有类型 `int`），函数体为 `x + 1`，且返回值的类型为 `int`。

可将函数标记为 `inline`。 有关 `inline` 的信息，请参阅[内联函数](inline-functions.md)。

## <a name="scope"></a>范围

在模块范围以外的任何级别范围上，重用某个值或函数名称不是错误。 重用某个名称时，后来声明的名称将隐藏前面声明的名称。 但在模块中的顶级别范围内，名称必须唯一。 例如，以下代码如果出现在模块范围内则会产生错误，而出现在某个函数内部则不会产生错误：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet101.fs)]

但以下代码在任何级别的范围内都可接受：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet102.fs)]

### <a name="parameters"></a>参数

参数的名称列在函数名称之后。 可以为参数指定类型，如下例所示：

```fsharp
let f (x : int) = x + 1
```

如果指定一个类型，则应让其跟在参数名称的后面，并用冒号与参数名称隔开。 如果省略参数的类型，编译器会推断参数类型。 例如，在以下函数定义中，会将参数 `x` 推断为 `int` 类型，这是因为 1 为类型 `int`。

```fsharp
let f x = x + 1
```

但编译器会尝试尽量使函数成为泛型。 例如，注意下面的代码：

```fsharp
let f x = (x, x)
```

该函数从任何类型的一个参数中创建一个元组。 因为未指定类型，所以该函数可用于任何参数类型。 有关详细信息，请参阅[自动泛化](../generics/automatic-generalization.md)。

## <a name="function-bodies"></a>函数体

函数体可包含本地变量和函数的定义。 此类变量和函数位于当前函数的主体范围内，而非其外部。 如果已启用轻量语法选项，则必须使用缩进来指示定义位于函数体中，如下例所示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet103.fs)]

有关详细信息，请参阅[代码格式设置准则](../../style-guide/formatting.md)和[详细语法](../verbose-syntax.md)。

## <a name="return-values"></a>返回值

编译器使用函数体中的最终表达式来确定返回值和类型。 编译器可能会从前面的表达式来推断最终表达式的类型。 在前一部分中所示的函数 `cylinderVolume` 中，可从文本 `3.14159` 的类型确定 `pi` 的类型为 `float`。 编译器使用 `pi` 的类型来确定表达式 `h * pi * r * r` 的类型为 `float`。 因此，函数的总体返回类型为 `float`。

若要显式指定返回值，请编写如下代码：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet105.fs)]

如上面编写的代码一样，编译器将 **float** 应用于整个函数；如果还打算将其应用于参数类型，可使用以下代码：

```fsharp
let cylinderVolume (radius : float) (length : float) : float
```

## <a name="calling-a-function"></a>调用函数

可以通过以下方式调用函数：指定函数名称，后跟一个空格，然后是由空格分隔的任何参数。 例如，若要调用函数 **cylinderVolume**，并将结果赋给值 **vol**，可编写以下代码：

```fsharp
let vol = cylinderVolume 2.0 3.0
```

## <a name="partial-application-of-arguments"></a>部分应用参数

如果提供的参数数目少于指定的参数数目，则可创建一个应采用其余参数的新函数。 这种处理参数的方法称为“柯里化”，而且这是 F# 等函数编程语言的一个特性。 例如，假设正在处理两种尺寸的管道：一种管道的半径为 **2.0**，另一种管道的半径为 **3.0**。 你可能会创建用于确定管道体积的函数，如下所示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet106.fs)]

然后，会根据需要提供附加参数，用来表示两个不同尺寸的管道的各种长度：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet107.fs)]

## <a name="recursive-functions"></a>递归函数

递归函数是调用自身的函数。 它们要求在指定 **let** 关键字之后指定 **rec** 关键字。 从函数的主体内调用递归函数与调用任何函数调用是一样的。 以下递归函数计算第 n 个斐波纳契数<sup></sup>。 斐波纳契数序列很早就为人所知，此序列中的每个连续的数字都是序列中前两个数字之和。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet108.fs)]

如果在编写递归函数时不细心或者不清楚一些特殊技术（例如累加器和延续的使用），则有些递归函数可能会造成执行效率低下或使程序堆栈溢出。

## <a name="function-values"></a>函数值

在 F# 中，所有函数都被视为值；实际上，它们被称为“函数值”。 因为函数是值，所以它们可用作其他函数的参数，或在需要使用值的其他上下文中使用。 下面是一个采用函数值作为参数的函数示例：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet109.fs)]

可通过使用 `->` 标记来指定函数值的类型。 此标记的左边是参数的类型，右边是返回值。 在前面的示例中，`apply1` 是一个采用函数 `transform` 作为参数的函数，其中 `transform` 是一个采用整数并返回其他整数的函数。 下面的代码演示如何使用 `apply1`：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet110.fs)]

运行前面的代码之后，`result` 的值将为 101。

多个参数之间由连续的 `->` 标记分隔，如下例所示：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet111.fs)]

结果是 200。

## <a name="lambda-expressions"></a>Lambda 表达式

*lambda 表达式* 是未命名的函数。 在上述示例中，可以不定义命名的函数 **increment** 和 **mul**，而使用如下的 lambda 表达式：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet112.fs)]

可通过使用 `fun` 关键字来定义 lambda 表达式。 lambda 表达式类似于函数定义，只不过使用了 `->` 标记将参数列表与函数体分隔，而不是使用 `=` 标记。 与在常规函数定义中一样，可推断或显式指定参数类型，并且从主体中最后一个表达式的类型推断出 lambda 表达式的返回类型。 有关详细信息，请参阅 [Lambda 表达式：`fun` 关键字](lambda-expressions-the-fun-keyword.md)。

## <a name="function-composition-and-pipelining"></a>函数组合和流水线处理

F# 中的函数可由其他函数组合而成。 由 **function1** 和 **function2** 这两个函数组合而成的另一个函数表示先应用 **function1**，接着应用 **function2**：

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet113.fs)]

结果为 202。

流水线处理可让函数调用链接在一起形成连续的操作。 以下是流水线处理的工作方式：

```fsharp
let result = 100 |> function1 |> function2
```

结果再次为 202。

组合运算符采用两个函数并返回一个函数；相反，流水线运算符采用一个函数和一个参数并返回一个值。 下面的代码示例通过显示函数签名和用法的差异，指出了流水线运算符和组合运算符之间的区别。

```fsharp
// Function composition and pipeline operators compared.

let addOne x = x + 1
let timesTwo x = 2 * x

// Composition operator
// ( >> ) : ('T1 -> 'T2) -> ('T2 -> 'T3) -> 'T1 -> 'T3
let Compose2 = addOne >> timesTwo

// Backward composition operator
// ( << ) : ('T2 -> 'T3) -> ('T1 -> 'T2) -> 'T1 -> 'T3
let Compose1 = addOne << timesTwo

// Result is 5
let result1 = Compose1 2

// Result is 6
let result2 = Compose2 2

// Pipelining
// Pipeline operator
// ( |> ) : 'T1 -> ('T1 -> 'U) -> 'U
let Pipeline2 x = addOne x |> timesTwo

// Backward pipeline operator
// ( <| ) : ('T -> 'U) -> 'T -> 'U
let Pipeline1 x = addOne <| timesTwo x

// Result is 5
let result3 = Pipeline1 2

// Result is 6
let result4 = Pipeline2 2
```

## <a name="overloading-functions"></a>重载函数

可以重载类型（而非函数）的方法。 有关详细信息，请参阅[方法](../members/methods.md)。

## <a name="see-also"></a>请参阅

- [值](../values/index.md)
- [F# 语言参考](../index.md)
