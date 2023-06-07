---
title: 数据类型疑难解答
ms.date: 07/20/2015
helpviewer_keywords:
- Char data type [Visual Basic], converting
- Decimal data type [Visual Basic], conversions
- data types [Visual Basic], troubleshooting
- literals [Visual Basic], default types
- type characters [Visual Basic], literal
- Mod operator [Visual Basic], in floating-point operations
- troubleshooting Visual Basic, data types
- troubleshooting data types [Visual Basic]
- floating-point numbers [Visual Basic], precision
- Boolean data type [Visual Basic], converting
- literal types [Visual Basic]
- literal type characters [Visual Basic]
- floating-point numbers [Visual Basic], imprecision
- String data type [Visual Basic], converting
- floating-point numbers [Visual Basic], comparison
- floating-point numbers
ms.assetid: 90040d67-b630-4125-a6ae-37195b079042
ms.openlocfilehash: 239e1c2f908a9023aeca6e92aff4633b60f27b69
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393398"
---
# <a name="troubleshooting-data-types-visual-basic"></a>数据类型疑难解答 (Visual Basic)

此页列出了对内部数据类型执行操作时可能出现的一些常见问题。

## <a name="floating-point-expressions-do-not-compare-as-equal"></a>浮点表达式的比较结果不相等

使用浮点数（[单数据类型](../../../language-reference/data-types/single-data-type.md)和[双精度数据类型](../../../language-reference/data-types/double-data-type.md)）时，请记住，它们存储为二进制小数。 这意味着，它们不能保持不是二进制小数的任何数量的精确表示形式（其形式为 k/（2 ^ n），其中 k 和 n 为整数）。 例如，0.5 （= 1/2）和0.3125 （= 5/16）可以作为精确值来保存，而0.2 （= 1/5）和0.3 （= 3/10）只能是近似值。

由于此不精确性，在对浮点值进行操作时，不能依赖于准确的结果。 具体而言，理论上相等的两个值可能具有略微不同的表示形式。

| 比较浮点数量 |
|---|
|1. 通过使用 <xref:System.Math.Abs%2A> <xref:System.Math> 命名空间中类的方法来计算其差异的绝对值 <xref:System> 。<br />2. 确定可接受的最大值，以便在其差异不大时，可以将两个数量视为适用于实用目的。<br />3. 将差异的绝对值与可接受的差异进行比较。|

下面的示例演示了两个值的错误和正确比较 `Double` 。

[!code-vb[VbVbalrDataTypes#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#10)]

上面的示例使用 <xref:System.Double.ToString%2A> 结构的方法， <xref:System.Double> 以便它可以指定比关键字使用更好的精度 `CStr` 。 默认值为15个数字，但 "G17" 格式将其扩展为17位数字。

## <a name="mod-operator-does-not-return-accurate-result"></a>Mod 运算符未返回准确的结果

由于浮点存储的不精确性，如果至少有一个操作数为浮点，则[Mod 运算符](../../../language-reference/operators/mod-operator.md)可能会返回意外的结果。

[Decimal 数据类型](../../../language-reference/data-types/decimal-data-type.md)不使用浮点表示形式。 许多在和中不精确的数字 `Single` `Double` `Decimal` （例如，0.2 和0.3）。 尽管算术 `Decimal` 比浮点时间慢，但其性能可能会降低，从而获得更好的精度。

|查找浮点数量的整数余数|
|---|
|1. 将变量声明为 `Decimal` 。<br />2. 使用文本类型字符将 `D` 文本强制 `Decimal` 置于，以防其值对于数据类型来说太大 `Long` 。|

下面的示例演示了浮点操作数的潜在不精确性。

[!code-vb[VbVbalrDataTypes#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#11)]

上面的示例使用 <xref:System.Double.ToString%2A> 结构的方法， <xref:System.Double> 以便它可以指定比关键字使用更好的精度 `CStr` 。 默认值为15个数字，但 "G17" 格式将其扩展为17位数字。

因为 `zeroPointTwo` 为 `Double` ，所以，0.2 的值是0.20000000000000001 的存储值的无限重复二进制小数。 此数量相除2.0 将产生9.9999999999999995，剩余0.19999999999999991。

在的表达式中 `decimalRemainder` ，文本类型字符 `D` 强制两个操作数为 `Decimal` ，0.2 具有精确的表示形式。 因此， `Mod` 运算符产生了预计的0.0 余数。

请注意，声明为是不够的 `decimalRemainder` `Decimal` 。 您还必须强制文本 `Decimal` ，或者默认使用， `Double` 并 `decimalRemainder` 接收与相同的不准确值 `doubleRemainder` 。

## <a name="boolean-type-does-not-convert-to-numeric-type-accurately"></a>布尔类型不会准确地转换为数值类型

[布尔数据类型](../../../language-reference/data-types/boolean-data-type.md)值不会存储为数字，并且存储的值不应与数字等效。 为了与早期版本兼容，Visual Basic 提供转换关键字（[CType 函数](../../../language-reference/functions/ctype-function.md)、、等 `CBool` `CInt` ）来转换 `Boolean` 和数值类型。 但是，其他语言有时会以不同的方式执行这些转换，这与 .NET Framework 方法相同。

永远不应编写依赖于和的等效数值的代码 `True` `False` 。 应尽可能将变量的使用限制 `Boolean` 为它们的设计逻辑值。 如果必须混合 `Boolean` 和数值，请确保了解所选的转换方法。

### <a name="conversion-in-visual-basic"></a>Visual Basic 中的转换

如果使用 `CType` 或 `CBool` 转换关键字将数值数据类型转换为，则 `Boolean` 0 变为， `False` 所有其他值将变为 `True` 。 `Boolean`使用转换关键字将值转换为数值类型时，将 `False` 变为0，并 `True` 变为-1。

### <a name="conversion-in-the-framework"></a>框架中的转换

<xref:System.Convert.ToInt32%2A> <xref:System.Convert> 命名空间中的类的方法 <xref:System> 将转换 `True` 为 + 1。

如果必须将值转换 `Boolean` 为数值数据类型，请注意使用哪种转换方法。

## <a name="character-literal-generates-compiler-error"></a>字符文本生成编译器错误

如果没有任何类型的字符，Visual Basic 会假设文本的默认数据类型。 字符文本的默认类型（用引号（）引起来） `" "` 为 `String` 。

`String`数据类型不会扩大到[Char 数据类型](../../../language-reference/data-types/char-data-type.md)。 这意味着，如果要将文本分配给 `Char` 变量，则必须进行收缩转换或将文本强制转换为 `Char` 类型。

|创建要分配给变量或常量的 Char 文本|
|---|
|1. 将变量或常数声明为 `Char` 。<br />2. 将字符值用引号（）引起来 `" "` 。<br />3. 在右双引号后跟文本类型字符 `C` ，以强制实施文本 `Char` 。 如果类型检查开关（[Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)）为，则这是必需的 `On` ，并且在任何情况下都是必需的。|

下面的示例演示了将文本成功和成功分配给变量的情况 `Char` 。

[!code-vb[VbVbalrDataTypes#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDataTypes/VB/Class1.vb#12)]

使用收缩转换始终存在风险，因为它们可能会在运行时失败。 例如， `String` 如果值包含多个字符，则从到的转换 `Char` 可能会失败 `String` 。 因此，更好的编程方法是使用 `C` 类型字符。

## <a name="string-conversion-fails-at-run-time"></a>字符串转换在运行时失败

[字符串数据类型](../../../language-reference/data-types/string-data-type.md)参与极少的扩大转换。 `String`仅扩大到自身，且 `Object` 仅 `Char` 和 `Char()` （ `Char` 数组）扩大到 `String` 。 这是因为 `String` 变量和常量可以包含其他数据类型不能包含的值。

如果类型检查开关（[Option Strict 语句](../../../language-reference/statements/option-strict-statement.md)）为 `On` ，则编译器不允许所有隐式收缩转换。 这包括涉及的内容 `String` 。 你的代码仍可使用转换关键字（如 `CStr` 和[CType 函数](../../../language-reference/functions/ctype-function.md)）来指示 .NET Framework 尝试转换。

> [!NOTE]
> 禁止将从集合中的元素转换 `For Each…Next` 为循环控制变量的收缩转换错误。 有关详细信息和示例，请参阅中的 "收缩转换" 一节[。下一语句](../../../language-reference/statements/for-each-next-statement.md)。

### <a name="narrowing-conversion-protection"></a>收缩转换保护

收缩转换的缺点是它们可能会在运行时失败。 例如，如果 `String` 变量包含除 "True" 或 "False" 之外的任何值，则不能将其转换为 `Boolean` 。 如果包含标点字符，则转换为任意数值类型的操作都将失败。 除非您知道 `String` 变量始终包含目标类型可以接受的值，否则不应尝试转换。

如果必须从转换 `String` 为另一种数据类型，最安全的过程是将尝试的转换包含在[Try .。。Catch .。。Finally 语句](../../../language-reference/statements/try-catch-finally-statement.md)。 这使你可以处理运行时失败。

### <a name="character-arrays"></a>字符数组

一个 `Char` 元素和一个数组， `Char` 它们都扩大到 `String` 。 但是，不 `String` 会扩大到 `Char()` 。 若要将 `String` 值转换为 `Char` 数组，可以使用类的 <xref:System.String.ToCharArray%2A> 方法 <xref:System.String?displayProperty=nameWithType> 。

### <a name="meaningless-values"></a>无意义值

通常情况下， `String` 值在其他数据类型中没有意义，转换非常有意义。 应尽可能将变量的使用限制 `String` 为为其设计的字符序列。 永远不应编写依赖于其他类型中的等效值的代码。

## <a name="see-also"></a>另请参阅

- [数据类型](index.md)
- [类型字符](type-characters.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [Visual Basic 中的类型转换](type-conversions.md)
- [数据类型](../../../language-reference/data-types/index.md)
- [Type Conversion Functions](../../../language-reference/functions/type-conversion-functions.md)
- [有效使用数据类型](efficient-use-of-data-types.md)
