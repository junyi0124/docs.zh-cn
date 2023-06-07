---
title: .NET Compiler Platform SDK 概念和对象模型
description: 此概述提供了高效使用 .NET 编译器 SDK 所需的背景。 介绍了 API 层、涉及的主要类型以及总体对象模型。
ms.date: 07/13/2020
ms.custom: mvc
ms.openlocfilehash: a65d282dd3c58279bbfd635c0386d50ce3f30055
ms.sourcegitcommit: e7748001b1cee80ced691d8a76ca814c0b02dd9b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/14/2020
ms.locfileid: "86374463"
---
# <a name="understand-the-net-compiler-platform-sdk-model"></a>了解 .NET Compiler Platform SDK 模型

编译器按照结构化规则处理代码，这些规则通常不同于用户读取和理解代码的方式。 基本了解编译器使用的模型对于了解生成基于 Roslyn 的工具时使用的 API 至关重要。

## <a name="compiler-pipeline-functional-areas"></a>编译器管道功能区域

.NET Compiler Platform SDK 通过提供镜像传统编译器管道的 API 层，作为使用者向用户公开 C# 和 Visual Basic 编译器的代码分析。

![编译器管道将源代码处理为对象代码的步骤](media/compiler-api-model/compiler-pipeline.png)

此管道的每个阶段都是一个单独的组件。 首先，分析阶段标记化源文本，并将其分析为遵循语言语法的语法。 随后，声明阶段分析源和导入的元数据，形成命名符号。 然后，绑定阶段将代码中的标识符与符号匹配。 最后，发出阶段发出所有信息均由编译器生成的程序集。

![编译器管道 API 提供对编译器管道中包含的每个步骤的访问权限](media/compiler-api-model/compiler-pipeline-api.png)

.NET Compiler Platform SDK 对应于每个阶段提供一个对象模型，该模型允许访问该阶段的信息。 分析阶段公开一个语法树，声明阶段公开一个分层符号表，绑定阶段公开编译器语义分析的结果，发出阶段公开生成 IL 字节代码的 API。

![编译器管道每个步骤的编译器 API 提供的语言服务](media/compiler-api-model/compiler-pipeline-lang-svc.png)

每个编译器将这些组件合并在一起，组成一个端到端整体。

这些 API 与 Visual Studio 使用的 API 相同。 例如，代码大纲和格式设置功能使用语法树，对象浏览器和导航功能使用符号表，重构和转到定义使用语义模型，编辑并继续使用所有这些信息，包括发出 API  。

## <a name="api-layers"></a>API 层

.NET 编译器 SDK 包含多个 API 层：编译器 API、诊断 API、脚本 API 和工作区 API。

### <a name="compiler-apis"></a>编译器 API

编译器层包含的对象模型（语法模型和语义模型）对应于在编译器管道的每个阶段公开的信息。 编译器层还包含编译器的单次调用的不可变快照，包括程序集引用、编译器选项和源代码文件。 C# 语言和 Visual Basic 语言由两个不同的 API 表示。 这两个 API 的形状类似，但针对每种语言的高保真进行了定制。 此层不包含 Visual Studio 组件的依赖项。

### <a name="diagnostic-apis"></a>诊断 API

在编译器分析过程中，编译器会生成一组诊断，包括语法、语义、明确赋值以及各种警告和信息性诊断。 编译器 API 层通过一个可扩展 API 公开诊断，该可扩展 API 允许将用户定义的分析器插入编译过程。 它支持随编译器定义的诊断一起，生成用户定义的诊断，例如由 StyleCop 或 FxCop 等工具生成的诊断。 以这种方式生成诊断有以下优势：与 MSBuild 和 Visual Studio 等工具（具体取决于对根据策略停止生成等体验进行的诊断）自然地集成、在编辑器中显示实时波形曲线，以及建议代码修复。

### <a name="scripting-apis"></a>脚本 API

托管 API 和脚本 API 是编译器层的一部分。 可使用它们来执行代码片段和累积运行时执行上下文。
C# 交互式 REPL （读取–求值–打印循环）可使用这些 API。 借助 REPL，可将 C# 用作脚本语言，在编写代码的同时，以交互方式执行代码。

### <a name="workspaces-apis"></a>工作区 API

工作区层包含工作区 API，是对整个解决方案执行代码分析和重构的起点。 它协助你将解决方案中项目的全部相关信息整理到单个对象模型中，可便于你直接访问编译器层对象模型，而无需分析文件、配置选项或管理项目到项目依赖关系。

此外，工作区层还包含一组 API，用于实现在 Visual Studio IDE 等主机环境中使用的代码分析和重构工具。 相关示例包括查找所有引用、格式设置和代码生成 API。

此层不包含 Visual Studio 组件的依赖项。
