---
title: 使用 Visual Studio Code 调试 .NET 控制台应用程序
description: 了解如何使用 Visual Studio Code 调试 .NET 控制台应用。
ms.date: 05/26/2020
ms.openlocfilehash: 7215ed4a93b31ebac313c04708734667148c4e02
ms.sourcegitcommit: 30fef5b0ed76e334377d28fa8e80159b29353e10
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "96556104"
---
# <a name="tutorial-debug-a-net-console-application-using-visual-studio-code"></a>教程：使用 Visual Studio Code 调试 .NET 控制台应用程序

本教程介绍了 Visual Studio Code 中可用于处理 .NET 应用的调试工具。

## <a name="prerequisites"></a>先决条件

- 本教程适用于在[使用 Visual Studio Code 创建 .NET 控制台应用程序](with-visual-studio-code.md)中创建的控制台应用。

## <a name="use-debug-build-configuration"></a>使用“调试”生成配置

“调试”和“发布”是 .NET Core 的内置生成配置 。 可使用“调试”生成配置进行调试，使用“发布”配置进行最终版本分发。

在“调试”配置中，程序使用完整符号调试信息编译，且不进行优化。 优化会使调试复杂化，因为源代码和生成的指令之间的关系更加复杂。 程序的发布配置进行了完全优化，且不包含任何符号调试信息。

默认情况下，Visual Studio Code 启动设置使用“调试”生成配置，因此不需要在调试之前对其进行更改。

1. 启动 Visual Studio Code。

1. 打开在[使用 Visual Studio Code 中创建 .NET 控制台应用程序](with-visual-studio-code.md)中创建的项目的文件夹。

## <a name="set-a-breakpoint"></a>设置断点

断点会在执行包含断点的代码行之前暂时中断执行应用程序。

1. 打开 *Program.cs* 文件。

1. 单击代码窗口的左边缘，在显示名称、日期和时间的行上设置断点。 左边缘在行号的左侧。 设置断点的其他方法是在选中代码行时按 <kbd>F9</kbd> 或从菜单中选择“运行” > “切换断点” 。

   Visual Studio Code 通过在左边缘显示红点来指示设置了断点的行。

   :::image type="content" source="media/debugging-with-visual-studio-code/breakpoint-set.png" alt-text="断点集":::

## <a name="set-up-for-terminal-input"></a>设置终端输入

断点位于 `Console.ReadLine` 方法调用之后。 调试控制台不接受正在运行的程序的终端输入。 若要在调试时处理终端输入，可以使用集成终端（Visual Studio Code 窗口之一）或外部终端。 本教程中使用集成终端。

1. 打开 .vscode/launch.json。

1. 将 `console` 设置从 `internalConsole` 更改为 `integratedTerminal`：

   ```json
   "console": "integratedTerminal",
   ```

1. 保存更改。

## <a name="start-debugging"></a>“启动调试”

1. 选择左侧菜单上的“调试”图标，打开“调试”视图。

   :::image type="content" source="media/debugging-with-visual-studio-code/select-debug-pane.png" alt-text="在 Visual Studio Code 中打开“调试”选项卡":::

1. 选择窗格顶部 .NET Core Launch（控制台）旁边的绿色箭头。 在调试模式下启动程序的其他方法是，按 <kbd>F5</kbd> 或从菜单中选择“运行” > “启动调试” 。

   :::image type="content" source="media/debugging-with-visual-studio-code/start-debugging.png" alt-text="开始调试":::

1. 选择“终端”选项卡以查看程序在等待响应之前 显示的“What is your name?”提示。

   :::image type="content" source="media/debugging-with-visual-studio-code/select-terminal.png" alt-text="选择“终端”选项卡":::

1. 在“终端”窗口中输入字符串以响应输入姓名提示，然后按 <kbd>Enter</kbd>。

   到达断点时，程序停止执行，然后执行 `Console.WriteLine` 方法。 “变量”窗口的“局部变量”部分显示当前正在执行的方法中定义的变量值 。

   :::image type="content" source="media/debugging-with-visual-studio-code/breakpoint-hit.png" alt-text="命中断点，显示局部变量":::

## <a name="use-the-debug-console"></a>使用“调试控制台”

在“调试控制台”窗口中，可以与正在调试的应用程序进行交互。 可更改变量值，看看这样会对程序产生哪些影响。

1. 选择“调试控制台”选项卡。

1. 在“调试控制台”窗口底部的提示符处输入 `name = "Gracie"`，然后按 <kbd>Enter</kbd>。

   :::image type="content" source="media/debugging-with-visual-studio-code/change-variable-values.png" alt-text="更改变量值":::

1. 在“调试控制台”窗口底部输入 `date = DateTime.Parse("2019-11-16T17:25:00Z").ToUniversalTime()`，然后按 <kbd>Enter</kbd>。

   “变量”窗口显示 `name` 和 `date` 变量的新值。

1. 选择工具栏中的“继续”按钮继续执行程序。 继续操作的另一种方法是按 <kbd>F5</kbd>。

   :::image type="content" source="media/debugging-with-visual-studio-code/continue-debugging.png" alt-text="继续调试":::

1. 再次选择“终端”选项卡。

   控制台窗口中显示的值对应于在“调试控制台”窗口中所做的更改。

   :::image type="content" source="media/debugging-with-visual-studio-code/changed-variable-values.png" alt-text="显示输入值的终端":::

1. 按任意键，退出应用程序并停止调试。

## <a name="set-a-conditional-breakpoint"></a>设置条件断点

程序显示用户输入的字符串。 如果用户没有输入任何内容，情况又如何呢？ 可以使用名为“条件断点”的有用调试功能对此进行测试。

1. 右键单击（在 macOS 上按住 <kbd>Ctrl</kbd> 并单击）表示断点的红点。 在上下文菜单中，选择“编辑断点”，打开可输入条件表达式的对话框。

   :::image type="content" source="media/debugging-with-visual-studio-code/breakpoint-context-menu.png" alt-text="断点上下文菜单":::

1. 在下拉菜单中选择 `Expression`，输入以下条件表达式，并按 <kbd>Enter</kbd>。

   ```csharp
   String.IsNullOrEmpty(name)
   ```

   :::image type="content" source="media/debugging-with-visual-studio-code/conditional-expression.png" alt-text="输入条件表达式":::

   每次命中断点时，调试器都会调用 `String.IsNullOrEmpty(name)` 方法，仅当该方法调用返回 `true` 时，它才会在此行上中断。

   可以指定命中次数（而不是条件表达式），这样程序就会在语句的执行次数达到指定值时中断执行。 另一种方法是指定筛选条件，这样就可以根据诸如线程标识符、进程名称或线程名称之类的特性来中断程序执行。

1. 通过按 F5<kbd></kbd> 调试来启动程序。

1. 在“终端”选项卡中，在系统提示输入姓名时按 <kbd>Enter</kbd>。

   由于符合指定的条件（`name` 为 `null` 或 <xref:System.String.Empty?displayProperty=nameWithType>），因此程序会在到达断点时以及在 `Console.WriteLine` 方法执行之前停止执行。

   “变量”窗口显示 `name` 变量的值为 `""` 或 <xref:System.String.Empty?displayProperty=nameWithType>。

1. 在“调试控制台”提示符中输入下面的语句并按 <kbd>Enter</kbd>，确认值为空字符串。 结果为 `true`。

   ```csharp
   name == String.Empty
   ```

   :::image type="content" source="media/debugging-with-visual-studio-code/expression-in-debug-console.png" alt-text="在执行语句后返回 true 值的调试控制台":::

1. 选择工具栏上的“继续”按钮，继续执行程序。

1. 选择“终端”选项卡，然后按任意键退出程序，停止调试。

1. 单击代码窗口左边缘上的点，清除断点。 清除断点的其他方法是在选中代码行时按 <kbd>F9</kbd> 或从菜单中选择“运行”>“切换断点”。

1. 如果收到断点条件将丢失的警告，请选择“删除断点”。

## <a name="step-through-a-program"></a>单步执行程序

使用 Visual Studio Code，还可以单步执行程序，并监视其执行情况。 通常可以设置断点，并通过程序代码的一小部分执行程序流。 由于此程序很小，因此可以单步执行整个程序。

1. 在 `Main` 方法的左大括号处设置一个断点。

1. 按 <kbd>F5</kbd> 启动调试。

   Visual Studio Code 突出显示断点行。

   此时，“变量”窗口显示 `args` 数组为空，`name` 和 `date` 具有默认值。

1. 选择“运行” > “单步执行”或按 <kbd>F11</kbd>。

   :::image type="content" source="media/debugging-with-visual-studio-code/step-into.png" alt-text="“单步执行”按钮":::

   Visual Studio Code 突出显示下一行。

1. 选择“运行” > “单步执行”或按 <kbd>F11</kbd>。

   Visual Studio Code 执行名称提示的 `Console.WriteLine` 并突出显示下一执行行。 下一行是 `name` 的 `Console.ReadLine`。 “变量”窗口保持不变，“终端”选项卡显示“What is your name? ” 提示。

1. 选择“运行” > “单步执行”或按 <kbd>F11</kbd>。

   Visual Studio 突出显示 `name` 变量赋值。 “变量”窗口显示 `name` 仍为 `null`。

1. 在“终端”选项卡中输入字符串，然后按 <kbd>Enter</kbd>，响应提示。

   输入字符串时，“终端”选项卡可能无法显示输入的字符串，但 <xref:System.Console.ReadLine%2A?displayProperty=nameWithType> 方法将捕获你的输入。

1. 选择“运行” > “单步执行”或按 <kbd>F11</kbd>。

   Visual Studio Code 突出显示 `date` 变量赋值。 “变量”窗口显示 <xref:System.Console.ReadLine%2A?displayProperty=nameWithType> 方法调用返回的值。 “终端”选项卡显示在提示符处输入的字符串。

1. 选择“运行” > “单步执行”或按 <kbd>F11</kbd>。

   “变量”窗口显示通过 <xref:System.DateTime.Now?displayProperty=nameWithType> 属性赋值后的 `date` 变量值。

1. 选择“运行” > “单步执行”或按 <kbd>F11</kbd>。

   Visual Studio Code 调用 <xref:System.Console.WriteLine(System.String,System.Object,System.Object)?displayProperty=nameWithType> 方法。 控制台窗口会显示格式化的字符串。

1. 选择“运行” > “跳出”或按 <kbd>Shift</kbd>+<kbd>F11</kbd>。

   :::image type="content" source="media/debugging-with-visual-studio-code/step-out.png" alt-text="“跳出”按钮":::

1. 选择“终端”选项卡。

   终端显示“按任意键退出…”

1. 按任意键退出程序。

## <a name="use-release-build-configuration"></a>使用“发布”生成配置

测试应用程序的“调试”版本后，还应该编译并测试“发布”版本。 发布版本包含编译器优化，这些优化可能会影响应用程序的行为。 例如，旨在提升性能的编译器优化可能会在多线程应用程序中创建争用条件。

若要构建和测试控制台应用程序的发布版本，请打开终端，并运行以下命令：

```dotnetcli
dotnet run --configuration Release
```

## <a name="additional-resources"></a>其他资源

* [在 Visual Studio Code 中进行调试](https://code.visualstudio.com/docs/editor/debugging)

## <a name="next-steps"></a>后续步骤

在本教程中，使用了 Visual Studio Code 调试工具。 在下一教程中，你将发布应用的可部署版本。

> [!div class="nextstepaction"]
> [使用 Visual Studio Code 发布 .NET 控制台应用程序](publishing-with-visual-studio-code.md)
