---
title: 使映像更易于在 .NET 中进行调试
description: 了解如何配置可执行映像，以便使用 IDE 开关和命令行选项更轻松地进行调试。
ms.date: 08/30/2018
helpviewer_keywords:
- images [.NET Framework], debugging
- executable image for debugging
- debugging [.NET Framework], executable images for
ms.assetid: 7d90ea7a-150f-4f97-98a7-f9c26541b9a3
ms.openlocfilehash: a3305dc864e7852c2336009503732a51868410d2
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90558506"
---
# <a name="making-an-image-easier-to-debug-in-net"></a>使映像更易于在 .NET 中进行调试

编译非托管代码时，可以通过设置 IDE 开关或命令线选项来配置可执行映像进行调试。 例如，可以使用 Visual C++ 中的 /Zi 命令行选项，使其发出调试符号文件（文件扩展名为 .pdb）****。 同样，/Od 命令行选项告知编译器禁用优化****。 生成的代码运行速度变慢，但调试起来更容易，但这是必需的。

在编译 .NET Framework 托管代码时，编译器（如 Visual C++、Visual Basic 和 c #）会将其源程序编译为 Microsoft 中间语言 (MSIL) 。 然后，在执行之前将 MSIL 编译为本机代码。 与非托管代码一样，可以通过设置 IDE 开关或命令行选项来配置可执行映像进行调试。 你还可以配置 JIT 编译以便进行调试，其方式大致相同。

这种 JIT 配置具有两个方面：

- 可以请求 JIT 编译器生成跟踪信息。 这使调试程序能将 MSIL 链与其机器码对应项相匹配，并跟踪存储本地变量和函数参数的位置。 从 .NET Framework 版本2.0 开始，JIT 编译器将始终生成跟踪信息，因此无需对其进行请求。

- 可以请求 JIT 编译器不优化生成的计算机代码。

通常，生成 MSIL 的编译器会根据你指定的 IDE 开关或命令行选项（例如/**Od**）适当地设置这些 JIT 编译器选项。

某些情况下，可能需要更改 JIT 编译器的行为，使其生成的机器码更易于调试。 例如，可能希望为零售版本或控制优化生成实时跟踪信息。 可以使用初始化 (.ini) 文件来执行此操作。

例如，如果您想要调试的程序集*MyApp.exe*调用，则可以在*MyApp.exe*所在的同一文件夹中创建一个名为*MyApp.ini*的文本文件，该文件包含以下三行：

```ini
[.NET Framework Debugging Control]
GenerateTrackingInfo=1
AllowOptimize=0
```

可将每个选项的值设置为 0 或 1，任何不存在选项默认为 0。 将 `GenerateTrackingInfo` 设为 1、`AllowOptimize` 设为 0，这可提供最简单的调试。

从 .NET Framework 版本2.0 开始，JIT 编译器将始终生成跟踪信息，而不考虑的值 `GenerateTrackingInfo` ; 但是， `AllowOptimize` 该值仍有效果。 如果使用 [Ngen.exe（本机映像生成器）](../tools/ngen-exe-native-image-generator.md)预编译本机映像而不进行优化，Ngen.exe 执行时 .ini 文件必须存在于 `AllowOptimize=0` 的目标文件夹。 如果在未优化的情况下预编译程序集，则必须先使用 NGen.exe **/uninstall** 选项删除预编译代码，然后再重新运行 Ngen.exe 将代码预编译为优化。 如果文件中不存在 .ini 文件，则默认情况下 Ngen.exe 将代码预编译为优化。

<xref:System.Diagnostics.DebuggableAttribute?displayProperty=nameWithType> 控制程序集的设置。 **DebuggableAttribute** 包含两个字段，用于控制 JIT 编译器是否应优化和/或生成跟踪信息。 从 .NET Framework 版本2.0 开始，JIT 编译器将始终生成跟踪信息。

对于零售版本，编译器不会设置任何 **DebuggableAttribute**。 默认情况下，JIT 编译器将生成最高性能，最难调试计算机代码。 启用实时跟踪会稍微降低性能，禁用优化会极大降低性能。

DebuggableAttribute 一次应用于整个程序集，而不是应用于程序集中的单个模块****。 因此，开发工具必须将自定义属性附加到程序集元数据令牌，如果已经创建程序集，则附加到名为 System.Runtime.CompilerServices.AssemblyAttributesGoHere 的类****。 然后，ALink 工具会将每个模块中的这些 **DebuggableAttribute** 属性升级到它们所属的程序集。 如果存在冲突，则 ALink 操作将失败。

> [!NOTE]
> 在 .NET Framework 1.0 版本中，当指定 /clr 和 /Zi 编译器选项时，Microsoft Visual C++ 编译器将添加 DebuggableAttribute************。 在 .NET Framework 版本1.1 中，你必须在代码中手动添加 **DebuggableAttribute** ，或使用 **/ASSEMBLYDEBUG** 链接器选项。

## <a name="see-also"></a>请参阅

- [调试、跟踪和分析](index.md)
- [启用 JIT 附加调试](enabling-jit-attach-debugging.md)
- [启用分析](/previous-versions/dotnet/netframework-4.0/s5ec0es1(v=vs.100))
