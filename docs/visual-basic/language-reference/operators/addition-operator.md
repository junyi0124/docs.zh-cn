---
title: + 运算符
ms.date: 07/20/2015
f1_keywords:
- vb.+
helpviewer_keywords:
- arithmetic operators [Visual Basic], addition
- + operator
- concatenation operators [Visual Basic], syntax
- strings [Visual Basic], concatenating
- sum operator [Visual Basic]
ms.assetid: 5694778f-0a2c-4539-8009-f66f318fb46d
ms.openlocfilehash: bc31e4c66c64d891e3fffd809b7ae99b9c9a0520
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873456"
---
# <a name="-operator-visual-basic"></a>+ 运算符 (Visual Basic)

将两个数相加或返回数值表达式的正值。 还可用于连接两个字符串表达式。  
  
## <a name="syntax"></a>语法  
  
```vb
expression1 + expression2
```
  
or

```vb  
+expression1  
```  
  
## <a name="parts"></a>组成部分  
  
|术语|定义|  
|---|---|  
|`expression1`|必需。 任何数值或字符串表达式。|  
|`expression2`|必需，除非 `+` 操作员计算负值。 任何数值或字符串表达式。|  
  
## <a name="result"></a>结果  

 如果 `expression1` 和 `expression2` 均为数值，则结果为其算术和。  
  
 如果 `expression2` 不存在，则 `+` 运算符为表达式的未更改值的 *一元* 标识运算符。 从这种意义上讲，操作包括保留的符号 `expression1` ，因此如果为负，则结果为负 `expression1` 。  
  
 如果 `expression1` 和 `expression2` 都是字符串，则结果是其值的串联。  
  
 如果 `expression1` 和 `expression2` 属于混合类型，则执行的操作取决于它们的类型、内容和 [Option Strict 语句](../statements/option-strict-statement.md)的设置。 有关详细信息，请参阅 "备注" 中的表。  
  
## <a name="supported-types"></a>支持的类型  

 所有数值类型，包括无符号和浮点类型以及和 `Decimal` `String` 。  
  
## <a name="remarks"></a>备注  

 通常， `+` 如果可能，将执行算术加法运算，并且仅当两个表达式都是字符串时才会进行连接。  
  
 如果两个表达式均为 `Object` ，则 Visual Basic 执行以下操作。  
  
|表达式的数据类型|编译器操作|  
|---|---|  
|这两个表达式都是数值数据类型 (、、、、、、、、、 `SByte` `Byte` `Short` `UShort` `Integer` `UInteger` `Long` `ULong` `Decimal` `Single` 或 `Double`) |添加。 Result 数据类型是适用于和的数据类型的数值类型 `expression1` `expression2` 。 请参阅 [运算符结果的数据类型](data-types-of-operator-results.md)中的 "整数算法" 表。|  
|这两个表达式的类型为 `String`|起来.|  
|一个表达式为数值数据类型，另一个表达式为字符串|如果 `Option Strict` 为 `On` ，则生成编译器错误。<br /><br /> 如果 `Option Strict` 为 `Off` ，则将隐式转换 `String` 为 `Double` 并添加。<br /><br /> 如果 `String` 无法转换为 `Double` ，则引发 <xref:System.InvalidCastException> 异常。|  
|一个表达式为数值数据类型，另一个表达式为 [Nothing](../nothing.md)|添加，其 `Nothing` 值为零。|  
|一个表达式是字符串，另一个表达式是 `Nothing`|串联，其 `Nothing` 值为 ""。|  
  
 如果一个表达式是 `Object` 表达式，Visual Basic 会执行以下操作。  
  
|表达式的数据类型|编译器操作|  
|---|---|  
|`Object` 表达式保存数字值，另一种是数值数据类型|如果 `Option Strict` 为 `On` ，则生成编译器错误。<br /><br /> 如果 `Option Strict` 为 `Off` ，则添加。|  
|`Object` 表达式保存一个数字值，另一个的类型为 `String`|如果 `Option Strict` 为 `On` ，则生成编译器错误。<br /><br /> 如果 `Option Strict` 为 `Off` ，则将隐式转换 `String` 为 `Double` 并添加。<br /><br /> 如果 `String` 无法转换为 `Double` ，则引发 <xref:System.InvalidCastException> 异常。|  
|`Object` 表达式保存一个字符串，另一个表达式为数值数据类型|如果 `Option Strict` 为 `On` ，则生成编译器错误。<br /><br /> 如果 `Option Strict` 为 `Off` ，则将字符串隐式转换 `Object` 为 `Double` 并添加。<br /><br /> 如果字符串 `Object` 无法转换为 `Double` ，则引发 <xref:System.InvalidCastException> 异常。|  
|`Object` 表达式保存了一个字符串，另一个的类型为 `String`|如果 `Option Strict` 为 `On` ，则生成编译器错误。<br /><br /> 如果 `Option Strict` 为 `Off` ，则隐式转换 `Object` 到 `String` 并连接。|  
  
 如果两个表达式都是 `Object` 表达式，则 Visual Basic 仅 () 执行以下操作 `Option Strict Off` 。  
  
|表达式的数据类型|编译器操作|  
|---|---|  
|两个 `Object` 表达式都保存数字值|添加。|  
|这两个 `Object` 表达式的类型为 `String`|起来.|  
|一个 `Object` 表达式保存一个数字值，另一个表达式保存一个字符串|将字符串隐式转换 `Object` 为 `Double` 并添加。<br /><br /> 如果字符串 `Object` 不能转换为数字值，则引发 <xref:System.InvalidCastException> 异常。|  
  
 如果其中一个 `Object` 表达式的计算结果为 [Nothing](../nothing.md) 或 <xref:System.DBNull> ，则 `+` 运算符将其视为 `String` 值为 "" 的。  
  
> [!NOTE]
> 使用 `+` 运算符时，可能无法确定是进行加法还是字符串串联。 使用 `&` 运算符进行串联以消除多义性，并提供自文档代码。  
  
## <a name="overloading"></a>重载  

 `+`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `+` 运算符来添加数字。 如果操作数均为数值，则 Visual Basic 计算算术结果。 算术结果表示两个操作数之和。  
  
 [!code-vb[VbVbalrOperators#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#6)]  
  
 还可以使用 `+` 运算符来串联字符串。 如果两个操作数都是字符串，Visual Basic 将它们连接起来。 连接结果表示一个字符串，该字符串包含两个操作数的内容。  
  
 如果操作数属于混合类型，则结果取决于 [Option Strict 语句](../statements/option-strict-statement.md)的设置。 下面的示例演示了在为时的结果 `Option Strict` `On` 。  
  
 [!code-vb[VbVbalrOperators#53](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class3.vb#53)]  
  
 [!code-vb[VbVbalrOperators#50](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#50)]  
[!code-vb[VbVbalrOperators#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#51)]  
  
 下面的示例演示了在为时的结果 `Option Strict` `Off` 。  
  
 [!code-vb[VbVbalrOperators#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#54)]  
  
 [!code-vb[VbVbalrOperators#50](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#50)]  
[!code-vb[VbVbalrOperators#52](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class2.vb#52)]  
  
 为了消除多义性，应使用 `&` 运算符而不是 `+` 串联。  
  
## <a name="see-also"></a>另请参阅

- [& 运算符](concatenation-operator.md)
- [串联运算符](concatenation-operators.md)
- [算术运算符](arithmetic-operators.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
- [Option Strict 语句](../statements/option-strict-statement.md)
