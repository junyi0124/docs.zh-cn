---
title: .NET 中的工具和诊断
author: IEvangelist
description: 了解 .NET 开发人员可用的开发和诊断工具。
ms.author: dapine
ms.date: 10/21/2020
ms.topic: overview
ms.openlocfilehash: 07c1a161a0bb429403db6852fe77749d83c19ec0
ms.sourcegitcommit: 98d20cb038669dca4a195eb39af37d22ea9d008e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2020
ms.locfileid: "96590987"
---
# <a name="tools-and-diagnostics-in-net"></a>.NET 中的工具和诊断

在本文中，你将了解 .NET 开发人员可用的各种工具。 借助 .NET，你有一个强大的软件开发工具包 (SDK) ，其中包含命令行接口 (CLI) 。 .NET CLI 可实现中的许多功能。 (Ide) 的已准备好的集成开发环境。 本文还提供了工作效率功能的资源，例如用于诊断性能问题的 .NET CLI 工具、内存泄漏、高 CPU、死锁和工具对代码分析的支持。

## <a name="net-sdk"></a>.NET SDK

.NET SDK 包括 .NET 运行时和 .NET CLI。 可以下载适用于 Windows、Linux、macOS 或 Docker 的 [.NET SDK](https://dotnet.microsoft.com/download) 。 有关详细信息，请参阅 [.NET SDK 概述](../core/sdk.md)。

## <a name="net-cli"></a>.NET CLI

.NET CLI 是一个跨平台的工具链，用于开发、构建、运行和发布 .NET 应用程序。 .NET CLI 附带了 .NET SDK。 有关详细信息，请参阅 [.NET CLI 概述](../core/tools/index.md)。

## <a name="ides"></a>IDE

可以在 [Visual Studio Code](https://code.visualstudio.com/docs)、 [Visual Studio](/visualstudio/windows)或 [Visual Studio for Mac](/visualstudio/mac)中编写 .net 应用程序。 有关支持云的开发环境的信息，请参阅 [Visual Studio Codespaces](/visualstudio/codespaces/overview/what-is-vsonline)。

## <a name="additional-tools"></a>其他工具

除了更常见的工具外，.NET 还提供适用于特定方案的工具。 一些用例包括卸载 .NET SDK 或 .NET 运行时、检索 Windows Communication Foundation (WCF) 元数据、生成代理源代码和序列化 XML。 有关详细信息，请参阅 [.net 附加工具概述](../core/additional-tools/index.md)。

## <a name="diagnostics-and-instrumentation"></a>诊断和检测

作为 .NET 开发人员，你可以使用常见的性能诊断工具来监视应用性能、使用跟踪分析应用、收集性能指标和分析转储文件。 使用事件计数器收集性能指标，并使用分析工具深入了解应用的执行方式。 有关详细信息，请参阅 [.net 诊断工具](../core/diagnostics/index.md)。

## <a name="code-analysis"></a>代码分析

.NET 编译器平台 (Roslyn) 分析器检查 c # 或 Visual Basic 代码以了解代码质量和代码样式问题。 有关详细信息，请参阅 [.net 源代码分析概述](code-analysis/overview.md)。
