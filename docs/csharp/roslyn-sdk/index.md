---
title: .NET Compiler Platform SDK (Roslyn API)
description: 了解如何使用 .NET Compiler Platform SDK（亦称为“Roslyn API”）来理解 .NET 代码、发现并修复错误。
ms.date: 10/10/2017
ms.custom: mvc
ms.openlocfilehash: 872bfd388f6974a6d99f769c43e5d341454518cc
ms.sourcegitcommit: 67cf756b033c6173a1bbd1cbd5aef1fccac99e34
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226668"
---
# <a name="the-net-compiler-platform-sdk"></a>.NET Compiler Platform SDK

编译器在验证代码语法和语义时生成应用代码的详细模型。 此模型可用于根据源代码生成可执行输出。 .NET Compiler Platform SDK 提供对此模型的访问权限。 我们越来越依赖 IntelliSense、重构、智能重命名、“查找所有引用”和“转到定义”等集成开发环境 (IDE) 功能来提高工作效率。 我们依靠代码分析工具来提升代码质量，并依靠代码生成器来帮助构造应用。 随着这些工具越来越智能化，它们需要越来越多地访问仅由编译器在处理应用代码时创建的模型。 这就是 Roslyn API 的核心任务所在：打开“不透明匣”，让工具和最终用户能够共享编译器生成的大量代码相关信息。
通过 Roslyn，编译器成为平台（而不是不透明的源代码输入和目标代码输出转换器）：可用于在工具和应用程序中完成代码相关任务的 API。

## <a name="net-compiler-platform-sdk-concepts"></a>.NET Compiler Platform SDK 概念

.NET Compiler Platform SDK 极大地降低了创建以代码为中心的工具和应用的门槛。 它将在元编程、代码生成和转换、C# 和 Visual Basic 语言的交互式使用，以及在域特定语言中嵌入 C# 和 Visual Basic 等方面，创造许多创新的机遇。

使用 .NET Compiler Platform SDK，可以生成分析器和代码修补程序，从而发现和更正编码错误。 分析器不仅理解代码的语法和结构，还能检测应更正的做法。 代码修补程序建议一处或多处修复，以修复分析器发现的编码错误。 通常情况下，分析器和关联的代码修补程序一起打包在一个项目中。

分析器和代码修补程序通过静态分析来理解代码。 它们既不运行代码，也未带来其他测试方面的好处。 但是，它们可以指出经常导致 bug 的做法、无法维护的代码或标准原则冲突。

.NET Compiler Platform SDK 提供了一组 API，可便于检查和理解 C# 或 Visual Basic 基准代码。 由于可以使用这一基准代码，因此能够利用 .NET Compiler Platform SDK 提供的语法和语义分析 API，更轻松地编写分析器和代码修补程序。 从复制由编译器执行的分析的繁重任务中解放出来，可以将精力放在为项目或库查找常见编码错误并进行修复的更为重要的任务上。

这带来的一个小小的好处是，分析器和代码修补程序较小，在 Visual Studio 中加载时占用的内存比为了理解项目中的代码而编写自己的基准代码占用的内存少得多。 利用编译器和 Visual Studio 使用的相同类，可以创建自己的静态分析工具。 也就是说，团队可以使用分析器和代码修补程序，而不会对 IDE 的性能造成显著影响。

在下列三个主要方案中，需要编写分析器和代码修补程序：

1. [*强制执行团队编码标准*](#enforce-team-coding-standards)
1. [*提供库包方面的指导*](#provide-guidance-with-library-packages)
1. [*提供常规指南*](#provide-general-guidance)

## <a name="enforce-team-coding-standards"></a>强制执行团队编码标准

许多团队都有编码标准，通过由其他团队成员评审代码的形式强制执行。 分析器和代码修补程序可以让这个过程更加高效。 当开发人员与团队中的其他成员共享工作内容时，其他成员会执行代码评审。 在有任何注释前，开发人员一直都在完成新功能。 开发人员不符合团队做法的习惯就会得到强化，时间可能长达几周之久。

分析器在开发人员编写代码的同时运行。 这样，开发人员就可以获得即时反馈，从而立即遵循相关指南。 只要开始原型设计，开发人员就养成了编写符合标准的代码的习惯。 当功能可供人员评审时，所有标准指南就已强制执行。

团队可以生成分析器和代码修补程序，以发现违反团队编码做法的最常见做法。 这些分析器和代码修补程序可以安装到每个开发人员的计算机上，以强制执行标准。

## <a name="provide-guidance-with-library-packages"></a>提供库包方面的指导

NuGet 上有大量适用于 .NET 开发人员的库。
一些来自 Microsoft，一些来自第三方公司，另一些来自社区成员和志愿者。 如果开发人员可以成功使用这些库后，它们的采用率和评价就会有所提升。

除了提供文档之外，还可以提供分析程序和代码修补程序，用于发现和更正库的常见错误用法。 这些即时更正有助于开发人员更快地成功使用库。

可以将分析器和代码修补程序与 NuGet 上的库一起打包。 在这种情况下，每个安装了 NuGet 包的开发人员都还将安装分析器包。 所有使用库的开发人员都会立即从团队获得指导，即有关错误和建议更正做法的即时反馈。

## <a name="provide-general-guidance"></a>提供常规指南

.NET 开发人员社区已发现效果理想的体验模式和最好避免使用的模式。 一些社区成员已创建分析器来强制执行这些推荐模式。 随着掌握的信息越来越多，新见解是无止境的。

这些分析器可以上传到 [Visual Studio Marketplace](https://marketplace.visualstudio.com/vs) 中，并由开发人员使用 Visual Studio 进行下载。 语言和平台的新手可以快速了解公认做法，并尽早在 .NET 之旅中高效工作。 随着使用越来越广泛，这些做法就会被社区采用。

## <a name="next-steps"></a>后续步骤

.NET Compiler Platform SDK 包括用于代码生成、分析和重构的最新语言对象模型。 此部分从概念上概述了 .NET Compiler Platform SDK。 有关更多详细信息，可以参阅快速入门、示例和教程部分。

若要详细了解 .NET Compiler Platform SDK 概念，可以参阅下列五个主题：

- [使用语法可视化工具浏览代码](syntax-visualizer.md)
- [了解编译器 API 模型](compiler-api-model.md)
- [处理语法](work-with-syntax.md)
- [处理语义](work-with-semantics.md)
- [处理工作区](work-with-workspace.md)

若要开始，需要安装 .NET 编译器平台 SDK：

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

<!--

Turn this on as more of the conceptual content is in place:
- Try the [Quickstarts](quickstart/index.md) to create your first tutorial.
- Experiment with one of the [Tutorials](tutorials/index.md).
- Explore the [Samples](samples/index.md) to see some simple analyzers.
- Read the [Concepts](concepts/index.md) to understand the ideas behind analyzers and code fixes.

-->
