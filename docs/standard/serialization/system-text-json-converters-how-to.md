---
title: 如何编写用于 JSON 序列化的自定义转换器 - .NET
description: 了解如何为 System.Text.Json 命名空间中提供的 JSON 序列化类创建自定义转换器。
ms.date: 12/09/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
zone_pivot_groups: dotnet-version
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
- converters
ms.openlocfilehash: 33334ccd8bad4ac5a9f5dccde79ff3ae09ca8f89
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008859"
---
# <a name="how-to-write-custom-converters-for-json-serialization-marshalling-in-net"></a>如何在 .NET 中编写用于 JSON 序列化（封送）的自定义转换器

本文介绍如何为 <xref:System.Text.Json> 命名空间中提供的 JSON 序列化类创建自定义转换器。 有关 `System.Text.Json` 简介，请参阅[如何在 .NET 中对 JSON 数据进行序列化和反序列化](system-text-json-how-to.md)。

转换器是一种将对象或值与 JSON 相互转换的类。 `System.Text.Json` 命名空间为映射到 JavaScript 基元的大多数基元类型提供内置转换器。 可以编写自定义转换器来实现以下目标：

* 重写内置转换器的默认行为。 例如，你可能希望通过 mm/dd/yyyy 格式来表示 `DateTime` 值。 默认情况下，支持 ISO 8601-1:2019，包括 RFC 3339 配置文件。 有关详细信息，请参阅 [System.Text.Json 中的 DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)。
* 支持自定义值类型。 例如，`PhoneNumber` 结构。

还可以编写自定义转换器，以使用当前版本中未包含的功能自定义或扩展 `System.Text.Json`。 本文后面部分介绍了以下方案：

::: zone pivot="dotnet-5-0"

* [将推断类型反序列化为对象属性](#deserialize-inferred-types-to-object-properties)。
* [支持多态反序列化](#support-polymorphic-deserialization)。
* [支持堆栈的往返\<T>](#support-round-trip-for-stackt)。
::: zone-end

::: zone pivot="dotnet-core-3-1"

* [将推断类型反序列化为对象属性](#deserialize-inferred-types-to-object-properties)。
* [支持包含非字符串键的字典](#support-dictionary-with-non-string-key)。
* [支持多态反序列化](#support-polymorphic-deserialization)。
* [支持堆栈的往返\<T>](#support-round-trip-for-stackt)。
::: zone-end

在为自定义转换器编写的代码中，请注意，使用新的 <xref:System.Text.Json.JsonSerializerOptions> 实例会带来重大性能损失。 有关详细信息，请参阅[重用 JsonSerializerOptions 实例](system-text-json-configure-options.md#reuse-jsonserializeroptions-instances)。

## <a name="custom-converter-patterns"></a>自定义转换器模式

用于创建自定义转换器的模式有两种：基本模式和工厂模式。 工厂模式适用于处理类型 `Enum` 或开放式泛型的转换器。 基本模式适用于非泛型或封闭式泛型类型。  例如，适用于以下类型的转换器需要工厂模式：

* <xref:System.Collections.Generic.Dictionary%602>
* <xref:System.Enum>
* <xref:System.Collections.Generic.List%601>

可以通过基本模式处理的类型的一些示例包括：

* `Dictionary<int, string>`
* `WeekdaysEnum`
* `List<DateTimeOffset>`
* <xref:System.DateTime>
* <xref:System.Int32>

基本模式创建的类可以处理一种类型。 工厂模式创建的类在运行时确定所需的特定类型，并动态创建适当的转换器。

## <a name="sample-basic-converter"></a>示例基本转换器

下面的示例是一个转换器，可重写现有数据类型的默认序列化。 该转换器将 mm/dd/yyyy 格式用于 `DateTimeOffset` 属性。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/DateTimeOffsetConverter.cs":::

## <a name="sample-factory-pattern-converter"></a>示例工厂模式转换器

下面的代码演示一个处理 `Dictionary<Enum,TValue>` 的自定义转换器。 该代码遵循工厂模式，因为第一个泛型类型参数是 `Enum`，第二个参数是开放参数。 `CanConvert` 方法仅对具有两个泛型参数的 `Dictionary` 返回 `true`，其中第一个参数是 `Enum` 类型。 内部转换器获取现有转换器，以处理在运行时为 `TValue` 提供的任何类型。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/DictionaryTKeyEnumTValueConverter.cs":::

前面的代码与本文后面的[支持包含非字符串键的字典](#support-dictionary-with-non-string-key)中演示的代码相同。

## <a name="steps-to-follow-the-basic-pattern"></a>遵循基本模式的步骤

以下步骤说明如何遵循基本模式来创建转换器：

* 创建一个派生自 <xref:System.Text.Json.Serialization.JsonConverter%601> 的类，其中 `T` 是要进行序列化和反序列化的类型。
* 重写 `Read` 方法，以反序列化传入 JSON 并将其转换为类型 `T`。 使用传递给方法的 <xref:System.Text.Json.Utf8JsonReader> 读取 JSON。
* 重写 `Write` 方法以序列化 `T` 类型的传入对象。 使用传递给方法的 <xref:System.Text.Json.Utf8JsonWriter> 写入 JSON。
* 仅当需要时才重写 `CanConvert` 方法。 当要转换的类型属于类型 `T` 时，默认实现会返回 `true`。 因此，仅支持类型 `T` 的转换器不需要重写此方法。 有关的确需要重写此方法的转换器的示例，请参阅本文后面的[多态反序列化](#support-polymorphic-deserialization)部分。

可以参阅[内置转换器源代码](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters/)作为用于编写自定义转换器的参考实现。

## <a name="steps-to-follow-the-factory-pattern"></a>遵循工厂模式的步骤

以下步骤说明如何遵循工厂模式来创建转换器：

* 创建一个从 <xref:System.Text.Json.Serialization.JsonConverterFactory> 派生的类。
* 重写 `CanConvert` 方法，以在要转换的类型是转换器可处理的类型时返回 true。 例如，如果转换器适用于 `List<T>`，则它可能仅处理 `List<int>`、`List<string>` 和 `List<DateTime>`。
* 重写 `CreateConverter` 方法，以返回将处理在运行时提供的要转换的类型的转换器类实例。
* 创建 `CreateConverter` 方法实例化的转换器类。

开放式泛型需要工厂模式，因为用于将对象与字符串相互转换的代码对于所有类型并不相同。 适用于开放式泛型类型（例如 `List<T>`）的转换器必须在幕后为封闭式泛型类型（例如 `List<DateTime>`）创建转换器。 必须编写代码来处理转换器可处理的每种封闭式泛型类型。

`Enum` 类型类似于开放式泛型类型：适用于 `Enum` 的转换器必须在幕后为特定 `Enum`（例如`WeekdaysEnum`）创建转换器。

## <a name="error-handling"></a>错误处理

序列化程序为 <xref:System.Text.Json.JsonException> 和 <xref:System.NotSupportedException> 异常类型提供特殊处理。

### <a name="jsonexception"></a>JsonException

如果你引发不带消息的 `JsonException`，则序列化程序会创建一条消息，其中包括导致错误的 JSON 部分的路径。 例如，语句 `throw new JsonException()` 会生成如以下示例的错误消息：

```output
Unhandled exception. System.Text.Json.JsonException:
The JSON value could not be converted to System.Object.
Path: $.Date | LineNumber: 1 | BytePositionInLine: 37.
```

如果你确实提供了消息（例如 `throw new JsonException("Error occurred")`），则序列化程序仍会设置 <xref:System.Text.Json.JsonException.Path>、<xref:System.Text.Json.JsonException.LineNumber> 和 <xref:System.Text.Json.JsonException.BytePositionInLine> 属性。

### <a name="notsupportedexception"></a>NotSupportedException

如果你引发 `NotSupportedException`，则始终会在消息中获取路径信息。 如果你提供了消息，则路径信息将追加到该消息中。 例如，语句 `throw new NotSupportedException("Error occurred.")` 会生成如以下示例的错误消息：

```output
Error occurred. The unsupported member type is located on type
'System.Collections.Generic.Dictionary`2[Samples.SummaryWords,System.Int32]'.
Path: $.TemperatureRanges | LineNumber: 4 | BytePositionInLine: 24
```

### <a name="when-to-throw-which-exception-type"></a>何时引发哪种异常类型

当 JSON 有效负载包含对于正在进行反序列化的类型无效的令牌时，引发 `JsonException`。

当你要禁止某些类型时，引发 `NotSupportedException`。 对于不支持的类型，序列化程序会自动引发此异常。 例如，出于安全原因，不支持 `System.Type`，因此尝试对其进行反序列化会导致 `NotSupportedException`。

可根据需要引发其他异常，但它们不会自动包括 JSON 路径信息。

## <a name="register-a-custom-converter"></a>注册自定义转换器

注册自定义转换器，使 `Serialize` 和 `Deserialize` 方法可使用它。 选择以下方法之一：

* 向 <xref:System.Text.Json.JsonSerializerOptions.Converters?displayProperty=nameWithType> 集合添加转换器类的实例。
* 将 [[JsonConverter]](xref:System.Text.Json.Serialization.JsonConverterAttribute) 特性应用于需要自定义转换器的属性。
* 将 [[JsonConverter]](xref:System.Text.Json.Serialization.JsonConverterAttribute) 特性应用于表示自定义值类型的类或结构。

## <a name="registration-sample---converters-collection"></a>注册示例 - 转换器集合

下面是一个示例，该示例将 <xref:System.ComponentModel.DateTimeOffsetConverter> 设为类型 <xref:System.DateTimeOffset> 的属性的默认值：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RegisterConverterWithConvertersCollection.cs" id="Serialize":::

假设序列化以下类型的实例：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

下面是演示使用自定义转换器的 JSON 输出的示例：

```json
{
  "Date": "08/01/2019",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

下面的代码使用的方法与使用自定义 `DateTimeOffset` 转换器进行反序列化相同：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RegisterConverterWithConvertersCollection.cs" id="Deserialize":::

## <a name="registration-sample---jsonconverter-on-a-property"></a>注册示例 - 属性上的 [JsonConverter]

下面的代码为 `Date` 属性选择自定义转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithConverterAttribute":::

用于序列化 `WeatherForecastWithConverterAttribute` 的代码不需要使用 `JsonSerializeOptions.Converters`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RegisterConverterWithAttributeOnProperty.cs" id="Serialize":::

用于反序列化的代码也不需要使用 `Converters`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RegisterConverterWithAttributeOnProperty.cs" id="Deserialize":::

## <a name="registration-sample---jsonconverter-on-a-type"></a>注册示例 - 类型上的 [JsonConverter]

下面的代码创建一个结构并向它应用 `[JsonConverter]` 属性：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/Temperature.cs":::

下面是适用于上述结构的自定义转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/TemperatureConverter.cs":::

结构上的 `[JsonConvert]` 属性将自定义转换器注册为类型 `Temperature` 的属性的默认值。 进行序列化或反序列化时，转换器会自动用于以下类型的 `TemperatureCelsius` 属性：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithTemperatureStruct":::

## <a name="converter-registration-precedence"></a>转换器注册优先级

在序列化或反序列化过程中，按以下顺序（从最高优先级到最低优先级来列出）为每个 JSON 元素选择转换器：

* 应用于属性的 `[JsonConverter]`。
* 向 `Converters` 集合添加的转换器。
* 应用于自定义值类型或 POCO 的 `[JsonConverter]`。

如果在 `Converters` 集合中注册了适用于某种类型的多个自定义转换器，则使用第一个为 `CanConvert` 返回 true 的转换器。

仅当未注册适用自定义转换器时，才会选择内置转换器。

## <a name="converter-samples-for-common-scenarios"></a>常见方案的转换器示例

以下各部分提供的转换器示例用于解决内置功能不处理的一些常见方案。

::: zone pivot="dotnet-5-0"

* [将推断类型反序列化为对象属性](#deserialize-inferred-types-to-object-properties)。
* [支持多态反序列化](#support-polymorphic-deserialization)。
* [支持堆栈的往返\<T>](#support-round-trip-for-stackt)。
::: zone-end

::: zone pivot="dotnet-core-3-1"

* [将推断类型反序列化为对象属性](#deserialize-inferred-types-to-object-properties)。
* [支持包含非字符串键的字典](#support-dictionary-with-non-string-key)。
* [支持多态反序列化](#support-polymorphic-deserialization)。
* [支持堆栈的往返\<T>](#support-round-trip-for-stackt)。
::: zone-end

### <a name="deserialize-inferred-types-to-object-properties"></a>将推断类型反序列化为对象属性

反序列化为类型 `object` 的属性时，将创建一个 `JsonElement` 对象。 这是因为反序列化程序不知道要创建的 CLR 类型，也不会尝试进行猜测。 例如，如果 JSON 属性具有“true”，则反序列化程序不会推断值为 `Boolean`，如果元素具有“01/01/2019”，则反序列化程序不会推断它是 `DateTime`。

类型推理可能不准确。 如果反序列化程序将没有小数点的 JSON 数字分析为 `long`，则当值最初序列化为 `ulong` 或 `BigInteger` 时，这可能会导致超出范围问题。 如果数字最初序列化为 `decimal`，则将具有小数点的数字分析为 `double` 可能会损失精度。

对于需要类型推理的方案，以下代码演示适用于 `object` 属性的自定义转换器。 代码：

* 将 `true` 和 `false` 转换为 `Boolean`
* 将不带小数的数字转换为 `long`
* 将带有小数的数字转换为 `double`
* 将日期转换为 `DateTime`
* 将字符串转换为 `string`
* 将其他所有内容转换为 `JsonElement`

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/ObjectToInferredTypesConverter.cs":::

下面的代码注册转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/DeserializeInferredTypesToObject.cs" id="Register":::

下面是一种具有 `object` 属性的示例类型：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithObjectProperties":::

以下要反序列化的 JSON 示例包含将作为 `DateTime`、`long` 和 `string` 进行反序列化的值：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
}
```

如果没有自定义转换器，则反序列化会将 `JsonElement` 放入每个属性中。

`System.Text.Json.Serialization` 命名空间中的[单元测试文件夹](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/)包含处理到 `object` 属性的反序列化的自定义转换器的更多示例。

::: zone pivot="dotnet-core-3-1"

### <a name="support-dictionary-with-non-string-key"></a>支持包含非字符串键的字典

对字典集合的内置支持适用于 `Dictionary<string, TValue>`。 即，键必须是字符串。 若要支持将整数或某种其他类型用作键的字典，需要自定义转换器。

下面的代码演示一个处理 `Dictionary<Enum,TValue>` 的自定义转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/DictionaryTKeyEnumTValueConverter.cs":::

下面的代码注册转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripDictionaryTkeyEnumTValue.cs" id="Register":::

该转换器可以序列化和反序列化使用以下 `Enum` 的以下类的 `TemperatureRanges` 属性：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithEnumDictionary":::

来自序列化的 JSON 输出类似于以下示例：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "TemperatureRanges": {
    "Cold": 20,
    "Hot": 40
  }
}
```

`System.Text.Json.Serialization` 命名空间中的[单元测试文件夹](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/)包含处理非字符串键字典的自定义转换器的更多示例。
::: zone-end

### <a name="support-polymorphic-deserialization"></a>支持多态反序列化

内置功能提供有限范围的[多态序列化](system-text-json-polymorphism.md)，但完全不支持反序列化。 反序列化需要自定义转换器。

例如，假设有一个 `Person` 抽象基类，其中包含 `Employee` 和 `Customer` 派生类。 多态反序列化意味着可以在设计时将 `Person` 指定为反序列化目标，JSON 中的 `Customer` 和 `Employee` 对象会在运行时正确地进行反序列化。 在反序列化过程中，必须查找标识 JSON 中所需类型的线索。 可用的线索类型因各个方案而异。 例如，可以使用鉴别器属性，或者可能必须依赖于特定属性是否存在。 `System.Text.Json` 的当前版本不提供属性来指定如何处理多态反序列化方案，因此需要自定义转换器。

下面的代码演示一个基类、两个派生类和适用于它们的一个自定义转换器。 该转换器使用鉴别器属性执行多态反序列化。 类型鉴别器不在类定义中，而是在序列化过程中创建，在反序列化过程中进行读取。

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/Person.cs" id="Person":::

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/PersonConverterWithTypeDiscriminator.cs":::

下面的代码注册转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripPolymorphic.cs" id="Register":::

该转换器可以反序列化通过用于序列化的相同转换器而创建的 JSON，例如：

```json
[
  {
    "TypeDiscriminator": 1,
    "CreditLimit": 10000,
    "Name": "John"
  },
  {
    "TypeDiscriminator": 2,
    "OfficeNumber": "555-1234",
    "Name": "Nancy"
  }
]
```

前面示例中的转换器代码会手动读取和写入每个属性。 一种替代方法是调用 `Deserialize` 或 `Serialize` 以执行某些工作。 有关示例，请参阅[此 StackOverflow 文章](https://stackoverflow.com/a/59744873/12509023)。

### <a name="support-round-trip-for-stackt"></a>支持堆栈的往返\<T>

如果将 JSON 字符串反序列化为 <xref:System.Collections.Generic.Stack%601> 对象，然后再序列化该对象，则堆栈的内容将按相反的顺序排列。 此行为适用于以下类型和接口以及从它们派生的用户定义类型：

* <xref:System.Collections.Stack>
* <xref:System.Collections.Generic.Stack%601>
* <xref:System.Collections.Concurrent.ConcurrentStack%601>
* <xref:System.Collections.Immutable.ImmutableStack%601>
* <xref:System.Collections.Immutable.IImmutableStack%601>

若要支持在堆栈中保留原始顺序的序列化和反序列化，则需要自定义转换器。

下面的代码演示了一个自定义转换器，用于实现与 `Stack<T>` 对象之间的来回转换：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/JsonConverterFactoryForStackOfT.cs":::

下面的代码注册转换器：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/RoundtripStackOfT.cs" id="Register":::

## <a name="handle-null-values"></a>处理 NULL 值

默认情况下，序列化程序处理 null 值，如下所示：

* 对于引用类型和 <xref:System.Nullable%601> 类型：

  * 它在序列化时不会将 `null` 传递到自定义转换器。
  * 它在反序列化时不会将 `JsonTokenType.Null` 传递到自定义转换器。
  * 它在反序列化时返回 `null` 实例。
  * 它在序列化时直接使用编写器写入 `null`。

* 对于不可为 null 的值类型：

  * 它在反序列化时将 `JsonTokenType.Null` 传递到自定义转换器。 （如果没有可用的自定义转换器，则由该类型的内部转换器引发 `JsonException` 异常。）

此 null 处理行为主要用于，通过跳过对转换器的额外调用来优化性能。 此外，它可避免在每个 `Read` 和 `Write` 方法重写开始时强制可以为 null 的类型的转换器检查 `null`。

::: zone pivot="dotnet-5-0"
若要启用自定义转换器来处理引用或值类型的 `null`，请重写 <xref:System.Text.Json.Serialization.JsonConverter%601.HandleNull%2A?displayProperty=nameWithType> 以返回 `true`，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/CustomConverterHandleNull.cs" highlight="19":::
::: zone-end

## <a name="other-custom-converter-samples"></a>其他自定义转换器示例

[从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md) 一文包含自定义转换器的其他示例。

`System.Text.Json.Serialization` 源代码中的[单元测试文件夹](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/)包含其他自定义转换器示例，例如：

* [在反序列化时将 null 转换为 0 的 Int32 转换器](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.NullValueType.cs)
* [在反序列化时允许同时使用字符串和数字值的 Int32 转换器](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Int32.cs)
* [枚举转换器](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Enum.cs)
* 接受外部数据的 [List\<T> 转换器](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.List.cs)
* [处理以逗号分隔的数字列表的 Long[] 转换器](https://github.com/dotnet/runtime/blob/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/tests/Serialization/CustomConverterTests.Array.cs)

如果需要创建修改现有内置转换器行为的转换器，则可以获取[现有转换器的源代码](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters)作为自定义的起点。

## <a name="additional-resources"></a>其他资源

* [内置转换器的源代码](https://github.com/dotnet/runtime/tree/81bf79fd9aa75305e55abe2f7e9ef3f60624a3a1/src/libraries/System.Text.Json/src/System/Text/Json/Serialization/Converters)
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
* [从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [自定义字符编码](system-text-json-character-encoding.md)
* [编写自定义序列化程序和反序列化程序](write-custom-serializer-deserializer.md)
* [DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
* [System.Text.Json.Serialization API 参考](xref:System.Text.Json.Serialization)
