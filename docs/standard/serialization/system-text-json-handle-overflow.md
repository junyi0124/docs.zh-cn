---
title: 如何使用 System.Text.Json 处理溢出 JSON
description: 了解在 .NET 中序列化为 JSON 和从 JSON 进行反序列化时如何处理溢出 JSON。
ms.date: 11/30/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 265ce4f77d353720419122d17c36e508a377b68f
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008911"
---
# <a name="how-to-handle-overflow-json-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 处理溢出 JSON

本文将介绍如何使用 `System.Text.Json` 命名空间处理溢出 JSON。

## <a name="handle-overflow-json"></a>处理溢出 JSON

反序列化时，可能会在 JSON 中收到不是由目标类型的属性表示的数据。 例如，假设目标类型如下：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

要反序列化的 JSON 如下：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "temperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

如果将显示的 JSON 反序列化为显示的类型，则 `DatesAvailable` 和 `SummaryWords` 属性无处可去，会丢失。 若要捕获额外数据（如这些属性），请将 [[JsonExtensionData]](xref:System.Text.Json.Serialization.JsonExtensionDataAttribute) 特性应用于类型 `Dictionary<string,object>` 或 `Dictionary<string,JsonElement>` 的属性：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithExtensionData":::

将前面显示的 JSON 反序列化为此示例类型时，额外数据会成为 `ExtensionData` 属性的键值对：

| Property | 值 | 说明 |
|--|--|--|
| `Date` | `"8/1/2019 12:00:00 AM -07:00"` |  |
| `TemperatureCelsius` | `0` | 区分大小写的不匹配（JSON 中的 `temperatureCelsius`），因此未设置属性。 |
| `Summary` | `"Hot"` |  |
| `ExtensionData` | `temperatureCelsius: 25` | 因为大小写不匹配，所以此 JSON 属性是额外属性，会成为字典中的键值对。 |
| `DatesAvailable` | `[ "8/1/2019 12:00:00 AM -07:00", "8/2/2019 12:00:00 AM -07:00" ]` | JSON 中的额外属性会成为键值对，将数组作为值对象。 |
| `SummaryWords` | `[ "Cool", "Windy", "Humid" ]` | JSON 中的额外属性会成为键值对，将数组作为值对象。 |

序列化目标对象时，扩展数据键值对会成为 JSON 属性，就如同它们处于传入 JSON 中一样：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 0,
  "Summary": "Hot",
  "temperatureCelsius": 25,
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

请注意，`ExtensionData` 属性名称不会出现在 JSON 中。 此行为使 JSON 可以进行往返，而不会丢失任何不会以其他方式进行反序列化的额外数据。

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何对 JSON 进行序列化和反序列化](system-text-json-how-to.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [自定义属性名称和值](system-text-json-customize-properties.md)
* [忽略属性](system-text-json-ignore-properties.md)
* [允许无效的 JSON](system-text-json-invalid-json.md)
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
