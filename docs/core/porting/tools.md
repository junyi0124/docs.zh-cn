---
title: 用于移植到 .NET Core 的工具
description: 了解可以用于移植到 .NET Core 的一些工具
author: cartermp
ms.date: 05/03/2020
ms.openlocfilehash: b8678ec72fe5d910d5f8cff4768106e7496f9276
ms.sourcegitcommit: 1274a1a4a4c7e2eaf56b38da76ef7cec789726ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/28/2020
ms.locfileid: "91406123"
---
# <a name="tools-to-help-with-porting-to-net-core"></a>用于帮助移植到 .NET Core 的工具

你可能会发现本文中列出的工具在移植时非常有用：

- [.NET 可移植性分析器](../../standard/analyzers/portability-analyzer.md) - 可以生成报告的工具链，该报告说明代码在 .NET Framework 和 .NET Core 之间的可移植性：
  - 作为[命令行工具](https://github.com/Microsoft/dotnet-apiport/releases)
  - 作为 [Visual Studio 扩展](https://marketplace.visualstudio.com/items?itemName=ConnieYau.NETPortabilityAnalyzer)
- [平台兼容性分析器](../../standard/analyzers/platform-compat-analyzer.md) - 一种 Roslyn 分析器，如果开发人员在 API 可能不可用时使用来自调用站点的平台专用 API，它会通知开发人员。
- [.NET API 分析器](../../standard/analyzers/api-analyzer.md) - 一种 Roslyn 分析器，可帮助检测跨平台应用和库中的平台兼容性问题。
- [try-convert](https://www.nuget.org/packages/try-convert/) - .NET Core 全局工具，可用于将项目或整个解决方案转换为 .NET SDK，包括将桌面应用迁移到 .NET Core。 如果创建了更复杂的生成（如自定义任务、目标或导入），并且拒绝许多与 .NET Core 不兼容的项目类型，则不建议使用此工具。
