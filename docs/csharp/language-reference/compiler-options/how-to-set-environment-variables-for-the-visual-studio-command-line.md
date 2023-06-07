---
description: 如何为 Visual Studio 命令行设置环境变量
title: 如何为 Visual Studio 命令行设置环境变量
ms.date: 12/20/2019
f1_keywords:
- cs.build.commandline
helpviewer_keywords:
- csc.exe, command-line builds
- Visual C#, command-line builds
- command-line compilers, Visual C#
- Vsvars32.bat
- builds [C#], command-line
- compilers, invoking from command line
- Vcvars32.bat file
- command line [C#], building from
- Visual C# compiler, enabling
- compiling source code, from command line
ms.assetid: 7ec09480-5612-4f6a-8d00-ad90ea9bca5d
ms.openlocfilehash: b985c85e2fddce459ed68b3d07ba7d54a8b2d0a7
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89125600"
---
# <a name="how-to-set-environment-variables-for-the-visual-studio-command-line"></a>如何为 Visual Studio 命令行设置环境变量

VsDevCmd.bat 文件设置适当的环境变量来生成命令行。

> [!NOTE]
> Visual Studio 2015 及更早版本基于相同目的使用 VSVARS32.bat，而不是 VsDevCmd.bat。 此文件保存在 \Program Files\Microsoft Visual Studio\\Version\Common7\Tools 或 Program Files (x86)\Microsoft Visual Studio\\Version\Common7\Tools。

如果安装了 Visual Studio 当前版本的计算机上还安装有 Visual Studio 的早期版本，则不可在同一命令提示符窗口中运行不同版本的 VsDevCmd.bat 和 VSVARS32.BAT。 转而应在其自身的窗口中运行各版本的命令。

### <a name="to-run-vsdevcmdbat"></a>若要运行 VsDevCmd.BAT

1. 从“开始”菜单，打开“VS 2019 开发人员命令提示”。  该提示位于“Visual Studio 2019”文件夹中。

2. 更改为安装目录的 \Program Files\Microsoft Visual Studio\\Version\\Offering\Common7\Tools 或 \Program Files (x86)\Microsoft Visual Studio\\Version\\Offering\Common7\Tools 子目录。  （Version 为 2019，即当前版本。 Offering 是 Enterprise、Professional 或 Community。）

3. 通过键入“VsDevCmd”运行 VsDevCmd.bat。

    > [!CAUTION]
    > VsDevCmd.bat 可能因计算机而异。 请勿使用其他计算机上的 VsDevCmd.bat 替换缺失或损坏的 VsDevCmd.bat 文件。 而是应重新运行安装程序以替换丢失的文件。

### <a name="available-options-for-vsdevcmdbat"></a>适用于 VsDevCmd.BAT 的选项

若要查看适用于 VsDevCmd.BAT 的选项，请运行以下包含 `-help` 选项的命令：

```console
VsDevCmd.bat -help
```

## <a name="see-also"></a>请参阅

- [在命令行上使用 csc.exe 生成](./command-line-building-with-csc-exe.md)
