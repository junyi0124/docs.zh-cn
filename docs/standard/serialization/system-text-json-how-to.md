---
title: 如何使用 C# 对 JSON 进行序列化和反序列化 - .NET
description: 了解如何使用 System.Text.Json 命名空间在 .NET 中向/从 JSON 进行序列化和反序列化。 包含示例代码。
ms.date: 12/02/2020
ms.custom: contperfq2
no-loc:
- System.Text.Json
- Newtonsoft.Json
zone_pivot_groups: dotnet-version
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 1ea4ff71b9e21bd7c5b12598581b33e1e96ebb19
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008833"
---
# <a name="how-to-serialize-and-deserialize-marshal-and-unmarshal-json-in-net"></a>如何在 .NET 中对 JSON 进行序列化和反序列化（封送和拆收）

本文演示如何使用 <xref:System.Text.Json?displayProperty=fullName> 命名空间向/从 JavaScript 对象表示法 (JSON) 进行序列化和反序列化。 如果要从 `Newtonsoft.Json` 移植现有代码，请参阅[如何迁移到 `System.Text.Json`](system-text-json-migrate-from-newtonsoft-how-to.md)。

方向和示例代码直接使用库，而不是通过框架（如 [ASP.NET Core](/aspnet/core/)）。

大多数序列化示例代码将 <xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> 设置为 `true`，以 JSON 进行优质打印（包含缩进和空格，以提高可读性）。 对于生产用途，通常对于此设置会接受默认值 `false`。

代码示例引用下面的类及其变体：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

## <a name="namespaces"></a>命名空间

<xref:System.Text.Json> 命名空间包含所有入口点和主要类型。 <xref:System.Text.Json.Serialization> 命名空间包含用于高级方案的特性和 API，以及特定于序列化和反序列化的自定义。 本文中演示的代码示例要求将 `using` 指令用于其中一个或两个命名空间：

```csharp
using System.Text.Json;
using System.Text.Json.Serialization;
```

> [!IMPORTANT]
> `System.Text.Json` 中不支持 <xref:System.Runtime.Serialization> 命名空间中的特性。

## <a name="how-to-write-net-objects-as-json-serialize"></a>如何将 .NET 对象编写为 JSON（序列化）

若要将 JSON 编写为字符串或文件，请调用 <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> 方法。

下面的示例将 JSON 创建为字符串：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToString.cs" id="Serialize":::

下面的示例使用同步代码创建 JSON 文件：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToFile.cs" id="Serialize":::

下面的示例使用异步代码创建 JSON 文件：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToFileAsync.cs" id="Serialize":::

前面的示例对要序列化的类型使用类型推理。 `Serialize()` 的重载采用泛型类型参数：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToString.cs" id="SerializeWithGenericParameter":::

### <a name="serialization-example"></a>序列化示例

下面是一个包含集合类型属性和用户定义类型的示例类：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithPOCOs":::

> [!TIP]
> “POCO”代表[普通旧 CLR 对象](https://en.wikipedia.org/wiki/Plain_old_CLR_object)。 POCO 是一种 .NET 类型，它不通过继承或特性等依赖于任何框架特定类型。

序列化以上类型的实例的 JSON 输出类似于下面的示例。 默认情况下，JSON 输出会缩小（将删除空格、缩进和换行符）：

```json
{"Date":"2019-08-01T00:00:00-07:00","TemperatureCelsius":25,"Summary":"Hot","DatesAvailable":["2019-08-01T00:00:00-07:00","2019-08-02T00:00:00-07:00"],"TemperatureRanges":{"Cold":{"High":20,"Low":-10},"Hot":{"High":60,"Low":20}},"SummaryWords":["Cool","Windy","Humid"]}
```

下面的示例演示相同的 JSON，但进行了格式设置（即，具有空格和缩进的优质打印版本）：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "DatesAvailable": [
    "2019-08-01T00:00:00-07:00",
    "2019-08-02T00:00:00-07:00"
  ],
  "TemperatureRanges": {
    "Cold": {
      "High": 20,
      "Low": -10
    },
    "Hot": {
      "High": 60,
      "Low": 20
    }
  },
  "SummaryWords": [
    "Cool",
    "Windy",
    "Humid"
  ]
}
```

## <a name="serialize-to-utf-8"></a>序列化为 UTF-8

若要序列化为 UTF-8，请调用 <xref:System.Text.Json.JsonSerializer.SerializeToUtf8Bytes%2A?displayProperty=nameWithType> 方法：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs" id="Serialize":::

还有一个采用 <xref:System.Text.Json.Utf8JsonWriter> 的 <xref:System.Text.Json.JsonSerializer.Serialize%2A> 重载可用。

序列化为 UTF-8 比使用基于字符串的方法大约快 5-10%。 出现这种差别的原因是字节（作为 UTF-8）不需要转换为字符串 (UTF-16)。

## <a name="serialization-behavior"></a>序列化行为

::: zone pivot="dotnet-5-0"

* 默认情况下，所有公共属性都会序列化。 可以[指定要忽略的属性](system-text-json-ignore-properties.md)。
* [默认编码器](xref:System.Text.Encodings.Web.JavaScriptEncoder.Default)会转义非 ASCII 字符、ASCII 范围内的 HTML 敏感字符以及根据 [RFC 8259 JSON 规范](https://tools.ietf.org/html/rfc8259#section-7)必须进行转义的字符。
* 默认情况下，JSON 会缩小。 可以[对 JSON 进行优质打印](#serialize-to-formatted-json)。
* 默认情况下，JSON 名称的大小写与 .NET 名称匹配。 可以[自定义 JSON 名称大小写](system-text-json-customize-properties.md)。
* 默认情况下，检测到循环引用并引发异常。 可以[保留引用并处理循环引用](system-text-json-preserve-references.md)。
* 默认情况下忽略[字段](../../csharp/programming-guide/classes-and-structs/fields.md)。 可以[包含字段](#include-fields)。

当你在 ASP.NET Core 应用中间接使用 System.Text.Json 时，某些默认行为会有所不同。 有关详细信息，请参阅 [JsonSerializerOptions 的 Web 默认值](system-text-json-configure-options.md#web-defaults-for-jsonserializeroptions)。
::: zone-end

::: zone pivot="dotnet-core-3-1"

* 默认情况下，所有公共属性都会序列化。 可以[指定要忽略的属性](system-text-json-ignore-properties.md)。
* [默认编码器](xref:System.Text.Encodings.Web.JavaScriptEncoder.Default)会转义非 ASCII 字符、ASCII 范围内的 HTML 敏感字符以及根据 [RFC 8259 JSON 规范](https://tools.ietf.org/html/rfc8259#section-7)必须进行转义的字符。
* 默认情况下，JSON 会缩小。 可以[对 JSON 进行优质打印](#serialize-to-formatted-json)。
* 默认情况下，JSON 名称的大小写与 .NET 名称匹配。 可以[自定义 JSON 名称大小写](system-text-json-customize-properties.md)。
* 检测到循环引用并引发异常。
* 将忽略[字段](../../csharp/programming-guide/classes-and-structs/fields.md)。
::: zone-end

支持的类型包括：
::: zone pivot="dotnet-5-0"

* 映射到 JavaScript 基元的 .NET 基元，如数值类型、字符串和布尔。
* 用户定义的[普通旧 CLR 对象 (POCO)](https://en.wikipedia.org/wiki/Plain_old_CLR_object)。
* 一维和交错数组 (`T[][]`)。
* 以下命名空间中的集合和字典。
  * <xref:System.Collections>
  * <xref:System.Collections.Generic>
  * <xref:System.Collections.Immutable>
  * <xref:System.Collections.Concurrent>
  * <xref:System.Collections.Specialized>
  * <xref:System.Collections.ObjectModel>
::: zone-end

::: zone pivot="dotnet-core-3-1"

* 映射到 JavaScript 基元的 .NET 基元，如数值类型、字符串和布尔。
* 用户定义的[普通旧 CLR 对象 (POCO)](https://en.wikipedia.org/wiki/Plain_old_CLR_object)。
* 一维和交错数组 (`ArrayName[][]`)。
* `Dictionary<string,TValue>`其中 `TValue` 是 `object`、`JsonElement` 或 POCO。
* 以下命名空间中的集合。
  * <xref:System.Collections>
  * <xref:System.Collections.Generic>
  * <xref:System.Collections.Immutable>
  * <xref:System.Collections.Concurrent>
  * <xref:System.Collections.Specialized>
  * <xref:System.Collections.ObjectModel>
::: zone-end

可以[实现自定义转换器](system-text-json-converters-how-to.md)以处理其他类型或提供内置转换器不支持的功能。

## <a name="how-to-read-json-as-net-objects-deserialize"></a>如何将 JSON 读取为 .NET 对象（反序列化）

若要从字符串或文件进行反序列化，请调用 <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> 方法。

下面的示例从字符串读取 JSON，并创建前面为[序列化示例](#serialization-example)显示的 `WeatherForecastWithPOCOs` 类的实例：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToString.cs" id="Deserialize":::

若要使用同步代码从文件进行反序列化，请将文件读入字符串中，如下面的示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToFile.cs" id="Deserialize":::

若要使用异步代码从文件进行反序列化，请调用 <xref:System.Text.Json.JsonSerializer.DeserializeAsync%2A> 方法：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToFileAsync.cs" id="Deserialize":::

## <a name="deserialize-from-utf-8"></a>从 UTF-8 进行反序列化

若要从 UTF-8 进行反序列化，请调用采用 `ReadOnlySpan<byte>` 或 `Utf8JsonReader` 的 <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType> 重载，如下面的示例中所示。 这些示例假设 JSON 处于名为 jsonUtf8Bytes 的字节数组中。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs" id="Deserialize1":::

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToUtf8.cs" id="Deserialize2":::

## <a name="deserialization-behavior"></a>反序列化行为

对 JSON 进行反序列化时，以下行为适用：

::: zone pivot="dotnet-5-0"

* 默认情况下，属性名称匹配区分大小写。 可以[指定不区分大小写](system-text-json-character-casing.md)。
* 如果 JSON 包含只读属性的值，则会忽略该值，并且不引发异常。
* 序列化程序会忽略非公共构造函数。
* 支持反序列化为不可变对象或只读属性。 请参阅[不可变类型和记录](system-text-json-immutability.md)。
* 默认情况下，支持将枚举作为数字。 可以[将枚举名称序列化为字符串](system-text-json-customize-properties.md#enums-as-strings)。
* 默认情况下忽略字段。 可以[包含字段](#include-fields)。
* 默认情况下，JSON 中的注释或尾随逗号会引发异常。 可以[允许注释和尾随逗号](system-text-json-invalid-json.md)。
* [默认最大深度](xref:System.Text.Json.JsonReaderOptions.MaxDepth)为 64。

当你在 ASP.NET Core 应用中间接使用 System.Text.Json 时，某些默认行为会有所不同。 有关详细信息，请参阅 [JsonSerializerOptions 的 Web 默认值](system-text-json-configure-options.md#web-defaults-for-jsonserializeroptions)。
::: zone-end

::: zone pivot="dotnet-core-3-1"

* 默认情况下，属性名称匹配区分大小写。 可以[指定不区分大小写](system-text-json-character-casing.md)。 ASP.NET Core 应用[默认指定不区分大小写](system-text-json-configure-options.md#web-defaults-for-jsonserializeroptions)。
* 如果 JSON 包含只读属性的值，则会忽略该值，并且不引发异常。
* 无参数构造函数（可以是公共的、内部的或专用的）用于反序列化。
* 不支持反序列化为不可变对象或只读属性。
* 默认情况下，支持将枚举作为数字。 可以[将枚举名称序列化为字符串](system-text-json-customize-properties.md#enums-as-strings)。
* 不支持字段。
* 默认情况下，JSON 中的注释或尾随逗号会引发异常。 可以[允许注释和尾随逗号](system-text-json-invalid-json.md)。
* [默认最大深度](xref:System.Text.Json.JsonReaderOptions.MaxDepth)为 64。

当你在 ASP.NET Core 应用中间接使用 System.Text.Json 时，某些默认行为会有所不同。 有关详细信息，请参阅 [JsonSerializerOptions 的 Web 默认值](system-text-json-configure-options.md#web-defaults-for-jsonserializeroptions)。
::: zone-end

可以[实现自定义转换器](system-text-json-converters-how-to.md)以提供内置转换器不支持的功能。

## <a name="serialize-to-formatted-json"></a>序列化为格式化 JSON

若要对 JSON 输出进行优质打印，请将 <xref:System.Text.Json.JsonSerializerOptions.WriteIndented?displayProperty=nameWithType> 设置为 `true`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripToString.cs" id="SerializePrettyPrint":::

下面是一个要进行序列化的示例类型，以及进行了优质打印的 JSON 输出：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

如果你通过相同的选项重复使用 `JsonSerializerOptions`，则请勿在每次使用时都创建新的 `JsonSerializerOptions` 实例。 对每个调用重复使用同一实例。 有关详细信息，请参阅[重用 JsonSerializerOptions 实例](system-text-json-configure-options.md#reuse-jsonserializeroptions-instances)。

## <a name="include-fields"></a>包含字段

::: zone pivot="dotnet-5-0"
在序列化或反序列化时，使用 <xref:System.Text.Json.JsonSerializerOptions.IncludeFields?displayProperty=nameWithType> 全局设置或 [[JsonInclude]](xref:System.Text.Json.Serialization.JsonIncludeAttribute) 特性来包含字段，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/Fields.cs" highlight="16,18,20,32-35":::

若要忽略只读字段，请使用 <xref:System.Text.Json.JsonSerializerOptions.IgnoreReadOnlyFields%2A?displayProperty=nameWithType> 全局设置。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 的 System.Text.Json 中不支持字段。 [自定义转换器](system-text-json-converters-how-to.md)可提供此功能。
::: zone-end

## <a name="httpclient-and-httpcontent-extension-methods"></a>HttpClient 和 HttpContent 扩展方法

::: zone pivot="dotnet-5-0"

序列化和反序列化来自网络的 JSON 有效负载是常见的操作。 [HttpClient](xref:System.Net.Http.Json.HttpClientJsonExtensions) 和 [HttpContent](xref:System.Net.Http.Json.HttpContentJsonExtensions) 上的扩展方法允许在单个代码行中执行这些操作。 这些扩展方法使用 [JsonSerializerOptions 的 Web 默认值](system-text-json-configure-options.md#web-defaults-for-jsonserializeroptions)。

以下示例说明了 <xref:System.Net.Http.Json.HttpClientJsonExtensions.GetFromJsonAsync%2A?displayProperty=nameWithType> 和 <xref:System.Net.Http.Json.HttpClientJsonExtensions.PostAsJsonAsync%2A?displayProperty=nameWithType> 的用法：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/HttpClientExtensionMethods.cs" highlight="26,33":::

此外还有 [HttpContent](xref:System.Net.Http.Json.HttpContentJsonExtensions) 上的 System.Text.Json 的扩展方法。
::: zone-end

::: zone pivot="dotnet-core-3-1"
`HttpClient` 和 `HttpContent` 上的扩展方法在 .NET Core 3.1 的 System.Text.Json 中不可用。
::: zone-end

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [自定义属性名称和值](system-text-json-customize-properties.md)
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
