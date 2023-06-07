---
title: 使用 Visual Studio for Mac 调试 .NET 控制台应用程序
description: 了解如何使用 Visual Studio Mac 调试 .NET 控制台应用。
ms.date: 11/30/2020
ms.openlocfilehash: e35bbef5582b6eec00ee48669a0f5d028eb051fb
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599611"
---
# <a name="tutorial-debug-a-net-console-application-using-visual-studio-for-mac"></a>教程：使用 Visual Studio for Mac 调试 .NET 控制台应用程序

本教程介绍了 Visual Studio for Mac 中提供的调试工具。

## <a name="prerequisites"></a>先决条件

- 本教程适用于在[使用 Visual Studio for Mac 创建 .NET 控制台应用程序](with-visual-studio-mac.md)中创建的控制台应用。

## <a name="use-debug-build-configuration"></a>使用“调试”生成配置

“调试”和“发布”是 Visual Studio 的内置生成配置 。 可使用“调试”生成配置进行调试，使用“发布”配置进行最终版本分发。

在“调试”配置中，程序使用完整符号调试信息编译，且不进行优化。 优化会使调试复杂化，因为源代码和生成的指令之间的关系更加复杂。 程序的发布配置进行了完全优化，且不包含任何符号调试信息。

默认情况下，Visual Studio for Mac 使用“调试”生成配置，因此不需要在调试之前对其进行更改。

1. 启动 Visual Studio for Mac。

1. 打开在[使用 Visual Studio for Mac 创建 .NET 控制台应用程序](with-visual-studio-mac.md)中创建的项目。

   当前的生成配置显示在工具栏上。 下面的工具栏图像显示 Visual Studio 配置为编译应用的“调试”版本：

   :::image type="content" source="media/debugging-with-visual-studio-mac/visual-studio-toolbar-debug.png" alt-text="突出显示“调试”的 Visual Studio 工具栏":::

## <a name="set-a-breakpoint"></a>设置断点

断点会在执行包含断点的代码行之前暂时中断执行应用程序。

1. 在显示名称、日期和时间的行上设置断点。 为此，请将光标放在代码行中，然后按 <kbd>⌘</kbd><kbd>\\</kbd> (<kbd>command</kbd>+<kbd>\\</kbd>)。 设置断点的另一种方法是从菜单中选择“运行” > “切换断点”。

   Visual Studio 通过突出显示此代码行并在左边缘显示红点来指示设置了断点的行。

   :::image type="content" source="media/debugging-with-visual-studio-mac/set-breakpoint-in-editor.png" alt-text="设置断点后的 Visual Studio 程序窗口":::

1. 按 <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>command</kbd>+<kbd>enter</kbd>) 以在调试模式下启动程序。 启动调试的另一种方法是从菜单中选择“运行” > “启动调试”。

1. 当程序提示输入名称时，在终端窗口中输入字符串，然后按 <kbd>Enter</kbd>。

1. 到达断点时，程序停止执行，然后执行 `Console.WriteLine` 方法。

   :::image type="content" source="media/debugging-with-visual-studio-mac/breakpoint-hit.png" alt-text="Visual Studio 中断点的屏幕截图":::

## <a name="use-the-immediate-window"></a>使用“即时”窗口

在“即时”窗口中，可以与正在调试的应用程序进行交互。 可以通过交互方式更改变量值，看看这样会对程序产生哪些影响。

1. 如果“即时”窗口不可见，请选择“视图” > “调试边栏” > “即时”来显示它。

1. 在“即时”窗口中输入 `name = "Gracie"`，然后按 <kbd>Enter</kbd>。

1. 在“即时”窗口中输入 `date = date.AddDays(1)`，然后按 <kbd>Enter</kbd>。

   “即时”窗口显示字符串变量的新值和 <xref:System.DateTime> 值的属性。

   :::image type="content" source="media/debugging-with-visual-studio-mac/immediate-window.png" alt-text="Visual Studio 中的“即时”窗口":::

   “局部变量”窗口显示当前正在执行的方法中定义的变量值。 “局部变量”窗口中会更新刚更改的变量的值。

   :::image type="content" source="media/debugging-with-visual-studio-mac/locals-window.png" alt-text="Visual Studio 中的“局部变量”窗口":::

1. 按 <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>command</kbd>+<kbd>enter</kbd>) 以继续调试。

   终端中显示的值对应于在“即时”窗口中所做的更改。

   如果看不到终端，请选择底部导航栏中的“终端 - HelloWorld”。

   :::image type="content" source="media/debugging-with-visual-studio-mac/terminal-hello-world.png" alt-text="底部导航栏中的“终端 - Hello World”":::

1. 按任意键退出程序。

1. 关闭终端窗口。

## <a name="set-a-conditional-breakpoint"></a>设置条件断点

程序显示用户输入的字符串。 如果用户没有输入任何内容，情况又如何呢？ 可以使用名为“条件断点”的有用调试功能对此进行测试。

1. 按 <kbd>Ctrl</kbd> 并单击表示断点的红点。 在上下文菜单中，选择“编辑断点”。

1. 在“编辑断点”对话框中，在遵循“且以下条件为 true”的字段中输入以下代码，然后选择“应用”  。

   ```csharp
   String.IsNullOrEmpty(name)
   ```

   :::image type="content" source="media/debugging-with-visual-studio-mac/breakpoint-settings.png" alt-text="显示断点设置面板的编辑器":::

   每次命中断点时，调试器都会调用 `String.IsNullOrEmpty(name)` 方法，仅当该方法调用返回 `true` 时，它才会在此行上中断。

   可以指定“命中次数”（而不是条件表达式），这样程序就会在语句的执行次数达到指定值时中断执行。

1. 按 <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>command</kbd>+<kbd>enter</kbd>) 以启动调试。

1. 在终端窗口中，在系统提示输入姓名时按 <kbd>Enter</kbd>。

   由于符合指定的条件（`name` 为 `null` 或 <xref:System.String.Empty?displayProperty=nameWithType>），因此程序会在到达断点时停止执行。

1. 选择“局部变量”窗口，其中显示当前正在执行的方法的局部变量值。 在这种情况下，`Main` 是当前正在执行的方法。 请注意，`name` 变量的值为 `""`，即 <xref:System.String.Empty?displayProperty=nameWithType>。

1. 还可以通过在“即时”窗口中输入 `name` 变量名称并按 <kbd>Enter</kbd> 来看到值为空字符串。

   :::image type="content" source="media/debugging-with-visual-studio-mac/immediate-window-output.png" alt-text="显示名称为空字符串的“即时”窗口":::

1. 按 <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>command</kbd>+<kbd>enter</kbd>) 以继续调试。

1. 在终端窗口中，按任意键退出程序。

1. 关闭终端窗口。

1. 单击代码窗口左边缘上的红点，清除断点。 清除断点的另一种方法是在选中代码行时选择“运行”>“切换断点”。

## <a name="step-through-a-program"></a>单步执行程序

使用 Visual Studio，还可以单步执行程序，并监视其执行情况。 通常可以设置断点，并通过程序代码的一小部分执行程序流。 由于此程序很小，因此可以单步执行整个程序。

1. 在标记 `Main` 方法的开头的大括号处设置断点（按 <kbd>command</kbd>+<kbd>\\</kbd>）。

1. 按 <kbd>⌘</kbd><kbd>↵</kbd> (<kbd>command</kbd>+<kbd>enter</kbd>) 以启动调试。

   Visual Studio 在包含断点的行上停止。

1. 按 <kbd>⇧</kbd><kbd>⌘</kbd><kbd>I</kbd> (<kbd>shift</kbd>+<kbd>command</kbd>+<kbd>I</kbd>) 或选择“运行” > “单步执行”以推进一行。

   Visual Studio 会在要执行的下一行旁边突出显示一个箭头。

   :::image type="content" source="media/debugging-with-visual-studio-mac/step-into-method.png" alt-text="Visual Studio 单步执行方法":::

   此时，“局部变量”窗口显示 `args` 数组为空，`name` 和 `date` 具有默认值。 此外，Visual Studio 还打开了一个空白终端。

1. 按 <kbd>⇧</kbd><kbd>⌘</kbd><kbd>I</kbd> (<kbd>shift</kbd>+<kbd>command</kbd>+<kbd>I</kbd>)。

   Visual Studio 突出显示包含 `name` 变量赋值的语句。 “局部变量”窗口显示 `name` 为 `null`，终端显示字符串“What is your name?”。

1. 在控制台窗口中输入字符串，然后按 <kbd>Enter</kbd>，从而响应提示。

1. 按 <kbd>⇧</kbd><kbd>⌘</kbd><kbd>I</kbd> (<kbd>shift</kbd>+<kbd>command</kbd>+<kbd>I</kbd>)。

   Visual Studio 突出显示包含 `date` 变量赋值的语句。 “局部变量”窗口显示 <xref:System.Console.ReadLine%2A?displayProperty=nameWithType> 方法调用返回的值。 终端显示在提示符处输入的字符串。

1. 按 <kbd>⇧</kbd><kbd>⌘</kbd><kbd>I</kbd> (<kbd>shift</kbd>+<kbd>command</kbd>+<kbd>I</kbd>)。

   “局部变量”窗口显示通过 <xref:System.DateTime.Now?displayProperty=nameWithType> 属性赋值后的 `date` 变量值。 终端无变化。

1. 按 <kbd>⇧</kbd><kbd>⌘</kbd><kbd>I</kbd> (<kbd>shift</kbd>+<kbd>command</kbd>+<kbd>I</kbd>)。

   Visual Studio 调用 <xref:System.Console.WriteLine(System.String,System.Object,System.Object)?displayProperty=nameWithType> 方法。 终端会显示格式化的字符串。

1. 按 <kbd>⇧</kbd><kbd>⌘</kbd><kbd>U</kbd> (<kbd>shift</kbd>+<kbd>command</kbd>+<kbd>U</kbd>) 或选择“运行” > “单步跳出”。

   终端会显示一条消息，并等待用户按任意键。

1. 按任意键退出程序。

## <a name="use-release-build-configuration"></a>使用“发布”生成配置

测试应用程序的“调试”版本后，还应该编译并测试“发布”版本。 发布版本包含编译器优化，可能会对应用程序的行为产生不良影响。 例如，旨在提升性能的编译器优化可能会在多线程应用程序中创建争用条件。

若要生成和测试控制台应用程序的发布版本，请执行以下步骤：

1. 将工具栏上的生成配置从“调试”更改为“发布”。

   :::image type="content" source="media/debugging-with-visual-studio-mac/visual-studio-toolbar-release.png" alt-text="突出显示“调试”的默认 Visual Studio 工具栏":::

1. 按 <kbd>⌥</kbd><kbd>⌘</kbd><kbd>↵</kbd> (<kbd>option</kbd>+<kbd>command</kbd>+<kbd>enter</kbd>) 以运行但不调试。

## <a name="next-steps"></a>后续步骤

在本教程中，你使用了 Visual Studio 调试工具。 在下一教程中，你将发布应用的可部署版本。

> [!div class="nextstepaction"]
> [使用 Visual Studio for Mac 发布 .NET 控制台应用程序](publishing-with-visual-studio-mac.md)
