---
title: 平台调用 (P/Invoke)
description: 了解如何在 .NET 中通过 P/Invoke 调用本机函数。
ms.date: 01/18/2019
ms.openlocfilehash: fa8b43edfba50fbc620f257c4e7caf1673f83235
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2020
ms.locfileid: "75706300"
---
# <a name="platform-invoke-pinvoke"></a>平台调用 (P/Invoke)

P/Invoke 是可用于从托管代码访问非托管库中的结构、回调和函数的一种技术。 大多数 P/Invoke API 包含在以下两个命名空间中：`System` 和 `System.Runtime.InteropServices`。 使用这两个命名空间可提供用于描述如何与本机组件通信的工具。

我们从最常见的示例着手。该示例在托管代码中调用非托管函数。 让我们从命令行应用程序显示一个消息框：

[!code-csharp[MessageBox](~/samples/snippets/standard/interop/pinvoke/messagebox.cs)]

上述示例非常简单，但确实演示了从托管代码调用非托管函数所需执行的操作。 让我们逐步分析该示例：

- 第 1 行显示 `System.Runtime.InteropServices` 命名空间（用于保存全部所需项）的 using 语句。
- 第 7 行引入 `DllImport` 属性。 此特性至关重要，因为它告诉运行时要加载非托管 DLL。 传入的字符串是目标函数所在的 DLL。 此外，它还指定哪些[字符集](./charset.md)用于封送字符串。 最后，它指定此函数调用 [SetLastError](/windows/desktop/api/errhandlingapi/nf-errhandlingapi-setlasterror)，且运行时应捕获相应错误代码，以便用户能够通过 <xref:System.Runtime.InteropServices.Marshal.GetLastWin32Error?displayProperty=nameWithType> 检索它。
- 第 8 行显示了 P/Invoke 的关键作用。 它定义了一个托管方法，该方法的签名与非托管方法**完全相同**。 可以看到，声明中包含一个新关键字 `extern`，告诉运行时这是一个外部方法。调用该方法时，运行时应在 `DllImport` 特性中指定的 DLL 内查找该方法。

该示例的剩余部分无非就是调用该方法，就像调用其他任何托管方法一样。

在 macOS 上也可以使用类似的示例。 需要更改 `DllImport` 属性中的库名称，因为 macOS 使用不同的方案来命名动态库。 下面的示例使用 `getpid(2)` 函数获取应用程序的进程 ID，然后控制台上列显该 ID：

[!code-csharp[getpid macOS](~/samples/snippets/standard/interop/pinvoke/getpid-macos.cs)]

它在 Linux 上也是类似的。 函数名称相同，因为 `getpid(2)` 是标准 [POSIX](https://en.wikipedia.org/wiki/POSIX) 系统调用。

[!code-csharp[getpid Linux](~/samples/snippets/standard/interop/pinvoke/getpid-linux.cs)]

## <a name="invoking-managed-code-from-unmanaged-code"></a>从非托管代码调用托管代码

运行时允许通信流量双向流通，这样，便可以使用函数指针从本机函数回调托管代码。 在托管代码中，与函数指针最接近的功能就是**委托**，正是凭借这个功能，才能从本机代码回调托管代码。

此功能的使用方式类似于上面所述的从托管代码调用本机进程。 对于给定的回调，需要定义一个与签名匹配的委托，并将其传入外部方法。 运行时将负责处理所有剩余工作。

[!code-csharp[EnumWindows](~/samples/snippets/standard/interop/pinvoke/enumwindows.cs)]

在演练示例之前，最好是回顾一下所要使用的非托管函数的签名。 要调用以枚举所有窗口的函数具有以下签名：`BOOL EnumWindows (WNDENUMPROC lpEnumFunc, LPARAM lParam);`

第一个参数是回调。 该回调具有以下签名：`BOOL CALLBACK EnumWindowsProc (HWND hwnd, LPARAM lParam);`

现在，让我们来演练示例：

- 示例中的第 9 行定义与非托管代码中回调签名匹配的委托。 请注意如何在托管代码中使用 `IntPtr` 表示 LPARAM 和 HWND 类型。
- 第 13 和 14 行从 user32.dll 库中引入 `EnumWindows` 函数。
- 第 17 - 20 行实现该委托。 在这个简单的示例中，我们只要将句柄输出到控制台。
- 最后，第 24 行调用外部方法并传入委托。

下面显示了 Linux 和 macOS 示例。 在这些平台上，我们可以使用 C 库 `libc` 中的 `ftw` 函数。 此函数用于遍历目录层次结构，它使用指向某个函数的指针作为其参数之一。 该函数具有以下签名：`int (*fn) (const char *fpath, const struct stat *sb, int typeflag)`。

[!code-csharp[ftw Linux](~/samples/snippets/standard/interop/pinvoke/ftw-linux.cs)]

macOS 示例使用相同的函数，唯一的差别在于 `DllImport` 特性的自变量，因为 macOS 将 `libc` 保留在不同的位置。

[!code-csharp[ftw macOS](~/samples/snippets/standard/interop/pinvoke/ftw-macos.cs)]

上面两个示例都依赖于参数，在这两种情况下，参数是作为托管类型提供的。 运行时将采取“适当的措施”，在另一个平台上将这些代码处理成等效的代码。 [类型封送](type-marshaling.md)页介绍了如何将类型封送到本机代码。

## <a name="more-resources"></a>更多资源

- [PInvoke.net wiki](https://www.pinvoke.net/) 是一个出色的 Wiki 站点，其中提供了有关常用 Windows API 以及如何调用这些 API 的信息。
- [C++/CLI 中的 P/Invoke](/cpp/dotnet/native-and-dotnet-interoperability)
- [有关 P/Invoke 的 Mono 文档](https://www.mono-project.com/docs/advanced/pinvoke/)
