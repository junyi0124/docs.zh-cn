---
title: 如何使用 System.Text.Json 自定义属性名称和值
description: 了解使用 .NET 中的 System.Text.Json 进行序列化时如何自定义属性名称和值。
ms.date: 11/30/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 4b88509313e719ea993e00d889bc6145f4976a2d
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008898"
---
# <a name="how-to-customize-property-names-and-values-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 自定义属性名称和值

默认情况下，属性名称和字典键在 JSON 输出中保持不变，包括大小写。 枚举值表示为数字。 本文将指导如何进行以下操作：

> [!NOTE]
> [Web 默认值](system-text-json-configure-options.md#web-defaults-for-jsonserializeroptions)为 camel 形式大小写。

* [自定义单个属性名称](#customize-individual-property-names)
* [将所有属性名称转换为 camel 大小写](#use-camel-case-for-all-json-property-names)
* [实现自定义属性命名策略](#use-a-custom-json-property-naming-policy)
* [将字典键转换为 camel 大小写](#camel-case-dictionary-keys)
* [将枚举转换为字符串和 camel 大小写](#enums-as-strings)

对于需要对 JSON 属性名称和值进行特殊处理的其他方案，可以[实现自定义转换器](system-text-json-converters-how-to.md)。

## <a name="customize-individual-property-names"></a>自定义单个属性名称

若要设置单个属性的名称，请使用 [[JsonPropertyName]](xref:System.Text.Json.Serialization.JsonPropertyNameAttribute) 特性。

下面是要进行序列化的示例类型和生成的 JSON：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithPropertyNameAttribute":::

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "Wind": 35
}
```

此特性设置的属性名称：

* 同时适用于两个方向（序列化和反序列化）。
* 优先于属性命名策略。

## <a name="use-camel-case-for-all-json-property-names"></a>对所有 JSON 属性名称使用 camel 大小写

若要对所有 JSON 属性名称使用 camel 大小写，请将 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> 设置为 `JsonNamingPolicy.CamelCase`，如下面的示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundTripCamelCasePropertyNames.cs" id="Serialize":::

下面是要进行序列化的示例类和 JSON 输出：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithPropertyNameAttribute":::

```json
{
  "date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "summary": "Hot",
  "Wind": 35
}
```

camel 大小写属性命名策略：

* 适用于序列化和反序列化。
* 由 `[JsonPropertyName]` 特性替代。 这便是示例中的 JSON 属性名称 `Wind` 不是 camel 大小写的原因。

## <a name="use-a-custom-json-property-naming-policy"></a>使用自定义 JSON 属性命名策略

若要使用自定义 JSON 属性命名策略，请创建派生自 <xref:System.Text.Json.JsonNamingPolicy> 的类，并替代 <xref:System.Text.Json.JsonNamingPolicy.ConvertName%2A> 方法，如下面的示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/UpperCaseNamingPolicy.cs":::

然后，将 <xref:System.Text.Json.JsonSerializerOptions.PropertyNamingPolicy?displayProperty=nameWithType> 属性设置为命名策略类的实例：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripPropertyNamingPolicy.cs" id="Serialize":::

下面是要进行序列化的示例类和 JSON 输出：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithPropertyNameAttribute":::

```json
{
  "DATE": "2019-08-01T00:00:00-07:00",
  "TEMPERATURECELSIUS": 25,
  "SUMMARY": "Hot",
  "Wind": 35
}
```

JSON 属性命名策略：

* 适用于序列化和反序列化。
* 由 `[JsonPropertyName]` 特性替代。 这便是示例中的 JSON 属性名称 `Wind` 不是大写的原因。

## <a name="camel-case-dictionary-keys"></a>Camel 大小写字典键

如果要序列化的对象的属性为 `Dictionary<string,TValue>` 类型，则 `string` 键可转换为 camel 大小写。 为此，请将 <xref:System.Text.Json.JsonSerializerOptions.DictionaryKeyPolicy> 设置为 `JsonNamingPolicy.CamelCase`，如下面的示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializeCamelCaseDictionaryKeys.cs" id="Serialize":::

使用名为 `TemperatureRanges` 且具有键值对 `"ColdMinTemp", 20` 和 `"HotMinTemp", 40` 的字典序列化对象会产生类似于以下示例的 JSON 输出：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "TemperatureRanges": {
    "coldMinTemp": 20,
    "hotMinTemp": 40
  }
}
```

字典键的 camel 大小写命名策略仅适用于序列化。 如果对字典进行反序列化，即使为 `DictionaryKeyPolicy`指定 `JsonNamingPolicy.CamelCase`，键也会与 JSON 文件匹配。

## <a name="enums-as-strings"></a>作为字符串的枚举

默认情况下，枚举会序列化为数字。 若要将枚举名称序列化为字符串，请使用 <xref:System.Text.Json.Serialization.JsonStringEnumConverter>。

例如，假设需要序列化以下具有枚举的类：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithEnum":::

如果 Summary 为 `Hot`，则默认情况下序列化的 JSON 具有数值 3：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": 3
}
```

下面的示例代码序列化枚举名称(而不是数值)，并将名称转换为 camel 大小写：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripEnumAsString.cs" id="Serialize":::

生成的 JSON 类似于以下示例：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "hot"
}
```

还可以反序列化枚举字符串名称，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripEnumAsString.cs" id="Deserialize":::

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何对 JSON 进行序列化和反序列化](system-text-json-how-to.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [忽略属性](system-text-json-ignore-properties.md)
* [允许无效的 JSON](system-text-json-invalid-json.md)
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
