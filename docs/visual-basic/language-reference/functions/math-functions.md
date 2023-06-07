---
title: 数学函数
ms.date: 01/27/2020
helpviewer_keywords:
- math functions, Visual Basic
- arithmetic operations, math functions
- math routines
- Atn function
ms.assetid: 4d2d82e7-6924-42fe-a4a7-b4dd5bebbd0c
ms.openlocfilehash: ea30ae3b30484c1a13d6d540f121c03afb30ba26
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794599"
---
# <a name="math-functions-visual-basic"></a>数学函数 (Visual Basic)

<xref:System.Math?displayProperty=nameWithType> 类的方法提供三角函数、对数和其他常见数学函数。

## <a name="remarks"></a>备注

下表列出了 <xref:System.Math?displayProperty=nameWithType> 类的方法。 可以在 Visual Basic 程序中使用这些内容：
  
|.NET 方法|描述|
|---------------------------|-----------------|
|<xref:System.Math.Abs%2A>|返回数字的绝对值。|
|<xref:System.Math.Acos%2A>|返回余弦值为指定数字的角度。|
|<xref:System.Math.Asin%2A>|返回正弦值为指定数字的角度。|
|<xref:System.Math.Atan%2A>|返回正切值为指定数字的角度。|
|<xref:System.Math.Atan2%2A>|返回正切值为两个指定数字的商的角度。|
|<xref:System.Math.BigMul%2A>|返回 2 32 位数字的完整乘积。|
|<xref:System.Math.Ceiling%2A>|返回大于或等于指定 `Decimal` 或 `Double`的最小整数值。|
|<xref:System.Math.Cos%2A>|返回指定角度的余弦值。|
|<xref:System.Math.Cosh%2A>|返回指定角度的双曲余弦值。|
|<xref:System.Math.DivRem%2A>|返回 2 32 位或64位有符号整数的商，并在输出参数中返回余数。|
|<xref:System.Math.Exp%2A>|返回发出到指定幂的 e （自然对数的底）。|
|<xref:System.Math.Floor%2A>|返回小于或等于指定 `Decimal` 或 `Double` 号的最大整数。|
|<xref:System.Math.IEEERemainder%2A>|返回由指定数字相除所得的余数所得的余数。|
|<xref:System.Math.Log%2A>|返回指定数字的自然对数（以 e 为底）或指定数字的对数。|
|<xref:System.Math.Log10%2A>|返回指定数字以 10 为底的对数。|
|<xref:System.Math.Max%2A>|返回两个数字中较大的一个。|
|<xref:System.Math.Min%2A>|返回两个数字中较小的一个。|
|<xref:System.Math.Pow%2A>|返回指定数字的指定次幂。|
|<xref:System.Math.Round%2A>|返回舍入到最接近的整数值或指定的小数位数的 `Decimal` 或 `Double` 值。|
|<xref:System.Math.Sign%2A>|返回表示数字符号的 `Integer` 值。|
|<xref:System.Math.Sin%2A>|返回指定角度的正弦值。|
|<xref:System.Math.Sinh%2A>|返回指定角度的双曲正弦值。|
|<xref:System.Math.Sqrt%2A>|返回指定数字的平方根。|
|<xref:System.Math.Tan%2A>|返回指定角度的正切值。|
|<xref:System.Math.Tanh%2A>|返回指定角度的双曲正切值。|
|<xref:System.Math.Truncate%2A>|计算指定 `Decimal` 或 `Double` 数值的整数部分。|

下表列出了在 .NET Framework 中不存在但添加到 .NET Standard 或 .NET Core 中的 <xref:System.Math?displayProperty=nameWithType> 类的方法：

|.NET 方法|描述|可用项|
|---------------------------|-----------------|-----------|
|<xref:System.Math.Acosh%2A>|返回双曲余弦值为指定数字的角度。|从 .NET Core 2.1 开始，.NET Standard 2。1|
|<xref:System.Math.Asinh%2A>|返回双曲正弦值为指定数字的角度。|从 .NET Core 2.1 开始，.NET Standard 2。1|
|<xref:System.Math.Atanh%2A>|返回双曲正切值为指定数字的角度。|从 .NET Core 2.1 开始，.NET Standard 2。1|
|<xref:System.Math.BitDecrement%2A>|返回小于 `x` 的下一个最小值。|从 .NET Core 3.0 开始|
|<xref:System.Math.BitIncrement%2A>|返回大于 `x` 的下一个最大值。|从 .NET Core 3.0 开始|
|<xref:System.Math.Cbrt%2A>|返回指定数字的立方根。|从 .NET Core 2.1 开始，.NET Standard 2。1|
|<xref:System.Math.Clamp%2A>|返回限制在 `min` 和 `max` 范围内（含首尾）的 `value`。|从 .NET Core 2.0 开始，.NET Standard 2。1|
|<xref:System.Math.CopySign%2A>|返回一个值，它具有 `x` 的大小和 `y` 的符号。|从 .NET Core 3.0 开始|
|<xref:System.Math.FusedMultiplyAdd%2A>|返回（x \* y） + z，舍入为一个三元运算。|从 .NET Core 3.0 开始|
|<xref:System.Math.ILogB%2A>|返回指定数字以 2 为底的整数对数。|从 .NET Core 3.0 开始|
|<xref:System.Math.Log2%2A>|返回指定数字以 2 为底的对数。|从 .NET Core 3.0 开始|
|<xref:System.Math.MaxMagnitude%2A>|返回两个双精度浮点数字中的较大值。|从 .NET Core 3.0 开始|
|<xref:System.Math.MinMagnitude%2A>|返回两个双精度浮点数字中的较小值。|从 .NET Core 3.0 开始|
|<xref:System.Math.ScaleB%2A>|返回已有效计算 x \* 2 ^ n。|从 .NET Core 3.0 开始|

若要在不进行限定的情况下使用这些函数，请将以下代码添加到你的源文件顶部，将 <xref:System.Math?displayProperty=nameWithType> 命名空间导入到你的项目中：

```vb
Imports System.Math
```

## <a name="example---abs"></a>示例-Abs

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Abs%2A> 方法来计算数字的绝对值。

```vb
Dim x As Double = Math.Abs(50.3)
Dim y As Double = Math.Abs(-50.3)
Console.WriteLine(x)
Console.WriteLine(y)
' This example produces the following output:
' 50.3
' 50.3
```  

## <a name="example---atan"></a>示例-Atan

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Atan%2A> 方法计算 pi 的值。

```vb
Public Function GetPi() As Double
    ' Calculate the value of pi.
    Return 4.0 * Math.Atan(1.0)
End Function
```

> [!NOTE]
> <xref:System.Math?displayProperty=nameWithType> 类包含 <xref:System.Math.PI?displayProperty=nameWithType> 常量字段。 您可以使用它，而不是对其进行计算。

## <a name="example---cos"></a>示例-Cos

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Cos%2A> 方法返回角度的余弦值。

```vb
Public Function Sec(angle As Double) As Double
    ' Calculate the secant of angle, in radians.
    Return 1.0 / Math.Cos(angle)
End Function
```

## <a name="example---exp"></a>示例-Exp

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Exp%2A> 方法返回 e 的幂。

```vb
Public Function Sinh(angle As Double) As Double
    ' Calculate hyperbolic sine of an angle, in radians.
    Return (Math.Exp(angle) - Math.Exp(-angle)) / 2.0
End Function
```

## <a name="example---log"></a>示例-日志

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Log%2A> 方法返回某个数字的自然对数。

```vb
Public Function Asinh(value As Double) As Double
    ' Calculate inverse hyperbolic sine, in radians.
    Return Math.Log(value + Math.Sqrt(value * value + 1.0))
End Function
```

## <a name="example---round"></a>示例-圆形

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Round%2A> 方法将数字舍入到最接近的整数。

```vb
Dim myVar2 As Double = Math.Round(2.8)
Console.WriteLine(myVar2)
' The code produces the following output:
' 3
```

## <a name="example---sign"></a>示例-Sign

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Sign%2A> 方法来确定数字的符号。

```vb
Dim mySign1 As Integer = Math.Sign(12)
Dim mySign2 As Integer = Math.Sign(-2.4)
Dim mySign3 As Integer = Math.Sign(0)
Console.WriteLine(mySign1)
Console.WriteLine(mySign2)
Console.WriteLine(mySign3)
' The code produces the following output:
' 1
' -1
' 0
```

## <a name="example---sin"></a>示例-Sin

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Sin%2A> 方法返回角度的正弦值。

```vb
Public Function Csc(angle As Double) As Double
    ' Calculate cosecant of an angle, in radians.
    Return 1.0 / Math.Sin(angle)
End Function
```

## <a name="example---sqrt"></a>示例-Sqrt

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Sqrt%2A> 方法来计算数字的平方根。

```vb
Dim mySqrt1 As Double = Math.Sqrt(4)
Dim mySqrt2 As Double = Math.Sqrt(23)
Dim mySqrt3 As Double = Math.Sqrt(0)
Dim mySqrt4 As Double = Math.Sqrt(-4)
Console.WriteLine(mySqrt1)
Console.WriteLine(mySqrt2)
Console.WriteLine(mySqrt3)
Console.WriteLine(mySqrt4)
' The code produces the following output:
' 2
' 4.79583152331272
' 0
' NaN
```

## <a name="example---tan"></a>示例-Tan

此示例使用 <xref:System.Math> 类的 <xref:System.Math.Tan%2A> 方法返回角度的正切值。

```vb
Public Function Ctan(angle As Double) As Double
    ' Calculate cotangent of an angle, in radians.
    Return 1.0 / Math.Tan(angle)
End Function
```

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.VBMath.Rnd%2A>
- <xref:Microsoft.VisualBasic.VBMath.Randomize%2A>
- <xref:System.Double.NaN>
- [派生的数学函数](../keywords/derived-math-functions.md)
- [算术运算符](../operators/arithmetic-operators.md)
