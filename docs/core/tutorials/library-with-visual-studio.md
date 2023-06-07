---
title: 使用 Visual Studio 创建 .NET 类库
description: 了解如何使用 Visual Studio 创建 .NET 类库。
ms.date: 08/07/2020
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet,contperf-fy21q1
ms.openlocfilehash: 2d9b02a155c950b77565a66417948568f5fa039f
ms.sourcegitcommit: d0990c1c1ab2f81908360f47eafa8db9aa165137
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513115"
---
# <a name="tutorial-create-a-net-class-library-using-visual-studio"></a>教程：使用 Visual Studio 创建 .NET 类库

在本教程中，将创建包含一个字符串处理方法的简单类库。

类库定义的是可以由应用程序调用的类型和方法。 如果库以 .NET Standard 2.0 为目标，则支持 .NET Standard 2.0 的任何 .NET 实现（包括 .NET Framework）均可调用该库。 如果库以 .NET 5 为目标，则以 .NET 5 为目标的任何应用程序均可调用该库。 本教程演示如何以 .NET 5 为目标。

创建类库后，可将其作为 NuGet 包或作为与使用该类库的应用程序捆绑在一起的组件进行分发。

## <a name="prerequisites"></a>必备条件

- 安装了具有“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019 版本 16.8 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。 选择此工作负载时，将自动安装 .NET 5.0 SDK。 本教程假设已启用“在‘新建项目’中显示所有 .NET Core 模板”，如[教程：使用 Visual Studio 创建 .NET 控制台应用程序](with-visual-studio.md)中所示。

## <a name="create-a-solution"></a>创建解决方案

首先，创建一个空白解决方案来放置类库项目。 Visual Studio 解决方案用作一个或多个项目的容器。 将其他相关项目添加到同一个解决方案中。

创建空白解决方案：

1. 启动 Visual Studio。

2. 在“开始”窗口上，选择“创建新项目”。

3. 在“创建新项目”页面上，在搜索框中输入“解决方案”。 选择“空白解决方案”模板，然后选择“下一步”。

   :::image type="content" source="media/library-with-visual-studio/blank-solution.png" alt-text="Visual Studio 中的空白解决方案模板":::

4. 在“配置新项目”页面上，在“项目名称”框中输入“ClassLibraryProjects”。 然后选择“创建”。

## <a name="create-a-class-library-project"></a>创建类库项目

1. 将名为“StringLibrary”的新 .NET 类库项目添加到解决方案。

   1. 在“解决方案资源管理器”中，右键单击解决方案并选择“添加” > “新建项目”  。

   1. 在“创建新项目”页面上，在搜索框中输入“库”。 从“语言”列表中选择“C#”或“Visual Basic”，然后从“平台”列表中选择“所有平台”。 选择“类库”模板，然后选择“下一步” 。

   1. 在“配置新项目”页的“项目名称”框中，输入“StringLibrary”，然后选择“下一步”   。

   1. 在“其他信息”页上，选择“.NET 5.0 (当前)”，然后选择“创建”  。

1. 请检查以确保库以 .NET 的正确版本为目标。 右键单击“解决方案资源管理器”中的库项目，然后选择“属性”。 “目标框架”文本框显示项目以 .NET 5.0 为目标。

1. 如果使用 Visual Basic，请清除“根命名空间”文本框中的文本。

   :::image type="content" source="./media/library-with-visual-studio/vb/library-project-properties.png" alt-text="类库的项目属性":::

   对于每个项目，Visual Basic 会自动创建一个与项目名称对应的命名空间。 在本教程中，通过使用代码文件中的 [`namespace`](../../visual-basic/language-reference/statements/namespace-statement.md) 关键字定义顶级命名空间。

1. 将 Class1.cs 或 Class1.vb 代码窗口中的代码替换为以下代码，并保存文件 。 如果未显示想要使用的语言，请更改页面顶部的语言选择器。

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibrary/Class1.cs":::
   :::code language="vb" source="./snippets/library-with-visual-studio/vb/StringLibrary/Class1.vb":::

   类库 `UtilityLibraries.StringLibrary`包含一个名为 `StartsWithUpper` 的方法。 此方法会返回 <xref:System.Boolean> 值，以指明当前字符串实例是否以大写字符开头。 Unicode 标准会区分大小写字符。 如果为大写字符，<xref:System.Char.IsUpper(System.Char)?displayProperty=nameWithType> 方法返回 `true`。

   `StartsWithUpper` 以[扩展方法](../../csharp/programming-guide/classes-and-structs/extension-methods.md)的形式进行实现，这样就可以将其作为 <xref:System.String> 类成员进行调用。

1. 在菜单栏上，选择“生成” > “生成解决方案”或按 <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>B</kbd>，验证项目是否编译正确 。

## <a name="add-a-console-app-to-the-solution"></a>向解决方案添加控制台应用

添加使用类库的控制台应用程序。 应用将提示用户输入字符串，并报告字符串是否以大写字符开头。

1. 将名为“ShowCase”的新 .NET 控制台应用程序添加到解决方案。

   1. 在“解决方案资源管理器”中右键单击解决方案并选择“添加” > “新建项目”。

   1. 在“创建新项目”页面，在搜索框中输入“控制台”。 从“语言”列表中选择“C#”或“Visual Basic”，然后从“平台”列表中选择“所有平台”。

   1. 选择“控制台应用程序”模板，然后选择“下一步” 。

   1. 在“配置新项目”页面，在“项目名称”框中输入“ShowCase”。 然后选择“下一步”  。

   1. 在“其他信息”页上，选择“目标框架”框中的“.NET 5.0 (当前)”  。 然后选择“创建”。

1. 在“Program.cs”或“Program.vb”文件的代码窗口中，将所有代码替换为以下代码 。

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/ShowCase/Program.cs":::
   :::code language="vb" source="./snippets/library-with-visual-studio/vb/ShowCase/Program.vb":::

   该代码使用 `row` 变量来维护写入到控制台窗口的数据行数计数。 如果大于或等于 25，该代码将清除控制台窗口，并向用户显示一条消息。

   该程序会提示用户输入字符串。 它会指明字符串是否以大写字符开头。 如果用户没有输入字符串就按 <kbd>Enter</kbd> 键，那么应用程序会终止，控制台窗口会关闭。

## <a name="add-a-project-reference"></a>添加项目引用

最初，新的控制台应用项目无权访问类库。 若要允许该项目调用类库中的方法，可以创建对类库项目的项目引用。

1. 在“解决方案资源管理器”中，右键单击 `ShowCase` 项目的“依赖项”节点，并选择“添加项目引用”  。

   :::image type="content" source="media/library-with-visual-studio/add-reference-context-menu.png" alt-text="在 Visual Studio 中添加引用上下文菜单":::

1. 在“引用管理器”对话框中，选择“StringLibrary”项目，然后选择“确定”按钮  。

   :::image type="content" source="media/library-with-visual-studio/manage-project-references.png" alt-text="选择了“StringLibrary”的“引用管理器”对话框":::

## <a name="run-the-app"></a>运行应用

1. 在“**解决方案资源管理器**”中，右键单击“**ShowCase**”项目，在上下文菜单中选择“**设为启动项目**”。

   :::image type="content" source="media/library-with-visual-studio/set-startup-project-context-menu.png" alt-text="Visual Studio 中用于设置启动项目的项目上下文菜单":::

1. 按 Ctrl+F5 编译并运行程序，而不进行调试<kbd></kbd><kbd></kbd>。

   :::image type="content" source="media/library-with-visual-studio/visual-studio-project-toolbar.png" alt-text="Visual Studio 中显示“调试”按钮的项目工具栏":::

1. 输入字符串并按 Enter 以试用程序，然后按 Enter 退出<kbd></kbd><kbd></kbd>。

   :::image type="content" source="media/library-with-visual-studio/run-showcase.png" alt-text="运行展示的控制台窗口":::

## <a name="additional-resources"></a>其他资源

* [使用 .NET CLI 开发库](libraries.md)
* [.NET Standard 版本及其支持的平台](../../standard/net-standard.md)。

## <a name="next-steps"></a>后续步骤

在本教程中，你创建了一个类库。 在下一教程中，你将了解如何对类库进行单元测试。

> [!div class="nextstepaction"]
> [使用 Visual Studio 对 .NET 类库进行单元测试](testing-library-with-visual-studio.md)

或者，你可以跳过自动单元测试，并了解如何通过创建 NuGet 包来共享库：

> [!div class="nextstepaction"]
> [使用 Visual Studio 创建和发布包](/nuget/quickstart/create-and-publish-a-package-using-visual-studio)

同时，还可以了解如何发布控制台应用。 如果从本教程中创建的解决方案发布控制台应用，类库将以 .dll 文件的形式随附。

> [!div class="nextstepaction"]
> [使用 Visual Studio 发布 .NET 控制台应用程序](publishing-with-visual-studio.md)
