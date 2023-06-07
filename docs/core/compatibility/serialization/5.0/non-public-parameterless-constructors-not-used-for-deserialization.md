---
title: 中断性变更：非公共的无参数构造函数不用于反序列化
description: 了解 .NET 5.0 中的以下中断性变更：非公共的无参数构造函数不再用于 JsonSerializer 的反序列化。
ms.date: 10/18/2020
ms.openlocfilehash: 6bdcc92c61008aa4ee27370bbac4dbf4ee3ef7c8
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95759274"
---
# <a name="non-public-parameterless-constructors-not-used-for-deserialization"></a>非公共的无参数构造函数不用于反序列化

为了在所有受支持的目标框架名字对象 (TFM) 之间保持一致，默认情况下，非公共的无参数构造函数不再用于 <xref:System.Text.Json.JsonSerializer> 的反序列化。

## <a name="change-description"></a>更改描述

支持 .NET Standard 2.0 及更高版本（即版本 4.6.0 - 4.7.2）的独立 [System.Text.Json NuGet 包](https://www.nuget.org/packages/System.Text.Json/)的行为与 .NET Core 3.0 和 3.1 上的内置行为不一致。 在 .NET Core 3.x 上，内部和专用构造函数可用于反序列化。 在独立包中，不允许使用非公共构造函数，如果未定义任何公共的无参数构造函数，则会引发 <xref:System.MissingMethodException>。

从 .NET 5.0 和 System.Text.Json NuGet 包 5.0.0 开始，NuGet 包与内置 API 的行为一致。 默认情况下，序列化程序会忽略非公共构造函数，包括无参数构造函数。 序列化程序使用下面一种构造函数进行反序列化：

- 用 <xref:System.Text.Json.Serialization.JsonConstructorAttribute> 注释的公共构造函数。
- 公共无参数构造函数。
- 公共参数化构造函数（如果它是唯一的公共构造函数）。

如果这些构造函数都不可用，则在尝试对该类型进行反序列化时，将引发 <xref:System.NotSupportedException>。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="reason-for-change"></a>更改原因

- 为了在 <xref:System.Text.Json?displayProperty=fullName> 针对其生成的所有目标框架名字对象 (TFM) 之间强制执行一致的行为（.NET Core 3.0 和更高版本以及 .NET Standard 2.0）
- 因为 <xref:System.Text.Json.JsonSerializer> 不应调用某类型的非公共外围应用，无论它是构造函数、属性还是字段。

## <a name="recommended-action"></a>建议的操作

- 如果你拥有该类型并且这样可行，则将无参数构造函数设为公共。
- 否则，针对该类型实现 `JsonConverter<T>`，并控制反序列化行为。

## <a name="affected-apis"></a>受影响的 API

- <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=fullName>
- <xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A?displayProperty=fullName>

<!--

### Affected APIs

- `Overload:System.Text.Json.JsonSerializer.Deserialize`
- `Overload:System.Text.Json.JsonSerializer.DeserializeAsync`

### Category

Serialization

-->
