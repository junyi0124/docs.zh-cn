---
title: 基本类型
description: '了解 F # 语言中使用的基本基本类型。'
ms.date: 08/15/2020
ms.openlocfilehash: 659ac8424c62985affcca1741e1b2a74c9c3ee8d
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557693"
---
# <a name="basic-types"></a>基本类型

本主题列出了在 F # 语言中定义的基本类型。 这些类型在 F # 中是最基本的，构成了几乎每个 F # 程序的基础。 它们是 .NET 基元类型的超集。

|类型|.NET 类型|说明|示例|
|----|---------|-----------|-------|
|`bool`|<xref:System.Boolean>|可能值为 `true` 和 `false`。|`true`/`false`|
|`byte`|<xref:System.Byte>|介于0到255之间的值。|`1uy`|
|`sbyte`|<xref:System.SByte>|值从-128 到127。|`1y`|
|`int16`|<xref:System.Int16>|值从-32768 到32767。|`1s`|
|`uint16`|<xref:System.UInt16>|介于0到65535之间的值。|`1us`|
|`int`|<xref:System.Int32>|值从-2147483648 到2147483647。|`1`|
|`uint`|<xref:System.UInt32>|介于0到4294967295之间的值。|`1u`|
|`int64`|<xref:System.Int64>|值介于-9223372036854775808 到9223372036854775807之间。|`1L`|
|`uint64`|<xref:System.UInt64>|介于0到18446744073709551615之间的值。|`1UL`|
|`nativeint`|<xref:System.IntPtr>|作为带符号整数的本机指针。|`nativeint 1`|
|`unativeint`|<xref:System.UIntPtr>|作为无符号整数的本机指针。|`unativeint 1`|
|`decimal`|<xref:System.Decimal>|至少具有28个有效数字的浮点数据类型。|`1.0`|
|`float`, `double`|<xref:System.Double>|64位浮点类型。|`1.0`|
|`float32`, `single`|<xref:System.Single>|32位浮点类型。|`1.0f`|
|`char`|<xref:System.Char>|Unicode 字符值。|`'c'`|
|`string`|<xref:System.String>|Unicode 文本。|`"str"`|
|`unit`|不适用|指示缺少实际值。 该类型仅有一个表示的形式值 `()` 。 Unit 值通常用作 `()` 占位符，其中需要值，但没有实际值可用或有意义。|`()`|

> [!NOTE]
> 可以使用 [bigint](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-bigint.html) 类型对64位整数类型的整数进行太大的计算。 `bigint` 不被视为基本类型;它是的缩写 `System.Numerics.BigInteger` 。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
