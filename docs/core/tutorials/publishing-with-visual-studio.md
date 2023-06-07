---
title: 使用 Visual Studio 发布 .NET 控制台应用程序
description: 了解如何使用 Visual Studio 创建运行 .NET 应用程序所需的一组文件。
ms.date: 06/08/2020
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet
ms.openlocfilehash: b0c6bd85ddf86f0eb11c56f01abb74a7e9786363
ms.sourcegitcommit: 5114e7847e0ff8ddb8c266802d47af78567949cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2020
ms.locfileid: "94915992"
---
# <a name="tutorial-publish-a-net-console-application-using-visual-studio"></a>教程：使用 Visual Studio 发布 .NET 控制台应用程序

本教程演示如何发布控制台应用，以便其他用户可以运行它。 发布应用程序会创建运行应用程序所需的一组文件。 若要部署文件，请将文件复制到目标计算机。

## <a name="prerequisites"></a>先决条件

- 本教程适用于在[使用 Visual Studio 创建 .NET 控制台应用程序](with-visual-studio.md)中创建的控制台应用。

## <a name="publish-the-app"></a>发布应用

1. 启动 Visual Studio。

1. 打开在[使用 Visual Studio 创建 .NET 控制台应用程序](with-visual-studio.md)中创建的 HelloWorld 项目。

1. 请确保 Visual Studio 正在使用“发布”生成配置。 必要时，将工具栏上的生成配置设置从“调试”更改为“发布”。

   :::image type="content" source="media/publishing-with-visual-studio/visual-studio-toolbar-release.png" alt-text="选定发布版本的 Visual Studio 工具栏":::

1. 右键单击“HelloWorld”项目（而不是 HelloWorld 解决方案），然后选择菜单中的“发布”。

   :::image type="content" source="media/publishing-with-visual-studio/publish-context-menu.png" alt-text="Visual Studio 发布上下文菜单":::

1. 在“发布”页的“目标”选项卡上，选择“文件夹”，然后选择“下一步”   。

   :::image type="content" source="media/publishing-with-visual-studio/pick-publish-target.png" alt-text="在 Visual Studio 中选择发布目标":::

1. 在“发布”页的“特定目标”选项卡上，选择“文件夹”，然后选择“下一步”   。

   :::image type="content" source="media/publishing-with-visual-studio/pick-specific-publish-target.png" alt-text="在 Visual Studio 中选择特定发布目标":::

1. 在“发布”页的“位置”选项卡上，选择“完成”  。

   :::image type="content" source="media/publishing-with-visual-studio/publish-page-loc-tab.png" alt-text="Visual Studio“发布”页的“位置”选项卡":::

1. 在“发布”窗口的“发布”选项卡上，选择“发布”  。

   :::image type="content" source="media/publishing-with-visual-studio/publish-page.png" alt-text="Visual Studio 发布窗口":::

## <a name="inspect-the-files"></a>检查文件

默认情况下，发布过程会创建依赖于框架的部署，在此类部署中，已发布的应用程序在已安装 .NET 运行时的计算机上运行。 用户可以通过双击可执行文件或从命令提示符发出 `dotnet HelloWorld.dll` 命令来运行发布的应用。

在下面的步骤中，查看由发布过程创建的文件。

1. 在“解决方案资源管理器”中，选择“显示所有文件” 。

1. 在项目文件夹中，展开 bin/Release/net5.0/publish。

   :::image type="content" source="media/publishing-with-visual-studio/published-files-output.png" alt-text="显示已发布文件的解决方案资源管理器":::

   如下图所示，已发布的输出包括以下文件：

   * HelloWorld.deps.json

      这是应用程序的运行时依赖项文件。 该文件定义了运行应用所需的 .NET 组件和库（包括包含应用程序的动态链接库）。 有关详细信息，请参阅[运行时配置文件](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md)。

   * HelloWorld.dll

      这是应用程序的[依赖于框架的部署](../deploying/deploy-with-cli.md#framework-dependent-deployment)版本。 若要执行此动态链接库，请在命令提示符处输入 `dotnet HelloWorld.dll`。 这种运行应用的方法适用于安装了 .NET 运行时的任何平台。

   * *HelloWorld.exe*

      这是应用程序的[依赖于框架的可执行文件](../deploying/deploy-with-cli.md#framework-dependent-executable)版本。 若要运行该版本，请在命令提示符处输入 `HelloWorld.exe`。 文件特定于操作系统。

   * HelloWorld.pdb（对于部署是可选的）

      这是调试符号文件。 尽管应在需要调试应用程序的已发布版本时保存此文件，但无需将此文件与应用程序一起部署。

   * HelloWorld.runtimeconfig.json

      这是应用程序的运行时配置文件。 该文件标识用于运行应用程序的 .NET 版本。 还可向其添加配置选项。 有关详细信息，请参阅 [.NET 运行时配置设置](../run-time-config/index.md#runtimeconfigjson)。

## <a name="run-the-published-app"></a>运行已发布的应用

1. 在“解决方案资源管理器”中，右键单击“模型”文件夹，然后选择“复制完整路径”。

1. 打开命令提示符，然后导航到“发布”文件夹。 为此，请输入 `cd`，然后粘贴完整路径。 例如：

   ```console
   cd C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\publish\
   ```

1. 使用可执行文件运行应用：

   1. 输入 `HelloWorld.exe`，然后按 <kbd>Enter</kbd>。

   1. 输入一个名字以响应提示，并按任意键退出。

1. 使用 `dotnet` 命令运行应用：

   1. 输入 `dotnet HelloWorld.dll`，然后按 <kbd>Enter</kbd>。

   1. 输入一个名字以响应提示，并按任意键退出。

## <a name="additional-resources"></a>其他资源

- [.NET 应用程序部署](../deploying/index.md)

## <a name="next-steps"></a>后续步骤

在本教程中，你发布了一个控制台应用。 在下一教程中，你将创建类库。

> [!div class="nextstepaction"]
> [使用 Visual Studio 创建 .NET 类库](library-with-visual-studio.md)
