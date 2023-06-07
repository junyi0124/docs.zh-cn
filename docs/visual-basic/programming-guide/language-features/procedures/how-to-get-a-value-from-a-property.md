---
title: 如何：从属性获取值
ms.date: 07/20/2015
helpviewer_keywords:
- property values [Visual Basic]
- Visual Basic code, procedures
- values [Visual Basic], properties
- Visual Basic code, properties
- properties [Visual Basic], values
ms.assetid: 3954423e-6ab7-4a4c-b55c-a8d27be47891
ms.openlocfilehash: 983e2fd22badf4296004404d885df0a07ab2dc74
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071555"
---
# <a name="how-to-get-a-value-from-a-property-visual-basic"></a>如何：从属性获取值 (Visual Basic)

通过在表达式中包含属性名称来检索属性的值。  
  
 该属性的 `Get` 过程检索值，但不按名称显式调用它。 像使用变量一样使用属性。 Visual Basic 对属性的过程进行调用。  
  
### <a name="to-retrieve-a-value-from-a-property"></a>从属性中检索值  
  
1. 在表达式中使用属性名称的方式与使用变量名相同。 可以在可以使用变量或常量的任何位置使用属性。  
  
     \- 或 -  
  
     使用相等 (后面的属性名称 `=`) 在赋值语句中登录。  
  
     下面的示例读取 Visual Basic 属性的值 `Now` ，隐式调用其 `Get` 过程。  
  
     [!code-vb[VbVbalrDateProperties#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDateProperties/VB/Module1.vb#4)]  
  
2. 如果属性采用参数，请在属性名称后面加上括号，以将参数列表括起来。 如果没有参数，则可以选择省略括号。  
  
3. 将参数置于括号中的参数列表内，用逗号分隔。 请确保以属性定义相应参数的相同顺序提供参数。  
  
 属性的值作为变量或常数加入表达式，或者将其存储在赋值语句左侧的变量或属性中。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [Property 过程](./property-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Property Statement](../../../language-reference/statements/property-statement.md)
- [Visual Basic 中属性和变量的差异](./differences-between-properties-and-variables.md)
- [如何：创建属性](./how-to-create-a-property.md)
- [如何：声明具有混合访问级别的属性](./how-to-declare-a-property-with-mixed-access-levels.md)
- [如何：调用 Property 过程](./how-to-call-a-property-procedure.md)
- [如何：在 Visual Basic 中声明和调用默认属性](./how-to-declare-and-call-a-default-property.md)
- [如何：在属性中放置值](./how-to-put-a-value-in-a-property.md)
