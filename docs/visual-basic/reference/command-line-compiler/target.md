---
title: -target
ms.date: 03/13/2018
helpviewer_keywords:
- target compiler options [Visual Basic]
- -target compiler options [Visual Basic]
- /target compiler options [Visual Basic]
ms.assetid: e0954147-548b-461f-9c4b-a8f88845616c
ms.openlocfilehash: 0ab28d55b2426a4efda112ab84da5e790909d565
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403065"
---
# <a name="-target-visual-basic"></a>-target (Visual Basic)

指定编译器输出的格式。

## <a name="syntax"></a>语法

```console
-target:{exe | library | module | winexe | appcontainerexe | winmdobj}
```

## <a name="remarks"></a>备注

下表概括了 `-target` 选项的效果。

|**选项**|**行为**|
|----------------|------------------|
|`-target:exe`|使编译器创建可执行控制台应用程序。<br /><br /> 这是未指定任何 `-target` 选项时的默认选项。 将使用 .exe 扩展名创建可执行文件。<br /><br /> 除非使用 `-out` 选项进行指定，否则输出文件名采用包含 `Sub Main` 过程的输入文件的名称。<br /><br /> 编译为 .exe 文件的源代码文件中只需要一个 `Sub Main` 过程。 使用 `-main` 编译器选项可指定包含 `Sub Main` 过程的类。|
|`-target:library`|使编译器创建动态链接库 (DLL)。<br /><br /> 将使用 .dll 扩展名创建动态链接库文件。<br /><br /> 除非使用 `-out` 选项指定，否则输出文件的名称采用第一个输入文件的名称。<br /><br /> 生成 DLL 时，不需要 `Sub Main` 过程。|
|`-target:module`|使编译器生成可添加到程序集的模块。<br /><br /> 将使用扩展名 .netmodule 创建输出文件。<br /><br /> .NET 公共语言运行时无法加载不包含程序集的文件。 但是，可以使用 `-reference` 将此类文件合并到程序集的程序集清单中。<br /><br /> 如果一个模块中的代码引用另一模块中的内部类型，则两个模块必须使用 `-reference` 合并到一个程序集清单中。<br /><br /> [-addmodule](addmodule.md) 选项从模块导入元数据。|
|`-target:winexe`|使编译器创建基于 Windows 的可执行应用程序。<br /><br /> 将使用 .exe 扩展名创建可执行文件。 基于 Windows 的应用程序从 .NET Framework 类库或使用 Windows API 提供用户界面。<br /><br /> 除非使用 `-out` 选项进行指定，否则输出文件名采用包含 `Sub Main` 过程的输入文件的名称。<br /><br /> 编译为 .exe 文件的源代码文件中只需要一个 `Sub Main` 过程。 如果代码具有多个附带 `Sub Main` 过程的类，请使用 `-main` 编译器选项指定包含 `Sub Main` 过程的类|
|`-target:appcontainerexe`|使编译器创建必须在应用容器中运行的基于 Windows 的可执行应用程序。 此设置设计为用于 Windows 8.x 应用商店应用程序。<br /><br /> appcontainerexe  设置在[可移植可执行](/windows/desktop/Debug/pe-format)文件的“特征”字段中设置一个位。 此位只是应用必须在应用容器中运行。 设置了该位时，如果 `CreateProcess` 方法尝试在应用容器外启动应用程序，则会发生错误。 除了此位设置外，-target:appcontainerexe  等效于 -target:winexe  。<br /><br /> 将使用 .exe 扩展名创建可执行文件。<br /><br /> 除非使用 `-out` 选项进行指定，否则输出文件名采用包含 `Sub Main` 过程的输入文件的名称。<br /><br /> 编译为 .exe 文件的源代码文件中只需要一个 `Sub Main` 过程。 如果代码包含多个附带 `Sub Main` 过程的类，请使用 `-main` 编译器选项指定包含 `Sub Main` 过程的类|
|`-target:winmdobj`|使编译器创建可转换为 Windows 运行时二进制 (.winmd) 文件的中间文件。 除了托管语言程序外，JavaScript 和 C++ 程序也可以使用该 .winmd 文件。<br /><br /> 将使用扩展名 .winmdobj 创建中间文件。<br /><br /> 除非使用 `-out` 选项进行指定，否则输出文件名采用第一个输入文件的名称。 `Sub Main` 过程不是必需的。<br /><br /> .winmdobj 文件设计为用作 <xref:Microsoft.Build.Tasks.WinMDExp> 导出工具的输入，以生成 Windows 元数据 (WinMD) 文件。 WinMD 文件的扩展名为 .winmd，同时包含原始库中的代码以及 JavaScript、C++ 和 Windows 运行时使用的 WinMD 定义。|

除非指定 `-target:module`，否则 `-target` 会使 .NET Framework 程序集清单添加到输出文件。

每个 Vbc.exe 实例最多生成一个输出文件。 如果多次指定编译器选项（如 `-out` 或 `-target`），则编译器处理的最后一个选项将生效。 有关编译中所有文件的信息会添加到清单中。 除用 `-target:module` 创建的文件之外，所有输出文件都包含清单中的程序集元数据。 使用 [Ildasm.exe（IL 反汇编程序）](../../../framework/tools/ildasm-exe-il-disassembler.md)可查看输出文件中的元数据。

`-target` 的缩写形式是 `-t`。

### <a name="to-set--target-in-the-visual-studio-ide"></a>若要在 Visual Studio IDE 中设置 -target

1. 在 **“解决方案资源管理器”** 中选择一个项目。 在“项目”菜单上，单击“属性”   。

2. 单击“应用程序”  选项卡。

3. 修改“应用程序类型”  框中的值。

## <a name="example"></a>示例

以下代码编译 `in.vb`，从而创建 `in.dll`：

```console
vbc -target:library in.vb
```

## <a name="see-also"></a>请参阅

- [Visual Basic 命令行编译器](index.md)
- [-main](main.md)
- [-out (Visual Basic)](out.md)
- [-reference (Visual Basic)](reference.md)
- [-addmodule](addmodule.md)
- [-moduleassemblyname](moduleassemblyname.md)
- [.NET 中的程序集](../../../standard/assembly/index.md)
- [示例编译命令行](sample-compilation-command-lines.md)
