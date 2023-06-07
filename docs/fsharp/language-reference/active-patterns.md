---
title: 活动模式
description: '了解如何使用活动模式来定义用 F # 编程语言细分输入数据的已命名分区。'
ms.date: 05/16/2016
ms.openlocfilehash: 7b6da38baa35f30ae6de8a930be60a4e4fc0fc0d
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493886"
---
# <a name="active-patterns"></a>活动模式

*活动模式*使您能够定义细分输入数据的命名分区，以便在模式匹配表达式中使用这些名称，就像对可区分联合使用这些名称一样。 可使用活动模式以自定义方式为每个分区分解数据。

## <a name="syntax"></a>语法

```fsharp
// Active pattern of one choice.
let (|identifier|) [arguments] valueToMatch = expression

// Active Pattern with multiple choices.
// Uses a FSharp.Core.Choice<_,...,_> based on the number of case names. In F#, the limitation n <= 7 applies.
let (|identifer1|identifier2|...|) valueToMatch = expression

// Partial active pattern definition.
// Uses a FSharp.Core.option<_> to represent if the type is satisfied at the call site.
let (|identifier|_|) [arguments ] valueToMatch = expression
```

## <a name="remarks"></a>备注

在前面的语法中，标识符是由*参数*表示的输入数据的分区名称，换句话说，是参数的所有值的集合的子集的名称。 活动模式定义中最多可以有七个分区。 *表达式*描述数据要分解到的窗体。 您可以使用活动模式定义来定义规则，这些规则用于确定作为参数提供的值所属于的指定分区。 （| 和 |）符号称为*香蕉剪辑*，此类 let 绑定创建的函数称为*活动识别器*。

例如，请考虑以下具有参数的活动模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5001.fs)]

可以在模式匹配表达式中使用活动模式，如下面的示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5002.fs)]

此程序的输出如下所示：

```console
7 is odd
11 is odd
32 is even
```

活动模式的另一个用法是通过多种方式分解数据类型，例如，当相同的基础数据具有各种可能的表示形式时。 例如， `Color` 对象可以分解为 RGB 表示形式或 HSB 表示形式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5003.fs)]

上述程序的输出如下所示：

```console
Red
 Red: 255 Green: 0 Blue: 0
 Hue: 360.000000 Saturation: 1.000000 Brightness: 0.500000
Black
 Red: 0 Green: 0 Blue: 0
 Hue: 0.000000 Saturation: 0.000000 Brightness: 0.000000
White
 Red: 255 Green: 255 Blue: 255
 Hue: 0.000000 Saturation: 0.000000 Brightness: 1.000000
Gray
 Red: 128 Green: 128 Blue: 128
 Hue: 0.000000 Saturation: 0.000000 Brightness: 0.501961
BlanchedAlmond
 Red: 255 Green: 235 Blue: 205
 Hue: 36.000000 Saturation: 1.000000 Brightness: 0.901961
```

结合使用这两种使用活动模式的方法，您可以将数据分区和分解为适当的形式，并对最方便计算的窗体中的相应数据执行适当的计算。

生成的模式匹配表达式使数据能够以一种易于阅读的方便方式写入，从而极大地简化了可能复杂的分支和数据分析代码。

## <a name="partial-active-patterns"></a>部分活动模式

有时，您需要只对部分输入空间进行分区。 在这种情况下，你编写一组部分模式，其中每个模式都匹配某些输入但无法匹配其他输入。 不会始终产生值的活动模式称为*部分活动模式*;它们具有作为选项类型的返回值。 若要定义部分活动模式，请在 \_ 香蕉剪辑中的模式列表的末尾使用通配符（）。 下面的代码演示如何使用部分活动模式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5004.fs)]

上一个示例的输出如下所示：

```console
1.100000 : Floating point
0 : Integer
0.000000 : Floating point
10 : Integer
Something else : Not matched.
```

使用部分活动的模式时，有时个别选择可能是不连续的或互斥的，但它们不需要。 在下面的示例中，模式正方形和模式多维数据集不是连续的，因为某些数字既是正方形又是多维数据集，例如64。 下面的程序使用和模式来合并正方形和立方体模式。 它输出了所有最大为1000的整数，它们都是平方和多维数据集，而是只是多维数据集。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5005.fs)]

输出如下所示：

```console
1 is a cube and a square
8 is a cube
27 is a cube
64 is a cube and a square
125 is a cube
216 is a cube
343 is a cube
512 is a cube
729 is a cube and a square
1000 is a cube
```

## <a name="parameterized-active-patterns"></a>参数化活动模式

活动模式始终为要匹配的项至少使用一个参数，但也可能采用其他参数，在这种情况下，将应用名为*参数化活动模式*的名称。 其他参数允许专用化常规模式。 例如，使用正则表达式分析字符串的活动模式通常包含正则表达式作为额外参数，如以下代码所示，它还使用 `Integer` 在上一个代码示例中定义的部分活动模式。 在此示例中，将为使用各种日期格式的正则表达式的字符串自定义常规 ParseRegex 活动模式。 整数活动模式用于将匹配的字符串转换为可传递到 DateTime 构造函数的整数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5006.fs)]

上述代码的输出如下所示：

```console
12/22/2008 12:00:00 AM 1/1/2009 12:00:00 AM 1/15/2008 12:00:00 AM 12/28/1995 12:00:00 AM
```

活动模式并不局限于模式匹配表达式，还可以在 let 绑定上使用它们。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5007.fs)]

上述代码的输出如下所示：

```console
Hello, random citizen!
Hello, George!
```

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [Match 表达式](match-expressions.md)
