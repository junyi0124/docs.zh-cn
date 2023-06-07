---
description: -platform（C# 编译器选项）
title: -platform（C# 编译器选项）
ms.date: 07/20/2015
f1_keywords:
- /platform
helpviewer_keywords:
- platform compiler option [C#]
- -platform compiler option [C#]
- /platform compiler option [C#]
ms.assetid: c290ff5e-47f4-4a85-9bb3-9c2525b0be04
ms.openlocfilehash: 3fdb030dfc141b011f5faa827a4e4bb45ae38d19
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89466009"
---
# <a name="-platform-c-compiler-options"></a>-platform（C# 编译器选项）

指定公共语言运行时 (CLR) 的哪个版本可以运行程序集。

## <a name="syntax"></a>语法

```console
-platform:string
```

## <a name="parameters"></a>参数

`string` \
anycpu（默认值）、anycpu32bitpreferred、ARM、x64、x86 或 Itanium。

## <a name="remarks"></a>备注

- anycpu（默认值）将程序集编译成可在任意平台上运行。 您的应用程序将尽可能作为 64 位进程运行；当只有 32 位模式可用时，才会回退到 32 位。

- anycpu32bitpreferred 将程序集编译成可在任意平台上运行。 在同时支持 64 位和 32 位应用程序的系统上，您的应用程序将以32 位模式运行。 只能为面向 .NET Framework 4.5 或更高版本的项目指定此选项。

- ARM 将程序集编译成可以在具有高级 RISC 计算机 (ARM) 处理器的计算机上运行。

- ARM64 编译程序集以在由 64 位 CLR 在具有支持 A64 指令集的高级 RISC 计算机 (ARM) 处理器的计算机上运行。

- x64 将程序集编译成可由支持 AMD64 或 EM64T 指令集的计算机上的 64 位 CLR 运行。

- x86 将程序集编译成可由 32 位、x86 可兼容 CLR 运行。

- Itanium 将程序集编译成可由配有 Itanium 处理器的计算机上的 64 位 CLR 运行。

在 64 位 Windows 操作系统上：

- 用 -platform:x86 编译的程序集将在 WOW64 下运行的 32 位 CLR 上执行。

- 用 -platform:anycpu 编译的 DLL 将在加载它的进程所在的同一 CLR 上执行。

- 用 -platform:anycpu 编译的可执行文件将在 64 位 CLR 上执行。

- 用 -platform:anycpu32bitpreferred 编译的可执行文件将在 32 位 CLR 上执行。

anycpu32bitpreferred 设置只对可执行文件 (.EXE) 有效，并且需要 .NET Framework 4.5 或更高版本。

有关开发 Windows 64 位操作系统上运行的应用程序的详细信息，请参阅 [64 位应用程序](../../../framework/64-bit-apps.md)。

### <a name="to-set-this-compiler-option-in-the-visual-studio-development-environment"></a>在 Visual Studio 开发环境中设置此编译器选项

1. 打开项目的“属性”  页。

2. 单击“生成”属性页。

3. 修改“平台目标”属性，对于面向 .NET Framework 4.5 或更高版本的项目，选择或清除“首选 32 位”复选框 。

> [!NOTE]
> `-platform` 在 Visual C# Express 的开发环境中不可用。

有关如何以编程方式设置此编译器选项的信息，请参阅 <xref:VSLangProj80.CSharpProjectConfigurationProperties3.PlatformTarget%2A>。

## <a name="example"></a>示例

下面的示例演示如何使用 -platform 选项来指定只有 64 位 Windows 操作系统上的 64 位 CLR 才能运行应用程序。

```console
csc -platform:anycpu filename.cs
```

## <a name="see-also"></a>另请参阅

- [C# 编译器选项](index.md)
- [管理项目和解决方案属性](/visualstudio/ide/managing-project-and-solution-properties)
