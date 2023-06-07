---
title: .NET 中的数字
ms.date: 10/18/2018
helpviewer_keywords:
- SIMD
- System.Numerics.Vectors
- vectors
- scientific computing
- Complex
- numerics
- BigInteger
ms.assetid: dfebc18e-acde-4510-9fa7-9a0f4aa3bd11
ms.openlocfilehash: f674f05e864e11c83bb2e046ed54b91afebf167e
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94831138"
---
# <a name="numerics-in-net"></a>.NET 中的数字

.NET 提供一系列数值整型和浮点型基元、<xref:System.Numerics.BigInteger?displayProperty=nameWithType>（没有理论上限或下限的整型类型）、<xref:System.Numerics.Complex?displayProperty=nameWithType>（表示复杂数字）和 <xref:System.Numerics> 命名空间中一组启用了 SIMD 的类型。
  
## <a name="integer-types"></a>整数类型

.NET 支持带有符号和无符号的 8 位、16 位、32 位和 64 位整数类型，如下表所示：
  
|类型|有符号/无符号|大小（以字节为单位）|最小值|最大值|  
|----------|----------------------|--------------------|-------------------|-------------------|  
|<xref:System.Byte?displayProperty=nameWithType>|无符号|1|0|255|  
|<xref:System.Int16?displayProperty=nameWithType>|有符号|2|-32,768|32,767|  
|<xref:System.Int32?displayProperty=nameWithType>|有符号|4|-2,147,483,648|2,147,483,647|  
|<xref:System.Int64?displayProperty=nameWithType>|有符号|8|-9,223,372,036,854,775,808|9,223,372,036,854,775,807|  
|<xref:System.SByte?displayProperty=nameWithType>|有符号|1|-128|127|  
|<xref:System.UInt16?displayProperty=nameWithType>|无符号|2|0|65,535|  
|<xref:System.UInt32?displayProperty=nameWithType>|无符号|4|0|4,294,967,295|  
|<xref:System.UInt64?displayProperty=nameWithType>|无符号|8|0|18,446,744,073,709,551,615|  
  
每个整数类型都支持一组标准算术运算符。 <xref:System.Math?displayProperty=nameWithType> 类为更广泛的数学函数集提供方法。

还可以使用 <xref:System.BitConverter?displayProperty=nameWithType> 类对整数值中的单个位进行运算。  

> [!NOTE]  
> 无符号整数类型不符合 CLS。 有关详细信息，请参阅 “[语言独立性和与语言无关的组件](language-independence-and-language-independent-components.md)”。

## <a name="biginteger"></a>BigInteger

<xref:System.Numerics.BigInteger?displayProperty=nameWithType> 结构是不可变类型，表示其值没有理论上限或下限的任意大型整数。 <xref:System.Numerics.BigInteger> 类型的方法几乎与其他整数类型的方法一致。
  
## <a name="floating-point-types"></a>浮点类型

.NET 包括三个基元浮点类型，如下表所列：
  
|类型|大小（以字节为单位）|大致范围|精度|  
|----------|--------|---------------------|--------------------|  
|<xref:System.Single?displayProperty=nameWithType>|4|±1.5 x 10<sup>−45</sup> 至 ±3.4 x 10<sup>38</sup>|大约 6-9 位数字|  
|<xref:System.Double?displayProperty=nameWithType>|8|±5.0 × 10<sup>−324</sup> 到 ±1.7 × 10<sup>308</sup>|大约 15-17 位数字|  
|<xref:System.Decimal?displayProperty=nameWithType>|16|±1.0 x 10<sup>-28</sup> 至 ±7.9228 x 10<sup>28</sup>|28-29 位|  
  
<xref:System.Single> 和 <xref:System.Double> 类型都支持表示非数字和无穷大的特殊值。 例如，<xref:System.Double> 类型提供以下值：<xref:System.Double.NaN?displayProperty=nameWithType>、<xref:System.Double.NegativeInfinity?displayProperty=nameWithType> 和 <xref:System.Double.PositiveInfinity?displayProperty=nameWithType>。 可以使用 <xref:System.Double.IsNaN%2A?displayProperty=nameWithType>、<xref:System.Double.IsInfinity%2A?displayProperty=nameWithType>、<xref:System.Double.IsPositiveInfinity%2A?displayProperty=nameWithType> 和 <xref:System.Double.IsNegativeInfinity%2A?displayProperty=nameWithType> 方法来测试这些特殊值。

每个浮点类型都支持一组标准的算术运算符。 <xref:System.Math?displayProperty=nameWithType> 类为更广泛的数学函数集提供方法。 .NET Core 2.0 及更高版本包含 <xref:System.MathF?displayProperty=nameWithType> 类，该类提供接受 <xref:System.Single> 类型的参数的方法。

还可以使用 <xref:System.BitConverter?displayProperty=nameWithType> 类对 <xref:System.Double> 和 <xref:System.Single> 值中的单个位进行运算。 <xref:System.Decimal?displayProperty=nameWithType> 结构具有自己处理十进制值单个位的方法（<xref:System.Decimal.GetBits%2A?displayProperty=nameWithType> 和 <xref:System.Decimal.%23ctor%28System.Int32%5B%5D%29>）以及一套执行其他数学运算的方法。
  
<xref:System.Double> 和 <xref:System.Single> 类型旨在用于本质上不精确的值（例如，两颗行星之间的距离）和无需高度精确和舍入误差小的应用程序。 在需要较高准确度和尽量减小舍入误差的情况下，使用 <xref:System.Decimal?displayProperty=nameWithType> 类型。

> [!NOTE]
> <xref:System.Decimal> 类型不会消除对舍入的要求。 相反，它最大限度地减少了因舍入而导致的错误。
  
## <a name="complex"></a>Complex

<xref:System.Numerics.Complex?displayProperty=nameWithType> 结构表示复数，即带实数部分和虚数部分的数字。 此类型支持一套标准的算术、比较、相等、显式和隐式转换运算符，以及数学、代数和三角方法。  
  
## <a name="simd-enabled-types"></a>启用了 SIMD 的类型

<xref:System.Numerics> 命名空间包含一组启用了 .NET SIMD 的类型。 SIMD (Single Instruction Multiple Data) 操作可以在硬件级别并行化。 这可以增加向量化计算的吞吐量，这在数学、科学和图形应用中很常见。
  
启用了 .NET SIMD 的类型如下：

- <xref:System.Numerics.Vector2>、<xref:System.Numerics.Vector3> 和 <xref:System.Numerics.Vector4> 类型，用于表示具有 2、3 和 4 <xref:System.Single> 值的向量。

- 两个矩阵类型：<xref:System.Numerics.Matrix3x2>（表示 3x2 矩阵）和 <xref:System.Numerics.Matrix4x4>（表示 4x4 矩阵）。

- <xref:System.Numerics.Plane> 类型，表示三维空间中的一个平面。

- <xref:System.Numerics.Quaternion> 类型，表示一个用于对三维物理旋转进行编码的向量。

- <xref:System.Numerics.Vector%601> 类型，表示指定数字类型的向量，并提供受益于 SIMD 支持的一组广泛的运算符。 <xref:System.Numerics.Vector%601> 实例的计数是固定的，但其值 <xref:System.Numerics.Vector%601.Count%2A?displayProperty=nameWithType> 取决于执行代码的计算机的 CPU。

  > [!NOTE]
  > <xref:System.Numerics.Vector%601> 类型随 .NET Core 和 .NET 5+ 一起提供，但 .NET Framework 中不提供。 如果你使用的是 .NET Framework，请安装 [System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors) NuGet 包来访问此类型。
  
启用了 SIMD 的类型以这样一种方式实现：即它们可以与未启用 SIMD 的硬件或 JIT 编译器一起使用。 要利用 SIMD 指令，你的 64 位应用必须由使用 RyuJIT 编译器的运行时运行，该编译器包含在 .NET Core 和 .NET Framework 4.6 及更高版本中。 它针对 64 位处理器增加了 SIMD 支持。

有关详细信息，请参阅[使用 SIMD 加速数值类型](simd.md)。

## <a name="see-also"></a>请参阅

- [标准数字格式字符串](base-types/standard-numeric-format-strings.md)
