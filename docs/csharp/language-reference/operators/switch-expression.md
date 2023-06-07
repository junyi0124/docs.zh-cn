---
title: switch 表达式 - C# 参考
description: 了解如何将 C# switch 表达式用于模式匹配和其他数据自检
ms.date: 03/19/2020
ms.openlocfilehash: 2249afc1ff1cc81e9ad423d910ebb95df8c787d4
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916657"
---
# <a name="switch-expression-c-reference"></a>switch 表达式（C# 参考）

本文介绍 C# 8.0 中引入的 `switch` 表达式。 有关 `switch` 语句的详细信息，请参阅[语句](../keywords/index.md)部分中关于 [`switch` 语句](../keywords/switch.md)的文章。

## <a name="basic-example"></a>基本示例

`switch` 表达式在表达式上下文中提供与 `switch` 类似的语义。 当 switch arm 生成值时，它提供简洁的语法。 下面的示例显示了 switch 表达式的结构。 它将联机地图中表示视觉方向的 `enum` 中的值转换为相应的基本方位：

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetBasicStructure":::

前面的示例展示了 switch 表达式的基本元素：

- *范围表达式*：前面的示例使用变量 `direction` 作为范围表达式。
- *switch expression arm*：每个 switch expression arm 都包含一个模式、  一个可选的 case guard  、`=>` 标记和一个表达式  。

switch 表达式  的结果是第一个 switch expression arm  的表达式的值，该 switch expression arm 的模式  与范围表达式  匹配，并且它的 case guard  （如果存在）求值为 `true`。 `=>` 标记右侧的表达式不能是表达式语句。 

switch expression arm  按文本顺序求值。 如果无法选择较低的 switch expression arm  ，编译器会发出错误，因为较高的 switch expression arm  匹配其所有值。

## <a name="patterns-and-case-guards"></a>模式和 case guard

switch expression arm 支持许多模式。 前面的示例使用常量模式。 常量模式将范围表达式与一个值进行比较。 该值必须是编译时常量。 类型模式  将范围表达式与已知类型进行比较。 下面的示例从序列中检索第三个元素。 它使用基于序列类型的不同方法：

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetTypePattern":::

模式可以是递归模式，其中模式会测试一个类型，如果该类型匹配，则该模式将匹配范围表达式上的一个或多个属性值。 可以使用递归模式来扩展前面的示例。 为包含少于 3 个元素的数组添加 switch expression arm。 下面的示例演示了递归模式：

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetRecursivePattern":::

递归模式可以检查范围表达式的属性，但不能执行任意代码。 你可以使用 `when` 子句中指定的 case guard  为其他序列类型提供类似的检查：

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetGuardCase":::

最后，可以添加 `_` 模式和 `null` 模式，以捕获不由任何其他 switch expression arm 处理的参数。 这会使 switch 表达式穷尽  ，这意味着将处理范围表达式的任何可能的值。 下面的示例添加了这些 expression arm：

:::code language="csharp" source="snippets/shared/SwitchExpressions.cs" id="SnippetExhaustive":::

前面的示例添加了 `null` 模式，并将 `IEnumerable<T>` 类型模式更改为 `_` 模式。 `null` 模式提供 null 检查作为 switch expression arm。 该 arm 的表达式引发 <xref:System.ArgumentNullException>。 `_` 模式与先前的 arm 未匹配的所有输入相匹配。 它必须在 `null` 检查之后执行，否则将与 `null` 输入匹配。

可以参阅 C# 语言规范建议，了解有关[递归模式](~/_csharplang/proposals/csharp-8.0/patterns.md#switch-expression)的详细信息。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [模式匹配](../../pattern-matching.md)
