---
title: 如何：调用返回值的过程
ms.date: 07/20/2015
helpviewer_keywords:
- procedure calls [Visual Basic], returning values
- Visual Basic code, procedures
- procedures [Visual Basic], calling
- procedures [Visual Basic], returning a value
ms.assetid: a445127b-0f5f-465a-98fb-3e514b93d115
ms.openlocfilehash: 53589f84c6675d1e7ae2a593341e5dac747132a9
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91083972"
---
# <a name="how-to-call-a-procedure-that-returns-a-value-visual-basic"></a>如何：调用返回值的过程 (Visual Basic)

`Function`过程将值返回到调用代码。 通过在赋值语句或表达式的右侧包含其名称和参数来调用它。  
  
### <a name="to-call-a-function-procedure-within-an-expression"></a>在表达式中调用函数过程  
  
1. 按照 `Function` 使用变量的相同方式使用过程名称。 可以 `Function` 在表达式中使用变量或常量的任何位置使用过程调用。  
  
2. 在过程名称后面加上括号，以将参数列表括起来。 如果没有参数，则可以选择省略括号。 不过，使用括号可使代码更易于阅读。  
  
3. 将参数置于括号中的参数列表内，用逗号分隔。 请确保以 `Function` 过程定义相应参数的相同顺序提供参数。  
  
     或者，您可以按名称传递一个或多个参数。 有关详细信息，请参阅 [按位置和按名称传递参数](./passing-arguments-by-position-and-by-name.md)。  
  
4. 从过程返回的值将参与表达式，就像变量或常量的值一样。  
  
### <a name="to-call-a-function-procedure-in-an-assignment-statement"></a>在赋值语句中调用函数过程  
  
1. 使用 `Function` 相等 (后面的过程名称 `=`) 在赋值语句中登录。  
  
2. 在过程名称后面加上括号，以将参数列表括起来。 如果没有参数，则可以选择省略括号。 不过，使用括号可使代码更易于阅读。  
  
3. 将参数置于括号中的参数列表内，用逗号分隔。 请确保按 `Function` 过程定义相应参数的顺序提供参数，除非你按名称传递它们。  
  
4. 从过程返回的值存储在赋值语句左侧的变量或属性中。  
  
## <a name="example"></a>示例  

 下面的示例调用 Visual Basic <xref:Microsoft.VisualBasic.Interaction.Environ%2A> 以检索操作系统环境变量的值。 第一行在 `Environ` 表达式内调用，第二行在赋值语句中调用它。 `Environ` 采用变量名称作为其唯一参数。 它将变量的值返回给调用代码。  
  
 [!code-vb[VbVbcnProcedures#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#7)]  
  
## <a name="see-also"></a>请参阅

- [Function 过程](./function-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Function 语句](../../../language-reference/statements/function-statement.md)
- [如何：创建返回值的过程](./how-to-create-a-procedure-that-returns-a-value.md)
- [如何：从过程返回值](./how-to-return-a-value-from-a-procedure.md)
- [如何：调用不返回值的过程](./how-to-call-a-procedure-that-does-not-return-a-value.md)
