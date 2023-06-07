---
title: 如何：声明具有混合访问级别的属性
ms.date: 07/20/2015
helpviewer_keywords:
- access levels [Visual Basic], properties
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- mixed access levels
- Visual Basic code, properties
- properties [Visual Basic], access levels
- Property statement [Visual Basic], declaring mixed access levels
ms.assetid: fdbb2d97-279a-4956-b26c-cbdfbc34915a
ms.openlocfilehash: 78363f7b2fb5b251f7409e53b2802baf83b05810
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91072699"
---
# <a name="how-to-declare-a-property-with-mixed-access-levels-visual-basic"></a>如何：声明具有混合访问级别的属性 (Visual Basic)

如果希望属性的 `Get` 和 `Set` 过程具有不同的访问级别，则可以在语句中使用更多的权限级别， `Property` 或在或语句中使用更严格的 `Get` 级别 `Set` 。 如果希望代码的某些部分能够获取属性的值，并且代码的某些其他部分可以更改值，则可以对属性使用混合访问级别。  
  
 有关访问级别的详细信息，请参阅 [Visual Basic 中的访问级别](../declared-elements/access-levels.md)。  
  
### <a name="to-declare-a-property-with-mixed-access-levels"></a>声明具有混合访问级别的属性  
  
1. 以正常方式声明属性，并指定限制性较低的访问级别 (如 `Public` 语句中) `Property` 。  
  
2. 声明 `Get` 或 `Set` 指定限制性更强的访问级别 (如 `Friend`) 。  
  
3. 不要指定其他属性过程的访问级别。 它假定在语句中声明了访问级别 `Property` 。 你可以仅对其中一个属性过程进行访问限制。  
  
     [!code-vb[VbVbcnProcedures#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#10)]  
  
     在前面的示例中，该 `Get` 过程具有与 `Protected` 属性本身相同的访问权限，而此 `Set` 过程具有 `Private` 访问权限。 派生自的类 `employee` 可以读取 `salary` 值，但只有 `employee` 类可以对其进行设置。  
  
## <a name="see-also"></a>请参阅

- [过程](./index.md)
- [Property 过程](./property-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Property Statement](../../../language-reference/statements/property-statement.md)
- [Visual Basic 中属性和变量的差异](./differences-between-properties-and-variables.md)
- [如何：创建属性](./how-to-create-a-property.md)
- [如何：调用 Property 过程](./how-to-call-a-property-procedure.md)
- [如何：在 Visual Basic 中声明和调用默认属性](./how-to-declare-and-call-a-default-property.md)
- [如何：在属性中放置值](./how-to-put-a-value-in-a-property.md)
- [如何：从属性获取值](./how-to-get-a-value-from-a-property.md)
