---
title: '@（指定响应文件）'
ms.date: 03/13/2018
helpviewer_keywords:
- '@ (Specify Response File) compiler option [Visual Basic]'
ms.assetid: a6847eaa-e5f9-4303-9421-45b55484b9ca
ms.openlocfilehash: 91cf1b5a55d16ab47a83fbd259dd1d83d8e9c31a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403091"
---
# <a name="-specify-response-file-visual-basic"></a>@（指定响应文件）(Visual Basic)

指定包含编译器选项和要编译的源代码文件的文件。

## <a name="syntax"></a>语法

```console
@response_file
```

## <a name="arguments"></a>自变量

`response_file`  
必需。 列出了编译器选项或要编译的源代码文件的文件。 如果文件名包含空格，则将名称括在引号 (" ") 内。

## <a name="remarks"></a>备注

编译器将处理在响应文件中指定的编译器选项和源代码文件，就好像已在命令行中指定了这些选项。

若要在一次编译中指定多个响应文件，请指定多个响应文件选项，如下所示。

```console
@file1.rsp @file2.rsp
```

在响应文件中，多个编译器选项和源代码文件可以出现在同一行中。 单个编译器选项的指定必须出现在同一行中（不能跨行）。 响应文件的注释可以 `#` 符号开头。

可以结合使用命令行上指定的选项与一个或多个响应文件中指定的选项。 编译器在遇到命令选项时会进行处理。 因此，命令行参数可以重写先前在响应文件中列出的选项。 反之，响应文件中的选项也将重写先前在命令行或其他响应文件中列出的选项。

Visual Basic 提供 Vbc.rsp 文件，该文件与 Vbc.exe 文件位于同一目录。 默认情况下，除非使用 `-noconfig` 选项，否则将包含 Vbc.rsp 文件。 有关详细信息，请参阅 [-noconfig](noconfig.md)。

> [!NOTE]
> `@` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。

## <a name="example"></a>示例

以下几行来自示例响应文件。

```console
# build the first output file
-target:exe
-out:MyExe.exe
source1.vb
source2.vb
```

## <a name="example"></a>示例

下面的示例展示如何将 `@` 选项与名为 `File1.rsp` 的响应文件一起使用。

```console
vbc @file1.rsp
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-noconfig](noconfig.md)
- [示例编译命令行](sample-compilation-command-lines.md)
