---
title: 如何：定义转换运算符
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- operators [Visual Basic], defining
- procedures [Visual Basic], operator
- operators [Visual Basic], overloading
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: 54203dfa-c24b-463f-9942-d5153e89e762
ms.openlocfilehash: 2fabcf6c6ceb38fe77d4eed4f02dcb5a5e447bf1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91085667"
---
# <a name="how-to-define-a-conversion-operator-visual-basic"></a>如何：定义转换运算符 (Visual Basic)

如果已定义类或结构，则可以在类或结构的类型与另一种数据类型 (如 `Integer` 、或) 之间定义类型转换运算符 `Double` `String` 。  
  
 将类型转换定义为类或结构中的 [CType 函数](../../../language-reference/functions/ctype-function.md) 过程。 所有转换过程必须是 `Public Shared` ，并且每个转换过程必须指定 [扩大](../../../language-reference/modifiers/widening.md) 或 [收缩](../../../language-reference/modifiers/narrowing.md)。  
  
 在类或结构上定义运算符也称为 *重载* 运算符。  
  
## <a name="example"></a>示例  

 下面的示例定义了名为和的结构之间的转换运算符 `digit` `Byte` 。  
  
 [!code-vb[VbVbcnProcedures#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#27)]  
  
 可以 `digit` 通过以下代码测试结构。  
  
 [!code-vb[VbVbcnProcedures#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>请参阅

- [运算符过程](./operator-procedures.md)
- [如何：定义运算符](./how-to-define-an-operator.md)
- [如何：调用运算符过程](./how-to-call-an-operator-procedure.md)
- [如何：使用定义运算符的类](./how-to-use-a-class-that-defines-operators.md)
- [Operator Statement](../../../language-reference/statements/operator-statement.md)
- [Structure 语句](../../../language-reference/statements/structure-statement.md)
- [如何：声明结构](../data-types/how-to-declare-a-structure.md)
- [隐式转换和显式转换](../data-types/implicit-and-explicit-conversions.md)
- [Widening and Narrowing Conversions](../data-types/widening-and-narrowing-conversions.md)
