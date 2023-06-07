---
title: 如何使用模式匹配以及 is 和 as 运算符安全地进行强制转换
description: 了解如何使用模式匹配方法将变量安全地转换为其他类型。 可以使用模式匹配以及 is 和 as 运算符来安全地转换类型。
ms.date: 09/05/2018
helpviewer_keywords:
- cast operators [C#], as and is operators
- as operator [C#]
- is operator [C#]
ms.openlocfilehash: f10ce837057cc61b84130f237a13af708849dfc5
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662961"
---
# <a name="how-to-safely-cast-by-using-pattern-matching-and-the-is-and-as-operators"></a>如何使用模式匹配以及 is 和 as 运算符安全地进行强制转换

由于是多态对象，基类类型的变量可以保存派生[类型](../programming-guide/types/index.md)。 要访问派生类型的实例成员，必须将值[强制转换](../programming-guide/types/casting-and-type-conversions.md)回派生类型。 但是，强制转换会引发 <xref:System.InvalidCastException> 风险。 C# 提供[模式匹配](../pattern-matching.md)语句，该语句只有在成功时才会有条件地执行强制转换。 C# 还提供 [is](../language-reference/operators/type-testing-and-cast.md#is-operator) 和 [as](../language-reference/operators/type-testing-and-cast.md#as-operator) 运算符来测试值是否属于特定类型。

下面的示例演示如何使用模式匹配 `is` 语句：

:::code language="csharp" source="../../../samples/snippets/csharp/how-to/safelycast/patternmatching/Program.cs" id="PatternMatchingIs":::

前面的示例演示了模式匹配语法的一些功能。 `if (a is Mammal m)` 语句将测试与初始化赋值相结合。 只有在测试成功时才会进行赋值。 变量 `m` 仅在已赋值的嵌入式 `if` 语句的范围内。 以后无法在同一方法中访问 `m`。 前面的示例还演示了如何使用 [`as` 运算符](../language-reference/operators/type-testing-and-cast.md#as-operator)将对象转换为指定类型。

也可以使用同一语法来测试[可为 null 的值类型](../language-reference/builtin-types/nullable-value-types.md)是否具有值，如以下示例所示：

:::code language="csharp" source="../../../samples/snippets/csharp/how-to/safelycast/nullablepatternmatching/Program.cs" id="PatternMatchingNullable":::

前面的示例演示了模式匹配用于转换的其他功能。 可以通过专门检查 `null` 值来测试 NULL 模式的变量。 当变量的运行时值为 `null` 时，用于检查类型的 `is` 语句始终返回 `false`。 模式匹配 `is` 语句不允许可以为 null 值的类型，如 `int?` 或 `Nullable<int>`，但你可以测试任何其他值类型。 上述示例中的 `is` 模式不局限于可为空的值类型。 也可以使用这些模式测试引用类型的变量具有值还是为 `null`。

前面的示例还演示如何在变量为其他类型的 `switch` 语句中使用类型模式。

如果需要测试变量是否为给定类型，但不将其分配给新变量，则可以对引用类型和可以为 null 的值类型使用 `is` 和 `as` 运算符。 以下代码演示如何在引入模式匹配以测试变量是否为给定类型前，使用 C# 语言中的 `is` 和 `as` 语句：

:::code language="csharp" source="../../../samples/snippets/csharp/how-to/safelycast/asandis/Program.cs" id="IsAndAs":::

正如你所看到的，将此代码与模式匹配代码进行比较，模式匹配语法通过在单个语句中结合测试和赋值来提供更强大的功能。 尽量使用模式匹配语法。
