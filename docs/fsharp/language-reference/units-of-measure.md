---
title: 度量单位
description: '了解 F # 中的浮点和有符号整数值如何具有关联的度量单位，这些度量值通常用于指示长度、数量和质量。'
ms.date: 08/15/2020
ms.openlocfilehash: 0262f89e80697dd86161c93fe37381122972b56f
ms.sourcegitcommit: 9c45035b781caebc63ec8ecf912dc83fb6723b1f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88811569"
---
# <a name="units-of-measure"></a>度量单位

F # 中的浮点和有符号整数值可以具有关联的度量单位，这通常用于指示长度、数量和质量等。 通过使用具有单位的数量，可以使编译器验证算术关系是否具有正确的单位，这有助于防止编程错误。

## <a name="syntax"></a>语法

```fsharp
[<Measure>] type unit-name [ = measure ]
```

## <a name="remarks"></a>备注

前面的语法将 *单位名称* 定义为度量单位。 可选部分用于根据以前定义的单元定义新的度量值。 例如，下面的行定义 `cm` (厘米) 的度量值。

```fsharp
[<Measure>] type cm
```

以下行将 (milliliter) 的度量值定义 `ml` 为三厘米 (`cm^3`) 。

```fsharp
[<Measure>] type ml = cm^3
```

在前面的语法中， *measure* 是涉及单元的公式。 在涉及单元的公式中，支持整数幂 (正和负) ，单位之间的空格指示两个单位的积， `*` 还指示单位的积，并 `/` 指示单位的商。 对于倒数单元，可以使用负整数幂或 `/` 指示单元公式的分子和分母之间的分隔。 分母中的多个单元应括在括号中。 后面由空格分隔的单位 `/` 被解释为分母的一部分，但后面的任何单位会被 `*` 解释为作为分子的一部分。

您可以在单位表达式中使用1，而不是将其用于表示一种单量维度，也可以与其他单位（如分子）一起使用。 例如，费率的单位将写为 `1/s` ，其中 `s` 指示秒。 单元公式中不使用括号。 不在单元公式中指定数值转换常量;但是，您可以单独定义单位的转换常量，并在单元检查计算中使用它们。

可以采用各种等效方式编写表示相同内容的单位公式。 因此，编译器将单元公式转换为一致的窗体，该窗体将负幂转换为 reciprocals，将单位分组为单个分子和分母，并 alphabetizes 分子和分母中的单位。

例如，单位公式 `kg m s^-2` 和 `m /s s * kg` 都转换为 `kg m/s^2` 。

在浮点表达式中使用度量单位。 将浮点数与关联的度量单位一起使用可以添加另一个级别的安全性，并有助于避免使用弱类型化浮点数时，公式中可能发生的单元不匹配错误。 如果您编写使用单元的浮点表达式，则表达式中的单位必须匹配。

您可以使用尖括号中的单元公式来注释文本，如下面的示例中所示。

```fsharp
1.0<cm>
55.0<miles/hour>
```

不在数字与尖括号之间放置空格;但是，可以包含文本后缀（如 `f` ），如下面的示例中所示。

```fsharp
// The f indicates single-precision floating point.
55.0f<miles/hour>
```

此类批注将文本的类型从其基元类型更改 (例如， `float`) 为维度类型（ `float<cm>` 在本例中为） `float<miles/hour>` 。 的单位批注 `<1>` 指示一个有量的量，其类型等效于不带 unit 参数的基元类型。

度量单位的类型是一个浮点型或有符号整型，以及一个额外的单位批注，用方括号括起来。 因此，当您将转换类型从 `g` (克) 转换为 `kg` (千克) 时，请按如下所示描述类型。

```fsharp
let convertg2kg (x : float<g>) = x / 1000.0<g/kg>
```

度量单位用于编译时单元检查，但不会在运行时环境中保留。 因此，它们不会影响性能。

度量单位可以应用于任何类型，而不只是浮点类型;但是，只有浮点类型、有符号的整数类型和 decimal 类型支持已标注数量。 因此，只对基元类型和包含这些基元类型的聚合使用度量单位有意义。

下面的示例阐释了度量单位的用法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6901.fs)]

下面的代码示例演示如何从有量可变浮点数转换为有尺寸的浮点值。 你只需乘以1.0，然后将维度应用于1.0。 可以将此抽象到类似的函数 `degreesFahrenheit` 。

此外，当你将已确定维度的值传递到需要有量维度浮点数的函数时，必须使用运算符取消单元或强制转换为 `float` `float` 。 在此示例中，将对的自变量进行除数， `1.0<degC>` `printf` 因为需要有 `printf` 量维度。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6902.fs)]

下面的示例会话显示了输出以及此代码的输入。

```console
Enter a temperature in degrees Fahrenheit.
90
That temperature in degrees Celsius is    32.22.
```

## <a name="using-generic-units"></a>使用通用单位

您可以编写对具有关联的度量单位的数据进行操作的泛型函数。 为此，可将一个类型与一个泛型单元一起指定为类型参数，如以下代码示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6903.fs)]

## <a name="creating-aggregate-types-with-generic-units"></a>用通用单位创建聚合类型

下面的代码演示如何创建一个聚合类型，该类型由具有泛型单元的单个浮点值组成。 这样，便可以创建适用于各种单位的单一类型。 此外，通过确保具有一组单位的泛型类型与具有不同单位集的相同泛型类型的类型不同，泛型单位会保留类型安全性。 此方法的基础是 `Measure` 特性可以应用于类型参数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6904.fs)]

## <a name="units-at-runtime"></a>运行时单位

度量单位用于静态类型检查。 在编译浮点值时，将消除度量单位，以便在运行时单元丢失。 因此，尝试实现依赖于在运行时检查单元的功能是不可能的。 例如， `ToString` 不能实现函数来打印单元。

## <a name="conversions"></a>转换

若要将具有单位的类型 (例如， `float<'u>`) 为不具有单位的类型，则可以使用标准转换函数。 例如，可以使用 `float` 将转换为 `float` 不具有单位的值，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6905.fs)]

若要将无单位的值转换为具有单位的值，可以将使用适当单位批注的1或1.0 值相乘。 但是，为了编写互操作性层，还可以使用某些显式函数将无单位值转换为具有单位的值。 它们位于 [fsharp.core](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-languageprimitives.html) 模块中。 例如，若要从无单位转换 `float` 为 `float<cm>` ，请使用 [FloatWithMeasure](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-languageprimitives.html#FloatWithMeasure)，如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet6906.fs)]

## <a name="units-of-measure-in-the-f-core-library"></a>F # 核心库中的度量单位

命名空间中提供了一个单元库 `FSharp.Data.UnitSystems.SI` 。 它在其符号形式中包含 SI 单位， (`m` 例如子命名空间中的计量) `UnitSymbols` ，以及其全名 (如 `meter` 子命名空间中的) `UnitNames` 。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
