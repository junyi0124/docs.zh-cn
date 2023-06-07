---
title: 如何：调用运算符过程
ms.date: 07/20/2015
helpviewer_keywords:
- operator procedures [Visual Basic], calling
- procedures [Visual Basic], operator
- procedure calls [Visual Basic], operator overloading
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- overloaded operators [Visual Basic], calling
- operator overloading
ms.assetid: 0dce42cc-f0b0-4c14-9f62-018b21f33497
ms.openlocfilehash: 0e88ff7b36535a709671a1f9b838f2b4488d1d37
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075182"
---
# <a name="how-to-call-an-operator-procedure-visual-basic"></a>如何：调用运算符过程 (Visual Basic)

在表达式中使用运算符符号调用运算符过程。 在转换运算符的情况下，调用 [CType 函数](../../../language-reference/functions/ctype-function.md) 将值从一种数据类型转换为另一种数据类型。  
  
 不要显式调用运算符过程。 只需在 `CType` 赋值语句或表达式中使用运算符（或函数），就像通常使用运算符的方法一样。 Visual Basic 调用运算符过程。  
  
 在类或结构上定义运算符也称为 *重载* 运算符。  
  
### <a name="to-call-an-operator-procedure"></a>调用运算符过程  
  
1. 用普通方法在表达式中使用运算符符号。  
  
2. 确保操作数的数据类型适用于运算符，并按正确的顺序排列。  
  
3. 运算符会按预期方式分配表达式的值。  
  
### <a name="to-call-a-conversion-operator-procedure"></a>调用转换运算符过程  
  
1. `CType`在表达式中使用。  
  
2. 确保操作数的数据类型适用于转换，并按正确的顺序排列。  
  
3. `CType` 调用转换运算符过程，并返回转换后的值。  
  
## <a name="example"></a>示例  

 下面的示例创建两个 <xref:System.TimeSpan> 结构，将它们相加，然后将结果存储在第三个 <xref:System.TimeSpan> 结构中。 <xref:System.TimeSpan>结构定义运算符过程以重载多个标准运算符。  
  
 [!code-vb[VbVbcnProcedures#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#29)]  
  
 由于 <xref:System.TimeSpan> 重载标准 `+` 运算符，因此当计算的值时，上面的示例将调用一个运算符过程 `combinedSpan` 。  
  
 有关调用会话运算符过程的示例，请参阅 [如何：使用定义运算符的类](./how-to-use-a-class-that-defines-operators.md)。  
  
## <a name="compile-the-code"></a>编译代码  

 请确保所用的类或结构定义要使用的运算符。  
  
## <a name="see-also"></a>请参阅

- [运算符过程](./operator-procedures.md)
- [如何：定义运算符](./how-to-define-an-operator.md)
- [如何：定义转换运算符](./how-to-define-a-conversion-operator.md)
- [Operator Statement](../../../language-reference/statements/operator-statement.md)
- [Widening](../../../language-reference/modifiers/widening.md)
- [Narrowing](../../../language-reference/modifiers/narrowing.md)
- [Structure 语句](../../../language-reference/statements/structure-statement.md)
- [如何：声明结构](../data-types/how-to-declare-a-structure.md)
- [隐式转换和显式转换](../data-types/implicit-and-explicit-conversions.md)
- [Widening and Narrowing Conversions](../data-types/widening-and-narrowing-conversions.md)
