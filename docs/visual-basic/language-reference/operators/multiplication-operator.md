---
title: '* 运算符'
ms.date: 07/20/2015
f1_keywords:
- vb.*
helpviewer_keywords:
- arithmetic operators [Visual Basic], multiplication
- operators [Visual Basic], multiplication
- '* operator [Visual Basic]'
- multiplication operator [Visual Basic], syntax
- math operators [Visual Basic]
ms.assetid: 2b210382-99da-4195-89ba-b1d06f5e89ad
ms.openlocfilehash: 7038fef4258d190b726a851b26f2a2840ff3c0ea
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873363"
---
# <a name="-operator-visual-basic"></a>* 运算符 (Visual Basic)

使两个数字相乘。  
  
## <a name="syntax"></a>语法  
  
```vb  
number1 * number2  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`number1`|必需。 任何数值表达式。|  
|`number2`|必需。 任何数值表达式。|  
  
## <a name="result"></a>结果  

 结果是和的乘积 `number1` `number2` 。  
  
## <a name="supported-types"></a>支持的类型  

 所有数值类型，包括无符号和浮点类型和 `Decimal` 。  
  
## <a name="remarks"></a>备注  

 结果的数据类型取决于操作数的类型。 下表显示了如何确定结果的数据类型。  
  
|操作数数据类型|Result 数据类型|  
|---|---|  
|这两个表达式都是整型数据类型 ([SByte](../data-types/sbyte-data-type.md)、 [Byte](../data-types/byte-data-type.md)、 [Short](../data-types/short-data-type.md)、 [UShort](../data-types/ushort-data-type.md)、 [Integer](../data-types/integer-data-type.md)、 [UInteger](../data-types/uinteger-data-type.md)、 [Long](../data-types/long-data-type.md)、 [ULong](../data-types/ulong-data-type.md)) |适用于和的数据类型的数值数据类型 `number1` `number2` 。 请参阅 [运算符结果的数据类型](data-types-of-operator-results.md)中的 "整数算法" 表。|  
|两个表达式都是 [小数](../data-types/decimal-data-type.md)|`Decimal`|  
|这两个表达式都是 [单](../data-types/single-data-type.md)|`Single`|  
|这两个表达式都是浮点数据类型 (`Single` 或 [Double](../data-types/double-data-type.md)) ，但不是两者 (都不是 `Single` `Decimal` 浮点数据类型) |`Double`|  
  
 如果表达式的计算结果不为 [空](../nothing.md)，则将其视为零。  
  
## <a name="overloading"></a>重载  

 `*`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 此示例使用 `*` 运算符将两个数字相乘。 结果就是两个操作数的乘积。  
  
 [!code-vb[VbVbalrOperators#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#4)]  
  
## <a name="see-also"></a>另请参阅

- [* = 运算符](multiplication-assignment-operator.md)
- [算术运算符](arithmetic-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
