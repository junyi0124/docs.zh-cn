---
title: -netcf
ms.date: 03/13/2018
f1_keywords:
- /netcf
- -netcf
helpviewer_keywords:
- -netcf compiler option [Visual Basic]
- netcf compiler option [Visual Basic]
- /netcf compiler option [Visual Basic]
ms.assetid: db7cfa59-c315-401c-a59b-0daf355343d6
ms.openlocfilehash: 16fb6b3b63c848ea6c09cc18b0fcc488670f0926
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397470"
---
# <a name="-netcf"></a>-netcf

将编译器设置为以 .NET Compact Framework 为目标。

## <a name="syntax"></a>语法

```console
-netcf
```

## <a name="remarks"></a>备注

`-netcf` 选项使 Visual Basic 编译器将 .NET Compact Framework（而不是完整的 .NET Framework）作为目标。 仅在完整的 .NET Framework 中存在的语言功能处于禁用状态。

`-netcf` 选项专门与 [-sdkpath](sdkpath.md) 一起使用。 `-netcf` 禁用的语言功能与 `-sdkpath` 目标文件中不存在的语言功能相同。

> [!NOTE]
> `-netcf` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。 加载 Visual Basic 设备项目时，将设置 `-netcf` 选项。

`-netcf` 选项会更改以下语言功能：

- 禁用 [End \<keyword> Statement](../../language-reference/statements/end-keyword-statement.md) 关键字，该关键字可终止程序执行。 以下程序在没有 `-netcf` 的情况下进行编译和运行，但在通过 `-netcf` 进行编译时将失败。

  [!code-vb[VbVbalrCompiler#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/netcf.vb#34)]

- 在所有窗体中禁用后期绑定。 当遇到识别的后期绑定情况时，会产生编译时错误。 以下程序在没有 `-netcf` 的情况下进行编译和运行，但在通过 `-netcf` 进行编译时将失败。

  [!code-vb[VbVbalrCompiler#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#35)]

- 禁用 [Auto](../../language-reference/modifiers/auto.md)、[Ansi](../../language-reference/modifiers/ansi.md) 和[Unicode](../../language-reference/modifiers/unicode.md) 修饰符。 [Declare 语句](../../language-reference/statements/declare-statement.md)的语法也修改为 `Declare Sub|Function name Lib "library" [Alias "alias"] [([arglist])]`。 以下代码显示 `-netcf` 对编译的影响。

  [!code-vb[VbVbalrCompiler#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCompiler/VB/OptionStrictOff.vb#36)]

- 在使用 `-netcf` 时，使用从 Visual Basic 中删除的 Visual Basic 6.0 关键字会产生不同的错误。 这会影响以下关键字的错误消息：

  - `Open`

  - `Close`

  - `Put`

  - `Print`

  - `Write`

  - `Input`

  - `Lock`

  - `Unlock`

  - `Seek`

  - `Width`

  - `Name`

  - `FreeFile`

  - `EOF`

  - `Loc`

  - `LOF`

  - `Line`

## <a name="example"></a>示例

以下代码使用 C 盘上 .NET Compact Framework 的默认安装目录中的 mscorlib.dll 和 Microsoft.VisualBasic.dll 版本，通过 .NET Compact Framework 编译 `Myfile.vb`。 通常，会使用 .NET Compact Framework 的最新版本。

```console
vbc -netcf -sdkpath:"c:\Program Files\Microsoft Visual Studio .NET 2003\CompactFrameworkSDK\v1.0.5000\Windows CE " myfile.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
- [-sdkpath](sdkpath.md)
