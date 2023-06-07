---
title: C# 保留的特性：可为空的静态分析
ms.date: 04/14/2020
description: 编译器会解释这些属性，以便为可为 null 和不可为 null 的引用类型提供更好的静态分析。
ms.openlocfilehash: 6678cd21de23d4ed391eff089e33939b5adff0fa
ms.sourcegitcommit: b59237ca4ec763969a0dd775a3f8f39f8c59fe24
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2020
ms.locfileid: "91955598"
---
# <a name="reserved-attributes-contribute-to-the-compilers-null-state-static-analysis"></a>保留的特性有助于编译器的 null 状态静态分析

在可为 null 的上下文中，编译器对代码执行静态分析，以确定所有引用类型变量的 null 状态：

- 非 null：静态分析确定将变量分配给非 null 值。
- 可能为 null  ：静态分析无法确定为变量分配非 null 值。

可以应用多个特性，以向编译器提供有关 API 语义的信息。 这些信息有助于编译器执行静态分析并确定变量何时不为 null。 本文提供每个特性的简要说明以及它们的使用方法。 所有示例都假设使用 C# 8.0 或更高版本，并且代码处于可为 null 的上下文中。

首先，让我们看一个熟悉的示例。 假设你的库具有以下用于检索资源字符串的 API：

```csharp
bool TryGetMessage(string key, out string message)
```

前面的示例遵循 .NET 中熟悉的 `Try*` 模式。 此 API 有两个引用参数：`key` 和 `message` 参数。 此 API 具有与这些参数的是否为 null 相关的以下规则：

- 调用方不应将 `null` 作为 `key` 的参数传递。
- 调用方可以传递值为 `null` 的变量作为 `message` 的参数。
- 如果 `TryGetMessage` 方法返回 `true`，则 `message` 的值不为 null。 如果返回值是 `false,`，则 `message`（及其 null 状态）的值为 null。

`key` 的规则可以用变量类型表示：`key` 应是不可为 null 的引用类型。 `message` 参数更复杂。 它允许 `null` 作为参数，但保证成功时，`out` 参数不为 null。 对于这些情况，需要使用更丰富的词汇来描述期望。

为表示有关变量的 null 状态的附加信息，已添加多个特性。 在 C# 8 引入可为 null 的引用类型之前编写的所有代码都是忽略 null 的  。 这意味着任何引用类型变量都可以为 null，但不需要进行 null 检查。 代码“可识别为 null”后，这些规则就会改变  。 引用类型不应该是 `null` 值，在取消引用之前，必须对照 `null` 检查可为 null 的引用类型。

API 的规则可能更复杂，正如你在 `TryGetValue` API 方案中看到的那样。 许多 API 对于变量何时可以或不可以为 `null` 有更复杂的规则。 在这些情况下，可使用以下属性之一来表示这些规则：

- [AllowNull](xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute)：不可为 null 的输入参数可以为 null。
- [DisallowNull](xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute)：可为 null 的输入参数不应为 null。
- [MaybeNull](xref:System.Diagnostics.CodeAnalysis.MaybeNullAttribute)：不可为 null 的返回值可以为 null。
- [NotNull](xref:System.Diagnostics.CodeAnalysis.NotNullAttribute)：可为 null 的返回值永远不会为 null。
- [MaybeNullWhen](xref:System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute)：当方法返回指定的 `bool` 值时，不可为 null 的输入参数可以为 null。
- [NotNullWhen](xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute)：当方法返回指定的 `bool` 值时，可为 null 的输入参数将不为 null。
- [NotNullIfNotNull](xref:System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute)：如果指定参数的参数不为 null，则返回值不为 null。
- [DoesNotReturn](xref:System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute)：方法从不返回。 换句话说，它总是引发异常。
- [DoesNotReturnIf](xref:System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute)：如果关联的 `bool` 参数具有指定值，则此方法永远不会返回。

上述说明是对每个特性的快速参考。 以下各节介绍了行为和含义。

添加这些特性将为编译器提供有关 API 规则的更多信息。 当调用代码在可为 null 的上下文中编译时，编译器将在调用方违反这些规则时发出警告。 这些特性不会对实现启用其他检查。

## <a name="specify-preconditions-allownull-and-disallownull"></a>指定前提条件：`AllowNull` 和 `DisallowNull`

请考虑一个从不返回 `null` 的读/写属性，因为它具有合理的默认值。 调用方在将 `null` 设置为该默认值时将其传递给 set 访问器。 例如，假设一个消息系统要求在聊天室中输入一个屏幕名称。 如果未提供任何内容，系统将生成一个随机名称：

```csharp
public string ScreenName
{
   get => _screenName;
   set => _screenName = value ?? GenerateRandomScreenName();
}
private string _screenName;
```

当你在忽略可为 null 的上下文中编译前面的代码时，一切都是正常的。 启用可为 null 的引用类型后，`ScreenName` 属性将成为不可为 null 的引用。 这对于 `get` 访问器是正确的：它从不返回 `null`。 调用方不需要检查返回的 `null` 属性。 但现在将属性设置为 `null` 将生成警告。 为了继续支持这种类型的代码，请将 <xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute?displayProperty=nameWithType> 特性添加到该属性，如以下代码所示：

```csharp
[AllowNull]
public string ScreenName
{
   get => _screenName;
   set => _screenName = value ?? GenerateRandomScreenName();
}
private string _screenName = GenerateRandomScreenName();
```

可能需要为 <xref:System.Diagnostics.CodeAnalysis> 添加一个 `using` 指令才能使用本文中讨论的特性和其他特性。 特性应用于属性，而不是 `set` 访问器。 `AllowNull` 特性指定前置条件，并且仅适用于输入  。 `get` 访问器有一个返回值，但没有输入参数。 因此，`AllowNull` 特性只适用于 `set` 访问器。

前面的示例演示了在参数上添加 `AllowNull` 特性时要查找的内容：

1. 该变量的一般约定是它不应为 `null`，因此需要一个不可为 null 的引用类型。
1. 虽然输入变量不是最常见的用法，但仍有一些方案需要 `null`。

大多数情况下，属性或 `in`、`out` 和 `ref` 参数需要此特性。 当变量通常为非 null 时，`AllowNull` 属性是最佳选择，但需要允许 `null` 作为前提条件。

将此情况与使用 `DisallowNull` 方案相比：使用此特性可以指定可为 null 引用类型的输入变量不应为 `null`。 请考虑一个特性，其中 `null` 是默认值，但客户端只能将其设置为非 null 值。 考虑下列代码：

```csharp
public string ReviewComment
{
    get => _comment;
    set => _comment = value ?? throw new ArgumentNullException(nameof(value), "Cannot set to null");
}
string _comment;
```

前面的代码是表达设计的最佳方式，`ReviewComment` 可以是 `null`，但不能设置为 `null`。 代码可识别为 null 后，你就可以使用 <xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute?displayProperty=nameWithType> 向调用方更清楚地表达此概念：

```csharp
[DisallowNull]
public string? ReviewComment
{
    get => _comment;
    set => _comment = value ?? throw new ArgumentNullException(nameof(value), "Cannot set to null");
}
string? _comment;
```

在可为 null 的上下文中，`ReviewComment` `get` 访问器可以返回默认值 `null`。 编译器会警告在访问之前必须进行检查。 此外，它警告调用方，即使它可能是 `null`，调用方也不应显式地将其设置为 `null`。 `DisallowNull` 特性还指定了前置条件，它不会影响 `get` 访问器  。 当你观察到以下特征时，可以使用 `DisallowNull` 特性：

1. 在核心方案中（通常是在首次实例化时），变量可以是 `null`。
1. 变量不应显式设置为 `null`。

这些情况在忽略 null 的代码中很常见  。 可能是在两个不同的初始化操作中设置了对象属性。 可能只有在某些异步工作完成后才能设置某些属性。

可使用 `AllowNull` 和 `DisallowNull` 特性指定变量上的前置条件可能与这些变量上的可为 null 注释不匹配。 这两个特性提供了关于 API 特征的更多细节。 此附加信息有助于调用方正确使用 API。 请记住，可使用以下特性指定前提条件：

- [AllowNull](xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute)：不可为 null 的输入参数可以为 null。
- [DisallowNull](xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute)：可为 null 的输入参数不应为 null。

## <a name="specify-post-conditions-maybenull-and-notnull"></a>指定后置条件：`MaybeNull` 和 `NotNull`

假设你有使用以下签名的方法：

```csharp
public Customer FindCustomer(string lastName, string firstName)
```

你可能已经编写了类似的方法，以便在未找到所查找的名称时返回 `null`。 `null` 清楚地表明未找到记录。 在本例中，你可能会将返回类型从 `Customer` 更改为 `Customer?`。 将返回值声明为可为 null 的引用类型可以清楚地指定此 API 的意图。

基于[泛型定义和为 null 性](../../nullable-migration-strategies.md#generic-definitions-and-nullability)中所述的原因，该技术不适用于泛型方法。 你可能具有遵循类似模式的泛型方法：

```csharp
public T Find<T>(IEnumerable<T> sequence, Func<T, bool> predicate)
```

不能指定返回值为 `T?`。 当找不到所需项时，此方法返回 `null`。 由于无法声明 `T?` 返回类型，因此需要将 `MaybeNull` 注释添加到方法返回：

```csharp
[return: MaybeNull]
public T Find<T>(IEnumerable<T> sequence, Func<T, bool> predicate)
```

前面的代码通知调用方，协定暗示了一个不可为 null 的类型，但是返回值可能实际上为 null  。  当 API 应为不可为 null 的类型（通常是泛型类型参数）时，请使用 `MaybeNull` 特性，但可能会有返回 `null` 的情况。

还可以指定返回值或者 `out` 或 `ref` 参数不为 null，即使该类型是可为 null 的引用类型。 请考虑一种确保数组足以容纳多个元素的方法。 如果输入参数没有容量，例程将分配新数组并将所有现有元素复制到其中。 如果输入参数是 `null`，则例程将分配新存储。 如果有足够的容量，例程将不执行任何操作：

```csharp
public void EnsureCapacity<T>(ref T[] storage, int size)
```

可以按如下方式调用此例程：

```csharp
// messages has the default value (null) when EnsureCapacity is called:
EnsureCapacity<string>(ref messages, 10);
// messages is not null.
EnsureCapacity<string>(messages, 50);
```

启用 null 引用类型后，需要确保前面的代码在编译时没有警告。 当方法返回时，`storage` 参数保证不为 null。 但是，可以使用 null 引用调用 `EnsureCapacity`。 可以将 `storage` 设为可为 null 的引用类型，并将 `NotNull` 后置条件添加到参数声明中：

```csharp
public void EnsureCapacity<T>([NotNull] ref T[]? storage, int size)
```

前面的代码清楚地表达了现有协定：调用方可以传递具有 `null` 值的变量，但返回值保证永远不为 null。 `NotNull` 特性对于 `ref` 和 `out` 参数最有用，其中 `null` 可以作为参数传递，但当方法返回时，该参数保证不为 null。

可以使用以下特性指定无条件后置条件：

- [MaybeNull](xref:System.Diagnostics.CodeAnalysis.MaybeNullAttribute)：不可为 null 的返回值可以为 null。
- [NotNull](xref:System.Diagnostics.CodeAnalysis.NotNullAttribute)：可为 null 的返回值永远不会为 null。

## <a name="specify-conditional-post-conditions-notnullwhen-maybenullwhen-and-notnullifnotnull"></a>指定有条件后置条件：`NotNullWhen`、`MaybeNullWhen` 和 `NotNullIfNotNull`

你可能很熟悉 `string` 方法 <xref:System.String.IsNullOrEmpty(System.String)?DisplayProperty=nameWithType>。 当参数为 null 或为空字符串时，此方法返回 `true`。 这是一种 null 检查格式：如果方法返回 `false`，调用方不需要 null 检查参数。 若要使这样的方法可识别为 null，需要将参数设置为可为 null 的引用类型，并添加 `NotNullWhen` 特性：

```csharp
bool IsNullOrEmpty([NotNullWhen(false)] string? value);
```

该特性通知编译器返回值为 `false` 的任何代码都不需要进行 null 检查。 添加特性通知编译器的静态分析，`IsNullOrEmpty` 执行必要的 null 检查：当它返回 `false` 时，输入参数不是 `null`。

```csharp
string? userInput = GetUserInput();
if (!string.IsNullOrEmpty(userInput))
{
   int messageLength = userInput.Length; // no null check needed.
}
// null check needed on userInput here.
```

对于 .NET Core 3.0，将对 <xref:System.String.IsNullOrEmpty(System.String)?DisplayProperty=nameWithType> 方法进行注释，如上面所示。 代码库中可能有类似的方法来检查对象的状态是否为 null 值。 编译器无法识别自定义的 null 检查方法，你需要自己添加注释。 添加特性时，编译器的静态分析将知道何时对测试变量进行了 null 检查。

这些特性的另一个用途是 `Try*` 模式。 `ref` 和 `out` 变量的后置条件通过返回值进行通信。 请考虑前面所示的方法：

```csharp
bool TryGetMessage(string key, out string message)
```

前面的方法遵循典型的 .NET 习惯用法：返回值指示是否将 `message` 设置为已找到的值，或者，如果未找到消息，则为默认值。 如果方法返回 `true`，`message` 的值不为 null；否则，该方法将 `message` 设置为 null。

你可以使用 `NotNullWhen` 特性来传达这个习惯用法。 更新可为 null 的引用类型的签名时，需要将 `message` 设为 `string?` 并添加一个特性：

```csharp
bool TryGetMessage(string key, [NotNullWhen(true)] out string? message)
```

在前面的示例中，`message` 的值在 `TryGetMessage` 返回 `true` 时不为 null。 你应以相同的方式在代码库中注释类似的方法：参数可以是 `null`，并且已知在方法返回 `true` 时不为 null。

还可能需要一个最终特性。 有时，返回值的 null 状态取决于一个或多个输入参数的 null 状态。 当某些输入参数不是 `null` 时，这些方法将返回非 null 值。 若要正确地注释这些方法，可以使用 `NotNullIfNotNull` 特性。 请考虑以下方法：

```csharp
string GetTopLevelDomainFromFullUrl(string url);
```

如果 `url` 参数不为 null，则输出不是 `null`。 启用可为 null 的引用后，只要 API 永不接受 null 输入，该签名就能正常运行。 但是，如果输入可以为 null，那么返回值也可以为 null。 因此，你可以将签名更改为以下代码：

```csharp
string? GetTopLevelDomainFromFullUrl(string? url);
```

这也是可行的，但通常会强制调用方实现额外的 `null` 检查。 协定是，只有当输入参数 `url` 是 `null` 时，返回值才会是 `null`。 若要表达该协定，你需要注释此方法，如以下代码所示：

```csharp
[return: NotNullIfNotNull("url")]
string? GetTopLevelDomainFromFullUrl(string? url);
```

返回值和参数都用 `?` 进行了注释，这表明两者都可以是 `null`。 该特性进一步阐明了当 `url` 参数不是 `null` 时，返回值不会为 null。

可以使用以下特性指定条件的后置条件：

- [MaybeNullWhen](xref:System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute)：当方法返回指定的 `bool` 值时，不可为 null 的输入参数可以为 null。
- [NotNullWhen](xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute)：当方法返回指定的 `bool` 值时，可为 null 的输入参数将不为 null。
- [NotNullIfNotNull](xref:System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute)：如果指定参数的输入参数不为 null，则返回值不为 null。

## <a name="verify-unreachable-code"></a>验证无法访问的代码

某些方法（通常是异常帮助程序或其他实用工具方法）始终通过引发异常来退出。 或者，帮助程序可以基于布尔参数的值引发异常。

在第一种情况下，可以将 `DoesNotReturn` 特性添加到方法声明中。 编译器通过三种方式为你提供帮助。 首先，如果存在方法可以退出而不引发异常的路径，编译器将发出警告。 其次，编译器会将调用该方法后的任何代码标记为不可访问，直到遇到适当的 `catch` 子句  。 第三，无法访问的代码不会影响任何 null 状态。 请考虑此方法：

```csharp
[DoesNotReturn]
private void FailFast()
{
    throw new InvalidOperationException();
}

public void SetState(object containedField)
{
    if (!isInitialized)
    {
        FailFast();
    }

    // unreachable code:
    _field = containedField;
}
```

在第二种情况下，需将 `DoesNotReturnIf` 特性添加到方法的布尔参数中。 你可以修改前面的示例，如下所示：

```csharp
private void FailFast([DoesNotReturnIf(false)] bool isValid)
{
    if (!isValid)
    {
        throw new InvalidOperationException();
    }
}

public void SetState(object containedField)
{
    FailFast(isInitialized);

    // unreachable code when "isInitialized" is false:
    _field = containedField;
}
```

## <a name="summary"></a>总结

[!INCLUDE [C# version alert](../../includes/csharp-version-alert.md)]

添加可为 null 的引用类型提供了一个初始词汇表，用于描述 API 对可能为 `null` 的变量的期望。 其他特性使用更丰富的词汇表来描述变量作为前置条件和后置条件的 null 状态。 这些特性更清楚地描述了你的期望，并为使用 API 的开发人员提供了更好的体验。

在为可为 null 的上下文中更新库时，添加这些特性可指导用户正确使用 API。 这些特性有助于你完全描述输入参数和返回值的 null 状态：

- [AllowNull](xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute)：不可为 null 的输入参数可以为 null。
- [DisallowNull](xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute)：可为 null 的输入参数不应为 null。
- [MaybeNull](xref:System.Diagnostics.CodeAnalysis.MaybeNullAttribute)：不可为 null 的返回值可以为 null。
- [NotNull](xref:System.Diagnostics.CodeAnalysis.NotNullAttribute)：可为 null 的返回值永远不会为 null。
- [MaybeNullWhen](xref:System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute)：当方法返回指定的 `bool` 值时，不可为 null 的输入参数可以为 null。
- [NotNullWhen](xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute)：当方法返回指定的 `bool` 值时，可为 null 的输入参数将不为 null。
- [NotNullIfNotNull](xref:System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute)：如果指定参数的输入参数不为 null，则返回值不为 null。
- [DoesNotReturn](xref:System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute)：方法从不返回。 换句话说，它总是引发异常。
- [DoesNotReturnIf](xref:System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute)：如果关联的 `bool` 参数具有指定值，则此方法永远不会返回。
