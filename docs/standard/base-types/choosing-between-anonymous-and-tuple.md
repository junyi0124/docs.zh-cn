---
title: 在匿名类型和元组类型之间进行选择
description: 了解何时适合选择匿名类型和元组类型。
author: IEvangelist
ms.author: dapine
ms.date: 07/01/2020
ms.openlocfilehash: f8465b2f22ecfafd739355ddd35635e2eee49232
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823194"
---
# <a name="choosing-between-anonymous-and-tuple-types"></a>在匿名类型和元组类型之间进行选择

选择适当的类型需要考虑其可用性和性能，还要与其他类型相比较并进行权衡。 匿名类型自 C# 3.0 即可使用，而泛型 <xref:System.Tuple%602?displayProperty=nameWithType> 类型是随 .NET Framework 4.0 引入的。 自那时起，通过语言级别支持引入了多个新选项，例如 <xref:System.ValueTuple%602?displayProperty=nameWithType>（它名副其实，提供具有匿名类型灵活性的值类型）。 在本文中，你将了解何时应选择一种类型而不是另一种。

## <a name="usability-and-functionality"></a>可用性和功能

匿名类型是在 C# 3.0 中随语言集成查询 (LINQ) 表达式引入的。 使用 LINQ，开发人员通常会将查询结果投影到匿名类型中，这些类型保存着来自其所处理对象的几个选择属性。 请考虑以下示例，它实例化 <xref:System.DateTime> 对象的一个数组，然后循环访问它们，并将其投影到具有两个属性的匿名类型。

```csharp-interactive
var dates = new[]
{
    DateTime.UtcNow.AddHours(-1),
    DateTime.UtcNow,
    DateTime.UtcNow.AddHours(1),
};

foreach (var anonymous in
             dates.Select(
                 date => new { Formatted = $"{date:MMM dd, yyyy hh:mm zzz}", date.Ticks }))
{
    Console.WriteLine($"Ticks: {anonymous.Ticks}, formatted: {anonymous.Formatted}");
}
```

使用 [`new`](../../csharp/language-reference/operators/new-operator.md) 运算符来实例化匿名类型，从声明推断属性名称和类型。 如果同一程序集中的两个或多个匿名对象初始值设定项指定了属性序列，这些属性采用相同顺序且具有相同的名称和类型，则编译器将对象视为相同类型的实例。 它们共享同一编译器生成的类型信息。

以上 C# 代码片段投影具有两个属性的匿名类型，非常类似于以下编译器生成的 C# 类：

```csharp
internal sealed class f__AnonymousType0
{
    public string Formatted { get; }
    public long Ticks { get; }

    public f__AnonymousType0(string formatted, long ticks)
    {
        Formatted = formatted;
        Ticks = ticks;
    }
}
```

有关详细信息，请参阅[匿名类型](../../csharp/programming-guide/classes-and-structs/anonymous-types.md)。 投影到 LINQ 查询时，元组具有相同的功能，你可以选择添加属性到元组。 这些元组会贯穿查询，就像匿名类型一样。 现在请考虑以下使用 `System.Tuple<string, long>` 的示例。

```csharp-interactive
var dates = new[]
{
    DateTime.UtcNow.AddHours(-1),
    DateTime.UtcNow,
    DateTime.UtcNow.AddHours(1),
};

foreach (var tuple in
            dates.Select(
                date => new Tuple<string, long>($"{date:MMM dd, yyyy hh:mm zzz}", date.Ticks)))
{
    Console.WriteLine($"Ticks: {tuple.Item2}, formatted: {tuple.Item1}");
}
```

在 <xref:System.Tuple%602?displayProperty=nameWithType> 中，实例公开编号项属性，如 `Item1`和 `Item2`。 由于属性名称只提供序号，因此这些属性名称让人更难理解属性值的意图。 此外，`System.Tuple` 类型是引用 `class` 类型。 而 <xref:System.ValueTuple%602?displayProperty=nameWithType> 是值 `struct` 类型。 以下 C# 代码片段使用 `ValueTuple<string, long>` 投影。 在此过程中，它使用文字语法进行分配。

```csharp-interactive
var dates = new[]
{
    DateTime.UtcNow.AddHours(-1),
    DateTime.UtcNow,
    DateTime.UtcNow.AddHours(1),
};

foreach (var (formatted, ticks) in
            dates.Select(
                date => (Formatted: $"{date:MMM dd, yyyy at hh:mm zzz}", date.Ticks)))
{
    Console.WriteLine($"Ticks: {ticks}, formatted: {formatted}");
}
```

有关元组的详细信息，请参阅[元组类型（C# 参考）](../../csharp/language-reference/builtin-types/value-tuples.md)或[元组 (Visual Basic)](../../visual-basic/programming-guide/language-features/data-types/tuples.md)。

以上示例在功能上是等效的；但其可用性和基础实现存在细微差异。

## <a name="tradeoffs"></a>权衡

你可能希望始终使用 <xref:System.ValueTuple>（而不是 <xref:System.Tuple>）和匿名类型，但应进行一些权衡考虑。 <xref:System.ValueTuple> 类型是可变的，而 <xref:System.Tuple> 是只读的。 匿名类型可用于表达式树，而元组不行。 下表概述了一些关键区别。

### <a name="key-differences"></a>主要区别

| “属性”                     | 访问修饰符 | 类型     | 自定义成员名称 | 析构支持 | 表达式树支持 |
|--------------------------|-----------------|----------|----------------------|------------------------|-------------------------|
| 匿名类型          | `internal`      | `class`  | ✔️                   | ❌                     | ✔️                     |
| <xref:System.Tuple>      | `public`        | `class`  | ❌                   | ❌                     | ✔️                     |
| <xref:System.ValueTuple> | `public`        | `struct` | ✔️                   | ✔️                     | ❌                     |

### <a name="serialization"></a>序列化

选择类型时的一个重要考量是，是否需要对其进行序列化。 序列化是将对象状态转换为可保持或传输的形式的过程。 有关详细信息，请参阅[序列化](../../csharp/programming-guide/concepts/serialization/index.md)。 当序列化非常重要时，创建 `class` 或 `struct` 优于使用匿名类型或元组类型。

## <a name="performance"></a>性能

这些类型各自的性能取决于场景。 主要影响涉及分配和复制之间的权衡。 在大多数情况下，影响很小。 如果可能出现重大影响，应采取措施来通知决策者。

## <a name="conclusion"></a>结束语

开发人员在元组和匿名类型之间进行选择时，需要考虑几个因素。 一般来说，如果不使用[表达式树](../../csharp/expression-trees.md)，并且你熟悉元组语法，请选择 <xref:System.ValueTuple>，因为它们提供可灵活命名属性的值类型。 如果使用表达式树并且想要命名属性，请选择匿名类型。 否则，请使用 <xref:System.Tuple>。

## <a name="see-also"></a>请参阅

- [匿名类型](../../csharp/programming-guide/classes-and-structs/anonymous-types.md)
- [表达式树](../../csharp/expression-trees.md)
- [元组类型（C# 参考）](../../csharp/language-reference/builtin-types/value-tuples.md)
- [元组 (Visual Basic)](../../visual-basic/programming-guide/language-features/data-types/tuples.md)
- [类型设计准则](../design-guidelines/type.md)
