---
title: 如何使用 System.Text.Json 保留引用
description: 了解在 .NET 中序列化为 JSON 和从 JSON 进行反序列化时如何保留引用并处理循环引用。
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
ms.openlocfilehash: d358c953c0979ca097c080fcd750d5ef95b07de0
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008729"
---
# <a name="how-to-preserve-references-and-handle-circular-references-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 保留引用并处理循环引用

::: zone pivot="dotnet-5-0"

若要保留引用并处理循环引用，请将 <xref:System.Text.Json.JsonSerializerOptions.ReferenceHandler%2A> 设置为 <xref:System.Text.Json.Serialization.ReferenceHandler.Preserve%2A>。 此设置会导致以下行为：

* 在序列化时：

  编写复杂类型时，序列化程序还会写入元数据属性（`$id`、`$values` 和 `$ref`）。

* 在反序列化时：

  需要元数据（虽然不是必需的），并且反序列化程序会尝试理解它。

下面的代码演示 `Preserve` 属性的用法。

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/PreserveReferences.cs" highlight="34":::

此功能不能用于保留值类型或不可变类型。 在反序列化时，将在读取整个有效负载后创建不可变类型的实例。 因此，如果对同一实例的引用出现在 JSON 有效负载中，则无法对其进行反序列化。

对于值类型、不可变类型和数组，不会序列化任何引用元数据。 反序列化时，如果发现 `$ref` 或 `$id`，则会引发异常。 但是，值类型忽略 `$id`（对于集合，则为 `$values`），以便可以反序列化使用 Newtonsoft.Json 序列化的有效负载。  Newtonsoft.Json 为此类类型序列化元数据。

为了确定对象是否相等，System.Text.Json 在比较两个对象实例时使用引用相等性 (<xref:System.Object.ReferenceEquals(System.Object,System.Object)?displayProperty=nameWithType>) 而不是值相等性 (<xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> 的 <xref:System.Collections.Generic.ReferenceEqualityComparer.Instance%2A?displayProperty=nameWithType>。

有关如何序列化和反序列化引用的详细信息，请参阅 <xref:System.Text.Json.Serialization.ReferenceHandler.Preserve%2A?displayProperty=nameWithType>。

<xref:System.Text.Json.Serialization.ReferenceResolver> 类定义在序列化和反序列化过程中保留引用的行为。 创建派生类以指定自定义行为。 有关示例，请参阅 [GuidReferenceResolver](https://github.com/dotnet/docs/blob/9d5e88edbd7f12be463775ffebbf07ac8415fe18/docs/standard/serialization/snippets/system-text-json-how-to-5-0/csharp/GuidReferenceResolverExample.cs)。

::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 中的 System.Text.Json 仅支持按值进行进行序列化，并对循环引用引发异常。
::: zone-end

## <a name="see-also"></a>请参阅

* [System.Text.Json 概述](system-text-json-overview.md)
* [如何对 JSON 进行序列化和反序列化](system-text-json-how-to.md)
* [对 JsonSerializerOptions 实例进行实例化](system-text-json-configure-options.md)
* [启用不区分大小写的匹配](system-text-json-character-casing.md)
* [自定义属性名称和值](system-text-json-customize-properties.md)
* [忽略属性](system-text-json-ignore-properties.md)
* [允许无效的 JSON](system-text-json-invalid-json.md)
* [处理溢出 JSON](system-text-json-handle-overflow.md)
* [不可变类型和非公共访问器](system-text-json-immutability.md)
* [多态序列化](system-text-json-polymorphism.md)
* [从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [自定义字符编码](system-text-json-character-encoding.md)
* [编写自定义序列化程序和反序列化程序](write-custom-serializer-deserializer.md)
* [编写用于 JSON 序列化的自定义转换器](system-text-json-converters-how-to.md)
* [DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
* [System.Text.Json.Serialization API 参考](xref:System.Text.Json.Serialization)
