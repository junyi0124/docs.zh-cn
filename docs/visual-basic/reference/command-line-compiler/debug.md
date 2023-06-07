---
title: -debug
ms.date: 03/10/2018
helpviewer_keywords:
- debug compiler switches
- /debug compiler option [Visual Basic]
- -debug compiler option [Visual Basic]
- debug compiler option [Visual Basic]
ms.assetid: c2b0bea5-1d5e-499f-9bd5-4f6c6b715ea2
ms.openlocfilehash: 60c6e512a648f093bb9c70b5af86d5719e544adc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408720"
---
# <a name="-debug-visual-basic"></a>-debug (Visual Basic)

使编译器生成调试信息，并将此信息放置在输出文件中。

## <a name="syntax"></a>语法

```console
-debug[+ | -]
```

or

```console
-debug:[full | pdbonly]
```

## <a name="arguments"></a>自变量

|术语|定义|
|---|---|
|`+` &#124; `-`|可选。 指定 `+` 或 `-debug` 将导致编译器生成调试信息并将此信息放在 .pdb 文件中。 指定 `-` 与不指定 `-debug` 具有相同的效果。|
|`full` &#124; `pdbonly`|可选。 指定编译器生成的调试信息的类型。 如果不指定 `-debug:pdbonly`，则默认值为 `full`，这使你能够将调试程序附加到正在运行的程序。 `pdbonly` 参数允许在调试程序中启动程序时进行源代码调试，但仅在正在运行的程序附加到调试器时才显示汇编语言代码。|

## <a name="remarks"></a>备注

使用此选项创建调试版本。 如果未指定 `-debug`、`-debug+` 或 `-debug:full`，则将无法调试程序的输出文件。

默认情况下，不会发出调试信息 (`-debug-`)。 若要发出调试信息，请指定 `-debug` 或 `-debug+`。

有关如何配置应用程序的调试性能的信息，请参阅[令映像更易于调试](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md)。

|在 Visual Studio 集成开发环境中设置 -debug|
|---|
|1.在“解决方案资源管理器”  中选择了项目的情况下，在“项目”  菜单上单击“属性”  。 <br />2.单击“编译”  选项卡。<br />3.单击“高级编译选项”  。<br />4.修改“生成调试信息”  框中的值。|

## <a name="example"></a>示例

下面的示例将调试信息置于 `App.exe` 输出文件中。

```console
vbc -debug -out:app.exe test.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-bugreport](bugreport.md)
- [示例编译命令行](sample-compilation-command-lines.md)
