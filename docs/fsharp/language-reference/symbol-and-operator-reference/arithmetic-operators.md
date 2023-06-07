---
title: 算术运算符
description: 了解F#编程语言中可用的算术运算符。
ms.date: 04/04/2018
ms.openlocfilehash: b783a0134541f11f06dde83af97676699b797da1
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630779"
---
# <a name="arithmetic-operators"></a>算术运算符

本主题介绍F#语言中可用的算术运算符。

## <a name="summary-of-binary-arithmetic-operators"></a>二进制算术运算符摘要

下表总结了可用于取消装箱整数和浮点类型的二进制算术运算符。

|二元运算符|说明|
|---------------|-----|
|`+`(加法, plus)|无. 在将数字相加并且总和超出了类型所支持的最大绝对值时, 可能会出现溢出情况。|
|`-`(减法, 减)|无. 当提取无符号类型或浮点值太小而不能由类型表示时, 可能出现下溢情况。|
|`*`(乘, 次)|无. 当数字相乘并且产品超出了类型所支持的最大绝对值时, 可能会出现溢出情况。|
|`/`(除以)|被零除会导致<xref:System.DivideByZeroException>整数类型。 对于浮点类型, 被零除会提供特殊的浮点值`+Infinity`或。 `-Infinity` 当浮点数太小而无法由类型表示时, 还可能出现下溢情况。|
|`%`(余数, rem)|返回除法运算的余数。 结果的符号与第一个操作数的符号相同。|
|`**`(求幂)|当结果超出了该类型的最大绝对值时, 可能会出现溢出情况。<br /><br />幂运算符仅适用于浮点类型。|

## <a name="summary-of-unary-arithmetic-operators"></a>一元算术运算符摘要

下表总结了可用于整型和浮点型的一元算术运算符。

|一元运算符|说明|
|--------------|-----|
|`+`测量|可应用于任何算术表达式。 不会更改值的符号。|
|`-`(求反, 负值)|可应用于任何算术表达式。 更改值的符号。|

整数类型的溢出或下溢的行为是环绕。 这会导致不正确的结果。 整数溢出是可能严重的问题, 如果未编写软件的安全问题, 则可能导致安全问题。 如果这对你的应用程序是一个问题, 请考虑使用中`Microsoft.FSharp.Core.Operators.Checked`的选中运算符。

## <a name="summary-of-binary-comparison-operators"></a>二进制比较运算符摘要

下表显示了可用于整型和浮点型的二进制比较运算符。 这些运算符返回类型`bool`为的值。

永远不应直接比较浮点数是否相等, 因为 IEEE 浮点表示形式不支持完全相等运算。 通过检查代码, 可以轻松验证的两个数字实际上可能具有不同的位表示形式。

|运算符|说明|
|--------|-----|
|`=`(相等, 等于)|这不是赋值运算符。 它仅用于比较。 这是一个泛型运算符。|
|`>`(大于)|这是一个泛型运算符。|
|`<`(小于)|这是一个泛型运算符。|
|`>=`(大于或等于)|这是一个泛型运算符。|
|`<=`(小于或等于)|这是一个泛型运算符。|
|`<>`(不等于)|这是一个泛型运算符。|

## <a name="overloaded-and-generic-operators"></a>重载的和泛型运算符

本主题中讨论的所有运算符都在**fsharp.core**命名空间中定义。 一些运算符使用静态解析的类型参数进行定义。 这意味着每个与该运算符一起使用的特定类型都有单独的定义。 所有一元和二元运算和位运算符都在此类别中。 比较运算符是泛型运算符, 因此适用于任何类型, 而不仅仅适用于基元算术类型。 可区分的联合和记录类型具有其自己的由F#编译器生成的自定义实现。 类类型使用方法<xref:System.Object.Equals%2A>。

泛型运算符可自定义。 若要自定义比较函数, <xref:System.Object.Equals%2A>请重写以提供您自己的自定义相等<xref:System.IComparable>比较, 然后实现。 接口有一个方法<xref:System.IComparable.CompareTo%2A> , 即方法。 <xref:System.IComparable?displayProperty=nameWithType>

## <a name="operators-and-type-inference"></a>运算符和类型推理

在表达式中使用运算符会限制对该运算符的类型推理。 而且, 使用运算符会阻止自动通用化, 因为使用运算符意味着算术类型。 如果没有任何其他信息, 编译器将F#推断`int`为算术表达式的类型。 您可以通过指定其他类型来重写此行为。 因此`function1` `int` ,以下`float`代码中的参数类型和返回类型将推断为, 但将的类型推断为。`function2`

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet3501.fs)]

## <a name="see-also"></a>请参阅

- [符号和运算符参考](index.md)
- [运算符重载](../operator-overloading.md)
- [位运算符](bitwise-operators.md)
- [布尔运算符](boolean-operators.md)
