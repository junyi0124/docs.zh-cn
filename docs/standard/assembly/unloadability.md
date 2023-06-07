---
title: 如何在 .NET Core 中使用和调试程序集可卸载性
description: 了解如何使用可回收的 AssemblyLoadContext 来加载和卸载托管程序集，以及如何调试可能阻止卸载成功的问题。
author: janvorli
ms.author: janvorli
ms.date: 02/05/2019
ms.openlocfilehash: 9d1f604816dcbd7a84a3692b3cfd24481532789a
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865341"
---
# <a name="how-to-use-and-debug-assembly-unloadability-in-net-core"></a>如何在 .NET Core 中使用和调试程序集可卸载性

从 .NET Core 3.0 开始，支持加载和随后卸载一组程序集功能。 在 .NET Framework 中，自定义应用域用于此目的，但 .NET Core 仅支持单个默认应用域。

.NET Core 3.0 及更高版本支持通过 <xref:System.Runtime.Loader.AssemblyLoadContext> 进行卸载。 可将一组程序集加载到可回收的 `AssemblyLoadContext` 中，在其中执行方法或仅使用反射检查它们，最后卸载 `AssemblyLoadContext`。 这会卸载加载到 `AssemblyLoadContext` 中的程序集。

使用 `AssemblyLoadContext` 和使用 AppDomain 进行卸载之间存在一个值得注意的差异。 对于 Appdomain，卸载为强制执行。 卸载时，会中止目标 AppDomain 中运行的所有线程，会销毁目标 AppDomain 中创建的托管 COM 对象，等等。 对于 `AssemblyLoadContext`，卸载是“协作式的”。 调用 <xref:System.Runtime.Loader.AssemblyLoadContext.Unload%2A?displayProperty=nameWithType> 方法只是为了启动卸载。 以下目标达成后，卸载完成：

- 没有线程将程序集中的方法加载到其调用堆栈上的 `AssemblyLoadContext` 中。
- 程序集中的任何类型都不会加载到 `AssemblyLoadContext`，这些类型的实例本身由以下引用：
  - `AssemblyLoadContext` 外部的引用，弱引用（<xref:System.WeakReference> 或 <xref:System.WeakReference%601>）除外。
  - `AssemblyLoadContext` 内部和外部的强垃圾回收器 (GC) 句柄（[GCHandleType.Normal](xref:System.Runtime.InteropServices.GCHandleType.Normal) 或 [GCHandleType.Pinned](xref:System.Runtime.InteropServices.GCHandleType.Pinned)）。

## <a name="use-collectible-assemblyloadcontext"></a>使用可回收的 AssemblyLoadContext

本部分包含详细的分步教程，其中演示了一种简单方法，可以将 .NET Core 应用程序加载到可回收的 `AssemblyLoadContext` 中，执行其入口点，然后将其卸载。 可以在 [https://github.com/dotnet/samples/tree/master/core/tutorials/Unloading](https://github.com/dotnet/samples/tree/master/core/tutorials/Unloading) 中找到完整示例。

### <a name="create-a-collectible-assemblyloadcontext"></a>创建可回收的 AssemblyLoadContext

需要从 <xref:System.Runtime.Loader.AssemblyLoadContext> 派生类，并替代其 <xref:System.Runtime.Loader.AssemblyLoadContext.Load%2A?displayProperty=nameWithType> 方法。 该方法解析对所有程序集的引用，这些程序集是加载到该 `AssemblyLoadContext` 中的程序集的依赖项。

以下代码是最简单的自定义 `AssemblyLoadContext` 示例：

[!code-csharp[Simple custom AssemblyLoadContext](~/samples/snippets/standard/assembly/unloading/simple_example.cs#1)]

如你所见，`Load` 方法返回 `null`。 这意味着所有依赖项程序集都会加载到默认上下文中，而新上下文仅包含显式加载到其中的程序集。

若要在 `AssemblyLoadContext` 中加载部分或全部依赖项，可以在 `Load` 方法中使用 `AssemblyDependencyResolver`。 `AssemblyDependencyResolver` 将程序集名称解析为绝对程序集文件路径。 解析程序使用加载到上下文中的主程序集的目录中的 .deps.json 文件和程序集文件  。

[!code-csharp[Advanced custom AssemblyLoadContext](~/samples/snippets/standard/assembly/unloading/complex_assemblyloadcontext.cs)]

### <a name="use-a-custom-collectible-assemblyloadcontext"></a>使用自定义且可回收的 AssemblyLoadContext

本部分假定使用的是 `TestAssemblyLoadContext` 的较简单版本。

可以创建自定义 `AssemblyLoadContext` 实例并将程序集加载到其中，如下所示：

[!code-csharp[Part 1](~/samples/snippets/standard/assembly/unloading/simple_example.cs#3)]

对于已加载的程序集引用的每个程序集，将调用 `TestAssemblyLoadContext.Load` 方法，这样 `TestAssemblyLoadContext` 可以决定从何处获取程序集。 在示例中，它返回 `null` 以指示它应从运行时默认加载程序集的位置加载到默认上下文中。

现在程序集已加载，可从中执行方法。 运行 `Main` 方法：

[!code-csharp[Part 2](~/samples/snippets/standard/assembly/unloading/simple_example.cs#4)]

在 `Main` 方法返回后，可通过在自定义 `AssemblyLoadContext` 上调用 `Unload` 方法或删除对 `AssemblyLoadContext` 的引用来启动卸载：

[!code-csharp[Part 3](~/samples/snippets/standard/assembly/unloading/simple_example.cs#5)]

这足以卸载测试程序集。 将所有上述内容放入单独的非可内联方法中，以确保 `TestAssemblyLoadContext`、`Assembly` 和 `MethodInfo` (`Assembly.EntryPoint`) 无法通过堆栈槽引用（实际或 JIT 引入的本地变量）保持活动状态。 这可以使 `TestAssemblyLoadContext` 保持活动状态并阻止其卸载。

此外，返回对 `AssemblyLoadContext` 的弱引用，以便之后可以使用它来检测卸载是否已完成。

[!code-csharp[Part 4](~/samples/snippets/standard/assembly/unloading/simple_example.cs#2)]

现在，可运行此函数以加载、执行和卸载程序集。

[!code-csharp[Part 5](~/samples/snippets/standard/assembly/unloading/simple_example.cs#6)]

但是，卸载不会立即完成。 如上所述，它依赖于垃圾回收器来收集测试程序集中的所有对象。 在许多情况下，没有必要等待卸载完成。 但是，某些情况下，了解卸载已经完成非常有用。 例如，你可能希望删除从磁盘加载到自定义 `AssemblyLoadContext` 中的程序集文件。 在这种情况下，可以使用以下代码片段。 它会触发垃圾回收并等待循环中挂起的终结器，直到自定义 `AssemblyLoadContext` 的弱引用被设置为 `null`，表示已收集目标对象。 在大多数情况下，只需要通过循环传递一次。 但对于更复杂的情况，由 `AssemblyLoadContext` 中运行的代码所创建的对象具有终结器，则可能需要更多传递。

[!code-csharp[Part 6](~/samples/snippets/standard/assembly/unloading/simple_example.cs#7)]

### <a name="the-unloading-event"></a>卸载事件

某些情况下，加载到自定义 `AssemblyLoadContext` 中的代码可能需要在启动卸载时执行一些清理。 例如，可能需要停止线程或清理某些强 GC 句柄。 在这种情况下可以使用 `Unloading` 事件。 执行必要清理的处理程序可与此事件挂钩。

### <a name="troubleshoot-unloadability-issues"></a>解决可卸载性问题

由于卸载的协作性质，很容易忘记可能使内容在可回收的 `AssemblyLoadContext` 中保持活动状态并阻止卸载的引用。 以下是可以包含引用实体（其中一些不明显）的摘要：

- 保存于可回收的 `AssemblyLoadContext` 外部、存储在堆栈槽或处理器寄存器（由用户代码显式创建或由实时 (JIT) 编译器隐式创建的方法本地变量）中的常规引用、静态变量或强（固定）GC 句柄以及以可传递的方式指向：
  - 加载到可回收的 `AssemblyLoadContext` 中的程序集。
  - 此类程序集中的类型。
  - 此类程序集中的类型的实例。
- 从加载到可回收的 `AssemblyLoadContext` 程序集中运行代码的线程。
- 在可回收的 `AssemblyLoadContext` 中创建的自定义非可回收 `AssemblyLoadContext` 类型的实例。
- 将回调设置为自定义 `AssemblyLoadContext` 中的方法的挂起 <xref:System.Threading.RegisteredWaitHandle> 实例。

> [!TIP]
> 在以下情况下可能会出现存储在堆栈槽或处理器寄存器中、可阻止卸载 `AssemblyLoadContext` 的对象引用：
>
> - 当直接将函数调用结果传递给另一个函数时，即使没有用户创建的本地变量。
> - 当 JIT 编译器保留对可在方法中的某个点使用的对象的引用时。

## <a name="debug-unloading-issues"></a>调试卸载问题

调试卸载问题可能比较繁琐。 你可能会遇到这样的情况：你不知道哪些内容可以使 `AssemblyLoadContext` 保持活动状态，但卸载会失败。 帮助解决此问题的最佳武器是带有 SOS 插件的 WinDbg（Unix 上的 LLDB）。 需要查找哪些内容使属于特定 `AssemblyLoadContext` 的 `LoaderAllocator` 保持活动状态。 SOS 插件可让你查看 GC 堆对象、其层次结构和根。

若要将插件加载到调试器中，请在调试器命令行中输入以下命令：

在 WinDbg（似乎 WinDbg 在进入 .NET Core 应用程序时会自动执行此操作）中：

```console
.loadby sos coreclr
```

在 LLDB 中：

```console
plugin load /path/to/libsosplugin.so
```

调试卸载时出现问题的示例程序。 源代码包含在以下内容中。 在 WinDbg 下运行它时，程序将在尝试检查卸载是否成功后立即进入调试器。 然后即可开始查找原因。

> [!TIP]
> 如果使用 Unix 上的 LLDB 进行调试，则以下示例中的 SOS 命令的前面没有 `!`。

```console
!dumpheap -type LoaderAllocator
```

此命令转储类型名称包含 GC 堆中的 `LoaderAllocator` 的所有对象。 下面是一个示例：

```console
         Address               MT     Size
000002b78000ce40 00007ffadc93a288       48
000002b78000ceb0 00007ffadc93a218       24

Statistics:
              MT    Count    TotalSize Class Name
00007ffadc93a218        1           24 System.Reflection.LoaderAllocatorScout
00007ffadc93a288        1           48 System.Reflection.LoaderAllocator
Total 2 objects
```

在下面的“Statistics:”部分中，检查属于 `System.Reflection.LoaderAllocator` 的 `MT` (`MethodTable`)，这是我们关注的对象。 然后在开头的列表中，查找 `MT` 匹配该条目的条目，并获取对象本身的地址。 在示例中，其为“000002b78000ce40”。

现在我们知道了 `LoaderAllocator` 对象的地址，可使用另一个命令来查找其 GC 根：

```console
!gcroot -all 0x000002b78000ce40
```

此命令转储通向 `LoaderAllocator` 实例的对象引用链。 该列表以根（使 `LoaderAllocator` 保持活动状态的实体）开头，因此为问题的核心。 根可以是堆栈槽、处理器寄存器、GC 句柄或静态变量。

以下是 `gcroot` 命令输出的示例：

```console
Thread 4ac:
    000000cf9499dd20 00007ffa7d0236bc example.Program.Main(System.String[]) [E:\unloadability\example\Program.cs @ 70]
        rbp-20: 000000cf9499dd90
            ->  000002b78000d328 System.Reflection.RuntimeMethodInfo
            ->  000002b78000d1f8 System.RuntimeType+RuntimeTypeCache
            ->  000002b78000d1d0 System.RuntimeType
            ->  000002b78000ce40 System.Reflection.LoaderAllocator

HandleTable:
    000002b7f8a81198 (strong handle)
    -> 000002b78000d948 test.Test
    -> 000002b78000ce40 System.Reflection.LoaderAllocator

    000002b7f8a815f8 (pinned handle)
    -> 000002b790001038 System.Object[]
    -> 000002b78000d390 example.TestInfo
    -> 000002b78000d328 System.Reflection.RuntimeMethodInfo
    -> 000002b78000d1f8 System.RuntimeType+RuntimeTypeCache
    -> 000002b78000d1d0 System.RuntimeType
    -> 000002b78000ce40 System.Reflection.LoaderAllocator

Found 3 roots.
```

下一步是确定根所在的位置以便对其进行修复。 最简单的情况是根为堆栈槽或处理器寄存器。 在这种情况下，`gcroot` 会显示其框架包含根的函数的名称以及执行该函数的线程。 困难的情况是根为静态变量或 GC 句柄。

在上面的示例中，第一个根为 `System.Reflection.RuntimeMethodInfo` 类型的本地变量，存储在函数 `example.Program.Main(System.String[])` 的框架中，地址为 `rbp-20`（`rbp` 意为处理器寄存器 `rbp`，-20 意为该寄存器的十六进制偏移量）。

第二个根为普通（强）`GCHandle`，其包含对 `test.Test` 类实例的引用。

第三个根为固定的 `GCHandle`。 这实际上是一个静态变量，但遗憾的是，无法进行澄清。 引用类型的静态变量存储在内部运行时结构中的托管对象数组中。

另一种可阻止卸载 `AssemblyLoadContext` 的情况为，线程具有来源于加载到其堆栈上的 `AssemblyLoadContext` 程序集的方法框架。 可通过转储所有线程的托管调用堆栈进行检查：

```console
~*e !clrstack
```

此命令表示“将 `!clrstack` 命令应用于所有线程”。 以下是此示例中该命令的输出。 遗憾的是，Unix 上的 LLDB 无法将命令应用于所有线程，因此，必须手动切换线程并重复 `clrstack` 命令。 忽略调试器显示“无法审核托管堆栈”的所有线程。

```console
OS Thread Id: 0x6ba8 (0)
        Child SP               IP Call Site
0000001fc697d5c8 00007ffb50d9de12 [HelperMethodFrame: 0000001fc697d5c8] System.Diagnostics.Debugger.BreakInternal()
0000001fc697d6d0 00007ffa864765fa System.Diagnostics.Debugger.Break()
0000001fc697d700 00007ffa864736bc example.Program.Main(System.String[]) [E:\unloadability\example\Program.cs @ 70]
0000001fc697d998 00007ffae5fdc1e3 [GCFrame: 0000001fc697d998]
0000001fc697df28 00007ffae5fdc1e3 [GCFrame: 0000001fc697df28]
OS Thread Id: 0x2ae4 (1)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x61a4 (2)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x7fdc (3)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x5390 (4)
Unable to walk the managed stack. The current thread is likely not a
managed thread. You can run !threads to get a list of managed threads in
the process
Failed to start stack walk: 80070057
OS Thread Id: 0x5ec8 (5)
        Child SP               IP Call Site
0000001fc70ff6e0 00007ffb5437f6e4 [DebuggerU2MCatchHandlerFrame: 0000001fc70ff6e0]
OS Thread Id: 0x4624 (6)
        Child SP               IP Call Site
GetFrameContext failed: 1
0000000000000000 0000000000000000
OS Thread Id: 0x60bc (7)
        Child SP               IP Call Site
0000001fc727f158 00007ffb5437fce4 [HelperMethodFrame: 0000001fc727f158] System.Threading.Thread.SleepInternal(Int32)
0000001fc727f260 00007ffb37ea7c2b System.Threading.Thread.Sleep(Int32)
0000001fc727f290 00007ffa865005b3 test.Program.ThreadProc() [E:\unloadability\test\Program.cs @ 17]
0000001fc727f2c0 00007ffb37ea6a5b System.Threading.Thread.ThreadMain_ThreadStart()
0000001fc727f2f0 00007ffadbc4cbe3 System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object)
0000001fc727f568 00007ffae5fdc1e3 [GCFrame: 0000001fc727f568]
0000001fc727f7f0 00007ffae5fdc1e3 [DebuggerU2MCatchHandlerFrame: 0000001fc727f7f0]

```

如你所见，最后一个线程具有 `test.Program.ThreadProc()`。 这是从加载到 `AssemblyLoadContext` 的程序集中的函数，因此它使 `AssemblyLoadContext` 保持活动状态。

## <a name="example-source-with-unloadability-issues"></a>具有可卸载性问题的示例源

上面的调试示例中使用了以下代码。

### <a name="main-testing-program"></a>主要测试程序

[!code-csharp[Main testing program](~/samples/snippets/standard/assembly/unloading/unloadability_issues_example_main.cs)]

## <a name="program-loaded-into-the-testassemblyloadcontext"></a>程序已加载到 TestAssemblyLoadContext 中

以下代码表示传递给主测试程序中 `ExecuteAndUnload` 方法的 test.dll。

[!code-csharp[Program loaded into the TestAssemblyLoadContext](~/samples/snippets/standard/assembly/unloading/unloadability_issues_example_test.cs)]
