---
title: 实现 Dispose 方法
description: 本文介绍如何实现 Dispose 方法，该方法用于释放 .NET 中的代码使用的非托管资源。
ms.date: 09/08/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Dispose method
- garbage collection, Dispose method
ms.assetid: eb4e1af0-3b48-4fbc-ad4e-fc2f64138bf9
ms.openlocfilehash: ec731ff4c1020100c2b7ff5041d42291141a5209
ms.sourcegitcommit: 6d1ae17e60384f3b5953ca7b45ac859ec6d4c3a0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "94982402"
---
# <a name="implement-a-dispose-method"></a>实现 Dispose 方法

实现 <xref:System.IDisposable.Dispose%2A> 方法主要用于释放非托管资源。 处理 <xref:System.IDisposable> 实现的实例成员时，通常会级联 <xref:System.IDisposable.Dispose%2A> 调用。 实现 <xref:System.IDisposable.Dispose%2A> 有其他原因，例如，为了释放已分配的内存、删除已添加到集合中的项，或发出释放已获取的锁的信号。

[.NET 垃圾回收器](index.md)不会分配或释放非托管内存。 对象释放模式（称为“释放模式”）会对对象生存期强制施加顺序。 释放模式用于实现 <xref:System.IDisposable> 接口的对象，在与文件和管道句柄、注册表句柄、等待句柄或指向非托管内存块的指针交互时较为常见。 这是因为垃圾回收器无法回收非托管对象。

若要帮助确保始终适当地清理资源，<xref:System.IDisposable.Dispose%2A> 方法应为幂等，这样可以多次调用而不引发异常。 此外，<xref:System.IDisposable.Dispose%2A> 的后续调用不应执行任何操作。

为 <xref:System.GC.KeepAlive%2A?displayProperty=nameWithType> 方法提供的代码示例演示了垃圾回收如何引起终结器运行，而对该对象或其成员的非托管引用仍在使用中。 利用 <xref:System.GC.KeepAlive%2A?displayProperty=nameWithType>使对象从当前例程开始到调用此方法的那一刻为止都不适合进行垃圾回收，这是可行的。

## <a name="safe-handles"></a>安全句柄

编写对象终结器的代码是一项复杂的任务，如果处理不好可能会出现问题。 因此，建议你构造 <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> 对象，而非实现终结器。

<xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> 是一种抽象托管类型，该类型包装了可标识非托管资源的 <xref:System.IntPtr?displayProperty=nameWithType>。 在 Windows 上，它可能标识一个句柄，而在 Unix 上则可能标识一个文件描述符。 它提供了所有必要的逻辑，以确保在处理 `SafeHandle` 或删除对 `SafeHandle` 的所有引用并最终完成 `SafeHandle` 实例时，只释放该资源一次。

<xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> 是抽象基类。 派生类会为不同类型的句柄提供特定实例。 这些派生类可验证 <xref:System.IntPtr?displayProperty=nameWithType> 的哪些值被视为无效，以及如何实际释放句柄。 例如，<xref:Microsoft.Win32.SafeHandles.SafeFileHandle> 派生自 `SafeHandle` 以包装可标识打开的文件句柄/描述符的 `IntPtrs`，并重写其 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle?displayProperty=nameWithType> 方法来关闭它（通过 Unix 上的 `close` 函数或 Windows 上的 `CloseHandle` 函数）。 .NET 库中创建非托管资源的大多数 API 会将其包装在 `SafeHandle` 中，并根据需要返回此 `SafeHandle`，而不是返回原始指针。 在与非托管组件进行交互并获取非托管资源的 `IntPtr` 的情况下，你可以创建自己的 `SafeHandle` 类型进行包装。 因此，极少数非 `SafeHandle` 类型需要实现终结器；大多数可释放模式实现最终只包装其他受管理资源，其中某些资源可能是 `SafeHandle`。

<xref:Microsoft.Win32.SafeHandles> 命名空间中的以下派生类提供安全句柄：

- 用于文件、内存映射文件和管道的 <xref:Microsoft.Win32.SafeHandles.SafeFileHandle>、<xref:Microsoft.Win32.SafeHandles.SafeMemoryMappedFileHandle> 和 <xref:Microsoft.Win32.SafeHandles.SafePipeHandle> 类。
- 用于内存视图的 <xref:Microsoft.Win32.SafeHandles.SafeMemoryMappedViewHandle> 类。
- 用于加密构造的 <xref:Microsoft.Win32.SafeHandles.SafeNCryptKeyHandle>、<xref:Microsoft.Win32.SafeHandles.SafeNCryptProviderHandle> 和 <xref:Microsoft.Win32.SafeHandles.SafeNCryptSecretHandle> 类。
- 用于注册表项的 <xref:Microsoft.Win32.SafeHandles.SafeRegistryHandle> 类。
- 用于等待句柄的 <xref:Microsoft.Win32.SafeHandles.SafeWaitHandle> 类。

## <a name="dispose-and-disposebool"></a>Dispose() 和 Dispose(bool)

<xref:System.IDisposable> 接口需要实现单个无参数的方法 <xref:System.IDisposable.Dispose%2A>。 此外，任何非密封类都应具有要实现的附加 `Dispose(bool)` 重载方法：

- 一种没有参数的 `public` 非虚拟的（Visual Basic 中的 `NonInheritable`）<xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 实现。

- `protected virtual`（Visual Basic 中为 `Overridable`）`Dispose` 方法，其签名为：

  [!code-csharp[Conceptual.Disposable#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/dispose1.cs#8)]
  [!code-vb[Conceptual.Disposable#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/dispose1.vb#8)]

  > [!IMPORTANT]
  > 从终结器调用时，`disposing` 参数应为 `false`，从 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 方法调用时应为 `true`。 换言之，确定情况下调用时为 `true`，而在不确定情况下调用时为 `false`。

### <a name="the-dispose-method"></a>Dispose() 方法

由于 `public`、非虚拟（Visual Basic 中为 `NonInheritable`）、无参数的 `Dispose` 方法由该类型的使用者调用，因此其用途是释放非托管资源，执行常规清理，以及指示终结器（如果存在）不必运行。 释放与托管对象关联的实际内存始终是[垃圾回收器](index.md)的域。 因此，它具有标准实现：

[!code-csharp[Conceptual.Disposable#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/dispose1.cs#7)]
[!code-vb[Conceptual.Disposable#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/dispose1.vb#7)]

`Dispose` 方法执行所有对象清理，使垃圾回收器不再需要调用对象的 <xref:System.Object.Finalize%2A?displayProperty=nameWithType> 重写。 因此，调用 <xref:System.GC.SuppressFinalize%2A> 方法会阻止垃圾回收器运行终结器。 如果类型没有终结器，则对 <xref:System.GC.SuppressFinalize%2A?displayProperty=nameWithType> 的调用不起作用。 请注意，实际清除由 `Dispose(bool)` 方法重载执行。

### <a name="the-disposebool-method-overload"></a>Dispose(bool) 方法重载

在重载中，`disposing` 参数是一个 <xref:System.Boolean>，它指示方法调用是来自 <xref:System.IDisposable.Dispose%2A> 方法（其值为 `true`）还是来自终结器（其值为 `false`）。

方法的主体包含两个代码块：

- 释放非托管资源的块。 无论 `disposing` 参数的值如何，都会执行此块。
- 释放托管资源的条件块。 如果 `disposing` 的值为 `true`，则执行此块。 它释放的托管资源可包括：

  - **实现 <xref:System.IDisposable> 的托管对象。** 可用于调用其 <xref:System.IDisposable.Dispose%2A> 实现（级联释放）的条件块。 如果你已使用 <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType> 的派生类来包装非托管资源，则应在此处调用 <xref:System.Runtime.InteropServices.SafeHandle.Dispose?displayProperty=nameWithType> 实现。

  - **占用大量内存或使用短缺资源的托管对象。** 将大型托管对象引用分配到 `null`，使它们更有可能无法访问。 相比以非确定性方式回收它们，这样做释放的速度更快，此操作通常在条件块之外完成。

如果方法调用来自终结器，则应仅执行释放非托管资源的代码。 实施者负责确保假路径不会与可能已被回收的托管对象交互。 这一点很重要，因为垃圾回收器在终止期间销毁托管对象的顺序是不确定的。

## <a name="cascade-dispose-calls"></a>级联释放调用

如果你的类拥有一个字段或属性，并且其类型实现 <xref:System.IDisposable>，则包含类本身还应实现 <xref:System.IDisposable>。 实例化 <xref:System.IDisposable> 实现并将其存储为实例成员的类，也负责清理。 这是为了帮助确保引用的可释放类型可通过 <xref:System.IDisposable.Dispose%2A> 方法明确执行清理。 在本例中，类为 `sealed`（Visual Basic 中为 `NotInheritable`）。

[!code-csharp[Conceptual.Disposable#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/disposable1.cs#1)]
[!code-vb[Conceptual.Disposable#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/disposable1.vb#1)]

## <a name="implement-the-dispose-pattern"></a>实现释放模式

所有非密封类或（未修改为 `NotInheritable` 的 Visual Basic 类）都应被视为潜在的基类，因为它们可以被继承。 如果为任何潜在基类实现释放模式，则必须提供以下内容：

- 调用 <xref:System.IDisposable.Dispose%2A> 方法的 `Dispose(bool)` 实现。
- 执行实际清理的 `Dispose(bool)` 方法。
- 从包装非托管资源的 <xref:System.Runtime.InteropServices.SafeHandle> 派生的类（推荐），或对 <xref:System.Object.Finalize%2A?displayProperty=nameWithType> 方法的重写。 <xref:System.Runtime.InteropServices.SafeHandle> 类提供了终结器，因此你无需自行编写。

> [!IMPORTANT]
> 基类可以只引用托管对象，并实现释放模式。 在这些情况下，不需要终结器。 仅当直接引用非托管资源时，才需要终结器。

以下是一个常规模式，用于实现使用安全句柄的基类的释放模式：

[!code-csharp[System.IDisposable#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/base1.cs#3)]
[!code-vb[System.IDisposable#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/base1.vb#3)]

> [!NOTE]
> 上一个示例使用 <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> 对象阐释模式；可以使用派生自 <xref:System.Runtime.InteropServices.SafeHandle> 的任何对象来替代。 请注意，该示例不会正确实例化其 <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> 对象。

以下是一个常规模式，用于实现重写 <xref:System.Object.Finalize%2A?displayProperty=nameWithType> 的基类的释放模式。

[!code-csharp[System.IDisposable#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/base2.cs#5)]
[!code-vb[System.IDisposable#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/base2.vb#5)]

> [!TIP]
> 在 C# 中，通过重写 <xref:System.Object.Finalize%2A?displayProperty=nameWithType> 创建一个[终结器](../../csharp/programming-guide/classes-and-structs/destructors.md)。 在 Visual Basic 中，这是通过 `Protected Overrides Sub Finalize()` 完成的。

## <a name="implement-the-dispose-pattern-for-a-derived-class"></a>实现派生类的释放模式

从实现 <xref:System.IDisposable> 接口的类派生的类不应实现 <xref:System.IDisposable>，因为 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> 的基类实现由其派生类继承。 若要清理派生类，请提供以下内容：

- `protected override void Dispose(bool)` 方法，用于替代基类方法并执行派生类的实际清理。 此方法还必须调用基类的 `base.Dispose(bool)`（Visual Basic 中为 `MyBase.Dispose(bool)`）方法，并传递参数的释放状态。
- 从包装非托管资源的 <xref:System.Runtime.InteropServices.SafeHandle> 派生的类（推荐），或对 <xref:System.Object.Finalize%2A?displayProperty=nameWithType> 方法的重写。 <xref:System.Runtime.InteropServices.SafeHandle> 类提供了一个使你无需编写代码的终结器。 如果你提供了终结器，它必须调用 `disposing` 参数为 `false` 的 `Dispose(bool)` 重载。

以下是一个常规模式，用于实现使用安全句柄的派生类的释放模式：

[!code-csharp[System.IDisposable#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/derived1.cs#4)]
[!code-vb[System.IDisposable#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/derived1.vb#4)]

> [!NOTE]
> 上一个示例使用 <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> 对象阐释模式；可以使用派生自 <xref:System.Runtime.InteropServices.SafeHandle> 的任何对象来替代。 请注意，该示例不会正确实例化其 <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> 对象。

以下是一个常规模式，用于实现重写 <xref:System.Object.Finalize%2A?displayProperty=nameWithType> 的派生类的释放模式：

[!code-csharp[System.IDisposable#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.idisposable/cs/derived2.cs#6)]
[!code-vb[System.IDisposable#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.idisposable/vb/derived2.vb#6)]

## <a name="implement-the-dispose-pattern-with-safe-handles"></a>使用安全句柄实现释放模式

下面的示例阐释了基类 `DisposableStreamResource` 的释放模式，此模式使用安全句柄封装非托管资源。 它定义 `DisposableStreamResource` 类，该类使用 <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> 包装表示打开的文件的 <xref:System.IO.Stream> 对象。 此类还包含一个属性 `Size`，该属性返回文件流中的总字节数。

[!code-csharp[Conceptual.Disposable#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/base1.cs#9)]
[!code-vb[Conceptual.Disposable#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/base1.vb#9)]

## <a name="implement-the-dispose-pattern-for-a-derived-class-with-safe-handles"></a>使用安全句柄实现派生类的释放模式

下面的示例阐释派生类 `DisposableStreamResource2` 的释放模式，该类继承自上一个示例中显示的 `DisposableStreamResource` 类。 此类额外添加一种方法（即 `WriteFileInfo`），并使用 <xref:Microsoft.Win32.SafeHandles.SafeFileHandle> 对象包装可写文件的句柄。

[!code-csharp[Conceptual.Disposable#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.disposable/cs/derived1.cs#10)]
[!code-vb[Conceptual.Disposable#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.disposable/vb/derived1.vb#10)]

## <a name="see-also"></a>请参阅

- <xref:System.GC.SuppressFinalize%2A>
- <xref:System.IDisposable>
- <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType>
- <xref:Microsoft.Win32.SafeHandles>
- <xref:System.Runtime.InteropServices.SafeHandle?displayProperty=nameWithType>
- <xref:System.Object.Finalize%2A?displayProperty=nameWithType>
- [定义和使用类和结构 (C++/CLI)](/cpp/dotnet/how-to-define-and-consume-classes-and-structs-cpp-cli)
