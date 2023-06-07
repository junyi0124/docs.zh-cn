---
title: System.Delegate 和 `delegate` 关键字
description: 详细介绍 .NET 中支持委托的类以及这些类映射到“delegate”关键字的方式。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: f3742fda-13c2-4283-8966-9e21c2674393
ms.openlocfilehash: 9df8ad68f6bfa62863ee047875b6419fc81ad779
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062458"
---
# <a name="systemdelegate-and-the-delegate-keyword"></a>System.Delegate 和 `delegate` 关键字

[上一页](delegates-overview.md)

本文介绍 .NET 中支持委托的类以及这些类映射到 `delegate` 关键字的方式。

## <a name="define-delegate-types"></a>定义委托类型

我们从“delegate”关键字开始，因为这是你在使用委托时会使用的主要方法。 编译器在你使用 `delegate` 关键字时生成的代码会映射到调用 <xref:System.Delegate> 和 <xref:System.MulticastDelegate> 类的成员的方法调用。

可使用类似于定义方法签名的语法来定义委托类型。 只需向定义添加 `delegate` 关键字即可。

我们继续使用 List.Sort() 方法作为示例。 第一步是为比较委托创建类型：

```csharp
// From the .NET Core library

// Define the delegate type:
public delegate int Comparison<in T>(T left, T right);
```

编译器会生成一个类，它派生自与使用的签名匹配的 `System.Delegate`（在此例中，是返回一个整数并具有两个参数的方法）。 该委托的类型是 `Comparison`。 `Comparison` 委托类型是泛型类型。 有关泛型的详细信息，请参阅[此处](programming-guide/generics/index.md)。

请注意，语法可能看起来像是声明变量，但它实际上是声明类型  。 可以在类中、直接在命名空间中、甚至是在全局命名空间中定义委托类型。

> [!NOTE]
> 建议不要直接在全局命名空间中声明委托类型（或其他类型）。

编译器还会为此新类型生成添加和移除处理程序，以便此类的客户端可以对实例的调用列表添加和移除方法。 编译器会强制所添加或移除的方法的签名与声明该方法时使用的签名匹配。

## <a name="declare-instances-of-delegates"></a>声明委托的实例

定义委托之后，可以创建该类型的实例。
与 C# 中的所有变量一样，不能直接在命名空间中或全局命名空间中声明委托实例。

```csharp
// inside a class definition:

// Declare an instance of that type:
public Comparison<T> comparator;
```

变量的类型是 `Comparison<T>`（前面定义的委托类型）。 变量的名称是 `comparator`。

 上面的代码片段在类中声明了一个成员变量。 还可以声明作为局部变量或方法参数的委托变量。

## <a name="invoke-delegates"></a>调用委托

可通过调用某个委托来调用处于该委托调用列表中的方法。 在 `Sort()` 方法内部，代码会调用比较方法以确定放置对象的顺序：

```csharp
int result = comparator(left, right);
```

在上面的行中，代码会调用  附加到委托的方法。
可将变量视为方法名称，并使用普通方法调用语法调用它。

该代码行进行了不安全假设：不保证目标已添加到委托。 如果未附加目标，则上面的行会导致引发 `NullReferenceException`。 用于解决此问题的惯例比简单 null 检查更加复杂，在此[系列](delegates-patterns.md)的后面部分中会进行介绍。

## <a name="assign-add-and-remove-invocation-targets"></a>分配、添加和删除调用目标

这是委托类型的定义方式，以及声明和调用委托实例的方式。

要使用 `List.Sort()` 方法的开发人员需要定义签名与委托类型定义匹配的方法，并将它分配给排序方法使用的委托。 此分配会将方法添加到该委托对象的调用列表。

假设要按长度对字符串列表进行排序。 比较函数可能如下所示：

```csharp
private static int CompareLength(string left, string right) =>
    left.Length.CompareTo(right.Length);
```

方法声明为私有方法。 这没有什么不对。 你可能不希望此方法是公共接口的一部分。 它仍可以在附加到委托时用作比较方法。 调用代码会将此方法附加到委托对象的目标列表，并且可以通过该委托访问它。

通过将该方法传递给 `List.Sort()` 方法来创建该关系：

```csharp
phrases.Sort(CompareLength);
```

请注意，在不带括号的情况下使用方法名称。 将方法用作参数会告知编译器将方法引用转换为可以用作委托调用目标的引用，并将该方法作为调用目标进行附加。

还可以通过声明“`Comparison<string>`”类型的变量并进行分配来显式执行操作：

```csharp
Comparison<string> comparer = CompareLength;
phrases.Sort(comparer);
```

在用作委托目标的方法是小型方法的用法中，经常使用 [lambda 表达式](language-reference/operators/lambda-expressions.md)语法来执行分配：

```csharp
Comparison<string> comparer = (left, right) => left.Length.CompareTo(right.Length);
phrases.Sort(comparer);
```

[后续部分](delegates-patterns.md)中更详细地介绍了如何对委托目标使用 lambda 表达式。

Sort() 示例通常将单个目标方法附加到委托。 但是，委托对象支持将多个目标方法附加到委托对象的调用列表。

## <a name="delegate-and-multicastdelegate-classes"></a>委托和 MulticastDelegate 类

上面介绍的语言支持可提供在使用委托时通常需要的功能和支持。 这些功能采用 .NET Core Framework 中的两个类进行构建：<xref:System.Delegate> 和 <xref:System.MulticastDelegate>。

`System.Delegate` 类及其单个直接子类 `System.MulticastDelegate` 可提供框架支持，以便创建委托、将方法注册为委托目标以及调用注册为委托目标的所有方法。

有趣的是，`System.Delegate` 和 `System.MulticastDelegate` 类本身不是委托类型。 它们为所有特定委托类型提供基础。 相同的语言设计过程要求不能声明派生自 `Delegate` 或 `MulticastDelegate` 的类。 C# 语言规则禁止这样做。

相反，C# 编译器会在你使用 C# 语言关键字声明委托类型时，创建派生自 `MulticastDelegate` 的类的实例。

此设计起源于 C# 和 .NET 的第一版。 设计团队的一个目标是确保在使用委托时，语言强制实施类型安全。 这意味着确保使用正确类型和数量的参数来调用委托。 并且在编译时正确指示任何返回类型。 委托是 1.0 .NET 版本的一部分（在泛型出现之前）。

强制实施此类型安全的最佳方法是让编辑器创建表示所使用的方法签名的具体委托类。

即使不能直接创建派生类，也会使用对这些类定义的方法。 我们来讨论一下在使用委托时会使用的最常见方法。

要记住的首要且最重要的事实是，使用的每个委托都派生自 `MulticastDelegate`。 多播委托意味着通过委托进行调用时，可以调用多个方法目标。 原始设计考虑区分只能附加并调用一个目标方法的委托与可以附加并调用多个目标方法的委托。 该区分被证明在实际中不如最初设想那么有用。 已创建了两个不同的类，并且自初始公开发行以来便一直处于框架中。

对委托最常使用的方法是 `Invoke()` 和 `BeginInvoke()` / `EndInvoke()`。 `Invoke()` 会调用已附加到特定委托实例的所有方法。 如上面所见，通常会通过对委托变量使用方法调用语法来调用委托。 如在[此系列后面部分](delegates-patterns.md)中所见，一些模式可直接使用这些方法。

现在你已了解支持委托的语言语法和类，我们来看一下如何使用、创建和调用强类型委托。

[下一部分](delegates-strongly-typed.md)
