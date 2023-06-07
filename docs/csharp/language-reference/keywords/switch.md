---
description: switch（C# 参考）
title: C# switch 语句
ms.date: 04/09/2019
f1_keywords:
- switch_CSharpKeyword
- switch
- case
- case_CSharpKeyword
- defaultcase_CSharpKeyword
helpviewer_keywords:
- switch statement [C#]
- switch keyword [C#]
- case statement [C#]
- default keyword [C#]
ms.assetid: 44bae8b8-8841-4d85-826b-8a94277daecb
ms.openlocfilehash: 2d12091f3c3f10048a98f5f0ecf6a381087faf41
ms.sourcegitcommit: e078b7540a8293ca1b604c9c0da1ff1506f0170b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "91997690"
---
# <a name="switch-c-reference"></a>switch（C# 参考）

本文介绍 `switch` 语句。 有关 `switch` 表达式（在 C# 8.0 中引入）的信息，请参阅 [表达式和运算符](../operators/index.md)部分中有关 [`switch` 表达式](../operators/switch-expression.md)的文章。

`switch` 是一个选择语句，它根据与匹配表达式匹配的模式，从候选列表中选择单个开关部分进行执行。

[!code-csharp[switch#1](~/samples/snippets/csharp/language-reference/keywords/switch/switch1.cs#1)]

如果针对 3 个或更多条件测试单个表达式，`switch` 语句通常用作 [if-else](if-else.md) 构造的替代项。 例如，以下 `switch` 语句确定类型为 `Color` 的变量是否具有三个值之一：

[!code-csharp[switch#3](~/samples/snippets/csharp/language-reference/keywords/switch/switch3.cs#1)]

它相当于使用 `if`-`else` 构造的以下示例。

[!code-csharp[switch#3a](~/samples/snippets/csharp/language-reference/keywords/switch/switch3a.cs#1)]

## <a name="the-match-expression"></a>匹配表达式

匹配表达式提供与 `case` 标签中的模式相匹配的值。 语法为：

```csharp
   switch (expr)
```

在 C# 6 及更低版本中，匹配表达式必须是返回以下类型值的表达式：

- [字符型](../builtin-types/char.md)。
- [字符串](../builtin-types/reference-types.md)。
- [bool](../builtin-types/bool.md)。
- [整数](../builtin-types/integral-numeric-types.md)值，例如 `int` 或 `long`。
- [枚举](../builtin-types/enum.md)值。

从 C# 7.0 开始，匹配表达式可以是任何非 null 表达式。

## <a name="the-switch-section"></a>开关部分

`switch` 语句包含一个或多个开关部分。 每个 switch 部分包含一个或多个 case 标签（case 或 default 标签），后接一个或多个语句。 `switch` 语句最多可包含一个置于任何 switch 部分中的 default 标签。 以下示例显示了一个简单的 `switch` 语句，该语句包含三个 switch 部分，每个部分包含两个语句。 第二个 switch 部分包含 `case 2:` 和 `case 3:` 标签。

`switch` 语句中可以包含任意数量的开关部分，每个开关部分可以具有一个或多个 case 标签，如以下示例所示。 但是，任何两个 case 标签不可包含相同的表达式。

[!code-csharp[switch#2](~/samples/snippets/csharp/language-reference/keywords/switch/switch2.cs#1)]

switch 语句执行中只有一个开关部分。 C# 禁止从一个 switch 部分继续执行到下一个 switch 部分。 因此，下面的代码生成编译器错误 CS0163：“控件不能从一个 case 标签 (\<case label>) 贯穿到另一个 case 标签。”

```csharp
switch (caseSwitch)
{
    // The following switch section causes an error.
    case 1:
        Console.WriteLine("Case 1...");
        // Add a break or other jump statement here.
    case 2:
        Console.WriteLine("... and/or Case 2");
        break;
}
```

通常通过使用 [break](break.md)、[goto](goto.md) 或 [return](return.md) 语句显式退出开关部分来满足此要求。 不过，以下代码也有效，因为它确保程序控制权无法贯穿到 `default` switch 部分。

[!code-csharp[switch#4](~/samples/snippets/csharp/language-reference/keywords/switch/switch4.cs#1)]

在 case 标签与匹配表达式匹配的开关部分中执行语句列表时，先执行第一个语句，再执行整个语句列表，通常执行到跳转语句（如 `break`、`goto case`、`goto label`、`return` 或 `throw`）为止。 此时，控件在 `switch` 语句之外进行传输或传输到另一个 case 标签。 如果使用的是 `goto` 语句，必须将控制权移交给常量标签。 此限制是必要的，因为尝试将控制权移交给非常数标签可能会产生不良的副作用，如将控制权移交给代码中的意外位置，或创建无限循环。

## <a name="case-labels"></a>Case 标签

每个 case 标签指定一个模式与匹配表达式（前面示例中的 `caseSwitch` 变量）进行比较。 如果它们匹配，则将控件传输到包含**首次**匹配 case 标签的开关部分。 如果 case 标签模式与匹配表达式不匹配，控制权会转让给带 `default` case 标签的部分（若有）。 如果没有 `default` case，将不会执行任何 switch 部分中的语句，并且会将控制权转让到 `switch` 语句之外。

有关 `switch` 语句和模式匹配的信息，请参阅使用 `switch` 语句的 [模式匹配](#pattern-matching-with-the-switch-statement)部分。

因为 C# 6 仅支持常量模式且禁止重复常量值，所以 case 标签定义了互斥值，而且只能有一个模式与匹配表达式匹配。 因此，`case` 语句显示的顺序并不重要。

然而，在 C# 7.0 中，因为支持其他模式，所以 case 标签不需要定义互斥值，并且多个模式可以与匹配表达式相匹配。 因为仅执行包含匹配模式的首次开关部分中的语句，所以 `case` 语句显示的顺序很重要。 如果 C# 检测到开关部分的 case 语句或语句等效于或是先前语句的子集，它将生成编译错误 CS8120：“开关 case 已由先前 case 处理。”

以下示例说明了使用各种非互斥模式的 `switch` 语句。 如果你移动 `case 0:` switch 部分，使之不再是 `switch` 语句中的第一部分，C# 会生成编译器错误，因为值为零的整数是所有整数的子集（由 `case int val` 语句定义的模式）。

[!code-csharp[switch#5](~/samples/snippets/csharp/language-reference/keywords/switch/switch5.cs#1)]

你可以通过以下两种方法之一更正此问题并消除编译器警告：

- 更改开关部分的顺序。

- 在 `case` 标签中使用 [when clause](#the-case-statement-and-the-when-clause) 子句。

## <a name="the-default-case"></a>`default` case

如果匹配表达式与其他任何 `case` 标签都不匹配，`default` case 指定要执行的 switch 部分。 如果没有 `default` case，且匹配表达式与其他任何 `case` 标签都不匹配，程序流就会贯穿 `switch` 语句。

`default` case 可以在 `switch` 语句中以任何顺序显示。 无论它在源代码中的顺序如何，始终都将在计算所有 `case` 标签后，最后计算它。

## <a name="pattern-matching-with-the-switch-statement"></a>使用 `switch` 语句的模式匹配

每个 `case` 语句定义一个模式，如果它与匹配表达式相匹配，则会导致执行其包含的开关部分。 所有版本的 C# 都支持常量模式。 其余模式从 C# 7.0 开始支持。

### <a name="constant-pattern"></a>常量模式

常量模式测试匹配表达式是否等于指定常量。 语法为：

```csharp
   case constant:
```

其中 *constant* 是要测试的值。 *constant* 可以是以下任何常数表达式：

- [bool](../builtin-types/bool.md) 文本：`true` 或 `false`。
- 任何[整型](../builtin-types/integral-numeric-types.md)常数，例如 `int`、`long` 或 `byte`。
- 已声明 `const` 变量的名称。
- 一个枚举常量。
- [字符型](../builtin-types/char.md)文本。
- [字符串](../builtin-types/reference-types.md)文本。

常数表达式的计算方式如下：

- 如果 *expr* 和 *constant* 均为整型类型，则 C# 相等运算符确定表示式是否返回 `true`（即，是否为 `expr == constant`）。

- 否则，由对静态 [Object.Equals(expr, constant)](xref:System.Object.Equals(System.Object,System.Object)) 方法的调用确定表达式的值。

以下示例使用常量模式来确定特定日期是否为周末、工作周的第一天、工作周的最后一天或工作周的中间日期。 它根据 <xref:System.DayOfWeek> 枚举的成员计算当前日期的 <xref:System.DateTime.DayOfWeek?displayProperty=nameWithType> 属性。

[!code-csharp[switch#7](~/samples/snippets/csharp/language-reference/keywords/switch/const-pattern.cs#1)]

以下示例使用常量模式在模拟自动咖啡机的控制台应用程序中处理用户输入。

[!code-csharp[switch#6](~/samples/snippets/csharp/language-reference/keywords/switch/switch6.cs)]

### <a name="type-pattern"></a>类型模式

类型模式可启用简洁类型计算和转换。 使用 `switch` 语句执行模式匹配时，会测试表达式是否可转换为指定类型，如果可以，则将其转换为该类型的一个变量。 语法为：

```csharp
   case type varname
```

其中 *type* 是 *expr* 结果要转换到的类型的名称，*varname* 是 *expr* 结果要转换到的对象（如果匹配成功）。 自 C# 7.1 起，expr 的编译时类型可能为泛型类型参数。

如果以下任一条件成立，则 `case` 表达式为 `true`：

- *expr* 是与 *type* 具有相同类型的一个实例。

- *expr* 是派生自 *type* 的类型的一个实例。 换言之，*expr* 结果可以向上转换为 *type* 的一个实例。

- *expr* 具有属于 *type* 的一个基类的编译时类型，*expr* 还具有属于 *type* 或派生自 *type* 的运行时类型。 变量的编译时类型是其类型声明中定义的变量类型。 变量的运行时类型是分配给该变量的实例类型。

- *expr* 是实现 *type* 接口的类型的一个实例。

如果 case 表达式为 true，将会明确分配 *varname*，并且仅在开关部分中具有本地作用域。

请注意，`null` 与任何类型都不匹配。 若要匹配 `null`，请使用以下 `case` 标签：

```csharp
case null:
```

以下示例使用类型模式来提供有关各种集合类型的信息。

[!code-csharp[type-pattern#1](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern.cs#1)]

可以创建泛型方法，使用集合的类型作为类型参数（而不是使用 `object`），如下面的代码所示：

[!code-csharp[type-pattern#3](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern3.cs#1)]

泛型版本与第一个示例有两点不同。 首先，无法使用 `null` case。 无法使用任何常量 case 是因为，编译器无法将任意类型 `T` 转换为除 `object` 之外的任何类型。 曾经的 `default` case 现在测试是否有非 null `object`。 也就是说，`default` case 测试只针对 `null`。

如果没有模式匹配，则可能按以下方式编写此代码。 使用类型模式匹配可消除测试转换结果是否为 `null` 或执行重复转换的必要，从而生成更紧凑易读的代码。

[!code-csharp[type-pattern2#1](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern2.cs#1)]

## <a name="the-case-statement-and-the-when-clause"></a>`case` 语句和 `when` 子句

从 C# 7.0 开始，因为 case 语句不需要互相排斥，因此可以添加 `when` 子句来指定必须满足的附加条件使 case 语句计算为 true。 `when` 子句可以是返回布尔值的任何表达式。

下面的示例定义了 `Shape` 基类、从 `Shape` 派生的 `Rectangle` 类以及从 `Rectangle` 派生的 `Square` 类。 它使用 `when` 子句，以确保 `ShowShapeInfo` 将已分配相等长度和宽度的 `Rectangle` 对象视为 `Square`（即使它尚未实例化为 `Square` 对象）。 此方法不会尝试显示值为 `null` 的对象或面积为零的形状的相关信息。

[!code-csharp[when-clause#1](~/samples/snippets/csharp/language-reference/keywords/switch/when-clause.cs#1)]

请注意，不会执行尝试测试 `Shape` 对象是否为 `null` 的示例中的 `when` 子句。 测试是否为 `null` 的正确类型模式是 `case null:`。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的 [switch 语句](~/_csharplang/spec/statements.md#the-switch-statement)。 该语言规范是 C# 语法和用法的权威资料。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [if-else](if-else.md)
- [模式匹配](../../pattern-matching.md)
