---
title: / 运算符
ms.date: 07/20/2015
f1_keywords:
- vb./
helpviewer_keywords:
- division operator [Visual Basic], floating point
- floating-point numbers [Visual Basic], division operator
- slash (/) operator
- zero, division by zero
- operators [Visual Basic], arithmetic
- arithmetic operators [Visual Basic], division
- division [Visual Basic], by zero
- operators [Visual Basic], division
- division operator [Visual Basic], syntax
- / operator [Visual Basic]
- math operators [Visual Basic]
ms.assetid: 335e97f2-c434-439e-9064-76973a051101
ms.openlocfilehash: 765a80d45908e0ecf17e4c21b748dbf6b2a4c0f5
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90867029"
---
# <a name="-operator-visual-basic"></a>/ 运算符 (Visual Basic)

使两个数字相除，返回浮点结果。  
  
## <a name="syntax"></a>语法  
  
```vb  
expression1 / expression2  
```  
  
## <a name="parts"></a>组成部分  

 `expression1`  
 必需。 任何数值表达式。  
  
 `expression2`  
 必需。 任何数值表达式。  
  
## <a name="supported-types"></a>支持的类型  

 所有数值类型，包括无符号和浮点类型和 `Decimal` 。  
  
## <a name="result"></a>结果  

 结果是除以的全部商 `expression1` `expression2` ，包括余数。  
  
 [\ 运算符 (Visual Basic) ](integer-division-operator.md)返回整数商，这会删除余数。  
  
## <a name="remarks"></a>备注  

 结果的数据类型取决于操作数的类型。 下表显示了如何确定结果的数据类型。  
  
|操作数数据类型|Result 数据类型|  
|------------------------|----------------------|  
|这两个表达式都是整型数据类型 ([SByte](../data-types/sbyte-data-type.md)、 [Byte](../data-types/byte-data-type.md)、 [Short](../data-types/short-data-type.md)、 [UShort](../data-types/ushort-data-type.md)、 [Integer](../data-types/integer-data-type.md)、 [UInteger](../data-types/uinteger-data-type.md)、 [Long](../data-types/long-data-type.md)、 [ULong](../data-types/ulong-data-type.md)) |`Double`|  
|一个表达式是[单一](../data-types/single-data-type.md)数据类型，另一个表达式不是[双精度](../data-types/double-data-type.md)型|`Single`|  
|一个表达式是 [Decimal](../data-types/decimal-data-type.md) 数据类型，另一个表达式不是 [单个](../data-types/single-data-type.md) 或 [双精度](../data-types/double-data-type.md)|`Decimal`|  
|其中一个表达式为 [Double](../data-types/double-data-type.md) 数据类型|`Double`|  
  
 在执行除法运算之前，所有整数数值表达式都将扩展到 `Double` 。 如果将结果赋给整数数据类型，Visual Basic 会尝试将结果从转换 `Double` 为该类型。 如果结果不适合该类型，这可能会引发异常。 具体而言，请参阅此帮助页上的 "尝试被零除"。  
  
 如果 `expression1` 或的 `expression2` 计算结果不为 [空](../nothing.md)，则将其视为零。  
  
## <a name="attempted-division-by-zero"></a>尝试被零除  

 如果 `expression2` 计算结果为零，则 `/` 运算符的行为不同于不同的操作数数据类型。 下表显示了可能的行为。  
  
|操作数数据类型|`expression2`为零时的行为|  
|------------------------|---------------------------------------|  
|浮点 (`Single` 或 `Double`) |<xref:System.Double.PositiveInfinity> <xref:System.Double.NegativeInfinity> 如果也为零，则返回无穷 (或) ，或者 <xref:System.Double.NaN> (不是数字) `expression1`|  
|`Decimal`|却 <xref:System.DivideByZeroException>|  
|整数 (有符号或无符号) |尝试转换回整型类型会引发 <xref:System.OverflowException> ，因为整型类型不能接受 <xref:System.Double.PositiveInfinity> 、 <xref:System.Double.NegativeInfinity> 或 <xref:System.Double.NaN>|  
  
> [!NOTE]
> `/`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 此示例使用 `/` 运算符来执行浮点除法。 结果是两个操作数的商。  
  
 [!code-vb[VbVbalrOperators#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#16)]  
  
 前面的示例中的表达式返回值2.5 和3.333333。 请注意，结果始终是浮点 (`Double`) ，即使两个操作数都是整数常量。  
  
## <a name="see-also"></a>另请参阅

- [/= 运算符 (Visual Basic) ](floating-point-division-assignment-operator.md)
- [\ 运算符 (Visual Basic) ](integer-division-operator.md)
- [运算符结果的数据类型](data-types-of-operator-results.md)
- [算术运算符](arithmetic-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
