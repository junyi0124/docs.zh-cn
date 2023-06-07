---
description: foreach，in（C# 参考）
title: C# foreach 语句
ms.date: 09/18/2020
f1_keywords:
- foreach
- foreach_CSharpKeyword
helpviewer_keywords:
- foreach keyword [C#]
- foreach statement [C#]
- in keyword [C#]
ms.assetid: 5a9c5ddc-5fd3-457a-9bb6-9abffcd874ec
ms.openlocfilehash: ea8a6f86595348a32b707caf9782f84147fefc87
ms.sourcegitcommit: 43ed174f085840ca18a791dc89fe833174da766d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/21/2020
ms.locfileid: "90828885"
---
# <a name="foreach-in-c-reference"></a>foreach，in（C# 参考）

`foreach` 语句为类型实例中实现 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> 或 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 接口的每个元素执行语句或语句块，如以下示例所示：

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="1" interactive="try-dotnet-method" :::

`foreach` 语句并不限于这些类型。 可以将其与满足以下条件的任何类型的实例一起使用：

- 类型具有公共无参数 `GetEnumerator` 方法，其返回类型为类、结构或接口类型。 从 C# 9.0 开始，`GetEnumerator` 方法可以是类型的[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)。
- `GetEnumerator` 方法的返回类型具有公共 `Current` 属性和公共无参数 `MoveNext` 方法（其返回类型为 <xref:System.Boolean>）。

下面的示例使用 `foreach` 语句，其中包含 <xref:System.Span%601?displayProperty=nameWithType> 类型的实例，该实例不实现任何接口：

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="2" :::

从 C# 7.3 开始，如果枚举器的 `Current` 属性返回[引用返回值](ref.md#reference-return-values)（`ref T`，其中 `T` 为集合元素类型），就可以使用 `ref` 或 `ref readonly` 修饰符来声明迭代变量，如下面的示例所示：

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="RefSpan" :::

从 C# 8.0 开始，可以使用 `await foreach` 语句来使用异步数据流，即实现 <xref:System.Collections.Generic.IAsyncEnumerable%601> 接口的集合类型。 异步检索下一个元素时，可能会挂起循环的每次迭代。 下面的示例演示如何使用 `await foreach` 语句：

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="AwaitForeach" :::

默认情况下，在捕获的上下文中处理流元素。 如果要禁用上下文捕获，请使用 <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> 扩展方法。 有关同步上下文并捕获当前上下文的详细信息，请参阅[使用基于任务的异步模式](../../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)。 有关异步流的详细信息，请参阅 [C# 8.0 新增功能](../../whats-new/csharp-8.md)一文中的[异步流](../../whats-new/csharp-8.md#asynchronous-streams)部分。

在 `foreach` 语句块中的任何点上，可以使用 [break](break.md) 语句中断循环，或者可以使用 [continue](continue.md) 语句继续执行到循环中的下一次迭代。 还可以使用 [goto](goto.md)、[return](return.md) 或 [throw](throw.md) 语句退出 `foreach` 循环。

如果 `foreach` 语句应用为 `null`，则会引发 <xref:System.NullReferenceException>。 如果 `foreach` 语句的源集合为空，则 `foreach` 循环的正文不会被执行，而是被跳过。

## <a name="type-of-an-iteration-variable"></a>迭代变量的类型

可以使用 `var` 关键字让编译器推断 `foreach` 语句中迭代变量的类型，如以下代码所示：

```csharp
foreach (var item in collection) { }
```

还可以显式指定迭代变量的类型，如以下代码所示：

```csharp
IEnumerable<T> collection = new T[5];
foreach (V item in collection) { }
```

在上述窗体中，集合元素的类型 `T` 必须可隐式或显式地转换为迭代变量的类型 `V`。 如果从 `T` 到 `V` 的显式转换在运行时失败，`foreach` 语句将引发 <xref:System.InvalidCastException>。 例如，如果 `T` 是非密封类类型，则 `V` 可以是任何接口类型，甚至可以是 `T` 未实现的接口类型。 在运行时，集合元素的类型可以是从 `T` 派生并且实际实现 `V` 的类型。 如果不是这样，则会引发 <xref:System.InvalidCastException>。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [foreach 语句](~/_csharplang/spec/statements.md#the-foreach-statement)部分。

有关 C# 8.0 及更高版本中添加的功能的详细信息，请参阅以下功能建议说明：

- [异步流 (C# 8.0)](~/_csharplang/proposals/csharp-8.0/async-streams.md)
- [扩展 `GetEnumerator` 支持 `foreach` 循环 (C# 9.0)](~/_csharplang/proposals/csharp-9.0/extension-getenumerator.md)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 关键字](index.md)
- [对数组使用 foreach](../../programming-guide/arrays/using-foreach-with-arrays.md)
- [for 语句](for.md)
