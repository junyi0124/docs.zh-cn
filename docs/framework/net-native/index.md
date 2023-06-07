---
title: 使用 .NET Native 编译引用
ms.date: 03/30/2017
helpviewer_keywords:
- native compilation
- .NET and native code
- compilation with .NET Native
- .NET Native
- C# and native compilation
ms.assetid: 47cd5648-9469-4b1d-804c-43cc04384045
ms.openlocfilehash: 7601a6d5e7f49b6d8fc434ef772e2e69740f02cf
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90543928"
---
# <a name="compiling-apps-with-net-native"></a>使用 .NET Native 编译引用

.NET Native 是一种用于生成和部署 Visual Studio 2015 及更高版本附带的 Windows 应用的预编译技术。 它自动将以托管代码（C# 或 Visual Basic）编写并面向 .NET Framework 和 Windows 10 的发布版本的应用编译为本机代码。

通常，定位于 .NET Framework 的应用会被编译到中间语言 (IL) 中。 在运行时间，及时生成 (JIT) 编译器将 IL 翻译为本机代码。 与此相反，.NET Native 会直接将 Windows 应用编译为本机代码。 对于开发者，这意味着：

- 您的应用程序具有本机代码的性能。 通常，性能比首次编译为 IL 的代码更优越，并由 JIT 编译器编译为本机代码。

- 你可以继续用 C# 或 Visual Basic 进行编程。

- 你可以继续使用 .NET Framework 提供的资源，包括类库、自动内存管理、垃圾回收和异常处理。

对于你的应用程序的用户，.NET Native 具有以下优势：

- 大多数应用和方案的执行时间更快。

- 大多数应用和方案的启动时间更快。

- 低部署和更新成本。

- 优化的应用内存使用情况。

> [!IMPORTANT]
> 对于绝大多数应用和方案，与编译到 IL 或 NGEN 映像的应用相比，.NET Native 提供明显更快的启动时间和更高的性能。 但是，您的结果可能会有所不同。 若要确保你的应用程序已受益于 .NET Native 的性能增强，你应将其性能与应用程序的 non-.NET 本机版本的性能进行比较。 有关详细信息，请参阅 [性能会话概述](/visualstudio/profiling/performance-session-overview)。

但 .NET Native 只涉及到本机代码的编译。 它会改变 .NET Framework 应用的创建和执行方式。 具体而言：

- 在预编译期间，所需的 .NET Framework 部分会静态连接到你的应用。 这允许应用同 .NET Framework 的应用本地库一起运行，并且允许编译器执行全局分析，从而提供性能优势。 因此，应用甚至在 .NET Framework 更新后会启动得更快。

- .NET Native 运行时针对静态预编译进行了优化，在大多数情况下，可提供优异的性能。 同时，它保留了开发者认为非常高效的核心反射特性。

- .NET Native 使用的后端与 c + + 编译器相同，后者针对静态预编译方案进行了优化。

.NET Native 可以将 c + + 的性能优势引入到托管代码开发人员，因为它使用的是与 c + + 中的 c + + 相同或类似的工具，如下表所示。

||.NET Native|C++|
|-|----------------------------------------------------------------|-----------|
|库|.NET Framework + Windows 运行时|Win32 + Windows 运行时|
|编译器|UTC 优化编译器|UTC 优化编译器|
|已部署|随时可以运行的二进制代码|随时可以运行的二进制代码 (ASM)|
|运行时|MRT.dll（最短 CLR 运行时）|CRT.dll（C 运行时）|

对于适用于 Windows 10 的 Windows 应用，可将应用程序包（.appx 文件）中的 .NET 本机代码编译二进制文件上载到 Windows 应用商店。

## <a name="in-this-section"></a>本节内容

要查看有关带有 .NET Native 代码编译的应用开发的信息，请参阅以下主题：

- [.NET Native 代码编译入门：开发者经验介绍](getting-started-with-net-native.md)

- [.NET 本机和编译：](net-native-and-compilation.md) .NET 本机如何将你的项目编译为本机代码。

- [反射和 .NET Native](reflection-and-net-native.md)

  - [利用反射的 API](apis-that-rely-on-reflection.md)

  - [反射 API 引用](net-native-reflection-api-reference.md)

  - [运行时指令 (rd.xml) 配置文件引用](runtime-directives-rd-xml-configuration-file-reference.md)

- [序列化和元数据](serialization-and-metadata.md)

- [将 Windows 应用商店应用迁移到 .NET Native](migrating-your-windows-store-app-to-net-native.md)

- [.NET Native 一般疑难解答](net-native-general-troubleshooting.md)
