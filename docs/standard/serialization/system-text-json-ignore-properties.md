---
title: 如何使用 System.Text.Json 忽略属性
description: 了解使用 .NET 中的 System.Text.Json 进行序列化时如何忽略属性。
ms.date: 11/30/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
zone_pivot_groups: dotnet-version
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 6d703156d50a3e00a33cea5e15be2df911ed7c1b
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008807"
---
# <a name="how-to-ignore-properties-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 忽略属性

将 C# 对象序列化为 JavaScript 对象表示法 (JSON) 时，默认情况下，所有公共属性都会序列化。 如果不想让某些属性出现在生成的 JSON 中，则有几个选项可用。 本文将介绍如何根据各种条件忽略属性：

::: zone pivot="dotnet-5-0"

* [单个属性](#ignore-individual-properties)
* [所有只读属性](#ignore-all-read-only-properties)
* [所有 null 值属性](#ignore-all-null-value-properties)
* [所有默认值属性](#ignore-all-default-value-properties)
::: zone-end

::: zone pivot="dotnet-core-3-1"

* [单个属性](#ignore-individual-properties)
* [所有只读属性](#ignore-all-read-only-properties)
* [所有 null 值属性](#ignore-all-null-value-properties)
::: zone-end

## <a name="ignore-individual-properties"></a>忽略单个属性

若要忽略单个属性，请使用 [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute) 特性。

下面是要进行序列化的示例类型和 JSON 输出：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithIgnoreAttribute":::

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
}
```

::: zone pivot="dotnet-5-0"
可以通过设置 [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute) 特性的 `Condition` 属性来指定条件排除。 <xref:System.Text.Json.Serialization.JsonIgnoreCondition> 枚举提供下列选项：

* `Always` - 始终忽略属性。 如果未指定 `Condition`，则假设此选项。
* `Never` - 无论 `DefaultIgnoreCondition`、`IgnoreReadOnlyProperties` 和 `IgnoreReadOnlyFields` 全局设置如何，始终序列化和反序列化属性。
* `WhenWritingDefault` - 如果属性是引用类型 `null`可为 null 的值类型 `null` 或值类型 `default`，则在序列化中忽略属性。
* `WhenWritingNull` - 如果属性是引用类型 `null` 或可为 null 的值类型 `null`，则在序列化中忽略属性。

下面的示例说明 [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute) 特性的 `Condition` 属性的用法：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/JsonIgnoreAttributeExample.cs" highlight="10,13,16":::
::: zone-end

## <a name="ignore-all-read-only-properties"></a>忽略所有只读属性

如果属性包含公共 getter 而不是公共 setter，则属性为只读。 若要在序列化时忽略所有只读属性，请将 <xref:System.Text.Json.JsonSerializerOptions.IgnoreReadOnlyProperties?displayProperty=nameWithType> 设置为 `true`，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializeExcludeReadOnlyProperties.cs" id="Serialize":::

下面是要进行序列化的示例类型和 JSON 输出：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithROProperty":::

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
}
```

此选项仅适用于序列化。 在反序列化过程中，默认情况下会忽略只读属性。

::: zone pivot="dotnet-5-0"
此选项仅适用于属性。 若要在[序列化字段](system-text-json-how-to.md#include-fields)时忽略只读字段，请使用 <xref:System.Text.Json.JsonSerializerOptions.IgnoreReadOnlyFields%2A?displayProperty=nameWithType> 全局设置。
::: zone-end

## <a name="ignore-all-null-value-properties"></a>忽略所有 null 值属性

::: zone pivot="dotnet-5-0"
若要忽略所有 null 值属性，请将 <xref:System.Text.Json.JsonSerializerOptions.DefaultIgnoreCondition> 属性设置为 <xref:System.Text.Json.Serialization.JsonIgnoreCondition.WhenWritingNull>，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/IgnoreNullOnSerialize.cs" highlight="28":::

::: zone-end

::: zone pivot="dotnet-core-3-1"
若要在序列化时忽略所有 null 值属性，请将 <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues> 属性设置为 `true`，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializeExcludeNullValueProperties.cs" id="Serialize":::

下面是要进行序列化的示例对象和 JSON 输出：

| Property             | 值                         |
|----------------------|-------------------------------|
| `Date`               | `8/1/2019 12:00:00 AM -07:00` |
| `TemperatureCelsius` | `25`                          |
| `Summary`            | `null`                        |

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25
}
```

::: zone-end

## <a name="ignore-all-default-value-properties"></a>忽略所有默认值属性

::: zone pivot="dotnet-5-0"
若要防止对值类型属性中的默认值进行序列化，请将 <xref:System.Text.Json.JsonSerializerOptions.DefaultIgnoreCondition> 属性设置为 <xref:System.Text.Json.Serialization.JsonIgnoreCondition.WhenWritingDefault>，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/IgnoreValueDefaultOnSerialize.cs" highlight="28":::
::: zone-end

<xref:System.Text.Json.Serialization.JsonIgnoreCondition.WhenWritingDefault> 设置还会阻止对 null 值引用类型和可为 null 的值类型属性进行序列化。

::: zone pivot="dotnet-core-3-1"
在 .NET Core 3.1 的 `System.Text.Json` 中，无内置方式来阻止对值类型为默认值的属性进行序列化。
::: zone-end

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何对 JSON 进行序列化和反序列化](system-text-json-how-to.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [自定义属性名称和值](system-text-json-customize-properties.md)
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
