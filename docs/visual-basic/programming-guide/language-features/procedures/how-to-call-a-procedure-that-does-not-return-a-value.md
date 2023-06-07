---
title: 如何：调用不返回值的过程
ms.date: 07/20/2015
helpviewer_keywords:
- procedure calls [Visual Basic], returning values
- Visual Basic code, procedures
- procedures [Visual Basic], calling
ms.assetid: 259b49a3-a3c1-4254-ba8c-73cdc4127703
ms.openlocfilehash: 2686a4d9dc10cde209f558771feeb5ba4f4ccb21
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91075000"
---
# <a name="how-to-call-a-procedure-that-does-not-return-a-value-visual-basic"></a>如何：调用不返回值的过程 (Visual Basic)

`Sub`过程不会将值返回到调用代码。 您可以使用独立的调用语句显式调用它。 不能只是在表达式中使用其名称来调用它。  
  
### <a name="to-call-a-sub-procedure"></a>调用 Sub 过程  
  
1. 指定过程的名称 `Sub` 。  
  
2. 在过程名称后面加上括号，以将参数列表括起来。 如果没有参数，则可以选择省略括号。 不过，使用括号可使代码更易于阅读。  
  
3. 将参数置于括号中的参数列表内，用逗号分隔。 请确保以 `Sub` 过程定义相应参数的相同顺序提供参数。  
  
     下面的示例调用 Visual Basic <xref:Microsoft.VisualBasic.Interaction.AppActivate%2A> 函数来激活应用程序窗口。 <xref:Microsoft.VisualBasic.Interaction.AppActivate%2A> 使用窗口标题作为其唯一参数。 它不会将值返回到调用代码。 如果记事本进程未运行，则该示例引发 <xref:System.ArgumentException> 。 此 `Shell` 过程假定应用程序位于指定的路径中。  
  
     [!code-vb[VbVbalrCatRef#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCatRef/VB/Class1.vb#11)]  
  
## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.Interaction.Shell%2A>
- <xref:System.ArgumentException>
- [过程](./index.md)
- [Sub 过程](./sub-procedures.md)
- [过程形参和实参](./procedure-parameters-and-arguments.md)
- [Sub 语句](../../../language-reference/statements/sub-statement.md)
- [如何：创建过程](./how-to-create-a-procedure.md)
- [如何：调用返回值的过程](./how-to-call-a-procedure-that-returns-a-value.md)
- [如何：在 Visual Basic 中调用事件处理程序](./how-to-call-an-event-handler.md)
