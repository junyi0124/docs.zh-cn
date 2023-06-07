---
title: 使用 .NET Compiler Platform SDK 工作区模型
description: 此概述介绍了用于查询和操作代码的工作区和项目的类型。
ms.date: 10/15/2017
ms.custom: mvc
ms.openlocfilehash: d21873b132d5f0788033693a319e556feeac59a9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156878"
---
# <a name="work-with-a-workspace"></a>使用工作区

工作区  层是对整个解决方案执行代码分析和重构的起点。 此层内的工作区 API 有助于将解决方案中项目的全部相关信息组织为单个对象模型，可让用户直接访问编译器层对象模型（如源文本、语法树、语义模型和编译），而无需分析文件、配置选项，或管理项目内依赖项。

托管环境（如 IDE）提供对应于打开的解决方案的工作区。 此外，只需加载解决方案文件，即可在 IDE 外部使用此模型。

## <a name="workspace"></a>工作区

工作区是作为项目集合的解决方案的活动表示形式，每个工作区都包含一个文档集合。 工作区通常与随用户键入或操作属性而不断更改的主机环境关联。

<xref:Microsoft.CodeAnalysis.Workspace> 提供对当前解决方案模型的访问权限。 主机环境中发生更改时，工作区会引发相应事件，并更新 <xref:Microsoft.CodeAnalysis.Workspace.CurrentSolution?displayProperty=nameWithType> 属性。 例如，用户在一个源文档对应的文本编辑器中键入时，工作区使用一个事件来指示解决方案的整体模型已更改以及修改了哪个文档。 然后，可通过分析新模型的正确性、突出显示重要区域，或针对代码更改提供建议来响应这些更改。

此外，还可创建与主机环境断开连接或用于没有主机环境的应用程序的独立工作区。

## <a name="solutions-projects-documents"></a>解决方案、项目、文档

尽管每次按下一个键时工作区都可能会更改，仍可以隔离方式使用解决方案模型。

一个解决方案即是项目和文档的一个不可变模型。 这意味着可共享模型，而不会锁定或重复。 从 <xref:Microsoft.CodeAnalysis.Workspace.CurrentSolution?displayProperty=nameWithType> 属性获取解决方案实例后，该实例永远不会更改。 但是，与语法树和编译类似，可通过基于现有解决方案和特定更改构造新实例来修改解决方案。 要获取工作区以反映所做的更改，必须将更改后的解决方案显式应用回工作区。

项目是整体不可变解决方案模型的一部分。 它表示所有源代码文档、分析和编译选项，以及程序集和项目到项目引用。 可从项目中访问相应的编译，而无需确定项目依赖项或分析任何源文件。

文档也是整体不可变解决方案模型的一部分。 文档表示单个源文件，可从其中访问文件、语法树和语义模型的文本。

下图表示了如何将工作区关联到主机环境和工具，以及如何进行编辑。

![包含项目和源文件的工作区中不同元素之间的关系](media/work-with-workspace/workspace-obj-relations.png)

## <a name="summary"></a>总结

Roslyn 公开了一组编译器 API 和工作区 API，提供了有关源代码的丰富信息，并完全保真地记录了 C# 和 Visual Basic 语言。  .NET Compiler Platform SDK 极大地降低了创建以代码为中心的工具和应用程序的门槛。 它将在元编程、代码生成和转换、C# 和 Visual Basic 语言的交互式使用，以及在域特定语言中嵌入 C# 和 Visual Basic 等方面，创造许多创新的机遇。  
