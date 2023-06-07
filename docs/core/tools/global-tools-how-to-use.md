---
title: 教程：安装和使用 .NET 全局工具
description: 了解如何安装和使用 .NET 工具作为全局工具。
ms.topic: tutorial
ms.date: 02/12/2020
ms.openlocfilehash: 01b773516da92fb16fb0f67fc6617e0c70d17c9d
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94633889"
---
# <a name="tutorial-install-and-use-a-net-global-tool-using-the-net-cli"></a>教程：使用 .NET CLI 安装和使用 .NET 全局工具

 本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

本教程介绍如何安装和使用全局工具。 使用在[本系列的第一个教程](global-tools-how-to-create.md)中创建的工具。

## <a name="prerequisites"></a>先决条件

* 完成[本系列的第一个教程](global-tools-how-to-create.md)。

## <a name="use-the-tool-as-a-global-tool"></a>使用该工具作为全局工具

1. 通过运行 microsoft.botsay 项目文件夹中的 [dotnet tool install](dotnet-tool-install.md) 命令，从包中安装该工具  ：

   ```dotnetcli
   dotnet tool install --global --add-source ./nupkg microsoft.botsay
   ```

   `--global` 参数指示 .NET CLI 将工具二进制文件安装在自动添加到 PATH 环境变量的默认位置中。

   `--add-source` 参数指示 .NET CLI 临时使用 ./nupkg 目录作为 NuGet 包的附加源数据源。 为包提供了唯一名称，以确保它仅位于 ./nupkg  目录中，而不是在 Nuget.org 站点上。

   输出显示用于调用该工具和已安装的版本的命令：

   ```console
   You can invoke the tool using the following command: botsay
   Tool 'microsoft.botsay' (version '1.0.0') was successfully installed.
   ```

1. 调用该工具：

   ```console
   botsay hello from the bot
   ```

   > [!NOTE]
   > 如果此命令失败，则可能需要打开新终端来刷新 PATH。

1. 通过运行 [dotnet tool uninstall](dotnet-tool-uninstall.md) 命令来删除该工具：

   ```dotnetcli
   dotnet tool uninstall -g microsoft.botsay
   ```

## <a name="use-the-tool-as-a-global-tool-installed-in-a-custom-location"></a>使用该工具作为自定义位置中安装的全局工具

1. 从包中安装该工具。

   在 Windows 上：

   ```dotnetcli
   dotnet tool install --tool-path c:\dotnet-tools --add-source ./nupkg microsoft.botsay
   ```

   在 Linux 或 macOS 上：

   ```dotnetcli
   dotnet tool install --tool-path ~/bin --add-source ./nupkg microsoft.botsay
   ```

   `--tool-path` 参数指示 .NET CLI 将工具二进制文件安装在指定位置中。 如果目录不存在，则会创建该目录。 此目录不会自动添加到 PATH 环境变量中。

   输出显示用于调用该工具和已安装的版本的命令：

   ```console
   You can invoke the tool using the following command: botsay
   Tool 'microsoft.botsay' (version '1.0.0') was successfully installed.
   ```

1. 调用该工具：

   在 Windows 上：

   ```console
   c:\dotnet-tools\botsay hello from the bot
   ```

   在 Linux 或 macOS 上：

   ```console
   ~/bin/botsay hello from the bot
   ```

1. 通过运行 [dotnet tool uninstall](dotnet-tool-uninstall.md) 命令来删除该工具：

   在 Windows 上：

   ```dotnetcli
   dotnet tool uninstall --tool-path c:\dotnet-tools microsoft.botsay
   ```

   在 Linux 或 macOS 上：

   ```dotnetcli
   dotnet tool uninstall --tool-path ~/bin microsoft.botsay
   ```

## <a name="troubleshoot"></a>疑难解答

如果在学习本教程时收到错误消息，请参阅[排查 .NET 工具使用问题](troubleshoot-usage-issues.md)。

## <a name="next-steps"></a>后续步骤

在本教程中，已将工具作为全局工具安装和使用。 若要安装和使用与本地工具相同的工具，请转到下一教程。

> [!div class="nextstepaction"]
> [安装和使用本地工具](local-tools-how-to-use.md)
