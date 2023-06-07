---
title: 如何使用 System.Text.Json 序列化派生类的属性
description: 了解在 .NET 中序列化为 JSON 和从 JSON 进行反序列化时如何序列化多态对象。
ms.date: 11/30/2020
no-loc:
- System.Text.Json
- Newtonsoft.Json
helpviewer_keywords:
- JSON serialization
- serializing objects
- serialization
- objects, serializing
ms.openlocfilehash: c0bc16c60d3bf96a380bc29bbf7f4765f752b320
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97008742"
---
# <a name="how-to-serialize-properties-of-derived-classes-with-no-locsystemtextjson"></a>如何使用 System.Text.Json 序列化派生类的属性

本文将介绍如何使用 `System.Text.Json` 命名空间序列化派生类的属性。

## <a name="serialize-properties-of-derived-classes"></a>序列化派生类的属性

不支持多态类型层次结构的序列化。 例如，如果属性定义为接口或抽象类，则即使运行时类型具有其他属性，也只会序列化对接口或抽象类定义的属性。 此部分中介绍了此行为的例外情况。

例如，假设有一个 `WeatherForecast` 类和一个派生类 `WeatherForecastDerived`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WF":::

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFDerived":::

并且假设 `Serialize` 方法的类型参数在编译时为 `WeatherForecast`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs" id="SerializeDefault":::

在这种情况下，即使 `weatherForecast` 对象实际上是 `WeatherForecastDerived` 对象，也不会序列化 `WindSpeed` 属性。 仅序列化基类属性：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

此行为旨在帮助防止在运行时创建的派生类型中发生意外数据泄露。

若要序列化前面示例中派生类型的属性，请使用以下方法之一：

* 调用 <xref:System.Text.Json.JsonSerializer.Serialize%2A> 的重载，以便在运行时指定类型：

  :::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs" id="SerializeGetType":::

* 将要序列化的对象声明为 `object`。

  :::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs" id="SerializeObject":::

在前面的示例方案中，这两种方法都会使 `WindSpeed` 属性包含在 JSON 输出中：

```json
{
  "WindSpeed": 35,
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot"
}
```

> [!IMPORTANT]
> 这些方法只为要序列化的根对象提供多态序列化，而不为该根对象的属性提供。

如果将较低级别的对象定义为类型 `object`，则可以对它们进行多态序列化。 例如，假设 `WeatherForecast` 类具有一个名为 `PreviousForecast` 的属性，该属性可以定义为类型 `WeatherForecast` 或 `object`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithPrevious":::

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/WeatherForecast.cs" id="WFWithPreviousAsObject":::

如果 `PreviousForecast` 属性包含 `WeatherForecastDerived` 的实例：

* 序列化 `WeatherForecastWithPrevious` 的 JSON 输出不包含 `WindSpeed`。
* 序列化 `WeatherForecastWithPreviousAsObject` 的 JSON 输出包含 `WindSpeed`。

若要序列化 `WeatherForecastWithPreviousAsObject`，无需调用 `Serialize<object>` 或 `GetType`，因为根对象不是可能属于派生类型的对象。 下面的代码示例不调用 `Serialize<object>` 或 `GetType`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs" id="SerializeSecondLevel":::

前面的代码会正确地序列化 `WeatherForecastWithPreviousAsObject`：

```json
{
  "Date": "2019-08-01T00:00:00-07:00",
  "TemperatureCelsius": 25,
  "Summary": "Hot",
  "PreviousForecast": {
    "WindSpeed": 35,
    "Date": "2019-08-01T00:00:00-07:00",
    "TemperatureCelsius": 25,
    "Summary": "Hot"
  }
}
```

将属性定义为 `object` 的相同方法适用于接口。 假设具有以下接口和实现，并且要使用包含实现实例的属性序列化类：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/IForecast.cs":::

序列化 `Forecasts` 的实例时，只有 `Tuesday` 显示 `WindSpeed` 属性，因为 `Tuesday` 定义为 `object`：

:::code language="csharp" source="snippets/system-text-json-how-to/csharp/SerializePolymorphic.cs" id="SerializeInterface":::

下面的示例显示前面代码生成的 JSON：

```json
{
  "Monday": {
    "Date": "2020-01-06T00:00:00-08:00",
    "TemperatureCelsius": 10,
    "Summary": "Cool"
  },
  "Tuesday": {
    "Date": "2020-01-07T00:00:00-08:00",
    "TemperatureCelsius": 11,
    "Summary": "Rainy",
    "WindSpeed": 10
  }
}
```

有关多态序列化的详细信息，以及有关反序列化的信息，请参阅[如何从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md#polymorphic-serialization)。

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
* [不可变类型和非公共访问器](system-text-json-immutability.md)
* [从 Newtonsoft.Json 迁移到 System.Text.Json](system-text-json-migrate-from-newtonsoft-how-to.md)
* [自定义字符编码](system-text-json-character-encoding.md)
* [编写自定义序列化程序和反序列化程序](write-custom-serializer-deserializer.md)
* [编写用于 JSON 序列化的自定义转换器](system-text-json-converters-how-to.md)
* [DateTime 和 DateTimeOffset 支持](../datetime/system-text-json-support.md)
* [System.Text.Json API 参考](xref:System.Text.Json)
* [System.Text.Json.Serialization API 参考](xref:System.Text.Json.Serialization)
