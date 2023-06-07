---
title: .NET 程序集文件格式
description: 了解 .NET 程序集文件格式，使用它描述和包含 .NET 应用和库。
author: richlander
ms.date: 08/20/2019
ms.assetid: 6520323e-ff28-4c8a-ba80-e64a413199e6
ms.openlocfilehash: b1de3f46f04f24dd4bbb2f695de8741feb29f226
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95731535"
---
# <a name="net-assembly-file-format"></a>.NET 程序集文件格式

.NET 定义用于充分描述和包含 .NET 程序的二进制文件格式 - 程序集。 程序集用于程序本身以及所有依赖库。 .NET 程序可作为一个或多个程序集运行，除了相应的 .NET 实现外，无需其他任何项目。 本机依赖项（包括操作系统 API）是一个需要单独考虑的问题，虽然有时会使用 .NET 程序集格式来描述它们，但并未将其包含在此格式内（例如，WinRT）。

> 每个 CLI 组件都带有特定于该组件、用于声明、实现和引用的元数据。 因此，特定于组件的元数据被称为组件元数据，并且自 ECMA 335 I.9.1 生成的组件被认为是自描述组件和程序集。

该格式完全被指定并标准化为 [ECMA 335](https://www.ecma-international.org/publications/standards/Ecma-335.htm)。 所有 .NET 编译器和运行时都使用此格式。 已编入文档、偶尔更新的二进制格式的出现对于互操作性而言是一个主要优势（也可以说是要求）。 该格式上一次进行实质更新是在 2005 年 (.NET Framework 2.0)，目的是为了容纳泛型和处理器体系结构。

格式为 CPU 和 OS 不可知。 它被用作针对许多芯片和 CPU 的 .NET 实现的一部分。 虽然格式本身继承了 Windows，但它可用于所有操作系统。 对于 OS 互操作性，这可以说是一项最重大的选择，因为大部分值存储在 Little-endian 格式中。 它与计算机的指针大小没有特定关联（例如，32 位、64 位）。

.NET 程序集格式也详细介绍了给定程序和库的结构。 它介绍了程序集的内部组件，特别是定义的程序集引用和类型及其内部结构。 工具或 API 可读取和处理此信息以进行显示或做出程序化决策。

## <a name="format"></a>格式

.NET 二进制格式以 Windows [PE 文件](https://en.wikipedia.org/wiki/Portable_Executable)格式为基础。 实际上，.NET 类库符合 Windows PE，咋看之下会显示为 Windows 动态链接库 (DLL) 或应用程序可执行文件 (EXE)。 此特性在 Windows 上十分有用，它们可以伪装成本地可执行二进制文件，并获得相同的处理方式（例如，OS 加载，PE 工具）。

![程序集头](../media/assembly-format/assembly-headers.png)

来自 ECMA 335 II.25.1 的程序集头，运行时文件格式结构。

## <a name="process-the-assemblies"></a>处理程序集

可以编写工具或 API 来处理程序集。 程序集信息能够在运行时做出程序化决策、重新编写程序集、在编辑器中提供 API IntelliSense 以及生成文档。 <xref:System.Reflection?displayProperty=nameWithType>、<xref:System.Reflection.MetadataLoadContext?displayProperty=nameWithType> 和 [Mono.Cecil](https://www.mono-project.com/docs/tools+libraries/libraries/Mono.Cecil/) 是常用于此目的的典型工具。
