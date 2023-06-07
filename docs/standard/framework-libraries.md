---
title: 框架库
description: 了解库如何为许多通用类型和应用特定的类型、算法及实用工具功能提供实现。
author: richlander
ms.date: 06/20/2016
ms.assetid: 7b77b6c1-8367-4602-bff3-91e4c05ac643
ms.openlocfilehash: 4e71f81b0dd72f4569d8b6b319b9e97491533d28
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96438196"
---
# <a name="framework-libraries"></a>框架库

.NET 提供广泛的标准类库集，称为基类库（核心集）或框架类库（完整集）。 这些库为许多通用类型和应用特定的类型、算法及实用工具功能提供实现。 商用库和社区库都构建在框架类库的顶层，可让用户针对各种计算任务轻松使用现成的库。

每个 .NET 实现随附了这些库的子集。 任何 .NET 实现预期都要使用基类库 (BCL) API，原因有两种：开发人员需要这些 API，流行的库需要这些 API 才能运行。 位于 BCL 上层的应用特定的库（例如 ASP.NET）并不能在所有 .NET 实现中使用。

## <a name="base-class-libraries"></a>基类库

BCL 提供最基本的类型和实用工具功能，是其他所有 .NET 类库的基础。 BCL 旨在提供极其通用的实现，对所有工作负荷一视同仁。 性能始终是一个重要的考虑因素，因为应用可能会优先使用特定的策略，例如，优先考虑低延迟而不是高吞吐量，或者优先考虑低内存而不是低 CPU 使用率。 这些库在总体上可以保证高性能，同时会根据不同的性能考虑因素采取折衷方案。 对于大多数应用而言，这种方案相当成功。

## <a name="primitive-types"></a>基元类型

.NET 包含一组基元类型，所有程序都使用这些类型（使用程度或大或小）。 这些类型包含数据，例如数字、字符串、字节和任意对象。 C# 语言包括这些类型的关键字。 下面列出了这些类型的一组示例，以及匹配的 C# 关键字。

* <xref:System.Object?displayProperty=nameWithType> ([object](../csharp/language-reference/builtin-types/reference-types.md#the-object-type)) - CLR 类型系统中的最基本基类。 它位于类型层次结构的根级别。
* <xref:System.Int16?displayProperty=nameWithType> ([short](../csharp/language-reference/builtin-types/integral-numeric-types.md)) - 16 位带符号整数类型。 也存在无符号 <xref:System.UInt16>。
* <xref:System.Int32?displayProperty=nameWithType> ([int](../csharp/language-reference/builtin-types/integral-numeric-types.md)) - 32 位带符号整数类型。 也存在无符号 [UInt32](../csharp/language-reference/builtin-types/integral-numeric-types.md)。
* <xref:System.Single?displayProperty=nameWithType> ([float](../csharp/language-reference/builtin-types/floating-point-numeric-types.md)) - 32 位浮点类型。
* <xref:System.Decimal?displayProperty=nameWithType> ([decimal](../csharp/language-reference/builtin-types/floating-point-numeric-types.md)) -128 位十进制类型。
* <xref:System.Byte?displayProperty=nameWithType> ([byte](../csharp/language-reference/builtin-types/integral-numeric-types.md)) - 表示内存字节的无符号 8 位整数。
* <xref:System.Boolean?displayProperty=nameWithType> ([bool](../csharp/language-reference/builtin-types/bool.md)) - 表示 `true` 或 `false` 的布尔类型。
* <xref:System.Char?displayProperty=nameWithType> ([char](../csharp/language-reference/builtin-types/char.md)) - 表示 Unicode 字符的 16 位数字类型。
* <xref:System.String?displayProperty=nameWithType> ([string](../csharp/language-reference/builtin-types/reference-types.md#the-string-type)) -表示一系列字符。 与 `char[]` 不同，但会针对 `string` 中的每个 `char` 启用索引。

## <a name="data-structures"></a>数据结构

.NET 包含一组数据结构，这些结构是几乎所有 .NET 应用的工作主力。 它们主要是集合，不过也包括其他类型。

* <xref:System.Array>- 表示可通过索引访问的强类型对象的数组。 具有与构造相符的固定大小。
* <xref:System.Collections.Generic.List%601> - 表示可通过索引访问的对象的强类型列表。 可根据需要自动调整大小。
* <xref:System.Collections.Generic.Dictionary%602> -表示根据键编制索引的值的集合。 可以通过键访问值。 可根据需要自动调整大小。
* <xref:System.Uri> - 提供统一资源标识符 (URI) 的对象表示形式和对 URI 各部分的轻松访问。
* <xref:System.DateTime> - 表示时间上的一刻，通常以日期和当天的时间表示。

## <a name="utility-apis"></a>实用工具 API

.NET 包含一组可为许多重要任务提供功能的实用工具 API。

* <xref:System.Net.Http.HttpClient> - 用于发送 HTTP 请求以及从 URI 所标识资源接收 HTTP 响应的 API。
* <xref:System.Xml.Linq.XDocument> - 用于配合 LINQ 加载和查询 XML 文档的 API。
* <xref:System.IO.StreamReader> - 用于读取文件的 API。
* <xref:System.IO.StreamWriter> - 用于写入文件的 API。

## <a name="app-model-apis"></a>应用模型 API

某些公司提供了可与 .NET 配合使用的多个应用模型。

* [ASP.NET](https://www.asp.net) - 提供用于构建网站和服务的 Web 框架。 受 Windows、Linux 和 macOS 的支持（取决于 ASP.NET 版本）。
