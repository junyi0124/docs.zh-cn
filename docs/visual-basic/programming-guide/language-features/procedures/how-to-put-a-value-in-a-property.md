---
title: 如何：在属性中放置值
ms.date: 07/20/2015
helpviewer_keywords:
- property values [Visual Basic]
- Visual Basic code, procedures
- values [Visual Basic], properties
- Visual Basic code, properties
- properties [Visual Basic], values
ms.assetid: c39401e5-b5fc-4439-8f31-ed640f7ce6ed
ms.openlocfilehash: 7d85066d4678ee2a53b8339bee2db20374482579
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071399"
---
# <a name="how-to-put-a-value-in-a-property-visual-basic"></a>如何：在属性中放置值 (Visual Basic)

通过将属性名称放在赋值语句的左侧，可以在属性中存储值。  
  
 该属性的 `Set` 过程存储一个值，但不会按名称显式调用它。 像使用变量一样使用属性。 Visual Basic 对属性的过程进行调用。  
  
### <a name="to-store-a-value-in-a-property"></a>在属性中存储值  
  
1. 使用赋值语句左侧的属性名称。  
  
     下面的示例将 Visual Basic 属性的值设置 `TimeOfDay` 为 noon，并隐式调用其 `Set` 过程。  
  
     [!code-vb[VbVbcnProcedures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#11)]  
  
2. 如果属性采用参数，请在属性名称后面加上括号，以将参数列表括起来。 如果没有参数，则可以选择省略括号。  
  
3. 将参数置于括号中的参数列表内，用逗号分隔。 请确保以属性定义相应参数的相同顺序提供参数。  
  
4. 赋值语句右侧生成的值存储在属性中。  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.DateAndTime.TimeOfDay%2A>
- [Property 过程](./property-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Property Statement](../../../language-reference/statements/property-statement.md)
- [Visual Basic 中属性和变量的差异](./differences-between-properties-and-variables.md)
- [如何：创建属性](./how-to-create-a-property.md)
- [如何：声明具有混合访问级别的属性](./how-to-declare-a-property-with-mixed-access-levels.md)
- [如何：调用 Property 过程](./how-to-call-a-property-procedure.md)
- [如何：在 Visual Basic 中声明和调用默认属性](./how-to-declare-and-call-a-default-property.md)
- [如何：从属性获取值](./how-to-get-a-value-from-a-property.md)
