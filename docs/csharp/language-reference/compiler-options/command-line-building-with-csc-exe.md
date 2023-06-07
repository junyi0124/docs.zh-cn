---
description: 使用 csc.exe 实现命令行生成
title: 使用 csc.exe 实现命令行生成
ms.date: 04/19/2017
helpviewer_keywords:
- builds [C#]
- command line [C#]
ms.assetid: 66e70056-dd20-453c-a9b3-507e0478b015
ms.openlocfilehash: 9ffd164602862fce7f5e4f0982d3eda7cb403e60
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89125925"
---
# <a name="command-line-build-with-cscexe"></a>使用 csc.exe 实现命令行生成

通过在命令提示符处键入 C# 编译器的可执行文件名称 (csc.exe)，可调用该编译器。

如果使用“Visual Studio 开发人员命令提示”窗口，系统将设置所有必需的环境变量。 有关如何访问此工具的信息，请参阅 [Visual Studio 开发人员命令提示](../../../framework/tools/developer-command-prompt-for-vs.md)主题。

如果使用标准命令提示符窗口，则必须调整路径，然后才能从计算机的任意子目录调用 csc.exe。 还必须运行 VsDevCmd.bat 来设置适当的环境变量以支持命令行生成操作。 有关 VsDevCmd.bat 的详细信息，包括如何查找和运行它的说明，请参阅[如何为 Visual Studio 命令行设置环境变量](./how-to-set-environment-variables-for-the-visual-studio-command-line.md)。

如果你使用的计算机只安装有 Windows 软件开发工具包 (SDK)，则可以在“SDK 命令提示符”处使用 C# 编译器，该窗口可通过“Microsoft .NET Framework SDK”菜单选项打开。

也可以使用 MSBuild 以编程方式生成 C# 程序。 有关详细信息，请参阅 [MSBuild](/visualstudio/msbuild/msbuild)。

csc.exe 可执行文件通常位于 Windows 目录下的 Microsoft.NET\Framework\\\\<Version> 文件夹中  。 根据每台计算机上的具体配置，此位置可能有所不同。 如果计算机上安装了不止一个版本的 .NET Framework，您将发现此文件的多个版本。 有关此类安装的详细信息，请参阅[如何：确定安装的 .NET Framework 版本](../../../framework/migration-guide/how-to-determine-which-versions-are-installed.md)。

> [!TIP]
> 使用 Visual Studio IDE 生成项目时，可以在 **“输出”** 窗口显示 **“csc”** 命令以及与之关联的编译器选项。 若要显示此信息，请按照 [如何：查看、保存和配置生成日志文件](/visualstudio/ide/how-to-view-save-and-configure-build-log-files#to-change-the-amount-of-information-included-in-the-build-log)中的说明将日志数据的详细级别更改为 **“常规”** 或 **“详细”**。 重新生成项目之后，在 **“输出”** 窗口中搜索 **“csc”** 即可找到所调用的 C# 编译器。

 **在本主题中**

- [命令行语法规则](#rules-for-command-line-syntax-for-the-c-compiler)

- [示例命令行](#sample-command-lines-for-the-c-compiler)

- [C# 编译器和 C++ 编译器输出之间的差异](#differences-between-c-compiler-and-c-compiler-output)

## <a name="rules-for-command-line-syntax-for-the-c-compiler"></a>C# 编译器的命令行语法规则

在解释操作系统命令行上给出的参数时，C# 编译器使用下列规则：

- 参数用空白分隔，空白可以是一个空格或制表符。

- 插入符号 (^) 未被识别为转义符或者分隔符。 在传递给程序中的 `argv` 数组之前，该字符由操作系统中的命令行分析器处理。

- 无论其中有无空白，包含在双引号 ("string") 中的字符串均被解释为单个参数。 带引号的字符串可以嵌入在自变量内。

- 前面有反斜杠的双引号 (\\") 被解释为原义双引号字符 (")。

- 反斜杠按其原义解释，除非它们紧位于双引号之前。

- 如果偶数个反斜杠后跟双引号，则每对反斜杠中有一个反斜杠被置于 `argv` 数组中，而双引号被解释为字符串分隔符。

- 如果奇数个反斜杠后跟双引号，则每对反斜杠中有一个反斜杠被置于 `argv` 数组中，而双引号由剩余反斜杠进行“转义”。 这将在 `argv` 中添加一个原义双引号 (")。

## <a name="sample-command-lines-for-the-c-compiler"></a>C# 编译器的示例命令行

- 编译生成 File.exe 的 File.cs：

  ```console
  csc File.cs
  ```

- 编译生成 File.dll 的 File.cs：

  ```console
  csc -target:library File.cs
  ```

- 编译 File.cs 并创建 My.exe：

  ```console
  csc -out:My.exe File.cs
  ```

- 编译当前目录中的所有 C# 文件，对其进行优化并定义 DEBUG 符号。 输出为 File2.exe：

  ```console
  csc -define:DEBUG -optimize -out:File2.exe *.cs
  ```

- 编译当前目录中的所有 C# 文件，生成 File2.dll 的调试版本。 不显示徽标和警告：

  ```console
  csc -target:library -out:File2.dll -warn:0 -nologo -debug *.cs
  ```

- 将当前目录中的所有 C# 文件编译为 Something.xyz (DLL)：

  ```console
  csc -target:library -out:Something.xyz *.cs
  ```

## <a name="differences-between-c-compiler-and-c-compiler-output"></a>C# 编译器和 C++ 编译器输出之间的差异

调用 C# 编译器时，不会创建任何对象 (.obj) 文件，而是直接创建输出文件。 因此，C# 编译器不需要链接器。

## <a name="see-also"></a>另请参阅

- [C# 编译器选项](./index.md)
- [按字母顺序列出的 C# 编译器选项](./listed-alphabetically.md)
- [按类别列出的 C# 编译器选项](./listed-by-category.md)
- [Main() 和命令行参数](../../programming-guide/main-and-command-args/index.md)
- [命令行参数](../../programming-guide/main-and-command-args/command-line-arguments.md)
- [如何显示命令行参数](../../programming-guide/main-and-command-args/how-to-display-command-line-arguments.md)
- [Main() 返回值](../../programming-guide/main-and-command-args/main-return-values.md)
