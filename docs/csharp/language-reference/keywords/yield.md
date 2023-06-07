---
description: yield 上下文关键字 - C# 参考
title: yield 上下文关键字 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- yield
- yield_CSharpKeyword
helpviewer_keywords:
- yield keyword [C#]
ms.assetid: 1089194f-9e53-46a2-8642-53ccbe9d414d
ms.openlocfilehash: c8caf7e34397faf9f7085d6634287cffcb37eb08
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89141876"
---
# <a name="yield-c-reference"></a>yield（C# 参考）

如果你在语句中使用 `yield` [上下文关键字](index.md#contextual-keywords)，则意味着它在其中出现的方法、运算符或 `get` 访问器是迭代器。 通过使用 `yield` 定义迭代器，可在实现自定义集合类型的 <xref:System.Collections.Generic.IEnumerator%601> 和 <xref:System.Collections.IEnumerable> 模式时无需其他显式类（保留枚举状态的类，有关示例，请参阅 <xref:System.Collections.IEnumerator>）。

下面的示例演示了 `yield` 语句的两种形式。

```csharp
yield return <expression>;
yield break;
```

## <a name="remarks"></a>备注

使用 `yield return` 语句可一次返回一个元素。

可通过使用 [foreach](foreach-in.md) 语句或 LINQ 查询来使用从迭代器方法返回的序列。 `foreach` 循环的每次迭代都会调用迭代器方法。 迭代器方法运行到 `yield return` 语句时，会返回一个 `expression`，并保留当前在代码中的位置。 下次调用迭代器函数时，将从该位置重新开始执行。

可以使用 `yield break` 语句来终止迭代。

有关迭代器的详细信息，请参阅[迭代器](../../iterators.md)。

## <a name="iterator-methods-and-get-accessors"></a>迭代器方法和 get 访问器

迭代器的声明必须满足以下要求：

- 返回类型必须为 <xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator> 或 <xref:System.Collections.Generic.IEnumerator%601>。

- 声明不能有任何 [in](in-parameter-modifier.md)、[ref](ref.md) 或 [out](out-parameter-modifier.md) 参数。

返回 `yield` 或 <xref:System.Collections.IEnumerable> 的迭代器的 <xref:System.Collections.IEnumerator> 类型为 `object`。  如果迭代器返回 <xref:System.Collections.Generic.IEnumerable%601> 或 <xref:System.Collections.Generic.IEnumerator%601>，则必须将 `yield return` 语句中的表达式类型隐式转换为泛型类型参数。

以下情形中不能包含 `yield return` 或 `yield break` 语句：

- [Lambda 表达式](../operators/lambda-expressions.md)和[匿名方法](../operators/delegate-operator.md)。

- 包含不安全的块的方法。 有关详细信息，请参阅 [unsafe](unsafe.md)。

## <a name="exception-handling"></a>异常处理

不能将 `yield return` 语句置于 try-catch 块中。 可将 `yield return` 语句置于 try-finally 语句的 try 块中。

可将 `yield break` 语句置于 try 块或 catch 块中，但不能将其置于 finally 块中。

如果 `foreach` 主体（在迭代器方法之外）引发异常，则将执行迭代器方法中的 `finally` 块。

## <a name="technical-implementation"></a>技术实现

以下代码从迭代器方法返回 `IEnumerable<string>`，然后遍历其元素。

```csharp
IEnumerable<string> elements = MyIteratorMethod();
foreach (string element in elements)
{
   ...
}
```

调用 `MyIteratorMethod` 并不执行该方法的主体。 相反，该调用会将 `IEnumerable<string>` 返回到 `elements` 变量中。

在 `foreach` 循环迭代时，将为 <xref:System.Collections.IEnumerator.MoveNext%2A> 调用 `elements` 方法。 此调用将执行 `MyIteratorMethod` 的主体，直至到达下一个 `yield return` 语句。 `yield return` 语句返回的表达式不仅决定了循环体使用的 `element` 变量值，还决定了 `elements` 的 <xref:System.Collections.Generic.IEnumerator%601.Current%2A> 属性（它是 `IEnumerable<string>`）。

在 `foreach` 循环的每个后续迭代中，迭代器主体的执行将从它暂停的位置继续，直至到达 `yield return` 语句后才会停止。 在到达迭代器方法的结尾或 `foreach` 语句时，`yield break` 循环便已完成。

## <a name="example"></a>示例

下面的示例包含一个位于 `yield return` 循环内的 `for` 语句。 `Main` 方法中的 `foreach` 语句体的每次迭代都会创建对 `Power` 迭代器函数的调用。 对迭代器函数的每个调用将继续到 `yield return` 语句的下一次执行（在 `for` 循环的下一次迭代期间发生）。

迭代器方法的返回类型是 <xref:System.Collections.IEnumerable>（一种迭代器接口类型）。 当调用迭代器方法时，它将返回一个包含数字幂的可枚举对象。

[!code-csharp[csrefKeywordsContextual#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#5)]

## <a name="example"></a>示例

下面的示例演示一个作为迭代器的 `get` 访问器。 在该示例中，每个 `yield return` 语句返回一个用户定义的类的实例。

[!code-csharp[csrefKeywordsContextual#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#21)]

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [foreach, in](foreach-in.md)
- [迭代器](../../iterators.md)
