---
title: 多文件程序集
description: 通过使用命令行编译器或 Visual Studio 和 Visual C++ 来使用面向 .NET 的多文件程序集。 程序集中的文件必须包含程序集清单。
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], multifile
- entry point for assembly
- compiling assemblies
- command-line compilers
- assembly manifest, multifile assemblies
- code modules
- multifile assemblies
ms.assetid: 13509e73-db77-4645-8165-aad8dfaedff6
ms.openlocfilehash: a5fb41b3b136a325e6b8658da521cba3176e929f
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104637"
---
# <a name="multifile-assemblies"></a>多文件程序集

可以使用命令行编译器或 Visual Studio 和 Visual C++，创建面向 .NET Framework 的多文件程序集。 程序集中的一个文件必须包含程序集清单。 启动应用程序的程序集还必须包含入口点，如 `Main` 或 `WinMain` 方法。

例如，假设应用程序包含两个代码模块：Client.cs 和 Stringer.cs 。 Stringer.cs 创建由 Client.cs 中的代码引用的 `myStringer` 命名空间 。 Client.cs 包含作为应用程序入口点的 `Main` 方法。 在此示例中，编译两个代码模块，然后创建一个包含程序集清单的第三个文件，用于启动应用程序。 程序集清单引用 Client 和 Stringer 这两个模块 。

> [!NOTE]
> 多文件程序集只能有一个入口点，即使该程序集具有多个代码模块。

创建多文件程序集的原因有以下几个：

- 合并用不同语言编写的模块。 这是创建多文件程序集最常见的原因。

- 将不常用的类型放在只在需要时才下载的模块中，以优化应用程序的下载。

    > [!NOTE]
    > 如果要创建将使用 `<object>` 标记和 Microsoft Internet Explorer 来下载的应用程序，创建多文件程序集就很重要。 在此方案中，创建与只包含程序集清单的代码模块分开的文件。 Internet Explorer 首先下载程序集清单，然后创建辅助线程以下载所需的任何其他模块或程序集。 由于正在下载包含程序集清单的文件，Internet Explorer 将不响应用户的输入。 包含程序集清单的文件越小，Internet Explorer 不作响应的时间就越短。

- 合并由多个开发人员编写的代码模块。 虽然每一位开发人员都可以将各个代码模块编译成程序集，但这样会强制一些类型公开（如果所有模块均放在多文件程序集中，则不会公开）。

创建程序集后，可为包含程序集清单（并因此包含程序集）的文件签名，或者为文件及程序集指定强名称并将其放在全局程序集缓存中。

## <a name="see-also"></a>请参阅

- [如何：生成多文件程序集](build-multifile-assembly.md)
- [使用程序集编程](../../standard/assembly/index.md)
