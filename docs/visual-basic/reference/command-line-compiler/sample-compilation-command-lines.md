---
title: 示例编译命令行
ms.date: 03/13/2018
helpviewer_keywords:
- command line [Visual Basic], compilers
- compilation [Visual Basic], command-line
- command-line compilers
- compiling source code [Visual Basic], from command line
- Visual Basic compiler, sample command lines
ms.assetid: 5bfbb487-5f47-4267-969a-39dfb917beeb
ms.openlocfilehash: 496627d3b77b0382ae7d15c8225a6fbd41f1db73
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403117"
---
# <a name="sample-compilation-command-lines-visual-basic"></a>示例编译命令行 (Visual Basic)

作为从 Visual Studio 中编译 Visual Basic 程序的替代方法，可以从命令行进行编译，以生成可执行 (.exe) 文件或动态链接库 (.dll) 文件。

Visual Basic 命令行编译器支持一组完整的选项，这些选项控制输入和输出文件、程序集以及调试和预处理器选项。 每个选项都以两种可互换形式提供：`-option` 和 `/option`。 此文档仅显示 `-option` 形式。

下表列出可以进行修改以供自己使用的一些示例命令行。

|功能|使用|
|--------|---------|
|编译 File.vb 并创建 File.exe|`vbc -reference:Microsoft.VisualBasic.dll File.vb`|
|编译 File.vb 并创建 File.dll|`vbc -target:library File.vb`|
|编译 File.vb 并创建 My.exe|`vbc -out:My.exe File.vb`|
|编译 File.vb 并创建库和名为 File.dll 的引用程序集|`vbc -target:library -ref:.\debug\bin\ref\file.dll File.vb`|
|编译当前目录中的所有 Visual Basic 文件（打开了优化并定义了 `DEBUG` 符号），生成 File2.exe|`vbc -define:DEBUG=1 -optimize -out:File2.exe *.vb`|
|编译当前目录中的所有 Visual Basic 文件，生成 File2.dll 的调试版本而不显示徽标或警告|`vbc -target:library -out:File2.dll -nowarn -nologo -debug *.vb`|
|将当前目录中的所有 Visual Basic 文件编译为 Something.dll|`vbc -target:library -out:Something.dll *.vb`|

> [!TIP]
> 使用 Visual Studio IDE 生成项目时，可以在输出窗口中显示有关关联 vbc  命令及其编译器选项的信息。 若要显示此信息，请打开[“选项”对话框、“项目和解决方案”、“生成和运行”](/visualstudio/ide/reference/options-dialog-box-projects-and-solutions-build-and-run)，然后将“MSBuild 项目生成输出详细信息”  设置为“普通”  或更高级别的详细程度。

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [条件编译](../../programming-guide/program-structure/conditional-compilation.md)
