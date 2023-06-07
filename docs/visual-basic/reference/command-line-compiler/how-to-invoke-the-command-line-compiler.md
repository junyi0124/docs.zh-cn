---
title: 如何：调用命令行编译器
ms.date: 07/20/2015
helpviewer_keywords:
- command-line arguments
- vbc.exe
- Visual Basic compiler, starting
- command line [Visual Basic], arguments
ms.assetid: 0fd9a8f6-f34e-4c35-a49d-9b9bbd8da4a9
ms.openlocfilehash: 6def53d4a2d15dda3e3ac43abe35b3100f456fe9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408603"
---
# <a name="how-to-invoke-the-command-line-compiler-visual-basic"></a>如何：调用命令行编译器 (Visual Basic)

可以通过在命令行中键入可执行文件的名称（也称为 MS-DOS 提示符）来调用命令行编译器。 如果从默认的 Windows 命令提示符下进行编译，则必须键入可执行文件的完全限定的路径。 若要重写此默认行为，可以使用 Visual Studio 的开发人员命令提示，也可以修改 PATH 环境变量。 无论使用哪种方法，都可以通过键入编译器名称从任何目录中进行编译。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-invoke-the-compiler-using-the-developer-command-prompt-for-visual-studio"></a>使用 Visual Studio 的开发人员命令提示调用编译器

1. 打开 Microsoft Visual Studio 程序组中的“Visual Studio Tools 程序”文件夹。

2. 如果安装了 Visual Studio，则可以使用 Visual Studio 的开发人员命令提示从计算机上的任何目录访问编译器。

3. 调用 Visual Studio 开发人员命令提示。

4. 在命令行处，键入 `vbc.exe` sourceFileName  ，然后按 Enter。

    例如，如果将源代码存储在名为 `SourceFiles` 的目录中，则可以打开命令提示符并键入 `cd SourceFiles` 以更改为该目录。 如果该目录已包含一个名为 `Source.vb` 的源文件，则可以通过键入 `vbc.exe Source.vb` 对其进行编译。

## <a name="to-set-the-path-environment-variable-to-the-compiler-for-the-windows-command-prompt"></a>将 PATH 环境变量设置为 Windows 命令提示符的编译器

1. 使用 Windows Search 功能在本地磁盘上查找 Vbc.exe。

    编译器所在目录的确切名称取决于 Windows 目录的位置和安装的“.NET Framework”版本。 如果安装了多个版本的“.NET Framework”，必须确定要使用哪个版本（通常是最新版本）。

2. 在“开始”菜单中，右键单击“我的电脑”，然后从快捷菜单中单击“属性”    。

3. 单击“高级”选项卡，然后单击“环境变量”   。

4. 在“系统”变量窗格中，从列表中选择“路径”，然后单击“编辑”    。

5. 在“编辑系统变量”对话框中，将插入点移到“变量值”字段中字符串的末尾，然后键入一个分号 (;)，随后键入在步骤 1 中找到的完整目录名称   。

6. 单击“确定”  确认编辑内容并关闭对话框。

     更改 PATH 环境变量后，可以从计算机上的任何目录中的 Windows 命令提示符处运行 Visual Basic 编译器。

## <a name="to-invoke-the-compiler-using-the-windows-command-prompt"></a>使用 Windows 命令提示符调用编译器

1. 在“开始”菜单中，单击“附件”文件夹，然后打开“Windows 命令提示符”    。

2. 在命令行处，键入 `vbc.exe` sourceFileName  ，然后按 Enter。

     例如，如果将源代码存储在名为 `SourceFiles` 的目录中，则可以打开命令提示符并键入 `cd SourceFiles` 以更改为该目录。 如果该目录已包含一个名为 `Source.vb` 的源文件，则可以通过键入 `vbc.exe Source.vb` 对其进行编译。

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [条件编译](../../programming-guide/program-structure/conditional-compilation.md)
