---
title: 如何使用 System.Text.Json 支持某种无效的 JSON
description: 了解在 .NET 中序列化为 JSON 和从 JSON 进行反序列化时如何支持注释、尾随逗号和带引号的数字。
ms.date: 12/03/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
zone_pivot_groups: dotnet-version
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 2559b081010fb0a2fa208b121cb095efdeb8da2e
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009803"
---
# <a name="how-to-allow-some-kinds-of-invalid-json-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 支持某种无效的 JSON

本文将介绍如何能够在 JSON 中使用注释、尾随逗号和带引号的数字，以及如何将数字编写为字符串。

## <a name="allow-comments-and-trailing-commas"></a>允许注释和尾随逗号

默认情况下，JSON 中不允许使用注释和尾随逗号。 若要在 JSON 中允许注释，请将 <xref:System.Text.Json.JsonSerializerOptions.ReadCommentHandling?displayProperty=nameWithType> 属性设置为 `JsonCommentHandling.Skip`。
若要允许尾随逗号，请将 <xref:System.Text.Json.JsonSerializerOptions.AllowTrailingCommas?displayProperty=nameWithType> 属性设置为 `true`。 下面的示例演示如何允许这两者：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/DeserializeCommasComments.cs" id="Deserialize":::

下面是包含注释和尾随逗号的示例 JSON：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25, // Fahrenheit 77
  "Summary": "Hot", /* Zharko */
  // Comments on
  /* separate lines */
}
```

## <a name="allow-or-write-numbers-in-quotes"></a>允许或写入带引号的数字

::: zone pivot="dotnet-5-0"

某些序列化程序将数字编码为 JSON 字符串（括在引号中）。

例如：

```json
{
    "DegreesCelsius": "23"
}
```

不是：

```json
{
    "DegreesCelsius": 23
}
```

若要在整个输入对象图中序列化带引号的数字或接受带引号的数字，请设置 <xref:System.Text.Json.JsonSerializerOptions.NumberHandling%2A?displayProperty=nameWithType>，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/QuotedNumbers.cs" highlight="27-29":::

在通过 ASP.NET Core 间接使用 `System.Text.Json` 时，反序列化时允许使用带引号的数字，因为 ASP.NET Core 指定 [Web 默认选项](xref:System.Text.Json.JsonSerializerDefaults.Web)。

若要允许或写入特定属性、字段或类型的带引号的数字，请使用 [[JsonNumberHandling]](xref:System.Text.Json.Serialization.JsonNumberHandlingAttribute) 特性。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 中的 `System.Text.Json` 不支持序列化或反序列化用引号引起来的数字。 有关详细信息，请参阅[允许或写入带引号的数字](system-text-json-migrate-from-newtonsoft-how-to.md#allow-or-write-numbers-in-quotes)。
::: zone-end

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何对 JSON 进行序列化和反序列化](system-text-json-how-to.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [自定义属性名称和值](system-text-json-customize-properties.md)
* [忽略属性](system-text-json-ignore-properties.md)
* [处理溢出 JSON](system-text-json-handle-overflow.md)
* [保留引用](system-text-json-preserve-references.md)
* [不可变类型和非公共访问器](system-text-json-immutability.md)
* [多态序列化](system-text-json-polymorphism.md)
* [从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [自定义字符编码](system-text-json-character-encoding.md)
* [编写自定义序列化程序和反序列化程序](write-custom-serializer-deserializer.md)
* [编写用于 JSON 序列化的自定义转换器](system-text-json-converters-how-to.md)
* [DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
* [System.Text.Json.Serialization API 参考](xref:System.Text.Json.Serialization)
