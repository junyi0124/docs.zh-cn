---
title: 使用 Visual Studio for Mac 创建 .NET 类库
description: 了解如何使用 Visual Studio for Mac 创建 .NET 类库。
ms.date: 11/30/2020
ms.openlocfilehash: 1b6b26de06d18d505fa6dde3ff9779a3dab3f1e6
ms.sourcegitcommit: 9d525bb8109216ca1dc9e39c149d4902f4b43da5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/04/2020
ms.locfileid: "96599288"
---
# <a name="tutorial-create-a-net-class-library-using-visual-studio-for-mac"></a>教程：使用 Visual Studio for Mac 创建 .NET 类库

在本教程中，将创建包含一个字符串处理方法的类库。

类库定义的是可以由应用程序调用的类型和方法。 如果库以 .NET Standard 2.0 为目标，则支持 .NET Standard 2.0 的任何 .NET 实现（包括 .NET Framework）均可调用该库。 如果库以 .NET 5 为目标，则以 .NET 5 为目标的任何应用程序均可调用该库。 本教程演示如何以 .NET 5 为目标。

> [!NOTE]
> 你的反馈非常有价值。 有两种方法可以向开发团队提供有关 Visual Studio for Mac 的反馈：
>
> - 在 Visual Studio for Mac 中，从菜单选择“帮助” > “报告问题”，或从欢迎屏幕中选择“报告问题”，将打开一个窗口，以供填写 bug 报告。 可在[开发人员社区](https://aka.ms/feedback/report?space=41)门户中跟踪自己的反馈。
> - 若要提出建议，从菜单中选择“帮助” > “提供建议”，或从欢迎屏幕中选择“提供建议”，转到 [Visual Studio for Mac 开发人员社区网页](https://aka.ms/feedback/suggest?space=41)。

## <a name="prerequisites"></a>先决条件

* [安装 Visual Studio for Mac 版本 8.8 或更高版本](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)。 选择用于安装 .NET Core 的选项。 安装 Xamarin 对于 .NET 开发而言是可选项。 有关更多信息，请参见以下资源：

  * [教程：安装 Visual Studio for Mac](/visualstudio/mac/installation)。
  * [支持的 macOS 版本](../install/macos.md)。
  * [Visual Studio for Mac 支持的 .NET 版本](/visualstudio/mac/net-core-support)。

## <a name="create-a-solution-with-a-class-library-project"></a>创建包含类库项目的解决方案

Visual Studio 解决方案用作一个或多个项目的容器。 创建解决方案和解决方案中的类库项目。 稍后将其他相关项目添加到同一个解决方案中。

1. 启动 Visual Studio for Mac。

1. 在“开始”窗口中，选择“新建项目”。

1. 在“为新项目选择一个模板”对话框中选择“Web 和控制台” > “库” > “类库”然后选择“下一步”。

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-new-project.png" alt-text="“新建项目”对话框":::

1. 在“配置新的类库”对话框中，选择“.NET 5.0”，然后选择“下一步”。

1. 将项目命名为“StringLibrary”，并将解决方案命名为“ClassLibraryProjects”。 使“在解决方案目录中创建项目目录”保持选中状态。 选择“创建”。

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-new-project-options.png" alt-text="Visual Studio for Mac“新建项目”对话框选项":::

1. 从主菜单中，选择“视图” > “解决方案”，然后选择“停靠”图标使边栏保持打开状态。

   :::image type="content" source="media/library-with-visual-studio-mac/solution-dock-icon.png" alt-text="“解决方案”边栏的“停靠”图标":::

1. 在“解决方案”边栏中，展开 `StringLibrary` 节点以显示模板提供的类文件 *Class1.cs*。 按住 <kbd>Ctrl</kbd> 并单击该文件，从上下文菜单中选择“重命名”，然后将该文件重命名为“StringLibrary.cs”。 打开文件并将内容替换为以下代码：

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibrary/Class1.cs":::

1. 按 <kbd>⌘</kbd><kbd>S</kbd> (<kbd>command</kbd>+<kbd>S</kbd>) 以保存文件。

1. 在 IDE 窗口底部边距处选择“错误”，打开“错误”面板。 选择“生成输出”按钮。

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-error-button.png" alt-text="显示“错误”按钮的 Visual Studio Mac IDE 底部边距处":::

1. 从菜单中选择“生成” > “生成所有”。

   生成解决方案。 生成输出面板显示生成成功。

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-build-panel.png" alt-text="“错误”面板的 Visual Studio Mac 生成输出面板，其中显示生成成功消息":::

## <a name="add-a-console-app-to-the-solution"></a>向解决方案添加控制台应用

添加使用类库的控制台应用程序。 应用将提示用户输入字符串，并报告字符串是否以大写字符开头。

1. 在“解决方案”边栏中，按住 <kbd>Ctrl</kbd> 并单击 `ClassLibraryProjects` 解决方案。 通过从“Web 和控制台” > “应用”模板中选择模板来添加新的“控制台应用程序”项目，然后选择“下一步”。

1. 选择“.NET Core 5.0”作为“目标框架”，然后选择“下一步”。

1. 将项目命名为“ShowCase”。 选择“创建”以在解决方案中创建项目。

   :::image type="content" source="media/library-with-visual-studio-mac/add-showcase-project.png" alt-text="添加 ShowCase 项目":::

1. 打开 *Program.cs* 文件。 将代码替换为以下代码：

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/ShowCase/Program.cs":::

   该程序会提示用户输入字符串。 它会指明字符串是否以大写字符开头。 如果用户没有输入字符串就按 <kbd>Enter</kbd> 键，那么应用程序会终止，控制台窗口会关闭。

   该代码使用 `row` 变量来维护写入到控制台窗口的数据行数计数。 如果大于或等于 25，该代码将清除控制台窗口，并向用户显示一条消息。

## <a name="add-a-project-reference"></a>添加项目引用

最初，新的控制台应用项目无权访问类库。 若要允许该项目调用类库中的方法，可以创建对类库项目的项目引用。

1. 在“解决方案”边栏中，按住 <kbd>Ctrl</kbd> 并单击新“ShowCase”项目的“依赖项”节点。 在上下文菜单中，选择“添加引用”。

1. 在“引用”对话框中，选择“StringLibrary”，然后选择“确定”。

## <a name="run-the-app"></a>运行应用

1. 按住 Ctrl<kbd></kbd> 并单击 ShowCase 项目，然后从上下文菜单中选择“运行项目”。

1. 输入字符串并按 <kbd>Enter</kbd> 以试用程序，然后按 <kbd>Enter</kbd> 退出。

   :::image type="content" source="media/library-with-visual-studio-mac/visual-studio-mac-console-window.png" alt-text="Visual Studio for Mac 控制台窗口，显示正在运行的应用":::

## <a name="additional-resources"></a>其他资源

* [使用 .NET CLI 开发库](libraries.md)
* [Visual Studio 2019 for Mac 发行说明](/visualstudio/releasenotes/vs2019-mac-relnotes)
* [.NET Standard 版本及其支持的平台](../../standard/net-standard.md)。

## <a name="next-steps"></a>后续步骤

在本教程中，你创建了一个解决方案和一个库项目，并添加了一个使用该库的控制台应用项目。 在下一教程中，将向解决方案中添加单元测试项目。

> [!div class="nextstepaction"]
> [使用 Visual Studio for Mac 测试 .NET 类库](testing-library-with-visual-studio-mac.md)
