---
title: 强类型委托
description: 了解创建需要委托的功能时，如何使用泛型委托类型声明自定义类型。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 564a683d-352b-4e57-8bac-b466529daf6b
ms.openlocfilehash: 798e8b597389bc99d10e587ec417a4e717f28abc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146198"
---
# <a name="strongly-typed-delegates"></a>强类型委托

[上一页](delegate-class.md)

在上一篇文章中，使用 `delegate` 关键字创建了特定委托类型。

抽象的 Delegate 类为松散耦合和调用提供基础结构。 通过包含和实施添加到委托对象的调用列表的方法的类型安全性，具体的委托类型将变得更加有用。 使用 `delegate` 关键字并定义具体的委托类型时，编译器将生成这些方法。

实际上，无论何时需要不同的方法签名，这都会创建新的委托类型。 一段时间后此操作可能变得繁琐。 每个新功能都需要新的委托类型。

幸运的是，没有必要这样做。 .NET Core 框架包含几个在需要委托类型时可重用的类型。 这些是[泛型](programming-guide/generics/index.md)定义，因此需要新的方法声明时可以声明自定义。

第一个类型是 <xref:System.Action> 类型和一些变体：

```csharp
public delegate void Action();
public delegate void Action<in T>(T arg);
public delegate void Action<in T1, in T2>(T1 arg1, T2 arg2);
// Other variations removed for brevity.
```

有关协方差的文章中介绍了泛型类型参数的 `in` 修饰符。

`Action` 委托的变体可包含多达 16 个参数，如 <xref:System.Action%6016>。
重要的是这些定义对每个委托参数使用不同的泛型参数：这样可以具有最大的灵活性。 方法参数不需要但可能是相同的类型。

对任何具有 void 返回类型的委托类型使用一种 `Action` 类型。

此框架还包括几种可用于返回值的委托类型的泛型委托类型：

```csharp
public delegate TResult Func<out TResult>();
public delegate TResult Func<in T1, out TResult>(T1 arg);
public delegate TResult Func<in T1, in T2, out TResult>(T1 arg1, T2 arg2);
// Other variations removed for brevity
```

有关协方差的文章中介绍了所产生的泛型类型参数的 `out` 修饰符。

`Func` 委托的变体可包含多达 16 个输入参数，如 <xref:System.Func%6017>。
按照约定，结果的类型始终是所有 `Func` 声明中的最后一个类型参数。

对任何返回值的委托类型使用一种 `Func` 类型。

还有一种专门的 <xref:System.Predicate%601>
委托类型，可返回单个值的测试结果：

```csharp
public delegate bool Predicate<in T>(T obj);
```

你可能会注意到对于任何 `Predicate` 类型，均存在一个在结构上等效的 `Func` 类型，例如：

```csharp
Func<string, bool> TestForString;
Predicate<string> AnotherTestForString;
```

你可能认为这两种类型是等效的。 它们不是。
这两个变量不能互换使用。 一种类型的变量无法赋予另一种类型。 C# 类型系统使用的是已定义类型的名称，而不是其结构。

.NET Core 库中的所有这些委托类型定义意味着你不需要为创建的任何需要委托的新功能定义新的委托类型。 这些泛型定义应已提供大多数情况下所需要的所有委托类型。 只需使用所需的类型参数实例化其中一个类型。 对于可成为泛型算法的算法，这些委托可以用作泛型类型。

这样可以节省时间，并尽量减少为了使用委托而需要创建的新类型的数目。

在下一篇文章中，你将看到在实践中使用委托的几种通用模式。

[下一部分](delegates-patterns.md)
