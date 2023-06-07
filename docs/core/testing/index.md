---
title: 在 .NET 中测试
description: 本文简要概述了在 .NET 中测试的测试概念、术语和工具。
author: IEvangelist
ms.author: dapine
ms.date: 10/19/2020
ms.openlocfilehash: 137a8f4e3bc9e3be529d5034c233d283cf158b31
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824852"
---
# <a name="testing-in-net"></a>在 .NET 中测试

本文介绍了测试概念，并说明了如何使用不同类型的测试来验证代码。 有多种工具可用于测试 .NET 应用程序，如 [.NET CLI](#net-cli) 或[集成开发环境 (IDE)](#ide)。

## <a name="test-types"></a>测试类型

使用自动测试是确保应用程序代码按作者期望执行操作的一种绝佳方式。 本文介绍了单元测试、集成测试和负载测试。

### <a name="unit-tests"></a>单元测试

单元测试是一种试验单个软件组件或方法（也称为“工作单元”）的测试。 单元测试仅应测试开发人员控件内的代码。 它们不测试基础结构问题。 基础结构问题包括与数据库、文件系统和网络资源的交互。

有关创建单元测试的详细信息，请参阅[测试工具](#testing-tools)。

### <a name="integration-tests"></a>集成测试

*集成测试* 与单元测试的不同之处在于，它试验两个或更多软件组件一同工作（也称为其“集成”）的能力。 这些测试在更广泛范围的受测系统上运行，而单元测试则侧重于单个组件。 通常，集成测试会包括对基础结构问题的测试。

### <a name="load-tests"></a>负载测试

负载测试旨在确定系统是否可以处理指定的负载，例如，使用应用程序的并发用户数和应用程序响应性处理交互的能力。 有关 Web 应用程序负载测试的详细信息，请参阅 [ASP.NET Core 负载/压力测试](/aspnet/core/test/load-tests)。

## <a name="test-considerations"></a>测试注意事项

请记住，可以使用编写测试的[最佳做法](unit-testing-best-practices.md)。 例如，[测试驱动开发 (TDD)](https://deviq.com/test-driven-development) 是指先编写单元测试，再编写该单元测试要检查的代码。 TDD 就像先编写书籍大纲，再编写该书籍。 它旨在帮助开发人员编写更简单、更具可读性的高效代码。

## <a name="testing-tools"></a>测试工具

.NET 是一个多语言开发平台，可以为 [C#](../../csharp/index.yml)、[F#](../../fsharp/index.yml) 和 [Visual Basic](../../visual-basic/index.yml) 编写各种测试类型。 对于每种语言，可以在几个测试框架中进行选择。

### <a name="xunit"></a>xUnit

[xUnit](https://xunit.net) 是一个适用于 .NET 的免费、开源、面向社区的单元测试工具。 xUnit.net 由 NUnit v2 的原发明者编写，是针对单元测试 .NET 应用的最新技术。 xUnit.net 适用于 ReSharper、CodeRush、TestDriven.NET 和 [Xamarin](https://dotnet.microsoft.com/apps/xamarin)。 它是 [.NET Foundation](https://dotnetfoundation.org) 的项目，并在其行为准则下运行。

有关更多信息，请参见以下资源：

- [使用 C# 执行单元测试](unit-testing-with-dotnet-test.md)
- [使用 F# 执行单元测试](unit-testing-fsharp-with-dotnet-test.md)
- [使用 Visual Basic 执行单元测试](unit-testing-visual-basic-with-dotnet-test.md)

### <a name="nunit"></a>NUnit

[NUnit](https://nunit.org) 是适用于所有 .NET 语言的单元测试框架。 最初从 JUnit 移植而来，当前生产版本已被重写，添加了许多新功能和对各种 .NET 平台的支持。 它是 [.NET Foundation](https://dotnetfoundation.org) 的项目。

有关更多信息，请参见以下资源：

- [使用 C# 执行单元测试](unit-testing-with-nunit.md)
- [使用 F# 执行单元测试](unit-testing-fsharp-with-nunit.md)
- [使用 Visual Basic 执行单元测试](unit-testing-visual-basic-with-nunit.md)

### <a name="mstest"></a>MSTest

[MSTest](https://github.com/Microsoft/testfx-docs) 是适用于所有 .NET 语言的 Microsoft 测试框架。 它可以扩展，并且适用于 .NET CLI 和 Visual Studio。 有关更多信息，请参见以下资源：

- [使用 C# 执行单元测试](unit-testing-with-mstest.md)
- [使用 F# 执行单元测试](unit-testing-fsharp-with-mstest.md)
- [使用 Visual Basic 执行单元测试](unit-testing-visual-basic-with-mstest.md)

### <a name="net-cli"></a>.NET CLI

你可以使用 [dotnet test](../tools/dotnet-test.md) 命令，从 [.NET CLI](../tools/index.md) 运行解决方案单元测试。 .NET CLI 公开了 [集成开发环境 (IDE)](#ide) 通过用户界面提供的大部分功能。 .NET CLI 是跨平台的，可作为持续集成和交付管道的一部分使用。 .NET CLI 用于脚本化进程，以自动执行常见任务。

### <a name="ide"></a>IDE

无论使用的是 Visual Studio、Visual Studio for Mac 还是 Visual Studio Code，都有用于测试功能的图形用户界面。 IDE 提供了比 CLI 更多的功能，例如 [Live Unit Testing](/visualstudio/test/live-unit-testing)。 有关详细信息，请参阅 [Visual Studio 中的包含与排除测试](/visualstudio/test/live-unit-testing#include-and-exclude-test-projects-and-test-methods)。

## <a name="see-also"></a>另请参阅

有关详细信息，请参阅以下文章：

- [使用 .NET 执行单元测试的最佳实践](unit-testing-best-practices.md)
- ASP.NET Core 中的集成测试  
- [运行选择性单元测试](selective-unit-tests.md)
- [将代码覆盖率用于单元测试](unit-testing-code-coverage.md)
