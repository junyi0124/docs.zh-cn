---
title: 扩展方法 - C# 编程指南
description: C# 中的扩展方法使你能够向现有类型添加方法，而无需创建新的派生类型，重新编译或以其他方式修改原始类型。
ms.date: 03/19/2020
helpviewer_keywords:
- methods [C#], adding to existing types
- extension methods [C#]
- methods [C#], extension
ms.assetid: 175ce3ff-9bbf-4e64-8421-faeb81a0bb51
ms.openlocfilehash: 88d6cfd1b5262d4a4fae96cf53271467b5042319
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90541937"
---
# <a name="extension-methods-c-programming-guide"></a>扩展方法（C# 编程指南）

扩展方法使你能够向现有类型“添加”方法，而无需创建新的派生类型、重新编译或以其他方式修改原始类型。 扩展方法是一种静态方法，但可以像扩展类型上的实例方法一样进行调用。 对于用 C#、F# 和 Visual Basic 编写的客户端代码，调用扩展方法与调用在类型中定义的方法没有明显区别。

最常见的扩展方法是 LINQ 标准查询运算符，它将查询功能添加到现有的 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 类型。 若要使用标准查询运算符，请先使用 `using System.Linq` 指令将它们置于范围中。 然后，任何实现了 <xref:System.Collections.Generic.IEnumerable%601> 的类型看起来都具有 <xref:System.Linq.Enumerable.GroupBy%2A>、<xref:System.Linq.Enumerable.OrderBy%2A>、<xref:System.Linq.Enumerable.Average%2A> 等实例方法。 在 <xref:System.Collections.Generic.IEnumerable%601> 类型的实例（如 <xref:System.Collections.Generic.List%601> 或 <xref:System.Array>）后键入“dot”时，可以在 IntelliSense 语句完成中看到这些附加方法。

### <a name="orderby-example"></a>OrderBy 示例

下面的示例演示如何对一个整数数组调用标准查询运算符 `OrderBy` 方法。 括号里面的表达式是一个 lambda 表达式。 很多标准查询运算符采用 Lambda 表达式作为参数，但这不是扩展方法的必要条件。 有关详细信息，请参阅 [Lambda 表达式](../../language-reference/operators/lambda-expressions.md)。

[!code-csharp[csProgGuideExtensionMethods#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#3)]

扩展方法被定义为静态方法，但它们是通过实例方法语法进行调用的。 它们的第一个参数指定方法操作的类型。 参数前面是[此](../../language-reference/keywords/this.md)修饰符。 仅当你使用 `using` 指令将命名空间显式导入到源代码中之后，扩展方法才位于范围中。

下面的示例演示为 <xref:System.String?displayProperty=nameWithType> 类定义的一个扩展方法。 它是在非嵌套的、非泛型静态类内部定义的：

[!code-csharp[csProgGuideExtensionMethods#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#4)]

可使用此 `WordCount` 指令将 `using` 扩展方法置于范围中：

```csharp
using ExtensionMethods;
```

而且，可以使用以下语法从应用程序中调用该扩展方法：

```csharp
string s = "Hello Extension Methods";
int i = s.WordCount();
```

在代码中，可以使用实例方法语法调用该扩展方法。 编译器生成的中间语言 (IL) 会将代码转换为对静态方法的调用。 并未真正违反封装原则。 扩展方法无法访问它们所扩展的类型中的专用变量。

有关详细信息，请参阅[如何实现和调用自定义扩展方法](./how-to-implement-and-call-a-custom-extension-method.md)。

通常，你更多时候是调用扩展方法而不是实现你自己的扩展方法。 由于扩展方法是使用实例方法语法调用的，因此不需要任何特殊知识即可从客户端代码中使用它们。 若要为特定类型启用扩展方法，只需为在其中定义这些方法的命名空间添加 `using` 指令。 例如，若要使用标准查询运算符，请将此 `using` 指令添加到代码中：

```csharp
using System.Linq;
```

（你可能还必须添加对 System.Core.dll 的引用。）你将注意到，标准查询运算符现在作为可供大多数 <xref:System.Collections.Generic.IEnumerable%601> 类型使用的附加方法显示在 IntelliSense 中。

## <a name="binding-extension-methods-at-compile-time"></a>在编译时绑定扩展方法

可以使用扩展方法来扩展类或接口，但不能重写扩展方法。 与接口或类方法具有相同名称和签名的扩展方法永远不会被调用。 编译时，扩展方法的优先级总是比类型本身中定义的实例方法低。 换句话说，如果某个类型具有一个名为 `Process(int i)` 的方法，而你有一个具有相同签名的扩展方法，则编译器总是绑定到该实例方法。 当编译器遇到方法调用时，它首先在该类型的实例方法中寻找匹配的方法。 如果未找到任何匹配方法，编译器将搜索为该类型定义的任何扩展方法，并且绑定到它找到的第一个扩展方法。 下面的示例演示编译器如何确定要绑定到哪个扩展方法或实例方法。

## <a name="example"></a>示例

下面的示例演示 C# 编译器在确定是将方法调用绑定到类型上的实例方法还是绑定到扩展方法时所遵循的规则。 静态类 `Extensions` 包含为任何实现了 `IMyInterface` 的类型定义的扩展方法。 类 `A`、`B` 和 `C` 都实现了该接口。

`MethodB` 扩展方法永远不会被调用，因为它的名称和签名与这些类已经实现的方法完全匹配。

如果编译器找不到具有匹配签名的实例方法，它会绑定到匹配的扩展方法（如果存在这样的方法）。

[!code-csharp[csProgGuideExtensionMethods#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExtensionMethods/cs/extensionmethods.cs#5)]

## <a name="common-usage-patterns"></a>常见使用模式

### <a name="collection-functionality"></a>集合功能

过去，创建”集合类”通常是为了使给定类型实现 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 接口，并实现对该类型集合的功能。 创建这种类型的集合对象没有任何问题，但也可以通过对 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 使用扩展来实现相同的功能。 扩展的优势是允许从任何集合（如 <xref:System.Array?displayProperty=nameWithType> 或实现该类型 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 的 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>）调用功能。 可以在[本文前面的内容](#orderby-example)中找到使用 Int32 的数组的示例。

### <a name="layer-specific-functionality"></a>特定于层的功能

使用洋葱架构或其他分层应用程序设计时，通常具有一组域实体或数据传输对象，可用于跨应用程序边界进行通信。 这些对象通常不包含任何功能，或者只包含适用于应用程序的所有层的最少功能。 使用扩展方法可以添加特定于每个应用程序层的功能，而无需使用其他层中不需要的方法来向下加载对象。

```csharp
public class DomainEntity
{
    public int Id { get; set; }
    public string FirstName { get; set; }
    public string LastName { get; set; }
}

static class DomainEntityExtensions
{
    static string FullName(this DomainEntity value)
        => $"{value.FirstName} {value.LastName}";
}
```

### <a name="extending-predefined-types"></a>扩展预定义类型

当需要创建可重用功能时，我们无需创建新对象，而是可以扩展现有类型，例如 .NET 或 CLR 类型。 例如，如果不使用扩展方法，我们可能会创建 `Engine` 或 `Query` 类，对可从代码中的多个位置调用的 SQL Server 执行查询。 但是，如果换做使用扩展方法扩展 <xref:System.Data.SqlClient.SqlConnection?displayProperty=nameWithType> 类，就可以从与 SQL Server 连接的任何位置执行该查询。 一些其他示例可能是向 <xref:System.String?displayProperty=nameWithType> 类添加常见功能、扩展 <xref:System.IO.File?displayProperty=nameWithType>、<xref:System.IO.Stream?displayProperty=nameWithType> 以及 <xref:System.Exception?displayProperty=nameWithType> 对象的数据处理功能以实现特定的错误处理功能。 这些用例的类型仅受想象力和判断力的限制。

使用 `struct` 类型扩展预定义类型可能很困难，因为它们已通过值传递给方法。 这意味着将对结构的副本进行任何结构更改。 扩展方法退出后，将不显示这些更改。 从 C# 7.2 开始，可以将 `ref` 修饰符添加到扩展方法的第一个参数。 添加 `ref` 修饰符意味着第一个参数是通过引用传递的。 在这种情况下，可以编写扩展方法来更改要扩展的结构的状态。

## <a name="general-guidelines"></a>通用准则

尽管通过修改对象的代码来添加功能，或者在合理和可行的情况下派生新类型等方式仍是可取的，但扩展方法已成为在整个 .NET 生态系统中创建可重用功能的关键选项。 对于原始源不受控制、派生对象不合适或不可用，或者不应在功能适用范围之外公开功能的情况，扩展方法是一个不错的选择。

有关派生类型的详细信息，请参阅[继承](./inheritance.md)。

在使用扩展方法来扩展你无法控制其源代码的类型时，你需要承受该类型实现中的更改会导致扩展方法失效的风险。

如果确实为给定类型实现了扩展方法，请记住以下几点：

- 如果扩展方法与该类型中定义的方法具有相同的签名，则扩展方法永远不会被调用。
- 在命名空间级别将扩展方法置于范围中。 例如，如果你在一个名为 `Extensions` 的命名空间中具有多个包含扩展方法的静态类，则这些扩展方法将全部由 `using Extensions;` 指令置于范围中。

针对已实现的类库，不应为了避免程序集的版本号递增而使用扩展方法。 如果要向你拥有源代码的库中添加重要功能，请遵循适用于程序集版本控制的 .NET 准则。 有关详细信息，请参阅[程序集版本控制](../../../standard/assembly/versioning.md)。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [并行编程示例（这些示例包括许多示例扩展方法）](/samples/browse/?products=dotnet-core%2Cdotnet-standard&term=parallel)
- [Lambda 表达式](../../language-reference/operators/lambda-expressions.md)
- [标准查询运算符概述](../concepts/linq/standard-query-operators-overview.md)
- [Conversion rules for Instance parameters and their impact](/archive/blogs/sreekarc/conversion-rules-for-instance-parameters-and-their-impact)（实例参数及其影响的转换规则）
- [Extension methods Interoperability between languages](/archive/blogs/sreekarc/extension-methods-interoperability-between-languages)（语言间扩展方法的互操作性）
- [Extension methods and Curried Delegates](/archive/blogs/sreekarc/extension-methods-and-curried-delegates)（扩展方法和扩充委托）
- [Extension method Binding and Error reporting](/archive/blogs/sreekarc/extension-method-binding-and-error-reporting)（扩展方法绑定和错误报告）
