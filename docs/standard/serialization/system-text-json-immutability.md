---
title: 如何通过 System.Text.Json 使用不可变类型和非公共访问器
description: 了解在 .NET 中序列化为 JSON 和从 JSON 进行反序列化时如何使用不可变类型和非公共访问器。
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
ms.openlocfilehash: ff8ecec0d70c877b7cbbd0297b85f0d9578ab828
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008820"
---
# <a name="how-to-use-immutable-types-and-non-public-accessors-with-no-locsystemtextjson"></a>如何通过 System.Text.Json 使用不可变类型和非公共访问器

本文将介绍如何通过 `System.Text.Json` 命名空间使用不可变类型（如“记录”）。

## <a name="immutable-types-and-records"></a>不可变类型和记录

::: zone pivot="dotnet-5-0"
`System.Text.Json` 可以使用参数化构造函数，这可以反序列化不可变的类或结构。 对于类，如果唯一构造函数是参数化的构造函数，则将使用该构造函数。 对于结构或包含多个构造函数的类，通过应用 [[JsonConstructor]](xref:System.Text.Json.Serialization.JsonConstructorAttribute.%23ctor%2A) 特性来指定要使用的构造函数。 如果未使用该特性，则始终使用公共无参数构造函数（如果存在）。 该特性只能与公共构造函数一起使用。 以下示例使用 `[JsonConstructor]` 特性：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/ImmutableTypes.cs" highlight="13":::

还支持 C# 9 记录，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/Records.cs":::

对于因其所有属性资源库都是非公共的而不可变的类型，请参阅以下部分，了解[非公共属性访问器](#non-public-property-accessors)。
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 不支持 `JsonConstructorAttribute` 和 C# 9 记录。
::: zone-end

## <a name="non-public-property-accessors"></a>非公共属性访问器

::: zone pivot="dotnet-5-0"
若要允许使用非公共属性访问器，请使用 [[JsonInclude]](xref:System.Text.Json.Serialization.JsonIncludeAttribute) 特性，如以下示例中所示：

:::code language="csharp" source="snippets/system-text-json-how-to-5-0/csharp/NonPublicAccessors.cs" highlight="12,15":::
::: zone-end

::: zone pivot="dotnet-core-3-1"
.NET Core 3.1 不支持非公共属性访问器。 有关详细信息，请参阅[从 Newtonsoft.Json 迁移一文](system-text-json-migrate-from-newtonsoft-how-to.md#non-public-property-setters-and-getters)。
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
* [保留引用](system-text-json-preserve-references.md)
* [多态序列化](system-text-json-polymorphism.md)
* [从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [自定义字符编码](system-text-json-character-encoding.md)
* [编写自定义序列化程序和反序列化程序](write-custom-serializer-deserializer.md)
* [编写用于 JSON 序列化的自定义转换器](system-text-json-converters-how-to.md)
* [DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
* [System.Text.Json.Serialization API 参考](xref:System.Text.Json.Serialization)
