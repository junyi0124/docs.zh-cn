---
title: 如何：调用 Property 过程
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- Visual Basic code, properties
- procedures [Visual Basic], calling
- properties [Visual Basic], property procedures
- procedure calls [Visual Basic], property procedures
ms.assetid: 96bc4d74-d9c3-4b7a-954d-58ac8553cd94
ms.openlocfilehash: 0b35751136937d4cee5b3ca9669b43d3fbdf71a1
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071946"
---
# <a name="how-to-call-a-property-procedure-visual-basic"></a>如何：调用 Property 过程 (Visual Basic)

通过在属性中存储值或检索其值来调用属性过程。 访问属性的方式与访问变量的方式相同。  
  
 该属性的 `Set` 过程存储一个值，其 `Get` 过程检索值。 但是，不要按名称显式调用这些过程。 使用赋值语句或表达式中的属性，就像存储或检索变量的值一样。 Visual Basic 对属性的过程进行调用。  
  
### <a name="to-call-a-propertys-get-procedure"></a>调用属性的 Get 过程  
  
1. 在表达式中使用属性名称的方式与使用变量名相同。 可以在可以使用变量或常量的任何位置使用属性。  
  
     \- 或 -  
  
     使用相等 (后面的属性名称 `=`) 在赋值语句中登录。  
  
     下面的示例读取属性的值 <xref:Microsoft.VisualBasic.DateAndTime.Now%2A> ，隐式调用其 `Get` 过程。  
  
     [!code-vb[VbVbalrDateProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDateProperties/VB/Module1.vb#4)]  
  
2. 如果属性采用参数，请在属性名称后面加上括号，以将参数列表括起来。 如果没有参数，则可以选择省略括号。  
  
3. 将参数置于括号中的参数列表内，用逗号分隔。 请确保以属性定义相应参数的相同顺序提供参数。  
  
 属性的值作为变量或常数加入表达式，或者将其存储在赋值语句左侧的变量或属性中。  
  
### <a name="to-call-a-propertys-set-procedure"></a>调用属性的 Set 过程  
  
1. 使用赋值语句左侧的属性名称。  
  
     下面的示例设置属性的值 <xref:Microsoft.VisualBasic.DateAndTime.TimeOfDay%2A> ，隐式调用该 `Set` 过程。  
  
     [!code-vb[VbVbcnProcedures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#11)]  
  
2. 如果属性采用参数，请在属性名称后面加上括号，以将参数列表括起来。 如果没有参数，则可以选择省略括号。  
  
3. 将参数置于括号中的参数列表内，用逗号分隔。 请确保以属性定义相应参数的相同顺序提供参数。  
  
 赋值语句右侧生成的值存储在属性中。  
  
## <a name="see-also"></a>请参阅

- [Property 过程](./property-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Property Statement](../../../language-reference/statements/property-statement.md)
- [Visual Basic 中属性和变量的差异](./differences-between-properties-and-variables.md)
- [如何：创建属性](./how-to-create-a-property.md)
- [如何：声明具有混合访问级别的属性](./how-to-declare-a-property-with-mixed-access-levels.md)
- [如何：在 Visual Basic 中声明和调用默认属性](./how-to-declare-and-call-a-default-property.md)
- [如何：在属性中放置值](./how-to-put-a-value-in-a-property.md)
- [如何：从属性获取值](./how-to-get-a-value-from-a-property.md)
- [Get 语句](../../../language-reference/statements/get-statement.md)
- [Set 语句](../../../language-reference/statements/set-statement.md)
