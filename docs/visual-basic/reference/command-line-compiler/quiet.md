---
title: -quiet
ms.date: 07/20/2015
f1_keywords:
- -quiet
- quiet
helpviewer_keywords:
- -quiet compiler option [Visual Basic]
- /quiet compiler option [Visual Basic]
- quiet compiler option [Visual Basic]
ms.assetid: 5d77fa23-4c50-4708-8535-649912b098e8
ms.openlocfilehash: f894ed6a778e026ffd3976a63fe3b677eb6a9557
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400521"
---
# <a name="-quiet"></a>-quiet

阻止编译器显示与语法相关的错误和警告的代码。

## <a name="syntax"></a>语法

```console
-quiet
```

## <a name="remarks"></a>备注

默认情况，`-quiet` 是无效的。 当编译器报告与语法相关的错误或警告时，它还会输出源代码中的行。 对于分析编译器输出的应用程序，编译器仅输出诊断文本可能更方便。

在下面的示例中，`Module1` 在编译时不使用 `-quiet` 的情况下输出一个包含源代码的错误。

```vb
Module Module1
    Sub Main()
        x()
    End Sub
End Module
```

输出：

```console
C:\projects\vb2.vb(3) : error BC30451: 'x' is not declared. It may be inaccessible due to its protection level.

        x()
        ~
```

使用 `-quiet` 编译，编译器仅输出以下内容：

```console
E:\test\t2.vb(3) : error BC30451: Name 'x' is not declared.
```

> [!NOTE]
> `-quiet` 选项在 Visual Studio 开发环境内无法使用；仅当从命令行编译时才可用。

## <a name="example"></a>示例

下面的代码编译 `T2.vb`，并且不显示与语法相关的编译器诊断代码：

```console
vbc -quiet t2.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
