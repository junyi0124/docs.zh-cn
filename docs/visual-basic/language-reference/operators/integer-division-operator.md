---
title: '\ 运算符'
ms.date: 07/20/2015
f1_keywords:
- vb.\
- '\'
helpviewer_keywords:
- division operator [Visual Basic], integer
- integer division operator [Visual Basic]
- zero, division by zero
- arithmetic operators [Visual Basic], division
- division [Visual Basic], by zero
- backslash (\) [Visual Basic]
- '\ operator [Visual Basic]'
- integer quotient
- math operators [Visual Basic]
- quotients, integer
- truncation [Visual Basic], integer division
ms.assetid: 4b0ee347-950c-45c9-8e23-54bc85df208e
ms.openlocfilehash: cf2dc66532925d56cea6fd141f44a245bc2dd8dd
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90873392"
---
# <a name="-operator-visual-basic"></a>\ 运算符 (Visual Basic)

使两个数字相除，返回整数结果。  
  
## <a name="syntax"></a>语法  
  
```vb  
expression1 \ expression2  
```  
  
## <a name="parts"></a>组成部分  

 `expression1`  
 必需。 任何数值表达式。  
  
 `expression2`  
 必需。 任何数值表达式。  
  
## <a name="supported-types"></a>支持的类型  

 所有数值类型，包括无符号和浮点类型和 `Decimal` 。  
  
## <a name="result"></a>结果  

 结果是除以的整数商 `expression1` `expression2` ，它放弃了余数并仅保留整数部分。 这称为 " *截断*"。  
  
 Result 数据类型是适用于和的数据类型的数值类型 `expression1` `expression2` 。 请参阅 [运算符结果的数据类型](data-types-of-operator-results.md)中的 "整数算法" 表。  
  
 [/运算符 (Visual Basic) ](floating-point-division-operator.md)返回完整商，这将保留小数部分的剩余部分。  
  
## <a name="remarks"></a>备注  

 在执行除法运算之前，Visual Basic 会尝试将任何浮点数值表达式转换为 `Long` 。 如果 `Option Strict` 为 `On` ，则会发生编译器错误。 如果 `Option Strict` 为 `Off` ， <xref:System.OverflowException> 则当值超出 [Long 数据类型](../data-types/long-data-type.md)的范围时，可能会出现这种情况。 转换为的 `Long` 还服从于 *银行家的舍入*。 有关详细信息，请参阅 [类型转换函数](../functions/type-conversion-functions.md)中的 "小数部分"。  
  
 如果 `expression1` 或的 `expression2` 计算结果不为 [空](../nothing.md)，则将其视为零。  
  
## <a name="attempted-division-by-zero"></a>尝试被零除  

 如果 `expression2` 计算结果为零，则 `\` 运算符会引发 <xref:System.DivideByZeroException> 异常。 对于操作数的所有数值数据类型都是如此。  
  
> [!NOTE]
> `\`运算符可以*重载*，这意味着当操作数具有该类或结构的类型时，该类或结构可以重新定义其行为。 如果你的代码在该类或结构上使用此运算符，请确保了解其重新定义的行为。 有关详细信息，请参阅 [Operator Procedures](../../programming-guide/language-features/procedures/operator-procedures.md)。  
  
## <a name="example"></a>示例  

 下面的示例使用 `\` 运算符来执行整数除法。 结果是一个整数，表示两个操作数的整数商，余数被丢弃。  
  
 [!code-vb[VbVbalrOperators#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#18)]  
  
 前面的示例中的表达式分别返回值2、3、33和-22。  
  
## <a name="see-also"></a>另请参阅

- [\\= 运算符](integer-division-assignment-operator.md)
- [/Operator (Visual Basic) ](floating-point-division-operator.md)
- [Option Strict 语句](../statements/option-strict-statement.md)
- [算术运算符](arithmetic-operators.md)
- [Visual Basic 中的运算符优先级](operator-precedence.md)
- [按功能列出的运算符](operators-listed-by-functionality.md)
- [算术运算符 (Visual Basic)](../../programming-guide/language-features/operators-and-expressions/arithmetic-operators.md)
