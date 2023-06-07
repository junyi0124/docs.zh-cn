---
title: '预定义的配置文件 (代码分析) '
description: 了解如何使用预定义的 editorconfig 和规则集文件来面向特定类型的代码分析。
ms.date: 09/24/2020
ms.topic: conceptual
ms.openlocfilehash: 4937dcab1183fa3f63be4afc71627a7c7c08c6bd
ms.sourcegitcommit: 665f8fc55258356f4d2f4a6585b750c974b26675
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/30/2020
ms.locfileid: "96590482"
---
# <a name="predefined-configuration-files"></a>预定义配置文件

预定义的 EditorConfig 和 [规则集](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules) 文件可用，使你能够快速轻松地启用一类代码质量规则，如安全性或设计规则。 通过启用特定类别的规则，可以确定目标问题和特定条件。 若要访问这些预定义的文件，请安装 [CodeAnalysis. NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) NuGet 分析器包。

[CodeAnalysis NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) 为以下规则类别包括预定义的 EditorConfig 文件和规则集：

- ┮Τ砏玥
- 数据流
- 设计
- 文档
- 全球化
- 互操作性
- 可维护性
- 命名
- 性能
- 从 FxCop 移植
- 可靠性
- 安全性
- 使用情况

每类规则都有一个 EditorConfig 或规则集文件，用于：

- 启用类别 (中的所有规则，并禁用所有其他规则) 
- 使用每个规则的默认严重性并默认启用 (设置并禁用所有其他规则) 

> [!TIP]
> "所有规则" 类别具有一个附加的 EditorConfig 或规则集文件，用于禁用所有规则。 使用此文件可快速删除项目中的任何分析器警告或错误。

## <a name="predefined-editorconfig-files"></a>预定义的 EditorConfig 文件

NetAnalyzers 分析器包的预定义 EditorConfig 文件位于安装了 NuGet 包的 *EditorConfig* 子目录中。 例如，启用所有安全规则的 EditorConfig 文件位于 *EditorConfig/SecurityRulesEnabled/EditorConfig*。

将所选的 *editorconfig* 文件复制到项目的根目录。

## <a name="predefined-rule-sets"></a>预定义规则集

CodeAnalysis. NetAnalyzers 分析器包的预定义规则集文件位于安装 NuGet 包 *的规则集子目录中* 。 例如，启用所有安全规则的规则集文件位于规则集 */SecurityRulesEnabled*。 复制一个或多个规则集，并将其粘贴到包含项目的目录中。

## <a name="see-also"></a>另请参阅

- [分析器配置](https://github.com/dotnet/roslyn-analyzers/blob/master/docs/Analyzer%20Configuration.md)
- [EditorConfig 的 .NET 代码样式规则选项](code-style-rule-options.md)
