---
title: Main() 返回值 - C# 编程指南
description: 了解 Main() 返回值。 查看代码示例、编译器生成的代码和其他可用资源。
ms.date: 08/02/2017
helpviewer_keywords:
- Main method [C#], return values
ms.assetid: c2f5a1d8-1676-4bea-bc7e-44a97e72d5bc
ms.openlocfilehash: c7521f6aef79825a8cc20d5455588a2d684b9ccb
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "91609598"
---
# <a name="main-return-values-c-programming-guide"></a>Main() 返回值（C# 编程指南）

`Main` 方法可以返回 `void`：

 [!code-csharp[csProgGuideMain#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#12)]

还可以返回 `int`：

 [!code-csharp[csProgGuideMain#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#13)]

如果未使用 `Main` 的返回值，则返回 `void` 可以使代码变得略微简单。 但是，返回整数可使程序将状态信息传递给调用可执行文件的其他程序或脚本。 来自 `Main` 的返回值视为进程的退出代码。 如果从 `Main` 返回 `void`，则退出代码将为隐式 `0`。 以下示例演示了如何访问 `Main` 的返回值。

## <a name="example"></a>示例

此示例使用 [.NET Core](../../../core/introduction.md) 命令行工具。 如果不熟悉 .NET Core 命令行工具，可通过本[入门文章](../../../core/tutorials/with-visual-studio-code.md)进行了解。

修改 program.cs 中的 `Main` 方法，如下所示  ：

 [!code-csharp[csProgGuideMain#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#14)]

在 Windows 中执行程序时，从 `Main` 函数返回的任何值都存储在环境变量中。 可使用批处理文件中的 `ERRORLEVEL` 或 PowerShell 中的 `$LastExitCode` 来检索此环境变量。

可使用 [dotnet CLI](../../../core/tools/dotnet.md) `dotnet build` 命令构建应用程序。

接下来，创建一个 PowerShell 脚本来运行应用程序并显示结果。 将以下代码粘贴到文本文件中，并在包含该项目的文件夹中将其另存为 `test.ps1`。 可通过在 PowerShell 提示符下键入 `test.ps1` 来运行 PowerShell 脚本。

因为代码返回零，所以批处理文件将报告成功。 但是，如果将 MainReturnValTest.cs 更改为返回非零值，然后重新编译程序，则 PowerShell 脚本的后续执行将报告为失败。

```dotnetcli
dotnet run
```

```powershell
if ($LastExitCode -eq 0) {
    Write-Host "Execution succeeded"
} else
{
    Write-Host "Execution Failed"
}
Write-Host "Return value = " $LastExitCode
```

## <a name="sample-output"></a>示例输出

```txt
Execution succeeded
Return value = 0
```

## <a name="async-main-return-values"></a>Async Main 返回值

Async Main 返回值将在 `Main` 中调用异步方法时所需的样本代码移动到编译器生成的代码中。 以前，需要编写此结构来调用异步代码并确保程序运行至异步操作完成：

```csharp
public static void Main()
{
    AsyncConsoleWork().GetAwaiter().GetResult();
}

private static async Task<int> AsyncConsoleWork()
{
    // Main body here
    return 0;
}
```

现在，可以将其替代为：

[!code-csharp[AsyncMain](../../../../samples/snippets/csharp/main-arguments/program.cs#AsyncMain)]

新语法的优点是编译器始终生成正确的代码。

## <a name="compiler-generated-code"></a>编译器生成的代码

当应用程序入口点返回 `Task` 或 `Task<int>` 时，编译器生成一个新的入口点，该入口点调用应用程序代码中声明的入口点方法。 假设此入口点名为 `$GeneratedMain`，编译器将为这些入口点生成以下代码：

- `static Task Main()` 导致编译器发出 `private static void $GeneratedMain() => Main().GetAwaiter().GetResult();` 的等效项
- `static Task Main(string[])` 导致编译器发出 `private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();` 的等效项
- `static Task<int> Main()` 导致编译器发出 `private static int $GeneratedMain() => Main().GetAwaiter().GetResult();` 的等效项
- `static Task<int> Main(string[])` 导致编译器发出 `private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();` 的等效项

> [!NOTE]
>如果示例在 `Main` 方法上使用 `async` 修饰符，则编译器将生成相同的代码。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [C# 参考](../index.md)
- [Main() 和命令行参数](index.md)
- [如何显示命令行参数](./how-to-display-command-line-arguments.md)
