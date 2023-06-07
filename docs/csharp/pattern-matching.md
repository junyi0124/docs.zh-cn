---
title: 模式匹配 - C# 指南
description: 了解 C# 中的模式匹配表达式
ms.date: 04/10/2019
ms.technology: csharp-fundamentals
ms.assetid: 1e575c32-2e2b-4425-9dca-7d118f3ed15b
ms.openlocfilehash: aabd8afe9a94022557275adb313ca3bf95da7548
ms.sourcegitcommit: b4a46f6d7ebf44c0035627d00924164bcae2db30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91438026"
---
# <a name="pattern-matching"></a>模式匹配

模式可测试值是否具有特定形状  ，并且可以在值具有匹配形状时从值提取  信息。 模式匹配为当前已使用的算法提供了更简洁的语法。 你已使用现有语法创建了模式匹配算法。 编写了测试值的 `if` 或 `switch` 语句。 随后，在这些语句匹配时，可从该值提取并使用信息。 新的语法元素是你已熟悉的语句的扩展：`is` 和 `switch`。 这些新扩展将测试值与提取该信息合并在一起。

本文将介绍新语法，以展示它是如何实现可读的简洁代码的。 模式匹配可实现数据与代码分开的惯例，这与面向对象的设计不同，后者紧密耦合数据和控制它们的方法。

为了说明这些新的惯例，我们使用通过模式匹配语句表示几何形状的结构。 你可能熟悉如何通过生成类层次结构和创建[虚方法和重写方法](methods.md#inherited)，根据对象的运行时类型来自定义对象行为。

对于未在类层次结构中进行结构化的数据，无法使用这些技术。 当数据和方法分开时，需要其他工具。 新的模式匹配  构造可实现更简洁的语法，以基于这些数据的任何条件来检查数据和操作控制流。 你已编写了测试变量值的 `if` 语句和 `switch`。 你编写了测试变量类型的 `is` 语句。 模式匹配  为这些语句添加了新功能。

在本文中，你将生成计算不同几何形状的面积的方法。 但无需求助于面向对象的技术，也无需为不同的形状生成类层次结构。
你会改用模式匹配  。
执行此示例时，将此代码与它作为对象层次结构来构造的方式进行对比。 如果必须查询和控制的数据不是类层次结构，模式匹配可实现完善设计。

我们不会从抽象形状定义以及添加不同的特定形状类开始，而是从每个几何形状的简单纯数据定义开始：

[!code-csharp[ShapeDefinitions](../../samples/snippets/csharp/PatternMatching/Shapes.cs#01_ShapeDefinitions "Shape definitions")]

在这些结构中，我们编写一个方法来计算某种形状的面积。

## <a name="the-is-type-pattern-expression"></a>`is` 类型模式表达式

在 C# 7.0 之前，需要在一系列 `if` 和 `is` 语句中测试每种类型：

[!code-csharp[ClassicIsExpression](../../samples/snippets/csharp/PatternMatching/GeometricUtilities.cs#02_ClassicIsExpression "Classic type pattern using is")]

上面的代码是类型模式  的经典表达式：测试变量以确定其类型并基于该类型执行不同操作。

通过使用 `is` 表达式的扩展在测试成功时对变量赋值，此代码变得更加简单：

[!code-csharp[IsPatternExpression](../../samples/snippets/csharp/PatternMatching/GeometricUtilities.cs#03_IsPatternExpression "is pattern expression")]

在此更新的版本中，`is` 表达式会测试变量并将它分配给具有正确类型的新变量。 另请注意，此版本包含 `Rectangle` 类型（即 `struct`）。 新的 `is` 表达式使用值类型以及引用类型。

模式匹配表达式的语言规则可帮助避免误用匹配表达式的结果。 在上面的示例中，仅当相应的模式匹配表达式具有 `true` 结果时，变量 `s`、`c` 和 `r` 才处于范围内并进行明确赋值。 如果尝试在另一个位置中使用任一变量，则代码会生成编译器错误。

让我们从范围开始，来详细了解一下这两个规则。 变量 `c` 只处于第一个 `if` 语句的 `else` 分支的范围内。 变量 `s` 处于方法 `ComputeAreaModernIs` 的范围内。 这是因为 `if` 语句的每个分支都为变量建立单独的范围。 但 `if` 语句本身不会。 这意味着在 `if` 语句中声明的变量与 `if` 语句（即本例中的方法）所在的范围相同。 此行为并不是模式匹配专有，而是变量作用域以及 `if` 和 `else` 语句的已定义行为。

由于在 true 时进行明确赋值这一机制，当相应 `if` 语句为 true 时，会对变量 `c` 和 `s` 赋值。

> [!TIP]
> 本主题中的示例使用建议构造，其中的模式匹配 `is` 表达式在 `if` 语句的 `true` 分支中对匹配变量明确赋值。
> 可以通过编写 `if (!(shape is Square s))` 来反转逻辑，变量 `s` 会只在 `false` 分支中进行明确赋值。 虽然这是有效的 C#，不过不建议使用它，因为遵循该逻辑更令人困惑。

这些规则意味着，如果不符合模式，不可能意外访问模式匹配表达式的结果。

## <a name="using-pattern-matching-switch-statements"></a>使用模式匹配 `switch` 语句

随着时间推移，可能需要支持其他形状类型。 随着要测试的条件数增加，你会发现使用 `is` 模式匹配表达式可能会变得很不方便。 除了需要对要检查的每种类型使用 `if` 语句，`is` 表达式仅限为测试输入是否与单个类型匹配。 在这种情况下，会发现 `switch` 模式匹配表达式会是更好的选择。

传统 `switch` 语句是模式表达式：支持常量模式。
可以将变量与 `case` 语句中使用的任何常量进行比较：

[!code-csharp[ClassicSwitch](../../samples/snippets/csharp/PatternMatching/GeometricUtilities.cs#04_ClassicSwitch "Classic switch statement")]

`switch` 语句支持的唯一模式是常量模式。 它进一步限制为数字类型和 `string` 类型。
这些限制已移除，现在可以使用类型模式编写 `switch` 语句：

[!code-csharp[Switch Type Pattern](../../samples/snippets/csharp/PatternMatching/GeometricUtilities.cs#05_SwitchTypePattern "Compute with `switch` expression")]

模式匹配 `switch` 语句使用的语法对于使用过传统 C 样式 `switch` 语句的开发人员会比较熟悉。 会计算每个 `case`，并执行与输入变量匹配的条件下的代码。 代码执行无法从一个 case 表达式“贯穿”到下一个表达式；`case` 语句的语法要求，每个 `case` 都必须以 `break`、`return` 或 `goto` 结尾。

> [!NOTE]
> 用于跳转到其他标签的 `goto` 语句仅对常量模式（经典 switch 语句）有效。

有一些用于控制 `switch` 语句的重要新规则。 针对 `switch` 表达式中的变量类型的限制已移除。
任何类型（如此示例中的 `object`）都可以使用。 Case 表达式不再限制为常量值。 移除该限制意味着对 `switch` 部分重新排序可能会更改程序的行为。

限制为常量值时，最多只有一个 `case` 标签可以与 `switch` 表达式的值匹配。 将该规则与每个 `switch` 部分不得贯穿到下一个部分的规则相结合，这样 `switch` 部分便可以按任何顺序重新排列而不会影响行为。
现在，使用更加通用的 `switch` 表达式时，每个部分的顺序都非常重要。 `switch` 表达式按文本顺序进行计算。 执行会转移到与 `switch` 表达式匹配的第一个 `switch` 匹配。  
仅当其他任何 case 标签都不匹配时，才会执行 `default` case。 `default` case 最后一个进行计算（无论文本顺序如何）。 如果没有任何 `default` case，且其他 `case` 语句都不匹配，便会在 `switch` 语句后的语句处继续代码执行。 不会执行任何 `case` 标签代码。

## <a name="when-clauses-in-case-expressions"></a>`case` 表达式中的 `when` 语句

可以通过对 `case` 标签使用 `when` 子句，为面积为 0 的那些形状创建特殊 case。 边长为 0 的正方形，或半径为 0 的圆形的面积为 0。 可通过对 `case` 标签使用 `when` 语句来指定该条件：  

[!code-csharp[ComputeDegenerateShapes](../../samples/snippets/csharp/PatternMatching/GeometricUtilities.cs#07_ComputeDegenerateShapes "Compute shapes with 0 area")]

此更改演示了有关新语法的几个要点。 首先，多个 `case` 标签可以应用于一个 `switch` 部分。 在其中任何标签为 `true` 时执行语句块。 在此例中，如果 `switch` 表达式是面积为 0 的圆形或正方形，则方法返回常量 0。

此示例为第一个 `switch` 块在两个 `case` 标签中引入了两个不同的变量。 请注意，此 `switch` 块中的语句未使用变量 `c`（用于圆形）或 `s`（用于方形）。
这两个变量在此 `switch` 块中都未明确赋值。
如果有任一 case 匹配，则对其中一个变量明确赋值。
不过，无法判断在编译时对哪个  变量进行赋值，因为任一 case 在运行时都可能会匹配。 因此，大多数情况下，如果将多个 `case` 标签用于同一个块，不会在 `case` 语句中引入新变量，或只在 `when` 子句中使用变量。

添加了这些面积为 0 的形状之后，我们再添加一些形状类型：一个矩形和一个三角形：

[!code-csharp[AddRectangleAndTriangle](../../samples/snippets/csharp/PatternMatching/GeometricUtilities.cs#09_AddRectangleAndTriangle "Add rectangle and triangle")]

 这组更改为每个新形状的退化情况、标签和块添加 `case` 标签。

最后，可以添加 `null` case 来确保参数不是 `null`：

[!code-csharp[NullCase](../../samples/snippets/csharp/PatternMatching/GeometricUtilities.cs#10_NullCase "Add null case")]

`null` 模式的特殊行为十分有趣，因为模式中的常量 `null` 没有类型，但可以转换为任何引用类型或可以为 null 的值类型。 语言定义 `null` 值不会匹配任何类型模式（无论变量的编译时类型如何），而不是将 `null` 转换为任何类型。 此行为使新的基于 `switch` 的类型模式与 `is` 语句保持一致：如果检查的值为 `null`，`is` 语句始终返回 `false`。 它也更简单；也就是说，在检查类型后，便无需执行其他 null 检查。 可以从上面示例中的任何 case 块都没有 null 检查得知：此类检查不是必需的，因为与类型模式匹配即可保证非 null 值。

## <a name="var-declarations-in-case-expressions"></a>`case` 表达式中的 `var` 声明

引入 `var` 作为一种匹配表达式也为模式匹配引入了新规则。

第一条规则是 `var` 声明遵循正常的类型推理规则：推理出类型是 switch 表达式的静态类型。 根据此规则，类型始终匹配。

第二个规则是，`var` 声明没有其他类型模式表达式中包含的 null 检查。 也就是说，变量可为 NULL，只有在这种情况下，才必须执行 NULL 检查。

这两个规则表示，在许多情况下，`case` 表达式中的 `var` 声明匹配与 `default` 表达式相同的条件。
因为任何非默认事例都优先于 `default` 事例，所以永远不会执行 `default` 事例。

> [!NOTE]
> 如果 `default` 事例已编写但永远不会执行，编译器也不会发出警告。 这与在已列出所有可能事例的情况下的当前 `switch` 语句行为一致。

第三个规则引入了 `var` 事例可能适用的用途。 假设要进行模式匹配，其中输入是字符串，且要搜索已知命令值。 可以编写如下代码：

[!code-csharp[VarCaseExpression](../../samples/snippets/csharp/PatternMatching/Program.cs#VarCaseExpression "use a var case expression to filter white space")]

`var` 事例匹配 `null`、空字符串或任何仅包含空白符的字符串。 请注意，为了确保不会意外抛出 <xref:System.NullReferenceException>，上面的代码使用 `?.` 运算符。 `default` case 处理此命令分析程序不理解的其他任何字符串值。

在这个例子中，建议使用 `var` 事例表达式，而不是 `default` 表达式。

## <a name="conclusions"></a>结论

使用模式匹配构造  ，可以轻松管理继承层次结构未关联的不同变量和类型之间的控制流。 还可以控制逻辑以使用对变量进行测试的任何条件。 这可实现在生成分布程度更高的应用程序（其中的数据和操作这些数据的方法是分开的）时更常需要的模式和惯例。 你会注意到，此示例中使用的形状结构不包含任何方法，只包含只读属性。
模式匹配适用于任何数据类型。 你编写了检查对象并基于这些条件进行控制流决策的表达式。

将此示例中的代码与通过为抽象 `Shape` 和特定派生类（各自具有自己的用于计算面积的虚方法实现）创建类层次结构而产生的设计进行比较。 你通常会发现，在处理数据并希望将数据存储问题与行为问题分开时，模式匹配表达式可能是非常有用的工具。

## <a name="see-also"></a>请参阅

- [教程：使用模式匹配来构建类型驱动和数据驱动的算法](tutorials/pattern-matching.md)
