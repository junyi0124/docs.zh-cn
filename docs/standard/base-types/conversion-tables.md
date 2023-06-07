---
title: .NET 中的类型转换表
ms.date: 03/30/2017
helpviewer_keywords:
- widening conversions
- narrowing conversions
- type conversion, table
- converting types, narrowing conversions
- converting types, widening conversions
- base types, converting
- tables [.NET], type conversions
- data types [.NET], converting
ms.assetid: 0ea65c59-85eb-4a52-94ca-c36d3bd13058
ms.openlocfilehash: dc98a326155273805e3157d99755de2e97f83a46
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730209"
---
# <a name="type-conversion-tables-in-net"></a>.NET 中的类型转换表

当一种类型的值转换为大小相等或更大的另一类型时，将发生扩大转换。 当一种类型的值转换为较小的另一种类型时，将发生收缩转换。 本主题中的表格解释了这两种转换类型的行为。  
  
## <a name="widening-conversions"></a>扩大转换  

 下表列出了执行不会导致信息丢失的扩大转换。  
  
|类型|可在不丢失数据的情况下转换为|  
|----------|-------------------------------------------|  
|<xref:System.Byte>|<xref:System.UInt16>、<xref:System.Int16>、<xref:System.UInt32>、<xref:System.Int32>、<xref:System.UInt64>、<xref:System.Int64>、<xref:System.Single>、<xref:System.Double>、<xref:System.Decimal>|  
|<xref:System.SByte>|<xref:System.Int16>、 <xref:System.Int32>、 <xref:System.Int64>、 <xref:System.Single>、 <xref:System.Double>、 <xref:System.Decimal>|  
|<xref:System.Int16>|<xref:System.Int32>, <xref:System.Int64>, <xref:System.Single>, <xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.UInt16>|<xref:System.UInt32>, <xref:System.Int32>, <xref:System.UInt64>, <xref:System.Int64>, <xref:System.Single>, <xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.Char>|<xref:System.UInt16>, <xref:System.UInt32>, <xref:System.Int32>, <xref:System.UInt64>, <xref:System.Int64>, <xref:System.Single>, <xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.Int32>|<xref:System.Int64>, <xref:System.Double>, <xref:System.Decimal>|  
|<xref:System.UInt32>|<xref:System.Int64>、<xref:System.UInt64>、<xref:System.Double>、<xref:System.Decimal>|  
|<xref:System.Int64>|<xref:System.Decimal>|  
|<xref:System.UInt64>|<xref:System.Decimal>|  
|<xref:System.Single>|<xref:System.Double>|  
  
 一些目标为 <xref:System.Single> 或 <xref:System.Double> 的扩大转换可能会导致精度丢失。 下面的表格描述了有时会导致信息丢失的扩大转换。  
  
|类型|可转换为|  
|----------|-------------------------|  
|<xref:System.Int32>|<xref:System.Single>|  
|<xref:System.UInt32>|<xref:System.Single>|  
|<xref:System.Int64>|<xref:System.Single>， <xref:System.Double>|  
|<xref:System.UInt64>|<xref:System.Single>， <xref:System.Double>|  
|<xref:System.Decimal>|<xref:System.Single>， <xref:System.Double>|  
  
## <a name="narrowing-conversions"></a>收缩转换  

 目标为 <xref:System.Single> 或 <xref:System.Double> 的收缩转换可能会导致信息丢失。 如果目标类型无法正确表达源类型的大小，则结果类型将设置为常数 `PositiveInfinity` 或 `NegativeInfinity`。 `PositiveInfinity` 是正数除以 0 的结果，也在 <xref:System.Single> 或 <xref:System.Double> 的值大于 `MaxValue` 字段的值时返回。 `NegativeInfinity` 是负数除以 0 的结果，也在 <xref:System.Single> 或 <xref:System.Double> 的值小于 `MinValue` 字段的值时返回。 从 <xref:System.Double> 转换到 <xref:System.Single> 可能会导致 `PositiveInfinity` 或 `NegativeInfinity`。  
  
 收缩转换还可能导致其他数据类型的信息丢失。 不过，如果要转换的类型值不在目标类型的 `MaxValue` 和 `MinValue` 字段指定的范围内，就会抛出 <xref:System.OverflowException>，并且运行时会检查转换，以确保目标类型的值不超出它的 `MaxValue` 或 `MinValue`。 始终以这种方式检查使用 <xref:System.Convert?displayProperty=nameWithType> 类执行的转换。  
  
 下表列出了使用 <xref:System.Convert?displayProperty=nameWithType> 抛出 <xref:System.OverflowException> 的转换，或要转换类型的值不在生成类型的定义范围内的任何已检查转换。  
  
|类型|可转换为|  
|----------|-------------------------|  
|<xref:System.Byte>|<xref:System.SByte>|  
|<xref:System.SByte>|<xref:System.Byte>、<xref:System.UInt16>、<xref:System.UInt32>、<xref:System.UInt64>|  
|<xref:System.Int16>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.UInt16>|  
|<xref:System.UInt16>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>|  
|<xref:System.Int32>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>,<xref:System.UInt32>|  
|<xref:System.UInt32>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>|  
|<xref:System.Int64>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>,<xref:System.UInt32>,<xref:System.UInt64>|  
|<xref:System.UInt64>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>, <xref:System.UInt32>, <xref:System.Int64>|  
|<xref:System.Decimal>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>, <xref:System.UInt32>, <xref:System.Int64>, <xref:System.UInt64>|  
|<xref:System.Single>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>, <xref:System.UInt32>, <xref:System.Int64>, <xref:System.UInt64>|  
|<xref:System.Double>|<xref:System.Byte>, <xref:System.SByte>, <xref:System.Int16>, <xref:System.UInt16>, <xref:System.Int32>, <xref:System.UInt32>, <xref:System.Int64>, <xref:System.UInt64>|  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Convert?displayProperty=nameWithType>
- [.NET 中的类型转换](type-conversion.md)
