---
title: 教程：安装和使用 .NET 本地工具
description: 了解如何安装和使用 .NET 工具作为本地工具。
ms.topic: tutorial
ms.date: 02/12/2020
ms.openlocfilehash: 2cb25443706293b66325d43136afcd3fd886294d
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94633876"
---
# <a name="tutorial-install-and-use-a-net-local-tool-using-the-net-cli"></a>教程：使用 .NET CLI 安装和使用 .NET 本地工具

 本文适用于： ✔️ .NET Core 3.0 SDK 及更高版本

本教程介绍如何安装和使用本地工具。 使用在[本系列的第一个教程](global-tools-how-to-create.md)中创建的工具。

## <a name="prerequisites"></a>先决条件

* 完成[本系列的第一个教程](global-tools-how-to-create.md)。
* 安装 .NET Core 2.1 运行时。

  在本教程中，安装和使用面向 .NET Core 2.1 的工具，因此需要在计算机上安装该运行时。 若要安装 2.1 运行时，请转到 [.NET Core 2.1 下载页面](https://dotnet.microsoft.com/download/dotnet-core/2.1)并在“运行应用 - 运行时”  列中查找运行时安装链接。

## <a name="create-a-manifest-file"></a>创建清单文件

若要安装仅用于本地访问的工具（对于当前目录和子目录），必须将其添加到清单文件。

在“microsoft.botsay”文件夹中，向上导航一个级别到“repository”文件夹   ：

```console
cd ..
```

通过运行 [dotnet new](dotnet-new.md) 命令来创建清单文件：

```dotnetcli
dotnet new tool-manifest
```

输出指示文件创建成功。

```console
The template "Dotnet local tool manifest file" was created successfully.
```

.config/dotnet-tools.json 文件中尚无工具  ：

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {}
}
```

清单文件中列出的工具可用于当前目录和子目录。 当前目录是包含具有清单文件的 .config 目录的目录  。

使用引用本地工具的 CLI 命令时，SDK 会在当前目录和父目录中搜索清单文件。 如果它找到清单文件，但该文件不包含所引用的工具，则会通过父目录继续向上搜索。 搜索在找到所引用的工具或找到将 `isRoot` 设置为 `true` 的清单文件时结束。

## <a name="install-botsay-as-a-local-tool"></a>将 botsay 作为本地工具安装

从在第一个教程中创建的包中安装该工具：

```dotnetcli
dotnet tool install --add-source ./microsoft.botsay/nupkg microsoft.botsay
```

此命令将该工具添加到在上一步中创建的清单文件。 命令输出显示新安装的工具所在的清单文件：

 ```console
 You can invoke the tool from this directory using the following command:
 'dotnet tool run botsay' or 'dotnet botsay'
 Tool 'microsoft.botsay' (version '1.0.0') was successfully installed.
 Entry is added to the manifest file /home/name/repository/.config/dotnet-tools.json
 ```

.config/dotnet-tools.json 文件现在有一个工具  ：

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {
    "microsoft.botsay": {
      "version": "1.0.0",
      "commands": [
        "botsay"
      ]
    }
  }
}
```

## <a name="use-the-tool"></a>使用该工具

通过运行“repository”  文件夹中的 `dotnet tool run` 命令来调用该工具：

```dotnetcli
dotnet tool run botsay hello from the bot
```

## <a name="restore-a-local-tool-installed-by-others"></a>还原其他人安装的本地工具

通常将本地工具安装在存储库的根目录中。 将清单文件签入到存储库后，其他开发人员可以获得最新的清单文件。 若要安装清单文件中列出的所有工具，他们可以运行单个 `dotnet tool restore` 命令。

1. 打开 .config/dotnet-tools.json  文件并将内容替换为以下 JSON：

   ```json
   {
     "version": 1,
     "isRoot": true,
     "tools": {
       "microsoft.botsay": {
         "version": "1.0.0",
         "commands": [
           "botsay"
         ]
       },
       "dotnetsay": {
         "version": "2.1.3",
         "commands": [
           "dotnetsay"
         ]
       }
     }
   }
   ```

1. 将 `<name>` 替换为用于创建项目的名称。

1. 保存更改。

   进行此更改等同于在其他人安装项目目录的包 `dotnetsay` 后从存储库获取最新版本。

1. 运行 `dotnet tool restore` 命令。

   ```dotnetcli
   dotnet tool restore
   ```

   该命令生成的输出如以下示例所示：

   ```console
   Tool 'microsoft.botsay' (version '1.0.0') was restored. Available commands: botsay
   Tool 'dotnetsay' (version '2.1.3') was restored. Available commands: dotnetsay
   Restore was successful.
   ```

1. 验证工具是否可用：

   ```dotnetcli
   dotnet tool list
   ```

   输出是包和命令的列表，类似于以下示例：

   ```console
   Package Id      Version      Commands       Manifest
   --------------------------------------------------------------------------------------------
   microsoft.botsay 1.0.0        botsay         /home/name/repository/.config/dotnet-tools.json
   dotnetsay        2.1.3        dotnetsay      /home/name/repository/.config/dotnet-tools.json
   ```

1. 测试工具：

   ```dotnetcli
   dotnet tool run dotnetsay hello from dotnetsay
   dotnet tool run botsay hello from botsay
   ```

## <a name="update-a-local-tool"></a>更新本地工具

本地工具 `dotnetsay` 的已安装版本为 2.1.3。  最新版本是 2.1.4。 使用 [dotnet tool update](dotnet-tool-update.md) 命令将工具更新到最新版本。

```dotnetcli
dotnet tool update dotnetsay
```

输出指示新的版本号：

```console
Tool 'dotnetsay' was successfully updated from version '2.1.3' to version '2.1.4'
(manifest file /home/name/repository/.config/dotnet-tools.json).
```

update 命令查找包含包 ID 的第一个清单文件并对其进行更新。 如果搜索范围内的任何清单文件中都没有此类包 ID，SDK 会将新条目添加到最近的清单文件。 搜索范围上至父目录，直到找到具有 `isRoot = true` 的清单文件。

## <a name="remove-local-tools"></a>删除本地工具

通过运行 [dotnet tool uninstall](dotnet-tool-uninstall.md) 命令来删除已安装的工具：

```dotnetcli
dotnet tool uninstall microsoft.botsay
```

```dotnetcli
dotnet tool uninstall dotnetsay
```

## <a name="troubleshoot"></a>疑难解答

如果在学习本教程时收到错误消息，请参阅[排查 .NET 工具使用问题](troubleshoot-usage-issues.md)。

## <a name="see-also"></a>请参阅

有关详细信息，请参阅 [.NET Core 工具](global-tools.md)
