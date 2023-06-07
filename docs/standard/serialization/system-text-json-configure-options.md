---
title: 如何使用 System.Text.Json 实例化 JsonSerializerOptions
description: 了解如何避免性能问题，以及如何对 JsonSerializerOptions 实例使用可用的构造函数。
ms.date: 12/02/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
zone_pivot_groups: dotnet-version
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: 5f32e1369e58dd9550f28abc822f187dee46c022
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009828"
---
# <a name="how-to-instantiate-jsonserializeroptions-instances-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 实例化 JsonSerializerOptions 实例

本文介绍如何避免在使用 <xref:System.Text.Json.JsonSerializerOptions> 时出现性能问题。 还介绍了如何使用可用的参数化构造函数。

## <a name="reuse-jsonserializeroptions-instances"></a>重用 JsonSerializerOptions 实例

如果你通过相同的选项重复使用 `JsonSerializerOptions`，则请勿在每次使用时都创建新的 `JsonSerializerOptions` 实例。 对每个调用重复使用同一实例。 本指南适用于你为自定义转换器编写的代码，以及调用 <xref:System.Text.Json.JsonSerializer.Serialize%2A?displayProperty=nameWithType> 或 <xref:System.Text.Json.JsonSerializer.Deserialize%2A?displayProperty=nameWithType>时。

以下代码演示使用新的选项实例所带来的性能损失。

:::code language="csharp" source="snippets/system-text-json-configure-options/csharp/ReuseOptionsInstances.cs":::

前面的代码使用相同的选项实例对一个小型对象进行了 100,000 次序列化。 然后，它对相同的对象进行了相同次数的序列化，且每次都创建一个新的选项实例。 典型运行时的差值为 190 毫秒，而不是 40,140 毫秒。 如果增加迭代的次数，差值甚至越大。

当向序列化程序传递新的选项实例时，在对象图中每种类型的第一次序列化期间，序列化程序将执行一个预热阶段。 此预热包括创建序列化所需的元数据的缓存。 元数据包括对属性 Getter 的委托、Setter、构造函数参数、指定特性等。 此元数据缓存存储在选项实例中。 相同的预热过程和缓存适用于反序列化。

`JsonSerializerOptions` 实例中元数据缓存的大小取决于要序列化的类型的数量。 如果向序列化程序传递多种类型（例如动态生成的类型），缓存大小将继续增长，最终可导致 `OutOfMemoryException`。

## <a name="copy-jsonserializeroptions"></a>复制 JsonSerializerOptions

::: zone pivot="dotnet-5-0"
存在 [JsonSerializerOptions constructor](xref:System.Text.Json.JsonSerializerOptions.%23ctor(System.Text.Json.JsonSerializerOptions)) ，可让你使用与现有实例相同的选项来创建新的实例，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/CopyOptions.cs" highlight="29":::
::: zone-end

::: zone pivot="dotnet-core-3-1"
使用现有实例的 `JsonSerializerOptions` 构造函数在 .NET Core 3.1 中不可用。
::: zone-end

## <a name="web-defaults-for-jsonserializeroptions"></a>JsonSerializerOptions 的 Web 默认值

::: zone pivot="dotnet-5-0"
下面是具有 Web 应用不同默认值的选项：

* <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive%2A> = `true`
* <xref:System.Text.Json.JsonNamingPolicy> = <xref:System.Text.Json.JsonNamingPolicy.CamelCase>
* <xref:System.Text.Json.JsonSerializerOptions.NumberHandling%2A> = <xref:System.Text.Json.Serialization.JsonNumberHandling.AllowReadingFromString>

存在 [JsonSerializerOptions constructor](xref:System.Text.Json.JsonSerializerOptions.%23ctor(System.Text.Json.JsonSerializerDefaults)?view=net-5.0&preserve-view=true)，可让你通过 ASP.NET Core 对 Web 应用使用的默认选项创建新的实例，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/OptionsDefaults.cs" highlight="24":::
::: zone-end

::: zone pivot="dotnet-core-3-1"
下面是具有 Web 应用不同默认值的选项：

* <xref:System.Text.Json.JsonSerializerOptions.PropertyNameCaseInsensitive%2A> = `true`
* <xref:System.Text.Json.JsonNamingPolicy> = <xref:System.Text.Json.JsonNamingPolicy.CamelCase>

指定一组默认值的 `JsonSerializerOptions` 构造函数在 .NET Core 3.1 中不可用。
::: zone-end

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何对 JSON 进行序列化和反序列化](system-text-json-how-to.md)
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
