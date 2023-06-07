---
title: 数值数据类型
ms.date: 07/20/2015
helpviewer_keywords:
- integral types [Visual Basic], Visual Basic
- Short data type [Visual Basic], numeric data types
- Double data type [Visual Basic], numeric data types
- Long data type [Visual Basic], Visual Basic numeric data types
- numbers [Visual Basic], whole
- fractions
- numbers
- whole numbers
- integer numbers
- numbers [Visual Basic], integer
- fractional data types [Visual Basic]
- mantissas, of fractional numbers
- mantissas
- data types [Visual Basic], numeric
- Integer data type [Visual Basic], numeric data types
- exponent, of fractional numbers
- integers [Visual Basic]
- numeric data types [Visual Basic], Visual Basic
- Single data type [Visual Basic], numeric types
- Decimal data type [Visual Basic], numeric data types
ms.assetid: a27bd4d0-7e14-43eb-9cc4-b42eaab323c9
ms.openlocfilehash: 317c0862953e7bb866faa4712d42dfd5995ecf35
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91086226"
---
# <a name="numeric-data-types-visual-basic"></a>数值型数据类型 (Visual Basic)

Visual Basic 提供了几种 *数值数据类型* 来处理各种表示形式的数字。 *整数* 类型仅表示 (正值、负值和零) 的整数 *，而非整数类型表示* 同时包含整数部分和小数部分的数字。  
  
 有关显示 Visual Basic 数据类型的并行比较的表，请参阅 [数据类型](../../../language-reference/data-types/index.md)。  
  
## <a name="integral-numeric-types"></a>整数数值类型  

 *整数数据类型* 是指仅表示没有小数部分的数字的数据类型。  
  
 *带符号*的整型数据类型为[SByte 数据类型](../../../language-reference/data-types/sbyte-data-type.md) (8 位) ， [Short 数据类型](../../../language-reference/data-types/short-data-type.md) (16 位) ， [Integer 数据类型](../../../language-reference/data-types/integer-data-type.md) (32 位) ， [Long 数据类型](../../../language-reference/data-types/long-data-type.md) (64-位) 。 如果变量始终存储整数而不是小数，则将其声明为这些类型之一。  
  
 *无符号*整型类型为[Byte 数据类型](../../../language-reference/data-types/byte-data-type.md) (8 位) ， [UShort 数据类型](../../../language-reference/data-types/ushort-data-type.md) (16 位) ， [UInteger 数据类型](../../../language-reference/data-types/uinteger-data-type.md) (32 位) ，以及[ULong 数据类型](../../../language-reference/data-types/ulong-data-type.md) (64-位) 。 如果变量包含二进制数据或未知性质的数据，则将其声明为这些类型之一。  
  
### <a name="performance"></a>性能  

 与其他数据类型相比，算术运算比整型类型更快。 与 `Integer` Visual Basic 中的和类型相比，它们速度最快 `UInteger` 。  
  
### <a name="large-integers"></a>大整数  

 如果需要保留大于 `Integer` 数据类型可以保留的整数，则可以改用 `Long` 数据类型。 `Long` 变量可以包含从-9223372036854775808 到9223372036854775807的数字。 与的操作相比，其运行 `Long` 速度稍慢 `Integer` 。  
  
 如果需要更大的值，则可以使用 [Decimal 数据类型](../../../language-reference/data-types/decimal-data-type.md)。 `Decimal`如果不使用任何小数位数，则可以在变量中保存从-79228162514264337593543950335 到79228162514264337593543950335的数字。 但是，与 `Decimal` 任何其他数值数据类型相比，数字运算的速度要慢得多。  
  
### <a name="small-integers"></a>小整数  

 如果不需要数据类型的完整范围 `Integer` ，可以使用 `Short` 数据类型，该数据类型可以包含从-32768 到32767的整数。 对于最小整数范围， `SByte` 数据类型保存从-128 到127的整数。 如果有大量包含小整数的变量，则公共语言运行时有时可以 `Short` `SByte` 更有效地存储和变量，并节省内存消耗。 不过，和的 `Short` 操作 `SByte` 在一定程度上比更慢 `Integer` 。  
  
### <a name="unsigned-integers"></a>无符号整数  

 如果你知道你的变量从不需要保存负数，则可以使用*无符号类型* `Byte` 、 `UShort` 、 `UInteger` 和 `ULong` 。 这些数据类型中的每个数据类型都可以保存两倍于其对应的带符号类型 (`SByte` 、 `Short` 、 `Integer` 和 `Long`) 。 就性能而言，每个未签名的类型与其对应的已签名类型的有效性完全相同。 特别是， `UInteger` 共享的 `Integer` 区别是所有基本数值数据类型都是最有效的。  
  
## <a name="nonintegral-numeric-types"></a>非整型数值类型  

 非*整型数据类型*是指具有整数部分和小数部分的数字。  
  
 非整型数值数据类型为 `Decimal` (128 位固定点) ， [单数据类型](../../../language-reference/data-types/single-data-type.md) (32 位浮点) ， [双精度数据类型](../../../language-reference/data-types/double-data-type.md) (64 位浮点) 。 它们都是已签名的类型。 如果变量可以包含一个分数，则将其声明为这些类型之一。  
  
 `Decimal` 不是浮点数据类型。 `Decimal` 数字具有二进制整数值和整数比例因子，用于指定值的哪一部分是小数部分。  
  
 您可以使用 `Decimal` 变量作为 money 值。 优点是值的精度。 `Double`数据类型的速度更快，需要的内存更少，但可能会产生舍入误差。 `Decimal`数据类型将完整准确性保留为28个小数位数。  
  
 浮点 (`Single` 和 `Double`) 数字的范围大于 `Decimal` 数字，但可能会受到舍入错误的限制。 浮点类型支持的有效位数少于 `Decimal` ，但可表示更大的值。  
  
 非整数数字值可以表示为 mmmEeee，其中 mmm 是有效位数) 的 (*尾数* ，而 eee 是 10) 幂的 *指数* (。 非整数类型的最大正值是 7.9228162514264337593543950335 E + 28 for `Decimal` 、3.4028235 e + 38 for `Single` 和 1.79769313486231570 e + 308 （对于） `Double` 。  
  
### <a name="performance"></a>性能  

 `Double` 是最有效的小数数据类型，因为当前平台上的处理器以双精度执行浮点运算。 但是，与整数类型（例如）相比，具有的操作的 `Double` 速度并不像 `Integer` 。  
  
### <a name="small-magnitudes"></a>小型度  

 对于具有最小可能的最大度量值的数字 (最接近 0) ， `Double` 变量可以将数字保存为小的 4.94065645841246544 e-324 for 负值，并将 4.94065645841246544 e-324 用于正值。  
  
### <a name="small-fractional-numbers"></a>小小数值  

 如果不需要数据类型的完整范围 `Double` ，可以使用 `Single` 数据类型，该数据类型可以保存-3.4028235 e + 38 到 3.4028235 e + 38 之间的浮点数。 对于负值，变量的最小度为 `Single` -1.401298 e-45; 对于正值，则为 1.401298 e-45。 如果有大量包含较小浮点数的变量，则公共语言运行时有时可以 `Single` 更有效地存储变量，并节省内存消耗。  
  
## <a name="see-also"></a>请参阅

- [基本数据类型](elementary-data-types.md)
- [字符数据类型](character-data-types.md)
- [杂项数据类型](miscellaneous-data-types.md)
- [数据类型疑难解答](troubleshooting-data-types.md)
- [如何：调用需要使用无符号类型的 Windows 函数](../../com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
