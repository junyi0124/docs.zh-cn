---
title: 如何：定义运算符
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- operators [Visual Basic], defining
- procedures [Visual Basic], operator
- Visual Basic code, operators
- syntax [Visual Basic], Operator procedures
- operators [Visual Basic], overloading
- operator procedures [Visual Basic], about operator procedures
- return values [Visual Basic], Operator procedures
- operator overloading
ms.assetid: d4b0e253-092a-4e6e-9fe2-01f562140a29
ms.openlocfilehash: 5acbd0439ddbb956b80d56e23d11cd5e152f37ff
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91087396"
---
# <a name="how-to-define-an-operator-visual-basic"></a>如何：定义运算符 (Visual Basic)

如果已定义类或结构，则可在 `*` `<>` `And` 一个或两个操作数为类或结构的类型时定义标准运算符 (如、或) 的行为。  
  
 将标准运算符定义为类或结构中的运算符过程。 所有运算符过程必须是 `Public` `Shared` 。  
  
 在类或结构上定义运算符也称为 *重载* 运算符。  
  
## <a name="example"></a>示例  

 下面的示例 `+` 为一个名为的结构定义运算符 `height` 。 结构使用以英尺和英寸为单位的高度。 一 *英寸* 为2.54 厘米，1 *英尺* 为12英寸。 若要确保标准化的值 (英寸 < 12.0) ，则构造函数执行第 12 *模* 运算。 `+`运算符使用构造函数生成规范化值。  
  
 [!code-vb[VbVbcnProcedures#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#25)]  
  
 可以 `height` 通过以下代码测试结构。  
  
 [!code-vb[VbVbcnProcedures#26](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#26)]  

## <a name="see-also"></a>请参阅

- [运算符过程](./operator-procedures.md)
- [如何：定义转换运算符](./how-to-define-a-conversion-operator.md)
- [如何：调用运算符过程](./how-to-call-an-operator-procedure.md)
- [如何：使用定义运算符的类](./how-to-use-a-class-that-defines-operators.md)
- [Operator Statement](../../../language-reference/statements/operator-statement.md)
- [Structure 语句](../../../language-reference/statements/structure-statement.md)
- [如何：声明结构](../data-types/how-to-declare-a-structure.md)
- [Mod 运算符](../../../language-reference/operators/mod-operator.md)
