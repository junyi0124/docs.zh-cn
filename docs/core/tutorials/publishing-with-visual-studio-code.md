---
title: 使用 Visual Studio Code 发布 .NET 控制台应用程序
description: 了解如何使用 Visual Studio Code 和 .NET CLI 来创建运行 .NET 应用程序所需的一组文件。
ms.date: 11/17/2020
ms.openlocfilehash: 9cfe490203d2d3254103ad2f0a4c4ff74972ec64
ms.sourcegitcommit: 5114e7847e0ff8ddb8c266802d47af78567949cf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2020
ms.locfileid: "94915878"
---
# <a name="tutorial-publish-a-net-console-application-using-visual-studio-code"></a>教程：使用 Visual Studio Code 发布 .NET 控制台应用程序

本教程演示如何发布控制台应用，以便其他用户可以运行它。 发布应用程序会创建运行应用程序所需的一组文件。 若要部署文件，请将文件复制到目标计算机。

.NET CLI 用于发布应用，因此可以根据需要使用 Visual Studio Code 以外的代码编辑器来学习本教程。

## <a name="prerequisites"></a>先决条件

- 本教程适用于在[使用 Visual Studio Code 创建 .NET 控制台应用程序](with-visual-studio-code.md)中创建的控制台应用。

## <a name="publish-the-app"></a>发布应用

1. 启动 Visual Studio Code。

1. 打开在[使用 Visual Studio Code 创建 .NET 控制台应用程序](with-visual-studio-code.md)中创建的 HelloWorld 项目文件夹。

1. 从主菜单中选择“视图” > “终端” 。

   终端在 HelloWorld 文件夹中打开。

1. 运行下面的命令：

   ```dotnetcli
   dotnet publish --configuration Release
   ```

   默认生成配置为“调试”，因此此命令指定“版本”生成配置 。 版本生成配置的输出进行了完全优化，且具有最低限度的符号调试信息。

   该命令的输出类似于以下示例：

   ```output
   Microsoft (R) Build Engine version 16.7.0+b89cb5fde for .NET
   Copyright (C) Microsoft Corporation. All rights reserved.
     Determining projects to restore...
     All projects are up-to-date for restore.
     HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\HelloWorld.dll
     HelloWorld -> C:\Projects\HelloWorld\bin\Release\netcoreapp3.1\publish\
   ```

## <a name="inspect-the-files"></a>检查文件

默认情况下，发布过程中会创建依赖于框架的部署，在此类部署中，已发布的应用程序在已安装 .NET 运行时的计算机上运行。 若要运行已发布的应用，可以使用可执行文件，或从命令提示符中运行 `dotnet HelloWorld.dll` 命令。

在下面的步骤中，查看由发布过程创建的文件。

1. 在左侧导航栏中选择“资源管理器”。

1. 展开 bin/Release/net5.0/publish。

   :::image type="content" source="media/publishing-with-visual-studio-code/published-files-output.png" alt-text="显示已发布文件的资源管理器":::

   如下图所示，已发布的输出包括以下文件：

   * HelloWorld.deps.json

      这是应用程序的运行时依赖项文件。 它定义了运行该应用所需的 .NET 组件和库（包括包含该应用程序的动态链接库）。 有关详细信息，请参阅[运行时配置文件](https://github.com/dotnet/cli/blob/85ca206d84633d658d7363894c4ea9d59e515c1a/Documentation/specs/runtime-configuration-file.md)。

   * HelloWorld.dll

      这是应用程序的[依赖于框架的部署](../deploying/deploy-with-cli.md#framework-dependent-deployment)版本。 若要执行此动态链接库，请在命令提示符处输入 `dotnet HelloWorld.dll`。 这种运行应用的方法适用于安装了 .NET 运行时的任何平台。

   * HelloWorld.exe（在 Linux 上而不是在 macOS 上创建的 HelloWorld） 

      这是应用程序的[依赖于框架的可执行文件](../deploying/deploy-with-cli.md#framework-dependent-executable)版本。 文件特定于操作系统。

   * HelloWorld.pdb（对于部署是可选的）

      这是调试符号文件。 尽管应在需要调试应用程序的已发布版本时保存此文件，但无需将此文件与应用程序一起部署。

   * HelloWorld.runtimeconfig.json

      这是应用程序的运行时配置文件。 它标识用于运行应用程序的 .NET 版本。 还可向其添加配置选项。 有关详细信息，请参阅 [.NET 运行时配置设置](../run-time-config/index.md#runtimeconfigjson)。

## <a name="run-the-published-app"></a>运行已发布的应用

1. 在“资源管理器”中，右键单击“发布”文件夹（在 macOS 上按住 <kbd>Ctrl</kbd> 单击），然后选择“在终端中打开”。

   :::image type="content" source="media/publishing-with-visual-studio-code/open-in-terminal.png" alt-text="显示“在终端中打开”的上下文菜单":::

1. 在 Windows 或 Linux 上，使用可执行文件运行应用。

   1. 在 Windows 上，输入 `.\HelloWorld.exe`，然后按 <kbd>Enter</kbd>。

   1. 在 Linux 上，输入 `./HelloWorld`，然后按 <kbd>Enter</kbd>。

   1. 输入一个名字以响应提示，并按任意键退出。

1. 在任何平台上，使用 [`dotnet`](../tools/dotnet.md) 命令运行应用：

   1. 输入 `dotnet HelloWorld.dll`，然后按 <kbd>Enter</kbd>。

   1. 输入一个名字以响应提示，并按任意键退出。

## <a name="additional-resources"></a>其他资源

- [.NET 应用程序部署](../deploying/index.md)

## <a name="next-steps"></a>后续步骤

在本教程中，你发布了一个控制台应用。 在下一教程中，你将创建类库。

> [!div class="nextstepaction"]
> [使用 Visual Studio Code 创建 .NET 类库](library-with-visual-studio-code.md)
