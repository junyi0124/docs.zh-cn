---
title: 从 Newtonsoft.Json 迁移到 System.Text.Json - .NET
description: 了解如何从 Newtonsoft.Json 迁移到 System.Text.Json。 包含示例代码。
author: tdykstra
ms.author: tdykstra
no-loc:
- System.Text.Json
- Newtonsoft.Json
ms.date: 11/30/2020
zone_pivot_groups: dotnet-version
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 418637639790199755803bf374ef99af949ae9b3
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009893"
---
# <a name="how-to-migrate-from-no-locnewtonsoftjson-to-no-locsystemtextjson"></a>如何从 Newtonsoft.Json 迁移到 System.Text.Json

本文演示如何从 [Newtonsoft.Json](https://www.newtonsoft.com/json) 迁移到 <xref:System.Text.Json>。

`System.Text.Json` 命名空间提供用于序列化和反序列化 JavaScript 对象表示法 (JSON) 的功能。 `System.Text.Json` 库包含在 [.NET Core 3.1](https://dotnet.microsoft.com/download/dotnet-core/3.1) 和更高版本的运行时中。 对于其他目标框架，请安装 [System.Text.Json](https://www.nuget.org/packages/System.Text.Json) NuGet 包。 包支持以下框架：

* .NET Standard 2.0 及更高版本
* .NET Framework 4.7.2 及更高版本
* .NET Core 2.0、2.1 和 2.2

`System.Text.Json` 主要关注性能、安全性和标准符合性。 它在默认行为方面有一些重要差异，不打算具有与 `Newtonsoft.Json` 相同的功能。 对于某些方案，`System.Text.Json` 没有内置功能，但有建议解决方法。 对于其他方案，解决方法是不切实际的。 如果你的应用程序依赖于缺少的功能，请考虑[提交问题](https://github.com/dotnet/runtime/issues/new)以了解是否可以添加对你的方案的支持。

本文的大部分内容介绍如何使用 <xref:System.Text.Json.JsonSerializer> API，不过也包含有关如何使用 <xref:System.Text.Json.JsonDocument>（表示文档对象模型或 DOM）、<xref:System.Text.Json.Utf8JsonReader> 和 <xref:System.Text.Json.Utf8JsonWriter> 类型的指导。

## <a name="table-of-differences-between-no-locnewtonsoftjson-and-no-locsystemtextjson"></a>介绍 Newtonsoft.Json 与 System.Text.Json 之间差异的表格

下表列出 `Newtonsoft.Json` 功能和 `System.Text.Json` 等效功能。 这些等效功能分为以下类别：

* 受支持功能支持。 从 `System.Text.Json` 获取类似行为可能需要使用特性或全局选项。
* 不受支持，可能有解决方法。 解决方法是[自定义转换器](system-text-json-converters-how-to.md)，它们可能无法提供与 `Newtonsoft.Json` 功能完全相同的功能。 对于其中一些功能，提供示例代码作为示例。 如果你依赖于这些 `Newtonsoft.Json` 功能，迁移需要修改 .NET 对象模型或进行其他代码更改。
* 不受支持，解决方法不可行或无法提供。 如果你依赖于这些 `Newtonsoft.Json` 功能，则无法在不进行重大更改的情况下进行迁移。

::: zone pivot="dotnet-5-0"
| Newtonsoft.Json 功能                               | System.Text.Json 等效 |
|-------------------------------------------------------|-----------------------------|
| 默认情况下不区分大小写的反序列化           | ✔️ [PropertyNameCaseInsensitive 全局设置](#case-insensitive-deserialization) |
| Camel 大小写属性名称                             | ✔️ [PropertyNamingPolicy 全局设置](system-text-json-customize-properties.md#use-camel-case-for-all-json-property-names) |
| 最小字符转义                            | ✔️ [严格字符转义，可配置](#minimal-character-escaping) |
| `NullValueHandling.Ignore` 全局设置             | ✔️ [DefaultIgnoreCondition 全局选项](system-text-json-ignore-properties.md#ignore-all-null-value-properties) |[有条件地忽略属性](#conditionally-ignore-a-property)
| 允许注释                                        | ✔️ [ReadCommentHandling 全局设置](#comments) |
| 允许尾随逗号                                 | ✔️ [AllowTrailingCommas 全局设置](#trailing-commas) |
| 自定义转换器注册                         | ✔️ [优先级顺序不同](#converter-registration-precedence) |
| 默认情况下无最大深度                           | ✔️ [默认最大深度为 64，可配置](#maximum-depth) |
| `PreserveReferencesHandling` 全局设置           | ✔️ [ReferenceHandling 全局设置](#preserve-object-references-and-handle-loops) |
| 序列化或反序列化带引号的数字            | ✔️ [NumberHandling 全局设置，[JsonNumberHandling] 特性](#allow-or-write-numbers-in-quotes) |
| 反序列化为不可变类和结构          | ✔️ [JsonConstructor，C# 9 记录](#deserialize-to-immutable-classes-and-structs) |
| 支持字段                                    | ✔️ [IncludeFields 全局设置，[JsonInclude] 特性](#public-and-non-public-fields) |
| `DefaultValueHandling` 全局设置                 | ✔️ [DefaultIgnoreCondition 全局设置](#conditionally-ignore-a-property) |
| `[JsonProperty]` 上的 `NullValueHandling` 设置       | ✔️ [JsonIgnore 特性](#conditionally-ignore-a-property)  |
| `[JsonProperty]` 上的 `DefaultValueHandling` 设置    | ✔️ [JsonIgnore 特性](#conditionally-ignore-a-property)  |
| 反序列化具有非字符串键的 `Dictionary`          | ✔️ [受支持](#dictionary-with-non-string-key) |
| 支持非公共属性资源库和 Getter   | ✔️ [JsonInclude 特性](#non-public-property-setters-and-getters) |
| `[JsonConstructor]` 特性                         | ✔️ [[JsonConstructor] 特性](#specify-constructor-to-use-when-deserializing) |
| 支持范围广泛的类型                    | ⚠️ [某些类型需要自定义转换器](#types-without-built-in-support) |
| 多态序列化                             | ⚠️ [不受支持，解决方法，示例](#polymorphic-serialization) |
| 多态反序列化                           | ⚠️ [不受支持，解决方法，示例](#polymorphic-deserialization) |
| 将推断类型反序列化为 `object` 属性      | ⚠️ [不受支持，解决方法，示例](#deserialization-of-object-properties) |
| 将 JSON `null` 文本反序列化为不可为 null 的值类型 | ⚠️ [不受支持，解决方法，示例](#deserialize-null-to-non-nullable-type) |
| `[JsonProperty]` 特性上的 `Required` 设置        | ⚠️ [不受支持，解决方法，示例](#required-properties) |
| `DefaultContractResolver` 用于忽略属性       | ⚠️ [不受支持，解决方法，示例](#conditionally-ignore-a-property) |
| `DateTimeZoneHandling`、`DateFormatString` 设置   | ⚠️ [不受支持，解决方法，示例](#specify-date-format) |
| 回调                                             | ⚠️ [不受支持，解决方法，示例](#callbacks) |
| `JsonConvert.PopulateObject` 方法                   | ⚠️ [不受支持，解决方法](#populate-existing-objects) |
| `ObjectCreationHandling` 全局设置               | ⚠️ [不受支持，解决方法](#reuse-rather-than-replace-properties) |
| 在不带 setter 的情况下添加到集合                    | ⚠️ [不受支持，解决方法](#add-to-collections-without-setters) |
| `ReferenceLoopHandling` 全局设置                | ❌ [不受支持](#preserve-object-references-and-handle-loops) |
| 支持 `System.Runtime.Serialization` 特性 | ❌ [不受支持](#systemruntimeserialization-attributes) |
| `MissingMemberHandling` 全局设置                | ❌ [不受支持](#missingmemberhandling) |
| 允许不带引号的属性名称                   | ❌ [不受支持](#json-strings-property-names-and-string-values) |
| 字符串值前后允许单引号              | ❌ [不受支持](#json-strings-property-names-and-string-values) |
| 对字符串属性允许非字符串 JSON 值    | ❌ [不受支持](#non-string-values-for-string-properties) |
::: zone-end

::: zone pivot="dotnet-core-3-1"
| Newtonsoft.Json 功能                               | System.Text.Json 等效 |
|-------------------------------------------------------|-----------------------------|
| 默认情况下不区分大小写的反序列化           | ✔️ [PropertyNameCaseInsensitive 全局设置](#case-insensitive-deserialization) |
| Camel 大小写属性名称                             | ✔️ [PropertyNamingPolicy 全局设置](system-text-json-customize-properties.md#use-camel-case-for-all-json-property-names) |
| 最小字符转义                            | ✔️ [严格字符转义，可配置](#minimal-character-escaping) |
| `NullValueHandling.Ignore` 全局设置             | ✔️ [IgnoreNullValues 全局选项](system-text-json-ignore-properties.md#ignore-all-null-value-properties) |
| 允许注释                                        | ✔️ [ReadCommentHandling 全局设置](#comments) |
| 允许尾随逗号                                 | ✔️ [AllowTrailingCommas 全局设置](#trailing-commas) |
| 自定义转换器注册                         | ✔️ [优先级顺序不同](#converter-registration-precedence) |
| 默认情况下无最大深度                           | ✔️ [默认最大深度为 64，可配置](#maximum-depth) |
| 支持范围广泛的类型                    | ⚠️ [某些类型需要自定义转换器](#types-without-built-in-support) |
| 将字符串反序列化为数字                        | ⚠️ [不受支持，解决方法，示例](#allow-or-write-numbers-in-quotes) |
| 反序列化具有非字符串键的 `Dictionary`          | ⚠️ [不受支持，解决方法，示例](#dictionary-with-non-string-key) |
| 多态序列化                             | ⚠️ [不受支持，解决方法，示例](#polymorphic-serialization) |
| 多态反序列化                           | ⚠️ [不受支持，解决方法，示例](#polymorphic-deserialization) |
| 将推断类型反序列化为 `object` 属性      | ⚠️ [不受支持，解决方法，示例](#deserialization-of-object-properties) |
| 将 JSON `null` 文本反序列化为不可为 null 的值类型 | ⚠️ [不受支持，解决方法，示例](#deserialize-null-to-non-nullable-type) |
| 反序列化为不可变类和结构          | ⚠️ [不受支持，解决方法，示例](#deserialize-to-immutable-classes-and-structs) |
| `[JsonConstructor]` 特性                         | ⚠️ [不受支持，解决方法，示例](#specify-constructor-to-use-when-deserializing) |
| `[JsonProperty]` 特性上的 `Required` 设置        | ⚠️ [不受支持，解决方法，示例](#required-properties) |
| `[JsonProperty]` 特性上的 `NullValueHandling` 设置 | ⚠️ [不受支持，解决方法，示例](#conditionally-ignore-a-property)  |
| `[JsonProperty]` 特性上的 `DefaultValueHandling` 设置 | ⚠️ [不受支持，解决方法，示例](#conditionally-ignore-a-property)  |
| `DefaultValueHandling` 全局设置                 | ⚠️ [不受支持，解决方法，示例](#conditionally-ignore-a-property) |
| `DefaultContractResolver` 用于忽略属性       | ⚠️ [不受支持，解决方法，示例](#conditionally-ignore-a-property) |
| `DateTimeZoneHandling`、`DateFormatString` 设置   | ⚠️ [不受支持，解决方法，示例](#specify-date-format) |
| 回调                                             | ⚠️ [不受支持，解决方法，示例](#callbacks) |
| 支持公共和非公共字段              | ⚠️ [不受支持，解决方法](#public-and-non-public-fields) |
| 支持非公共属性资源库和 Getter   | ⚠️ [不受支持，解决方法](#non-public-property-setters-and-getters) |
| `JsonConvert.PopulateObject` 方法                   | ⚠️ [不受支持，解决方法](#populate-existing-objects) |
| `ObjectCreationHandling` 全局设置               | ⚠️ [不受支持，解决方法](#reuse-rather-than-replace-properties) |
| 在不带 setter 的情况下添加到集合                    | ⚠️ [不受支持，解决方法](#add-to-collections-without-setters) |
| `PreserveReferencesHandling` 全局设置           | ❌ [不受支持](#preserve-object-references-and-handle-loops) |
| `ReferenceLoopHandling` 全局设置                | ❌ [不受支持](#preserve-object-references-and-handle-loops) |
| 支持 `System.Runtime.Serialization` 特性 | ❌ [不受支持](#systemruntimeserialization-attributes) |
| `MissingMemberHandling` 全局设置                | ❌ [不受支持](#missingmemberhandling) |
| 允许不带引号的属性名称                   | ❌ [不受支持](#json-strings-property-names-and-string-values) |
| 字符串值前后允许单引号              | ❌ [不受支持](#json-strings-property-names-and-string-values) |
| 对字符串属性允许非字符串 JSON 值    | ❌ [不受支持](#non-string-values-for-string-properties) |
::: zone-end

这不是 `Newtonsoft.Json` 功能的详尽列表。 此列表包含在 [GitHub 问题](https://github.com/dotnet/runtime/issues?q=is%3Aopen+is%3Aissue+label%3Aarea-System.Text.Json)或 [StackOverflow](https://stackoverflow.com/questions/tagged/system.text.json) 文章中请求的许多方案。 如果对此处所列且当前没有示例代码的一个方案实现了解决方法，并且如果要共享解决方案，请在本页底部的“反馈”部分选择“此页面”。 这会在本文档的 GitHub 存储库中创建一个问题，并将它也列在此页面上的“反馈”部分中。

## <a name="differences-in-default-jsonserializer-behavior-compared-to-no-locnewtonsoftjson"></a>默认 JsonSerializer 行为相较于 Newtonsoft.Json 的差异

<xref:System.Text.Json> 在默认情况下十分严格，避免代表调用方进行任何猜测或解释，强调确定性行为。 该库是为了实现性能和安全性而特意这样设计的。 `Newtonsoft.Json` 默认情况下十分灵活。 设计中的这种根本差异是默认行为中以下许多特定差异的背后原因。

### <a name="case-insensitive-deserialization"></a>不区分大小写的反序列化

在反序列化过程中，默认情况下 `Newtonsoft.Json` 进行不区分大小写的属性名称匹配。 <xref:System.Text.Json> 默认值区分大小写，这可提供更好的性能，因为它执行精确匹配。 有关如何执行不区分大小写的匹配的信息，请参阅[不区分大小写的属性匹配](system-text-json-character-casing.md)。

如果使用 ASP.NET Core 间接使用 `System.Text.Json`，则无需执行任何操作即可获得类似于 `Newtonsoft.Json` 的行为。 ASP.NET Core 在使用 `System.Text.Json` 时，会为 [camel 大小写属性名称](system-text-json-customize-properties.md#use-camel-case-for-all-json-property-names)和不区分大小写的匹配指定设置。

::: zone pivot="dotnet-5-0"
默认情况下，ASP.NET Core 还允许反序列化[带引号的数字](#allow-or-write-numbers-in-quotes)。
::: zone-end

### <a name="minimal-character-escaping"></a>最小字符转义

在序列化过程中，`Newtonsoft.Json` 对于让字符通过而不进行转义相对宽松。 也就是说，它不会将它们替换为 `\uxxxx`（其中 `xxxx` 是字符的码位）。 对字符进行转义时，它会通过在字符前发出 `\` 来实现此目的（例如，`"` 会变为 `\"`）。 <xref:System.Text.Json> 会在默认情况下转义较多字符，以对跨站点脚本 (XSS) 或信息泄露攻击提供深度防御保护，并使用六字符序列执行此操作。 `System.Text.Json` 会在默认情况下转义所有非 ASCII 字符，因此如果在 `Newtonsoft.Json` 中使用 `StringEscapeHandling.EscapeNonAscii`，则无需执行任何操作。 `System.Text.Json` 在默认情况下还会转义 HTML 敏感字符。 有关如何替代默认 `System.Text.Json` 行为的信息，请参阅[自定义字符编码](system-text-json-character-encoding.md)。

### <a name="comments"></a>注释

在反序列化过程中，`Newtonsoft.Json` 在默认情况下会忽略 JSON 中的注释。 <xref:System.Text.Json> 默认值是对注释引发异常，因为 [RFC 8259](https://tools.ietf.org/html/rfc8259) 规范不包含它们。 有关如何允许注释的信息，请参阅[允许注释和尾随逗号](system-text-json-invalid-json.md)。

### <a name="trailing-commas"></a>尾随逗号

在反序列化过程中，默认情况下 `Newtonsoft.Json` 会忽略尾随逗号。 它还会忽略多个尾随逗号（例如 `[{"Color":"Red"},{"Color":"Green"},,]`）。 <xref:System.Text.Json> 默认值是对尾随逗号引发异常，因为 [RFC 8259](https://tools.ietf.org/html/rfc8259) 规范不允许使用它们。 有关如何使 `System.Text.Json` 接受它们的信息，请参阅[允许注释和尾随逗号](system-text-json-invalid-json.md)。 无法允许多个尾随逗号。

### <a name="converter-registration-precedence"></a>转换器注册优先级

自定义转换器的 `Newtonsoft.Json` 注册优先级如下所示：

* 属性上的特性
* 类型上的特性
* [转换器](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonSerializerSettings_Converters.htm) 集合

此顺序意味着 `Converters` 集合中的自定义转换器会由通过在类型级别应用特性而注册的转换器替代。 这两个注册都会由属性级别的特性替代。

自定义转换器的 <xref:System.Text.Json> 注册优先级是不同的：

* 属性上的特性
* <xref:System.Text.Json.JsonSerializerOptions.Converters> 集合
* 类型上的特性

此处的差别在于 `Converters` 集合中的自定义转换器会替代类型级别的特性。 此优先级顺序的目的是使运行时更改替代设计时选项。 无法更改优先级。

有关自定义转换器注册的详细信息，请参阅[注册自定义转换器](system-text-json-converters-how-to.md#register-a-custom-converter)。

### <a name="maximum-depth"></a>最大深度

`Newtonsoft.Json` 默认情况下没有最大深度限制。 对于 <xref:System.Text.Json>，默认限制为 64，可通过设置 <xref:System.Text.Json.JsonSerializerOptions.MaxDepth?displayProperty=nameWithType> 进行配置。

如果使用 ASP.NET Core 时间接使用 `System.Text.Json`，则默认的最大深度限制为 32。 默认值与模型绑定的默认值相同，并且在 [JsonOptions 类](https://github.com/dotnet/aspnetcore/blob/1f56888ea03f6a113587a6c4ac4d8b2ded326ffa/src/Mvc/Mvc.Core/src/JsonOptions.cs#L17-L20)中设置。

### <a name="json-strings-property-names-and-string-values"></a>JSON 字符串（属性名称和字符串值）

在反序列化过程中，`Newtonsoft.Json` 接受用双引号、单引号括起来或不带引号的属性名称。 它接受用双引号或单引号括起来的字符串值。 例如，`Newtonsoft.Json` 接受以下 JSON：

```json
{
  "name1": "value",
  'name2': "value",
  name3: 'value'
}
```

`System.Text.Json` 仅接受双引号中的属性名称和字符串值，因为 [RFC 8259](https://tools.ietf.org/html/rfc8259) 规范要求使用该格式，这是唯一视为有效 JSON 的格式。

用单引号括起来的值会导致 [JsonException](xref:System.Text.Json.JsonException)，并出现以下消息：

```output
''' is an invalid start of a value.
```

### <a name="non-string-values-for-string-properties"></a>字符串属性的非字符串值

`Newtonsoft.Json` 接受非字符串值（如数字或文本 `true` 和 `false`），以便反序列化为类型字符串的属性。 下面是 `Newtonsoft.Json` 成功反序列化为以下类的 JSON 示例：

```json
{
  "String1": 1,
  "String2": true,
  "String3": false
}
```

```csharp
public class ExampleClass
{
    public string String1 { get; set; }
    public string String2 { get; set; }
    public string String3 { get; set; }
}
```

`System.Text.Json` 不将非字符串值反序列化为字符串属性。 字符串字段接收的非字符串值会导致 [JsonException](xref:System.Text.Json.JsonException)，并出现以下消息：

```output
The JSON value could not be converted to System.String.
```

## <a name="scenarios-using-jsonserializer"></a>使用 JsonSerializer 的方案

下面一部分方案不受内置功能支持，但有解决方法可用。 解决方法是[自定义转换器](system-text-json-converters-how-to.md)，它们可能无法提供与 `Newtonsoft.Json` 功能完全相同的功能。 对于其中一些功能，提供示例代码作为示例。 如果你依赖于这些 `Newtonsoft.Json` 功能，迁移需要修改 .NET 对象模型或进行其他代码更改。

对于下面的一部分方案，解决方法不可行或无法提供。 如果你依赖于这些 `Newtonsoft.Json` 功能，则无法在不进行重大更改的情况下进行迁移。

### <a name="allow-or-write-numbers-in-quotes"></a>允许或写入带引号的数字

::: zone pivot="dotnet-5-0"
`Newtonsoft.Json` 可以序列化或反序列化由 JSON 字符串表示的数字（括在引号中）。 例如，它可以接受 `{"DegreesCelsius":"23"}` 而不是 `{"DegreesCelsius":23}`。 若要在 <xref:System.Text.Json> 中启用该行为，请将 <xref:System.Text.Json.JsonSerializerOptions.NumberHandling%2A?displayProperty=nameWithType> 设置为 <xref:System.Text.Json.Serialization.JsonNumberHandling.WriteAsString> 或 <xref:System.Text.Json.Serialization.JsonNumberHandling.AllowReadingFromString>，或使用 [[JsonNumberHandling]](xref:System.Text.Json.Serialization.JsonNumberHandlingAttribute) 特性。

如果使用 ASP.NET Core 间接使用 `System.Text.Json`，则无需执行任何操作即可获得类似于 `Newtonsoft.Json` 的行为。 ASP.NET Core 在使用 `System.Text.Json` 时指定 [Web 默认值](system-text-json-configure-options.md#web-defaults-for-jsonserializeroptions)，Web 默认值允许带引号的数字。

有关详细信息，请参阅[允许或写入带引号的数字](system-text-json-invalid-json.md)。
::: zone-end

::: zone pivot="dotnet-core-3-1"
`Newtonsoft.Json` 可以序列化或反序列化由 JSON 字符串表示的数字（括在引号中）。 例如，它可以接受 `{"DegreesCelsius":"23"}` 而不是 `{"DegreesCelsius":23}`。 若要在 .NET Core 3.1 的 <xref:System.Text.Json> 中启用该行为，请实现类似于以下示例的自定义转换器。 该转换器处理定义为 `long` 的属性：

* 它将这些属性序列化为 JSON 字符串。
* 它在反序列化期间接受 JSON 数字和括在引号中的数字。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/LongToStringConverter.cs":::

通过对各个 `long` 属性[使用特性](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property)或是通过向 [ 集合](system-text-json-converters-how-to.md#registration-sample---converters-collection)添加转换器<xref:System.Text.Json.JsonSerializerOptions.Converters>来注册此自定义转换器。
::: zone-end

### <a name="specify-constructor-to-use-when-deserializing"></a>指定要在反序列化时使用的构造函数

使用 `Newtonsoft.Json` `[JsonConstructor]` 特性可以指定在反序列化为 POCO 时要调用的构造函数。

::: zone pivot="dotnet-5-0"
`System.Text.Json` 还具有 [[JsonConstructor]](xref:System.Text.Json.Serialization.JsonConstructorAttribute) 特性。 有关详细信息，请参阅[不可变类型和记录](system-text-json-immutability.md)。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 中的 <xref:System.Text.Json> 仅支持无参数构造函数。 作为一种解决方法，可以在自定义转换器中调用所需的任何构造函数。 请参阅[反序列化为不可变类和结构](#deserialize-to-immutable-classes-and-structs)的示例。
::: zone-end

### <a name="conditionally-ignore-a-property"></a>有条件地忽略属性

`Newtonsoft.Json` 有多种方法可在序列化或反序列化时有条件地忽略属性：

* `DefaultContractResolver` 使你可以基于任意条件选择要包含或忽略的属性。
* `JsonSerializerSettings` 上的 `NullValueHandling` 和 `DefaultValueHandling` 设置使你指定应忽略所有 null 值或默认值属性。
* `[JsonProperty]` 特性上的 `NullValueHandling` 和 `DefaultValueHandling` 设置使你可以指定在设置为 null 或默认值时应忽略的单个属性。

::: zone pivot="dotnet-5-0"

<xref:System.Text.Json> 提供以下方法，用于在序列化期间忽略属性或字段：

* 属性上的 [[JsonIgnore]](system-text-json-ignore-properties.md#ignore-individual-properties) 特性会导致在序列化过程中从 JSON 中省略属性。
* [IgnoreReadOnlyProperties](system-text-json-ignore-properties.md#ignore-all-read-only-properties) 全局选项使你可以忽略所有只读属性。
* 如果你[包含字段](system-text-json-how-to.md#include-fields)，则 <xref:System.Text.Json.JsonSerializerOptions.IgnoreReadOnlyFields%2A?displayProperty=nameWithType> 全局选项使你可以忽略所有只读字段。
* 使用 `DefaultIgnoreCondition` 全局选项，你可以[忽略具有默认值的所有值类型属性](system-text-json-ignore-properties.md#ignore-all-default-value-properties)，或[忽略具有 null 值的所有引用类型属性](system-text-json-ignore-properties.md#ignore-all-null-value-properties)。

::: zone-end

::: zone pivot="dotnet-core-3-1"

.NET Core 3.1 中的 <xref:System.Text.Json> 提供以下方法，用于在序列化期间忽略属性：

* 属性上的 [[JsonIgnore]](system-text-json-ignore-properties.md#ignore-individual-properties) 特性会导致在序列化过程中从 JSON 中省略属性。
* [IgnoreNullValues](system-text-json-ignore-properties.md#ignore-all-null-value-properties) 全局选项使你可以忽略所有 null 值属性。
* [IgnoreReadOnlyProperties](system-text-json-ignore-properties.md#ignore-all-read-only-properties) 全局选项使你可以忽略所有只读属性。
::: zone-end

不能通过这些选项：

::: zone pivot="dotnet-5-0"

* 基于运行时计算的任意条件忽略所选属性。

::: zone-end

::: zone pivot="dotnet-core-3-1"

* 忽略具有类型的默认值的所有属性。
* 忽略具有类型的默认值的所选属性。
* 忽略值为 null 的所选属性。
* 基于运行时计算的任意条件忽略所选属性。

::: zone-end

对于该功能，可以编写自定义转换器。 下面是一个示例 POCO 和一个适用于它的自定义转换器，用于说明此方法：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecastRuntimeIgnoreConverter.cs":::

如果值为 null、空字符串或 "N/A"，则转换器会导致从序列化中省略 `Summary` 属性。

通过[对类使用特性](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-type)或是通过向 <xref:System.Text.Json.JsonSerializerOptions.Converters> 集合[添加转换器](system-text-json-converters-how-to.md#registration-sample---converters-collection)来注册此自定义转换器。

此方法在以下情况下需要其他逻辑：

* POCO 包含复杂属性。
* 需要处理特性（如 `[JsonIgnore]`）或选项（如自定义编码器）。

### <a name="public-and-non-public-fields"></a>公共和非公共字段

`Newtonsoft.Json` 可以序列化和反序列化字段以及属性。

::: zone pivot="dotnet-5-0"
在 <xref:System.Text.Json> 中，在序列化或反序列化时，使用 <xref:System.Text.Json.JsonSerializerOptions.IncludeFields?displayProperty=nameWithType> 全局设置或 [[JsonInclude]](xref:System.Text.Json.Serialization.JsonIncludeAttribute) 特性来包含公共字段。 有关示例，请参阅[包含字段](system-text-json-how-to.md#include-fields)。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 中的 <xref:System.Text.Json> 仅适用于公共属性。 自定义转换器可提供此功能。
::: zone-end

### <a name="preserve-object-references-and-handle-loops"></a>保留对象引用并处理循环

默认情况下，`Newtonsoft.Json` 按值进行序列化。 例如，如果对象包含两个属性，而这些属性包含对同一个 `Person` 对象的引用，该 `Person` 对象属性的值会在 JSON 重复。

`Newtonsoft.Json` 在 `JsonSerializerSettings` 上有一个 `PreserveReferencesHandling` 设置，可让你按引用进行序列化：

* 标识符元数据会添加到为第一个 `Person` 对象创建的 JSON。
* 为第二个 `Person` 对象创建的 JSON 包含对该标识符（而不是属性值）的引用。

`Newtonsoft.Json` 还具有一个 `ReferenceLoopHandling` 设置，使你可以忽略循环引用，而不是引发异常。

::: zone pivot="dotnet-5-0"
若要在 <xref:System.Text.Json> 中保留引用并处理循环引用，请将 <xref:System.Text.Json.JsonSerializerOptions.ReferenceHandler%2A?displayProperty=nameWithType> 设置为 <xref:System.Text.Json.Serialization.ReferenceHandler.Preserve%2A>。 `ReferenceHandler.Preserve` 设置等效于 `Newtonsoft.Json` 中的 `PreserveReferencesHandling` = `PreserveReferencesHandling.All`。

与 Newtonsoft.Json [ReferenceResolver](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonSerializer_ReferenceResolver.htm) 一样，<xref:System.Text.Json.Serialization.ReferenceResolver?displayProperty=fullName> 类定义在序列化和反序列化过程中保留引用的行为。 创建派生类以指定自定义行为。 有关示例，请参阅 [GuidReferenceResolver](https://github.com/dotnet/docs/blob/9d5e88edbd7f12be463775ffebbf07ac8415fe18/docs/standard/serialization/snippets/system-text-json-how-to-5-0/csharp/GuidReferenceResolverExample.cs)。

一些相关的 `Newtonsoft.Json` 功能不受支持：

* [JsonPropertyAttribute.IsReference](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonPropertyAttribute_IsReference.htm)
* [JsonPropertyAttribute.ReferenceLoopHandling](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonPropertyAttribute_ReferenceLoopHandling.htm)
* [JsonSerializerSettings.ReferenceLoopHandling](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonSerializerSettings_ReferenceLoopHandling.htm)

有关详细信息，请参阅[保留引用并处理循环引用](system-text-json-preserve-references.md)。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 中的 <xref:System.Text.Json> 仅支持按值进行进行序列化，并对循环引用引发异常。
::: zone-end

### <a name="dictionary-with-non-string-key"></a>包含非字符串键的字典

::: zone pivot="dotnet-5-0"
`Newtonsoft.Json` 和 `System.Text.Json` 都支持 `Dictionary<TKey, TValue>` 类型的集合。
::: zone-end

::: zone pivot="dotnet-core-3-1"
`Newtonsoft.Json` 支持类型 `Dictionary<TKey, TValue>` 的集合。 .NET Core 3.1 的 <xref:System.Text.Json> 中对字典集合的内置支持仅限于 `Dictionary<string, TValue>`。 即，键必须是字符串。

若要在 .NET Core 3.1 中支持将整数或某种其他类型用作键的字典，请创建转换器（类似于[如何编写自定义转换器](system-text-json-converters-how-to.md#support-dictionary-with-non-string-key)中的示例）。
::: zone-end

### <a name="types-without-built-in-support"></a>没有内置支持的类型

<xref:System.Text.Json> 不为以下类型提供内置支持：

* <xref:System.Data.DataTable> 和相关类型
* F# 类型（如[可区分联合](../../fsharp/language-reference/discriminated-unions.md)、[记录类型](../../fsharp/language-reference/records.md)和[匿名记录类型](../../fsharp/language-reference/anonymous-records.md)）。
* <xref:System.Dynamic.ExpandoObject>
* <xref:System.TimeZoneInfo>
* <xref:System.Numerics.BigInteger>
* <xref:System.TimeSpan>
* <xref:System.DBNull>
* <xref:System.Type>
* <xref:System.ValueTuple> 及其关联泛型类型

对于没有内置支持的类型，可以实现自定义转换器。

### <a name="polymorphic-serialization"></a>多态序列化

`Newtonsoft.Json` 会自动执行多态序列化。 有关 <xref:System.Text.Json> 的有限多态序列化功能的信息，请参阅[序列化派生类的属性](system-text-json-polymorphism.md)。

所述的解决方法是定义可能以类型 `object` 的形式包含派生类的属性。 如果无法这样，则另一种选择是为整个继承类型层次结构创建带有 `Write` 方法的转换器（类似于[如何编写自定义转换器](system-text-json-converters-how-to.md#support-polymorphic-deserialization)中的示例）。

### <a name="polymorphic-deserialization"></a>多态反序列化

`Newtonsoft.Json` 具有 `TypeNameHandling` 设置，它在序列化期间将类型名称元数据添加到 JSON。 它在反序列化期间使用元数据执行多态反序列化。 <xref:System.Text.Json> 可以执行有限范围的[多态序列化](system-text-json-polymorphism.md)，但不能执行多态反序列化。

若要支持多态反序列化，请创建转换器（类似于[如何编写自定义转换器](system-text-json-converters-how-to.md#support-polymorphic-deserialization)中的示例）。

### <a name="deserialization-of-object-properties"></a>对象属性的反序列化

当 `Newtonsoft.Json` 反序列化为 <xref:System.Object> 时，它会：

* 推断 JSON 有效负载中的基元值的类型（不是 `null`），并以装箱对象的形式返回存储的 `string`、`long`、`double`、`boolean` 或 `DateTime`。 基元值是单个 JSON 值，如 JSON 数字、字符串、`true`、`false` 或 `null`。
* 为 JSON 有效负载中的复杂值返回 `JObject` 或 `JArray`。 复杂值是括在大括号 (`{}`) 中的 JSON 键值对的集合或括在方括号 (`[]`) 中的值的列表。 括在大括号或方括号中的属性和值可以具有附加属性或值。
* 当有效负载具有 `null` JSON 文本时，返回空引用。

<xref:System.Text.Json> 在每次反序列化为 <xref:System.Object> 时，为基元和复数值存储装箱 `JsonElement`，例如：

* `object` 属性。
* `object` 字典值。
* `object` 数组值。
* 根 `object`。

但是，`System.Text.Json` 处理 `null` 的方式与 `Newtonsoft.Json` 相同，会在有效负载中包含 `null` JSON 文本时返回空引用。

若要为 `object` 实现类型推理，请创建转换器（类似于[如何编写自定义转换器](system-text-json-converters-how-to.md#deserialize-inferred-types-to-object-properties)中的示例）。

### <a name="deserialize-null-to-non-nullable-type"></a>将 null 反序列化为不可为 null 的类型

`Newtonsoft.Json` 在以下方案中不会引发异常：

* `NullValueHandling` 设置为 `Ignore`，并且
* 在反序列化过程中，JSON 对于不可为 null 的值类型包含 null 值。

在相同方案中，<xref:System.Text.Json> 会引发异常。 （`System.Text.Json` 中对应的 null 处理设置为 <xref:System.Text.Json.JsonSerializerOptions.IgnoreNullValues?displayProperty=nameWithType> = `true`。）

如果你拥有目标类型，在最佳解决方法是使相关属性可为 null（例如，将 `int` 更改为 `int?`）。

另一种解决方法是为类型创建转换器，如以下为 `DateTimeOffset` 类型处理 null 值的示例：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/DateTimeOffsetNullHandlingConverter.cs":::

通过[对属性使用特性](system-text-json-converters-how-to.md#registration-sample---jsonconverter-on-a-property)或是通过向 <xref:System.Text.Json.JsonSerializerOptions.Converters> 集合[添加转换器](system-text-json-converters-how-to.md#registration-sample---converters-collection)来注册此自定义转换器。

**注意：** 前面的转换器处理 null 值的方式与 `Newtonsoft.Json` 为指定默认值的 POCO 进行处理的方式不同。 例如，假设以下代码表示目标对象：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithDefault":::

并且假设使用前面的转换器反序列化以下 JSON：

```json
{
  "Date": null,
  "TemperatureCelsius": 25,
  "Summary": null
}
```

反序列化之后，`Date` 属性具有 1/1/0001 (`default(DateTimeOffset)`)，即，在构造函数中设置的值会被覆盖。 给定相同 POCO 和 JSON，`Newtonsoft.Json` 反序列化会将 1/1/2001 保留在 `Date` 属性中。

### <a name="deserialize-to-immutable-classes-and-structs"></a>反序列化为不可变类和结构

`Newtonsoft.Json` 可以反序列化为不可变类和结构，因为它可以使用具有参数的构造函数。

::: zone pivot="dotnet-5-0"
在 <xref:System.Text.Json> 中，使用 [[JsonConstructor]](xref:System.Text.Json.Serialization.JsonConstructorAttribute) 特性来指定参数化构造函数的用法。 C# 9 记录也是不可变的，并且支持作为反序列化目标。 有关详细信息，请参阅[不可变类型和记录](system-text-json-immutability.md)。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 中的 <xref:System.Text.Json> 仅支持公共无参数构造函数。 作为一种解决方法，可以在自定义转换器中调用具有参数的构造函数。

下面是具有多个构造函数参数的不可变结构：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/ImmutablePoint.cs" id="ImmutablePoint":::

下面是序列化和反序列化此结构的转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/ImmutablePointConverter.cs":::

通过向 <xref:System.Text.Json.JsonSerializerOptions.Converters> 集合[添加转换器](system-text-json-converters-how-to.md#registration-sample---converters-collection)来注册此自定义转换器。

有关处理开放式泛型属性的类似转换器的示例，请参阅[用于键/值对的内置转换器](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters/JsonValueConverterKeyValuePair.cs)。
::: zone-end

### <a name="required-properties"></a>必需的属性

在 `Newtonsoft.Json` 中，通过对 `[JsonProperty]` 特性设置 `Required` 来指定属性是必需的。 如果在 JSON 中没有为标记为必需的属性收到值，`Newtonsoft.Json` 会引发异常。

如果没有为目标类型的某个属性收到值，<xref:System.Text.Json> 不会引发异常。 例如，如果具有 `WeatherForecast` 类：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

以下 JSON 可反序列化，不会发生错误：

```json
{
    "TemperatureCelsius": 25,
    "Summary": "Hot"
}
```

若要使反序列化在 JSON 中没有 `Date` 属性时失败，请实现自定义转换器。 如果反序列化完成之后未设置 `Date` 属性，则以下示例转换器代码会引发异常：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecastRequiredPropertyConverter.cs":::

通过向 <xref:System.Text.Json.JsonSerializerOptions.Converters?displayProperty=nameWithType> 集合[添加转换器](system-text-json-converters-how-to.md#registration-sample---converters-collection)来注册此自定义转换器。

这种以递归方式调用转换器的模式要求使用 <xref:System.Text.Json.JsonSerializerOptions> 而不是使用属性注册转换器。 如果使用属性注册转换器，则自定义转换器将以递归方式调入其自身。 结果是一个以堆栈溢出异常结尾的无限循环。

使用选项对象注册转换器时，请通过在以递归方式调用 <xref:System.Text.Json.JsonSerializer.Serialize%2A> 或 <xref:System.Text.Json.JsonSerializer.Deserialize%2A> 时不传入选项对象来避免无限循环。 选项对象包含 <xref:System.Text.Json.JsonSerializerOptions.Converters%2A> 集合。 如果将它传递给 `Serialize` 或 `Deserialize`，则自定义转换器会调入其自身，从而产生导致堆栈溢出异常的无限循环。 如果默认选项不可行，请使用所需设置创建选项的新实例。 此方法会速度较慢，因为每个新实例都会独立缓存。

有一种替代模式，可在要转换的类上使用 `JsonConverterAttribute` 注册。 在此方法中，转换器代码对派生自要转换的类的类调用 `Serialize` 或 `Deserialize`。 派生类没有应用 `JsonConverterAttribute`。 在此替代的以下示例中：

* `WeatherForecastWithRequiredPropertyConverterAttribute` 是要进行反序列化并应用 `JsonConverterAttribute` 的类。
* `WeatherForecastWithoutRequiredPropertyConverterAttribute` 是不具有转换器属性的派生类。
* 转换器中的代码调用 `WeatherForecastWithoutRequiredPropertyConverterAttribute` 上的 `Serialize`和 `Deserialize` 以避免无限循环。 此方法对于序列化是一种性能开销，因为需要实例化额外的对象和复制属性值。

`WeatherForecast*` 类型如下：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithReqPptyConverterAttr":::

下面是转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecastRequiredPropertyConverterForAttributeRegistration.cs":::

如果需要处理特性（例如 [[JsonIgnore]](xref:System.Text.Json.Serialization.JsonIgnoreAttribute)）或不同选项（如自定义编码器），必需的属性转换器需要其他逻辑。 此外，示例代码不处理在构造函数中为其设置了默认值的属性。 而且此方法不区分以下情况：

* JSON 中缺少属性。
* JSON 中存在不可为 null 的类型的属性，但值是该类型的默认值，如 `int` 的值为零。
* JSON 中存在可为 null 的值类型的属性，但值为 null。

### <a name="specify-date-format"></a>指定日期格式

`Newtonsoft.Json` 提供多种方法来控制如何序列化和反序列化 `DateTime` 和 `DateTimeOffset` 类型的属性：

* `DateTimeZoneHandling` 设置可用于将所有 `DateTime` 值序列化为 UTC 日期。
* `DateFormatString` 设置和 `DateTime` 转换器可用于自定义日期字符串的格式。

<xref:System.Text.Json> 支持 ISO 8601-1:2019，包括 RFC 3339 配置文件。 此格式被广泛采用，无歧义，并且精确地进行往返。 若要使用任何其他格式，请创建自定义转换器。 有关详细信息，请参阅 [System.Text.Json 中的 DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)。

### <a name="callbacks"></a>回调

`Newtonsoft.Json` 使你可以在序列化或反序列化过程中的多个点执行自定义代码：

* OnDeserializing（开始反序列化对象时）
* OnDeserialized（对象反序列化完成时）
* OnSerializing（开始序列化对象时）
* OnSerialized（对象序列化完成时）

在 <xref:System.Text.Json> 中，可以通过编写自定义转换器来模拟回调。 以下示例演示适用于 POCO 的自定义转换器。 该转换器包含在与 `Newtonsoft.Json` 回调相对应的每个点显示消息的代码。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecastCallbacksConverter.cs":::

通过向 <xref:System.Text.Json.JsonSerializerOptions.Converters> 集合[添加转换器](system-text-json-converters-how-to.md#registration-sample---converters-collection)来注册此自定义转换器。

如果使用遵循前面示例的自定义转换器：

* `OnDeserializing` 代码无权访问新 POCO 实例。 若要在反序列化开始时操作新 POCO 实例，请将该代码放入 POCO 构造函数中。
* 通过在选项对象中注册转换器而在以递归方式调用 `Serialize` 或 `Deserialize` 时不传入选项对象，避免无限循环。

若要详细了解递归调用 `Serialize` 或 `Deserialize` 的自定义转换器，请参阅本文前面的[必需属性](#required-properties)部分。

### <a name="non-public-property-setters-and-getters"></a>非公共属性资源库和 Getter

`Newtonsoft.Json` 可以通过 `JsonProperty` 特性使用私有和内部属性 setter 和 getter。

::: zone pivot="dotnet-5-0"
<xref:System.Text.Json> 支持通过 [[JsonInclude]](xref:System.Text.Json.Serialization.JsonIncludeAttribute) 特性使用私有和内部属性资源库和 Getter。 有关示例代码，请参阅[非公共属性访问器](system-text-json-immutability.md)。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 中的 <xref:System.Text.Json> 仅支持公共资源库。 自定义转换器可提供此功能。
::: zone-end

### <a name="populate-existing-objects"></a>填充现有对象

`Newtonsoft.Json` 中的 `JsonConvert.PopulateObject` 方法将 JSON 文档反序列化为类的现有实例，而不是创建新实例。 <xref:System.Text.Json> 始终使用默认公共无参数构造函数创建目标类型的新实例。 自定义转换器可以反序列化为现有实例。

### <a name="reuse-rather-than-replace-properties"></a>重用而不是替换属性

`Newtonsoft.Json` `ObjectCreationHandling` 设置使你可以指定在反序列化过程中应重用属性中的对象，而不是进行替换。 <xref:System.Text.Json> 始终替换属性中的对象。  自定义转换器可提供此功能。

### <a name="add-to-collections-without-setters"></a>在不带 setter 的情况下添加到集合

在反序列化过程中，`Newtonsoft.Json` 会将对象添加到集合，即使属性没有 setter。 <xref:System.Text.Json> 会忽略没有 setter 的属性。 自定义转换器可提供此功能。

### <a name="systemruntimeserialization-attributes"></a>System.Runtime.Serialization 特性

<xref:System.Text.Json> 不支持 `System.Runtime.Serialization` 命名空间中的特性，如 `DataMemberAttribute` 和 `IgnoreDataMemberAttribute`。

### <a name="octal-numbers"></a>八进制数字

`Newtonsoft.Json` 将带前导零的数字视为八进制数字。 <xref:System.Text.Json> 不允许存在前导零，因为 [RFC 8259](https://tools.ietf.org/html/rfc8259) 规范不允许。

### <a name="missingmemberhandling"></a>MissingMemberHandling

`Newtonsoft.Json` 可以配置为在 JSON 包含目标类型中缺少的属性时，在反序列化过程中引发异常。 <xref:System.Text.Json> 会忽略 JSON 中的额外属性，但在使用 [[JsonExtensionData] 特性](system-text-json-handle-overflow.md)时除外。 对于缺少成员功能，没有解决方法。

### <a name="tracewriter"></a>TraceWriter

`Newtonsoft.Json` 使你可以使用 `TraceWriter` 进行调试，以查看序列化或反序列化所生成的日志。 <xref:System.Text.Json> 不执行日志记录。

## <a name="jsondocument-and-jsonelement-compared-to-jtoken-like-jobject-jarray"></a>与 JToken（如 JObject、JArray）相比的 JsonDocument 和 JsonElement

<xref:System.Text.Json.JsonDocument?displayProperty=fullName> 提供从现有 JSON 有效负载分析和生成只读文档对象模型 (DOM) 的功能。 DOM 提供对 JSON 有效负载中的数据的随机访问。 可以通过 <xref:System.Text.Json.JsonElement> 类型访问构成有效负载的 JSON 元素。 `JsonElement` 类型提供用于将 JSON 文本转换为常见 .NET 类型的 API。 `JsonDocument` 公开了 <xref:System.Text.Json.JsonDocument.RootElement> 属性。

### <a name="jsondocument-is-idisposable"></a>JsonDocument 为 IDisposable

`JsonDocument` 将内存中的数据视图生成到共用缓冲区中。 因此，与 `Newtonsoft.Json` 中的 `JObject` 或 `JArray` 不同，`JsonDocument` 类型实现 `IDisposable` 并且需要在 using 块中使用。

如果要将生存期所有权和释放责任转移到调用方，则只需从 API 返回 `JsonDocument`。 在大多数情况下，这不是必需的。 如果调用方需要处理整个 JSON 文档，则返回 <xref:System.Text.Json.JsonDocument.RootElement%2A> 的 <xref:System.Text.Json.JsonElement.Clone%2A>，这是 <xref:System.Text.Json.JsonElement>。 如果调用方需要处理 JSON 文档中的特定元素，则返回该 <xref:System.Text.Json.JsonElement> 的 <xref:System.Text.Json.JsonElement.Clone%2A>。 如果在不进行 `Clone` 的情况下直接返回 `RootElement` 或子元素，则在释放拥有返回的 `JsonElement` 的 `JsonDocument` 之后，调用方将无法访问它。

下面是一个要求你进行 `Clone` 的示例：

```csharp
public JsonElement LookAndLoad(JsonElement source)
{
    string json = File.ReadAllText(source.GetProperty("fileName").GetString());

    using (JsonDocument doc = JsonDocument.Parse(json))
    {
        return doc.RootElement.Clone();
    }
}
```

前面的代码需要包含 `fileName` 属性的 `JsonElement`。 它会打开 JSON 文件并创建一个 `JsonDocument`。 该方法假设调用方要处理整个文档，因此会返回 `RootElement` 的 `Clone`。

如果收到 `JsonElement` 并要返回子元素，则无需返回子元素的 `Clone`。 调用方负责使传入的 `JsonElement` 所属的 `JsonDocument` 保持活动状态。 例如：

```csharp
public JsonElement ReturnFileName(JsonElement source)
{
   return source.GetProperty("fileName");
}
```

### <a name="jsondocument-is-read-only"></a>JsonDocument 为只读

<xref:System.Text.Json> DOM 无法添加、删除或修改 JSON 元素。 它这样设计是为了实现性能，并减少用于分析常见 JSON 有效负载大小（即 < 1 MB）的分配。 如果你的方案当前使用可修改的 DOM，则以下解决方法之一可能是可行的：

* 若要从头开始生成 `JsonDocument`（即，不将现有 JSON 有效负载传入到 `Parse` 方法），请使用 `Utf8JsonWriter` 编写 JSON 文本，并分析这样做的输出以创建新 `JsonDocument`。
* 若要修改现有 `JsonDocument`，请使用它编写 JSON 文本（在编写时进行更改），并分析这样做的输出以创建新 `JsonDocument`。
* 若要合并现有 JSON 文档（与 `Newtonsoft.Json` 中的 `JObject.Merge` 或 `JContainer.Merge` API 等效），请参阅[此 GitHub 问题](https://github.com/dotnet/corefx/issues/42466#issuecomment-570475853)。

### <a name="jsonelement-is-a-union-struct"></a>JsonElement 是联合结构

`JsonDocument` 将 `RootElement` 公开为类型 <xref:System.Text.Json.JsonElement> 的属性，该类型是包含任何 JSON 元素的联合结构类型。 `Newtonsoft.Json` 使用专用分层类型，如 `JObject`、`JArray`、`JToken` 等。 `JsonElement` 是可以搜索和枚举的内容，你可以使用 `JsonElement` 将 JSON 元素具体化为 .NET 类型。

### <a name="how-to-search-a-jsondocument-and-jsonelement-for-sub-elements"></a>如何搜索子元素的 JsonDocument 和 JsonElement

使用 `Newtonsoft.Json` 中的 `JObject` 或 `JArray` 搜索 JSON 令牌的速度往往相对较快，因为它们是在某个字典中查找。 相比之下，对 `JsonElement` 进行搜索需要对属性进行线性搜索，因此速度相对较慢（例如在使用 `TryGetProperty` 时）。 <xref:System.Text.Json> 旨在最大程度减少初始分析时间，而不是查找时间。 因此，在通过 `JsonDocument` 对象搜索时，请使用以下方法优化性能：

* 使用内置枚举器（<xref:System.Text.Json.JsonElement.EnumerateArray%2A> 和 <xref:System.Text.Json.JsonElement.EnumerateObject%2A>），而不是执行自己的索引或循环。
* 不要使用 `RootElement` 通过每个属性对整个 `JsonDocument` 执行线性搜索。 而是基于 JSON 数据的已知结构对嵌套 JSON 对象进行搜索。 例如，如果要在 `Student` 对象中查找 `Grade` 属性，请循环访问 `Student` 对象，并获取每个对象的 `Grade` 值，而不是搜索所有 `Grade` 对象来查找 `JsonElement` 属性。 执行后者将导致不必要浏览相同数据。

有关代码示例，请参阅[使用 JsonDocument 访问数据](write-custom-serializer-deserializer.md#use-jsondocument-for-access-to-data)。

## <a name="utf8jsonreader-compared-to-jsontextreader"></a>Utf8JsonReader 与 JsonTextReader 的比较

<xref:System.Text.Json.Utf8JsonReader?displayProperty=fullName> 是面向 UTF-8 编码 JSON 文本的一个高性能、低分配的只进读取器，从 [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601) 或 [ReadOnlySequence\<byte>](xref:System.Buffers.ReadOnlySequence%601) 读取信息。 `Utf8JsonReader` 是一种低级类型，可用于生成自定义分析器和反序列化程序。

以下各节说明使用 `Utf8JsonReader` 的推荐编程模式。

### <a name="utf8jsonreader-is-a-ref-struct"></a>Utf8JsonReader 是 ref struct

由于 `Utf8JsonReader` 类型是 ref struct，因此它具有[某些限制](../../csharp/language-reference/builtin-types/struct.md#ref-struct)。 例如，它无法作为字段存储在 ref struct 之外的类或结构中。 若要实现高性能，此类型必须为 `ref struct`，因为它需要缓存输入 [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601)（这本身便是 ref struct）。 此外，此类型是可变的，因为它包含状态。 因此，它按引用传递而不是按值传递。 按值传递会产生结构副本，状态更改会对调用方不可见。 这与 `Newtonsoft.Json` 不同，因为 `Newtonsoft.Json` `JsonTextReader` 是一个类。 有关如何使用 ref struct 的详细信息，请参阅[编写安全有效的 C# 代码](../../csharp/write-safe-efficient-code.md)。

### <a name="read-utf-8-text"></a>读取 UTF-8 文本

若要在使用 `Utf8JsonReader` 时实现可能的最佳性能，请读取已编码为 UTF-8 文本（而不是 UTF-16 字符串）的 JSON 有效负载。 有关代码示例，请参阅[使用 Utf8JsonReader 筛选数据](write-custom-serializer-deserializer.md#filter-data-using-utf8jsonreader)。

### <a name="read-with-a-stream-or-pipereader"></a>使用流或 PipeReader 进行读取

`Utf8JsonReader` 支持从 UTF-8 编码的 [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601) 或 [ReadOnlySequence\<byte>](xref:System.Buffers.ReadOnlySequence%601)（这是从 <xref:System.IO.Pipelines.PipeReader> 读取的结果）进行读取。

对于同步读取，可以读取 JSON 有效负载，直到流的末尾进入字节数组中，并将该数组传递给读取器。 若要从字符串（编码为 UTF-16）进行读取，请调用 <xref:System.Text.Encoding.UTF8>.<xref:System.Text.Encoding.GetBytes%2A> 以首先将字符串转码为 UTF-8 编码的字节数组。 然后将该数组传递给 `Utf8JsonReader`。

由于 `Utf8JsonReader` 将输入视为 JSON 文本，因此 UTF-8 字节顺序标记 (BOM) 被视为无效 JSON。 调用方需要在将数据传递给读取器之前将该标记筛选出来。

有关代码示例，请参阅[使用 Utf8JsonReader](write-custom-serializer-deserializer.md#use-utf8jsonreader)。

### <a name="read-with-multi-segment-readonlysequence"></a>使用多段 ReadOnlySequence 进行读取

如果 JSON 输入是 [ReadOnlySpan\<byte>](xref:System.ReadOnlySpan%601)，则在运行读取循环时，可以从读取器上的 `ValueSpan` 属性访问每个 JSON 元素。 但是，如果输入是 [ReadOnlySequence\<byte>](xref:System.Buffers.ReadOnlySequence%601)（这是从 <xref:System.IO.Pipelines.PipeReader> 读取的结果），则某些 JSON 元素可能会跨 `ReadOnlySequence<byte>` 对象的多个段。 无法在连续内存块中从 <xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A> 访问这些元素。 而是在每次将多段 `ReadOnlySequence<byte>` 作为输入时，轮询读取器上的 <xref:System.Text.Json.Utf8JsonReader.HasValueSequence%2A> 属性，以确定如何访问当前 JSON 元素。 下面是推荐模式：

```csharp
while (reader.Read())
{
    switch (reader.TokenType)
    {
        // ...
        ReadOnlySpan<byte> jsonElement = reader.HasValueSequence ?
            reader.ValueSequence.ToArray() :
            reader.ValueSpan;
        // ...
    }
}
```

### <a name="use-valuetextequals-for-property-name-lookups"></a>使用 ValueTextEquals 进行属性名称查找

不要使用 <xref:System.Text.Json.Utf8JsonReader.ValueSpan%2A> 通过对属性名称查找调用 <xref:System.MemoryExtensions.SequenceEqual%2A> 来执行逐字节比较。 改为调用 <xref:System.Text.Json.Utf8JsonReader.ValueTextEquals%2A>，因为该方法会对在 JSON 中转义的任何字符取消转义。 下面的示例演示如何搜索名为“name”的属性：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/ValueTextEqualsExample.cs" id="DefineUtf8Var":::

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/ValueTextEqualsExample.cs" id="UseUtf8Var" highlight="9":::

### <a name="read-null-values-into-nullable-value-types"></a>将 null 值读取到可为 null 的值类型中

`Newtonsoft.Json` 提供返回 <xref:System.Nullable%601> 的 API，如 `ReadAsBoolean`（它通过返回 `bool?` 来处理 `Null` `TokenType`）。 内置 `System.Text.Json` API 仅返回不可为 null 的值类型。 例如，<xref:System.Text.Json.Utf8JsonReader.GetBoolean%2A?displayProperty=nameWithType> 返回 `bool`。 如果它在 JSON 中发现 `Null`，则会引发异常。 下面的示例演示两种用于处理 null 的方法，一种方法是返回可为 null 的值类型，另一种方法是返回默认值：

```csharp
public bool? ReadAsNullableBoolean()
{
    _reader.Read();
    if (_reader.TokenType == JsonTokenType.Null)
    {
        return null;
    }
    if (_reader.TokenType != JsonTokenType.True && _reader.TokenType != JsonTokenType.False)
    {
        throw new JsonException();
    }
    return _reader.GetBoolean();
}
```

```csharp
public bool ReadAsBoolean(bool defaultValue)
{
    _reader.Read();
    if (_reader.TokenType == JsonTokenType.Null)
    {
        return defaultValue;
    }
    if (_reader.TokenType != JsonTokenType.True && _reader.TokenType != JsonTokenType.False)
    {
        throw new JsonException();
    }
    return _reader.GetBoolean();
}
```

### <a name="multi-targeting"></a>多目标

如果需要继续为某些目标框架使用 `Newtonsoft.Json`，则可以使用多目标，并具有两种实现。 但是，这并非易事，需要进行一些 `#ifdefs` 和源文件复制。 共享尽可能多代码的一种方法是围绕 `Utf8JsonReader` 和 `Newtonsoft.Json` `JsonTextReader` 创建 `ref struct` 包装器。 此包装器会统一公共外围应用，同时隔离行为差异。 这使你可以隔离主要对类型的构造进行的更改，以及按引用传递新类型。 下面是 [Microsoft.Extensions.DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/) 库遵循的模式：

* [UnifiedJsonReader.JsonTextReader.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonReader.JsonTextReader.cs)
* [UnifiedJsonReader.Utf8JsonReader.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonReader.Utf8JsonReader.cs)

## <a name="utf8jsonwriter-compared-to-jsontextwriter"></a>Utf8JsonWriter 与 JsonTextWriter 的比较

<xref:System.Text.Json.Utf8JsonWriter?displayProperty=fullName> 是一种高性能方式，从常见 .NET 类型（例如，`String`、`Int32` 和 `DateTime`）编写 UTF-8 编码的 JSON 文本。 该编写器是一种低级类型，可用于生成自定义序列化程序。

以下各节说明使用 `Utf8JsonWriter` 的推荐编程模式。

### <a name="write-with-utf-8-text"></a>使用 UTF-8 文本进行编写

若要在使用 `Utf8JsonWriter` 时实现可能的最佳性能，请编写已编码为 UTF-8 文本（而不是 UTF-16 字符串）的 JSON 有效负载。 使用 <xref:System.Text.Json.JsonEncodedText> 可缓存已知字符串属性名称和值并预先编码为静态，并将这些内容传递给编写器，而不是使用 UTF-16 字符串文本。 这比缓存并使用 UTF-8 字节数组更快。

如果需要进行自定义转义，此方法也适用。 `System.Text.Json` 不允许在编写字符串时禁用转义。 但是，可以将自己的自定义 <xref:System.Text.Encodings.Web.JavaScriptEncoder> 作为一个选项传入编写器，或创建自己的 `JsonEncodedText` 以使用你的 `JavascriptEncoder` 进行转义，然后编写 `JsonEncodedText` 而不是字符串。 有关详细信息，请参阅[自定义字符编码](system-text-json-character-encoding.md)。

### <a name="write-raw-values"></a>编写原始值

`Newtonsoft.Json` `WriteRawValue` 方法可编写原始 JSON（其中需要值）。 <xref:System.Text.Json> 没有直接等效项，但下面是确保仅编写有效 JSON 的解决方法：

```csharp
using JsonDocument doc = JsonDocument.Parse(string);
doc.WriteTo(writer);
```

### <a name="customize-character-escaping"></a>自定义字符转义

`JsonTextWriter` 的 [StringEscapeHandling](https://www.newtonsoft.com/json/help/html/T_Newtonsoft_Json_StringEscapeHandling.htm) 设置提供用于转移所有非 ASCII 字符或 HTML 字符的选项。 默认情况下，`Utf8JsonWriter` 会转义所有非 ASCII 和 HTML 字符。 进行此转义是出于深度防御安全原因。 若要指定不同的转义策略，请创建 <xref:System.Text.Encodings.Web.JavaScriptEncoder> 并设置 <xref:System.Text.Json.JsonWriterOptions.Encoder?displayProperty=nameWithType>。 有关详细信息，请参阅[自定义字符编码](system-text-json-character-encoding.md)。

### <a name="customize-json-format"></a>自定义 JSON 格式

`JsonTextWriter` 包含以下设置（`Utf8JsonWriter` 对于它们没有等效项）：

* [缩进](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_Indentation.htm) - 指定要缩进的字符数。 `Utf8JsonWriter` 始终执行 2 字符缩进。
* [IndentChar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_IndentChar.htm) - 指定要用于缩进的字符。  `Utf8JsonWriter` 始终使用空格。
* [QuoteChar](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteChar.htm) - 指定要用于围绕字符串值的字符。  `Utf8JsonWriter` 始终使用双引号。
* [QuoteName](https://www.newtonsoft.com/json/help/html/P_Newtonsoft_Json_JsonTextWriter_QuoteName.htm) - 指定是否要使用引号围绕属性名称。  `Utf8JsonWriter` 始终使用引号围绕它们。

没有解决方法可让你自定义 `Utf8JsonWriter` 以这些方式生成的 JSON。

### <a name="write-null-values"></a>编写 null 值

若要使用 `Utf8JsonWriter` 编写 null 值，请调用：

* <xref:System.Text.Json.Utf8JsonWriter.WriteNull%2A>，用于将具有 null 的键值对编写为值。
* <xref:System.Text.Json.Utf8JsonWriter.WriteNullValue%2A>用于将 null 编写为 JSON 数组的元素。

对于字符串属性，如果字符串为 null，则 <xref:System.Text.Json.Utf8JsonWriter.WriteString%2A> 和 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A> 等效于 `WriteNull` 和 `WriteNullValue`。

### <a name="write-timespan-uri-or-char-values"></a>编写 Timespan、Uri 或 char 值

`JsonTextWriter` 提供 `WriteValue` 方法以用于 [TimeSpan](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_18.htm)、[Uri](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_22.htm) 和 [char](https://www.newtonsoft.com/json/help/html/M_Newtonsoft_Json_JsonTextWriter_WriteValue_3.htm) 值。 `Utf8JsonWriter` 没有等效方法。 而是将这些值格式化为字符串（例如，通过调用 `ToString()`）并调用 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A>。

### <a name="multi-targeting"></a>多目标

如果需要继续为某些目标框架使用 `Newtonsoft.Json`，则可以使用多目标，并具有两种实现。 但是，这并非易事，需要进行一些 `#ifdefs` 和源文件复制。 共享尽可能多代码的一种方法是围绕 `Utf8JsonWriter` 和 `Newtonsoft` `JsonTextWriter` 创建包装器。 此包装器会统一公共外围应用，同时隔离行为差异。 这使你可以隔离主要对类型的构造进行的更改。 [Microsoft.Extensions.DependencyModel](https://www.nuget.org/packages/Microsoft.Extensions.DependencyModel/3.1.0/) 库遵循：

* [UnifiedJsonWriter.JsonTextWriter.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonWriter.JsonTextWriter.cs)
* [UnifiedJsonWriter.Utf8JsonWriter.cs](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/installer/managed/Microsoft.Extensions.DependencyModel/UnifiedJsonWriter.Utf8JsonWriter.cs)

## <a name="additional-resources"></a>其他资源

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何对 JSON 进行序列化和反序列化](system-text-json-how-to.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [自定义属性名称和值](system-text-json-customize-properties.md)
* [忽略属性](system-text-json-ignore-properties.md)
* [允许无效的 JSON](system-text-json-invalid-json.md)
* [处理溢出 JSON](system-text-json-handle-overflow.md)
* [保留引用](system-text-json-preserve-references.md)
* [不可变类型和非公共访问器](system-text-json-immutability.md)
* [多态序列化](system-text-json-polymorphism.md)
* [自定义字符编码](system-text-json-character-encoding.md)
* [编写自定义序列化程序和反序列化程序](write-custom-serializer-deserializer.md)
* [编写用于 JSON 序列化的自定义转换器](system-text-json-converters-how-to.md)
* [DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
* [System.Text.Json.Serialization API 参考](xref:System.Text.Json.Serialization)
