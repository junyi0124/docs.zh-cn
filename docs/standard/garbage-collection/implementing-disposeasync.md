---
title: 实现 DisposeAsync 方法
description: 了解如何实现 DisposeAsync 和 DisposeAsyncCore 方法来执行异步资源清理。
author: IEvangelist
ms.author: dapine
ms.date: 12/09/2020
dev_langs:
- csharp
helpviewer_keywords:
- DisposeAsync method
- garbage collection, DisposeAsync method
ms.openlocfilehash: f04ac6695864b96cdcb7efeb6eb8e1d9551e1d14
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009685"
---
# <a name="implement-a-disposeasync-method"></a>实现 DisposeAsync 方法

已将 <xref:System.IAsyncDisposable?displayProperty=nameWithType> 接口作为 C# 8.0 的一部分引入。 需要执行资源清理时，可以实现 <xref:System.IAsyncDisposable.DisposeAsync?displayProperty=nameWithType> 方法，就像[实现 Dispose 方法](implementing-dispose.md)一样。 但是，其中一个主要区别是，此实现允许异步清理操作。 <xref:System.IAsyncDisposable.DisposeAsync> 返回表示异步释放操作的 <xref:System.Threading.Tasks.ValueTask>。

通常，当实现 <xref:System.IAsyncDisposable> 接口时，类还将实现 <xref:System.IDisposable> 接口。 <xref:System.IAsyncDisposable> 接口的一种良好实现模式是为同步或异步释放做好准备。 用于实现释放模式的所有指南也适用于异步实现。 本文假设你已熟悉如何[实现 Dispose 方法](implementing-dispose.md)。

## <a name="disposeasync-and-disposeasynccore"></a>DisposeAsync() 和 DisposeAsyncCore()

<xref:System.IAsyncDisposable> 接口声明单个无参数方法 <xref:System.IAsyncDisposable.DisposeAsync>。 任何非密封类都应具有另外一个也返回 <xref:System.Threading.Tasks.ValueTask> 的 `DisposeAsyncCore()` 方法。

- 没有参数的 `public` <xref:System.IAsyncDisposable.DisposeAsync?displayProperty=nameWithType> 实现。
- 一个 `protected virtual ValueTask DisposeAsyncCore()` 方法，其签名为：

  ```csharp
  protected virtual ValueTask DisposeAsyncCore()
  {
  }
  ```

### <a name="the-disposeasync-method"></a>DisposeAsync() 方法

`public` 无参数的 `DisposeAsync()` 方法在 `await using` 语句中隐式调用，其用途是释放非托管资源，执行常规清理，以及指示终结器（如果存在）不必运行。 释放与托管对象关联的内存始终是[垃圾回收器](index.md)的域。 因此，它具有标准实现：

```csharp
public async ValueTask DisposeAsync()
{
    // Perform async cleanup.
    await DisposeAsyncCore();

    // Dispose of unmanaged resources.
    Dispose(false);
    // Suppress finalization.
    GC.SuppressFinalize(this);
}
```

> [!NOTE]
> 与释放模式相比，异步释放模式的主要差异在于，从 <xref:System.IAsyncDisposable.DisposeAsync> 到 `Dispose(bool)` 重载方法的调用被赋予 `false` 作为参数。 但实现 <xref:System.IDisposable.Dispose?displayProperty=nameWithType> 方法时，改为传递 `true`。 这有助于确保与同步释放模式的功能等效性，并进一步确保仍调用终结器代码路径。 换句话说，`DisposeAsyncCore()` 方法将异步释放托管资源，因此不希望也同步释放这些资源。 因此，调用 `Dispose(false)` 而非 `Dispose(true)`。

### <a name="the-disposeasynccore-method"></a>DisposeAsyncCore() 方法

`DisposeAsyncCore()` 方法旨在执行受管理资源的异步清理，或对 `DisposeAsync()` 执行级联调用。 当子类继承作为 <xref:System.IAsyncDisposable> 的实现的基类时，它会封装常见的异步清理操作。 `DisposeAsyncCore()` 方法是 `virtual`，以便派生类可以在其重写中定义其他清理。

> [!TIP]
> 如果 <xref:System.IAsyncDisposable> 的实现是 `sealed`，则不需要 `DisposeAsyncCore()` 方法，异步清理可直接在 <xref:System.IAsyncDisposable.DisposeAsync?displayProperty=nameWithType> 方法中执行。

## <a name="implement-the-async-dispose-pattern"></a>实现异步释放模式

所有非密封类都应被视为潜在的基类，因为它们可以被继承。 如果为任何潜在基类实现异步释放模式，则必须提供 `protected virtual ValueTask DisposeAsyncCore()` 方法。 下面是使用 <xref:System.Text.Json.Utf8JsonWriter?displayProperty=nameWithType> 的异步释放模式的实现示例。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/disposeasync.cs":::

前面的示例使用 <xref:System.Text.Json.Utf8JsonWriter>。 有关 `System.Text.Json` 的详细信息，请参阅[如何从 Newtonsoft.Json 迁移到 System.Text.Json](../serialization/system-text-json-migrate-from-newtonsoft-how-to.md)。

## <a name="implement-both-dispose-and-async-dispose-patterns"></a>同时实现释放模式和异步释放模式

可能需要同时实现 <xref:System.IDisposable> 和 <xref:System.IAsyncDisposable> 接口，尤其是当类范围包含这些实现的实例时。 这样做可确保你可以正确地级联清理调用。 下面是一个示例类，它实现两个接口并演示清理的正确指导。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/dispose-and-disposeasync.cs":::

<xref:System.IDisposable.Dispose?displayProperty=nameWithType> 和 <xref:System.IAsyncDisposable.DisposeAsync?displayProperty=nameWithType> 实现都是简单的样板代码。

在 `Dispose(bool)` 重载方法中，如果 <xref:System.IDisposable> 实例不为 `null`，则有条件地将其释放。 <xref:System.IAsyncDisposable> 实例被强制转换为 <xref:System.IDisposable>，如果该实例也不为 `null`，也将被释放。 然后，将这两个实例都分配给 `null`。

使用 `DisposeAsyncCore()` 方法时，遵循相同的逻辑方法。 如果 <xref:System.IAsyncDisposable> 实例不为 `null`，则等待其对 `DisposeAsync().ConfigureAwait(false)` 的调用。 如果 <xref:System.IDisposable> 实例也是 <xref:System.IAsyncDisposable> 的实现，也将其异步释放。 然后，将这两个实例都分配给 `null`。

## <a name="using-async-disposable"></a>使用异步释放

要正确使用实现 <xref:System.IAsyncDisposable> 接口的对象，请将 [await](../../csharp/language-reference/operators/await.md)和 [using](../../csharp/language-reference/keywords/using-statement.md) 关键字结合使用。 请考虑以下示例，其中 `ExampleAsyncDisposable` 类进行了实例化，然后包装在 `await using` 语句中。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/proper-await-using.cs":::

> [!IMPORTANT]
> 使用 <xref:System.IAsyncDisposable> 接口的 <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait(System.IAsyncDisposable,System.Boolean)> 扩展方法配置延续任务在其原始上下文或计划程序上的封送方式。 有关 `ConfigureAwait` 的详细信息，请参阅 [ConfigureAwait FAQ](https://devblogs.microsoft.com/dotnet/configureawait-faq/)。

对于不需要使用 `ConfigureAwait` 的情况，可以按如下所示简化 `await using` 语句：

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/await-using-non-configureawait.cs":::

此外，它还可以编写为使用 [using 声明](../../csharp/whats-new/csharp-8.md#using-declarations)的隐式范围。

:::code language="csharp" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/await-using-declaration.cs":::

## <a name="stacked-usings"></a>堆叠的 using

在创建和使用实现 <xref:System.IAsyncDisposable> 的多个对象的情况下，残存错误条件中具有 <xref:System.Threading.Tasks.ValueTask.ConfigureAwait%2A> 的堆叠 `await using` 语句可能会阻止调用 <xref:System.IAsyncDisposable.DisposeAsync>。 若要确保始终调用 <xref:System.IAsyncDisposable.DisposeAsync>，应避免堆叠。 下面的三个代码示例显示要改用的可接受模式。

### <a name="acceptable-pattern-one"></a>可接受的模式一

:::code language="csharp" id="one" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/stacked-await-usings.cs":::

在前面的示例中，每个异步清理操作的范围都显式地限定在 `await using` 块下。 外部范围由 `objOne` 设置其大括号的方法来定义；若将 `objTwo` 括起来，这样就会先处理 `objTwo`，然后处理 `objOne`。 这两个 `IAsyncDisposable` 实例都使三种 <xref:System.IAsyncDisposable.DisposeAsync> 方法等待，从而执行其异步清理操作。 嵌套调用，而不是堆叠调用。

### <a name="acceptable-pattern-two"></a>可接受的模式二

:::code language="csharp" id="two" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/stacked-await-usings.cs":::

在前面的示例中，每个异步清理操作的范围都显式地限定在 `await using` 块下。 在每个块的末尾，相应的 `IAsyncDisposable` 实例使其 <xref:System.IAsyncDisposable.DisposeAsync> 方法等待，从而执行其异步清理操作。 按顺序排列调用，而不是堆叠调用。 在此场景中，首先处理 `objOne`，然后处理 `objTwo`。

### <a name="acceptable-pattern-three"></a>可接受的模式三

:::code language="csharp" id="three" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/stacked-await-usings.cs":::

在前面的示例中，每个异步清理操作都通过包含的方法主体隐式限定了范围。 在封闭块的末尾，`IAsyncDisposable` 实例执行其异步清理操作。 此运行顺序与它们声明的顺序相反，这意味着 `objTwo` 在 `objOne` 之前被处理。

### <a name="unacceptable-pattern"></a>无法接受的模式

如果从 `AnotherAsyncDisposable` 构造函数引发异常，则 `objOne` 不会得到正确处理：

:::code language="csharp" id="dontdothis" source="../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.asyncdisposable/stacked-await-usings.cs":::

> [!TIP]
> 避免此模式，因为它可能导致意外行为。

## <a name="see-also"></a>请参阅

有关 `IDisposable` 和 `IAsyncDisposable` 的双重实现示例，请参阅 [GitHub](https://github.com/dotnet/runtime/blob/035b729d829368c2790d825bd02db14f0c0fd2ea/src/libraries/System.Text.Json/src/System/Text/Json/Writer/Utf8JsonWriter.cs#L297-L345) 上的 <xref:System.Text.Json.Utf8JsonWriter> 源代码。

- <xref:System.IAsyncDisposable>
- <xref:System.IAsyncDisposable.DisposeAsync?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait(System.IAsyncDisposable,System.Boolean)>
