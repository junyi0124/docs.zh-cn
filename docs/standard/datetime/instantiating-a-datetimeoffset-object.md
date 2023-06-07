---
title: 实例化 DateTimeOffset 对象
description: 阅读如何实例化 (使实例在 .NET 中) DateTimeOffset 对象。 了解 & 时间文本、构造函数、隐式类型转换 & 详细信息。
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- instantiating time zone objects
- time zone objects [.NET], instantiation
- DateTimeOffset structure, converting to DateTime
- DateTimeOffset structure, instantiating
ms.assetid: 9648375f-d368-4373-a976-3332ece00c0a
ms.openlocfilehash: b0eae79e9c39694bdc87622a4c442083791c6109
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817804"
---
# <a name="instantiating-a-datetimeoffset-object"></a>实例化 DateTimeOffset 对象

<xref:System.DateTimeOffset> 结构提供了多种创建新 <xref:System.DateTimeOffset> 值的方法。 其中的许多方法都直接对应于可用于实例化新值的方法 <xref:System.DateTime> ，增强功能使你能够指定日期和时间值与协调世界时 (UTC) 的偏移量。 具体而言，可以 <xref:System.DateTimeOffset> 通过以下方式实例化某个值：

- 使用日期和时间文本。

- 通过调用 <xref:System.DateTimeOffset> 构造函数。

- 通过将值隐式转换为 <xref:System.DateTimeOffset> 值。

- 分析日期和时间的字符串表示形式。

本主题提供了更详细的详细信息和代码示例，这些示例演示了实例化新值的方法 <xref:System.DateTimeOffset> 。

## <a name="date-and-time-literals"></a>日期和时间文本

对于支持它的语言，实例化值最常用的方法之一 <xref:System.DateTime> 是将日期和时间作为硬编码文本值提供。 例如，下面的 Visual Basic 代码创建一个对象，该 <xref:System.DateTime> 对象的值为2008年1月1日上午 10:00 AM。

[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#1)]

<xref:System.DateTimeOffset> 使用支持文本的语言时，还可以使用日期和时间文本初始化值 <xref:System.DateTime> 。 例如，下面的 Visual Basic 代码创建 <xref:System.DateTimeOffset> 对象。

[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#2)]

如控制台输出所示， <xref:System.DateTimeOffset> 以这种方式创建的值分配了本地时区的偏移量。 这意味着， <xref:System.DateTimeOffset> 如果代码在不同的计算机上运行，则使用字符文本分配的值不会识别单个时间点。

## <a name="datetimeoffset-constructors"></a>DateTimeOffset 构造函数

<xref:System.DateTimeOffset>类型定义六个构造函数。 其中四个直接对应于 <xref:System.DateTime> 构造函数，另外还有一个类型为的参数，用于 <xref:System.TimeSpan> 定义日期和时间与 UTC 的偏移量。 这允许您 <xref:System.DateTimeOffset> 根据其各自的日期和时间组成部分的值来定义一个值。 例如，下面的代码使用这四个构造函数来实例化 <xref:System.DateTimeOffset> 值为 7/1/2008 12:05 AM + 01:00 的对象。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#3)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#3)]

请注意，当 <xref:System.DateTimeOffset> 使用 <xref:System.Globalization.PersianCalendar> 对象作为其构造函数的其中一个参数进行实例化的对象的值显示到控制台时，它将表示为公历中的日期，而不是波斯历。 若要使用波斯历输出日期，请参阅主题中的示例 <xref:System.Globalization.PersianCalendar> 。

其他两个构造函数 <xref:System.DateTimeOffset> 从值创建对象 <xref:System.DateTime> 。 其中第一个参数具有一个参数，即 <xref:System.DateTime> 要转换为值的值 <xref:System.DateTimeOffset> 。 所得值的偏移量 <xref:System.DateTimeOffset> 取决于 <xref:System.DateTime.Kind%2A> 构造函数的单个参数的属性。 如果其值为 <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> ，则将偏移量设置为等于 <xref:System.TimeSpan.Zero?displayProperty=nameWithType> 。 否则，会将其时差设置为等于本地时区的时差。 下面的示例演示如何使用此构造函数实例化 <xref:System.DateTimeOffset> 表示 UTC 和本地时区的对象：

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#4)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#4)]

> [!NOTE]
> 调用 <xref:System.DateTimeOffset> 具有单个参数的构造函数的重载 <xref:System.DateTime> 等效于执行到值的隐式转换 <xref:System.DateTime> <xref:System.DateTimeOffset> 。

从值创建对象的第二个构造函数 <xref:System.DateTimeOffset> <xref:System.DateTime> 具有两个参数： <xref:System.DateTime> 要转换的值，以及 <xref:System.TimeSpan> 表示日期和时间与 UTC 的偏移量的值。 此偏移值必须对应于 <xref:System.DateTime.Kind%2A> 构造函数的第一个参数的属性，否则 <xref:System.ArgumentException> 将引发。 如果 <xref:System.DateTime.Kind%2A> 第一个参数的属性为 <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> ，则第二个参数的值必须是 <xref:System.TimeSpan.Zero?displayProperty=nameWithType> 。 如果 <xref:System.DateTime.Kind%2A> 第一个参数的属性为 <xref:System.DateTimeKind.Local?displayProperty=nameWithType> ，则第二个参数的值必须为本地系统时区的偏移量。 如果 <xref:System.DateTime.Kind%2A> 第一个参数的属性为 <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> ，则偏移量可以是任何有效的值。 下面的代码演示了对此构造函数的调用，以将其转换 <xref:System.DateTime> 为 <xref:System.DateTimeOffset> 值。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#5)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#5)]

## <a name="implicit-type-conversion"></a>隐式类型转换

该 <xref:System.DateTimeOffset> 类型支持一种隐式类型转换：从 <xref:System.DateTime> 值到 <xref:System.DateTimeOffset> 值。 隐式类型转换是指从一种类型转换为另一种类型，而无需显示转换（通过 C# 或 Visual Basic）且不会丢失信息。 它可以实现如下代码。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#6)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#6)]

所得值的偏移量 <xref:System.DateTimeOffset> 取决于 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性值。 如果其值为 <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> ，则将偏移量设置为等于 <xref:System.TimeSpan.Zero?displayProperty=nameWithType> 。 如果其值为 <xref:System.DateTimeKind.Local?displayProperty=nameWithType> 或 <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> ，则将偏移量设置为等于本地时区的偏移量。

## <a name="parsing-the-string-representation-of-a-date-and-time"></a>分析日期和时间的字符串表示形式

<xref:System.DateTimeOffset>类型支持四种方法，使你可以将日期和时间的字符串表示形式转换为 <xref:System.DateTimeOffset> 值：

- <xref:System.DateTimeOffset.Parse%2A>，它尝试将日期和时间的字符串表示形式转换为 <xref:System.DateTimeOffset> 值，并在转换失败时引发异常。

- <xref:System.DateTimeOffset.TryParse%2A>，它尝试将日期和时间的字符串表示形式转换为 <xref:System.DateTimeOffset> 值， `false` 如果转换失败，则返回。

- <xref:System.DateTimeOffset.ParseExact%2A>，它尝试将指定格式的日期和时间的字符串表示形式转换为 <xref:System.DateTimeOffset> 值。 如果转换失败，该方法将引发异常。

- <xref:System.DateTimeOffset.TryParseExact%2A>，它尝试将指定格式的日期和时间的字符串表示形式转换为 <xref:System.DateTimeOffset> 值。 如果转换失败，该方法将返回 `false`。

下面的示例演示了对这四种字符串转换方法中的每一种的调用，以实例化某个 <xref:System.DateTimeOffset> 值。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#7)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#7)]

## <a name="see-also"></a>另请参阅

- [日期、时间和时区](index.md)
