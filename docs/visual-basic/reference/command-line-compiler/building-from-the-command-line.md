---
title: 从命令行生成
ms.date: 07/20/2015
helpviewer_keywords:
- builds [Visual Basic], command-line
- Visual Basic compiler, about Visual Basic compiler
- command line [Visual Basic], compilers
- command line [Visual Basic], building from
- command line [Visual Basic], builds
- compilers [Visual Basic], invoking from command line
- command-line builds
- compiling source code
- command-line compilers [Visual Basic], Visual Basic
- command line [Visual Basic], Visual Basic
ms.assetid: e61947e9-a42e-4717-a699-5f70a98cdd03
ms.openlocfilehash: ec6ae3328c2042af950d1ee78a33d3de97219f10
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414293"
---
# <a name="building-from-the-command-line-visual-basic"></a>从命令行生成 (Visual Basic)

Visual Basic 项目由一个或多个单独的源文件组成。 在名为编译的过程中，这些文件会一起合并到一个包中，这是可作为应用程序运行的单个可执行文件。

Visual Basic 提供命令行编译器作为替代方法，用于在 Visual Studio 集成开发环境 (IDE) 中编译程序。 命令行编译器旨在用于不需要 IDE 中的完整功能集的情况（例如，为系统内存或存储空间有限的计算机进行使用或编写）。

若要从 Visual Studio IDE 中编译源文件，请从“生成”  菜单中选择“生成”  命令。

> [!TIP]
> 使用 Visual Studio IDE 生成项目文件时，可以在输出窗口中显示有关关联 vbc  命令及其开关的信息。 若要显示此信息，请打开[“选项”对话框、“项目和解决方案”、“生成和运行”](/visualstudio/ide/reference/options-dialog-box-projects-and-solutions-build-and-run)，然后将“MSBuild 项目生成输出详细信息”  设置为“普通”  或更高级别的详细程度。 有关详细信息，请参阅[如何：查看、保存和配置生成日志文件](/visualstudio/ide/how-to-view-save-and-configure-build-log-files)。

可以使用 MSBuild 在命令提示符下编译项目 (.vbproj) 文件。 有关详细信息，请参阅[命令行参考](/visualstudio/msbuild/msbuild-command-line-reference)和[演练：使用 MSBuild](/visualstudio/msbuild/walkthrough-using-msbuild)。

## <a name="in-this-section"></a>本节内容

[如何：调用命令行编译器](how-to-invoke-the-command-line-compiler.md) \
介绍如何在 MS-DOS 提示符下或从特定子目录调用命令行编译器。

[示例编译命令行](sample-compilation-command-lines.md) \
提供可以进行修改以供自己使用的示例命令行的列表。

## <a name="related-sections"></a>相关章节

[Visual Basic 命令行编译器](index.md) \
提供按字母顺序或用途组织的编译器选项的列表。

[条件编译](../../programming-guide/program-structure/conditional-compilation.md) \
介绍如何编译代码的特定部分。

[在 Visual Studio 中生成和清理项目和解决方案](/visualstudio/ide/building-and-cleaning-projects-and-solutions-in-visual-studio) \
介绍如何组织将包含在不同生成中的内容、选择项目属性以及确保项目按正确顺序生成。
