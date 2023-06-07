---
title: 如何：生成 .NET Framework 单文件程序集
description: 了解如何在 .NET 中生成单文件程序集。 单文件程序集可以是面向 .NET 的库 (.dll)，也可以是可执行程序 (.exe) 文件。
ms.date: 08/20/2019
helpviewer_keywords:
- assembly manifest, single-file assemblies
- library assemblies
- command-line compilers
- assemblies [.NET Framework], single-file
- output file name for assemblies
- code modules
- single-file assemblies
dev_langs:
- csharp
- vb
ms.assetid: a6063221-43a5-4d3e-814c-288a4ec69aec
ms.openlocfilehash: bdffa9417a7d52e9c825ca6455997b9bfa7408e4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96271520"
---
# <a name="how-to-build-a-net-framework-single-file-assembly"></a>如何：生成 .NET Framework 单文件程序集

单文件程序集（最简单的程序集类型）包含类型信息和实现，以及[程序集清单](../../standard/assembly/manifest.md)。 可以使用命令行编译器或 Visual Studio 创建面向 .NET Framework 的单文件程序集。 默认情况下，编译器创建扩展名为 .exe 的程序集文件。

> [!NOTE]
> 适用于 C# 和 Visual Basic 的 Visual Studio 只能用于创建单文件程序集。 如果要创建多文件程序集，则必须使用命令行编译器或 Visual C++。

以下步骤说明如何使用命令行编译器创建单文件程序集。

## <a name="create-an-assembly-with-an-exe-extension"></a>创建扩展名为 .exe 的程序集

在命令提示符处，键入下列命令：

\<*compiler command*> \<*module name*>

在此命令中，“compiler command”是代码模块中所用语言的编译器命令，“module name”是要编译为程序集的代码模块的名称。

以下示例从名为 `myCode` 的代码模块创建名为 myCode.exe 的程序集。

```csharp
csc myCode.cs
```

```vb
vbc myCode.vb
```

## <a name="create-an-assembly-with-an-exe-extension-and-specify-the-output-file-name"></a>创建扩展名为 .exe 的程序集并指定输出文件名

在命令提示符处，键入下列命令：

\<*compiler command*> /out:\<*file name*> \<*module name*>

在此命令中，“compiler command”是代码模块中所用语言的编译器命令，“file name”是输出文件名称，而“module name”是要编译为程序集的代码模块的名称。

以下示例从名为 `myCode` 的代码模块创建名为 myAssembly.exe 的程序集。

```csharp
csc -out:myAssembly.exe myCode.cs
```

```vb
vbc -out:myAssembly.exe myCode.vb
```

## <a name="create-library-assemblies"></a>创建库程序集

 库程序集与类库相似。 它包含将由其他程序集引用的类型，但没有开始执行的入口点。

要创建库程序集，请在命令提示符处键入以下命令：

\<*compiler command*> -t:library \<*module name*>

在此命令中，“compiler command”是代码模块中所用语言的编译器命令，“module name”是要编译为程序集的代码模块的名称。 也可以使用其他编译器选项，例如 -out: 选项。

以下示例从名为 `myCode` 的代码模块创建名为 myCodeAssembly.dll 的库程序集。

```csharp
csc -out:myCodeLibrary.dll -t:library myCode.cs
```

```vb
vbc -out:myCodeLibrary.dll -t:library myCode.vb
```

## <a name="see-also"></a>请参阅

- [创建程序集](../../standard/assembly/create.md)
- [多文件程序集](multifile-assemblies.md)
- [如何：生成多文件程序集](build-multifile-assembly.md)
- [使用程序集编程](../../standard/assembly/index.md)
